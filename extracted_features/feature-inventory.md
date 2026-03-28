---
type: feature-inventory
source_app: Lisa General_Demo (CFO vibe-coded accounting app)
extracted_by: Windsurf/Claude
purpose: Claude-optimized context doc for planning tool ingestion
related_docs:
  - user-journeys.md
  - classification-rules.md
  - excel-output-mapping.md
---

# Feature Inventory — Statement-to-Trial-Balance App

Detailed feature specifications extracted from Lisa's accounting demo. Each feature includes an ID, description, acceptance criteria, source location in Lisa's code, and which user journey(s) exercise it.

---

## 1. PDF Ingestion

### F-ING-01: PDF Text Extraction

**Description:** Extract raw text from PDF bank and credit card statements using pdfplumber. Supports multi-page documents. Text is extracted page-by-page and concatenated into a list of lines.

**Source:**
- `ingestion_core/demo_utils.py` → `extract_text_lines()`
- `plumber_dump.py` (standalone extraction for layout analysis)
- `pdfplumber.bat` (batch runner for Plumber_Sample folder)

**Acceptance Criteria:**
- Given a text-based PDF, extract all text lines across all pages
- Lines are normalized (whitespace cleaned, empty lines removed)
- If extraction produces no text or `(cid:xx)` artifacts, fail with a clear error
- Support both bank statements and credit card/LOC statements

**Journeys:** J1 (step 2), J2 (step 4b)

---

### F-ING-02: Plugin-Based Statement Type Detection

**Description:** Automatically determine which institution/format a PDF belongs to by running all registered plugins against the extracted text. Each plugin scores its confidence (0.0–1.0) based on keyword matching. The highest-scoring plugin above the minimum threshold (0.60) is selected.

**Source:**
- `ingestion_core/detect.py` → `detect_plugin()`
- Each plugin's `detect()` method (e.g., `ingestion_core/plugins/bmo_bank_v1.py`)

**Acceptance Criteria:**
- Run all registered plugins against the first ~250 lines of extracted text
- Select the plugin with the highest confidence score
- If no plugin exceeds the minimum confidence threshold (0.60), raise a `ValueError` with a ranked list of the top 3 candidates and their scores
- Return both the winning plugin and its confidence score
- Detection is based on statement content (keyword matching), not filename

**Journeys:** J1 (step 6), J2 (step 4c)

---

### F-ING-03: Auto-Discovery of Input PDFs

**Description:** When no `run_list.csv` is present, the system auto-discovers all PDF files in the `inputs/` directory, ingests each one through the plugin pipeline, and extracts month/opening/closing/account metadata automatically.

**Source:**
- `lbty_master_runner.py` → `main()` lines 1264–1304

**Acceptance Criteria:**
- Scan `inputs/` for all `.pdf` files (sorted alphabetically)
- For each PDF: extract text → detect plugin → parse → extract period, opening balance, closing balance, account label
- Look up payroll overrides from `overrides.csv` by matching month + account_label
- Build a run manifest equivalent to what `run_list.csv` would provide
- Fail if no PDFs found

**Journeys:** J2 (step 4a)

---

### F-ING-04: Manual Run List (run_list.csv)

**Description:** Alternative to auto-discovery. A CSV file specifying which PDFs to process with pre-set values for month, opening/closing balances, and payroll totals.

**Source:**
- `lbty_master_runner.py` → `main()` lines 1257–1262

**Acceptance Criteria:**
- CSV columns: `month, pdf_filename, opening, closing, payroll_total`
- All columns required; system exits if any are missing
- Values override what the parser would extract
- Supports batch processing of multiple statements in a single run

**Journeys:** J2 (step 4a, alternative path)

---

## 2. Parsing

### F-PAR-01: Institution-Specific Deterministic Parsers

**Description:** Each supported institution has a dedicated parser that understands its specific PDF layout. Parsers extract transactions, balances, and metadata using deterministic pattern matching (regex, positional logic).

**Source:**
- `ingestion_core/parsers/bmo_bank_parser.py` — BMO Business Account
- `ingestion_core/parsers/cibc_bank_parser.py` — CIBC Bank
- `ingestion_core/parsers/cibc_visa_parser.py` — CIBC Visa Credit Card
- `ingestion_core/parsers/coastcapital_loc_parser.py` — Coast Capital Line of Credit

**Acceptance Criteria:**
- Each parser extracts: period (YYYY-MM), account_id, opening_balance, closing_balance, transaction rows (date, description, signed amount), instrument_type
- Parser handles date format variations per institution
- Parser handles wrapped transaction lines (descriptions spanning multiple lines)
- Parser ignores summary rows, headers, and non-transaction content
- Parser returns a `CanonicalStatement` object

**Journeys:** J1 (step 5), J2 (step 4d)

---

### F-PAR-02: Canonical Statement Model

**Description:** All parsed statements are normalized into a common `CanonicalStatement` data model regardless of source institution. This decouples parsing from downstream accounting logic.

**Source:**
- `ingestion_core/models.py` → `CanonicalStatement`, `CanonicalTransaction`
- `ingestion_core/enums.py` → `InstrumentType`, `TxnRole`

**Data Model:**
```
CanonicalStatement:
  statement_id: str (hash-based)
  source_file: str
  institution: str (e.g., "BMO", "CIBC")
  parser_id: str (e.g., "bmo_bank_v1")
  parser_confidence: float (0.0–1.0)
  instrument_type: BANK_ASSET | CREDIT_CARD_LIABILITY
  account_id: str (stable, e.g., "BMO_Account27371997_460")
  period: str (YYYY-MM)
  opening_balance: Decimal
  closing_balance: Decimal
  currency: str (default "CAD")
  statement_start_date: date
  statement_end_date: date
  transactions: List[CanonicalTransaction]
  metadata: dict

CanonicalTransaction:
  txn_id: str (hash-based)
  post_date: date
  description: str
  signed_amount: Decimal (+ = money in for bank, + = charge for card)
  role: DEPOSIT | WITHDRAWAL | TRANSFER_IN | TRANSFER_OUT | CHARGE | PAYMENT | INTEREST | REFUND | FEE | ADJUSTMENT
  raw_text: str
  section: str (optional)
  metadata: dict
```

**Acceptance Criteria:**
- All institution parsers produce `CanonicalStatement` objects
- Signed amount convention is consistent: BANK_ASSET: + = deposit, - = withdrawal; CREDIT_CARD_LIABILITY: + = charge, - = payment/refund
- Each transaction has a stable hash-based ID for traceability
- Metadata dict allows institution-specific fields without polluting the core schema

**Journeys:** J1 (step 5), J2 (step 4d)

---

### F-PAR-03: Multi-Instrument Type Support

**Description:** The system supports two fundamentally different financial instrument types with different polarity logic: bank accounts (assets) and credit cards/lines of credit (liabilities).

**Source:**
- `ingestion_core/enums.py` → `InstrumentType`
- `ingestion_core/adapters/canonical_to_txn.py` → `canonical_statement_to_txns()`
- `lbty_master_runner.py` → reconciliation logic (lines 1372–1381), journal builder (lines 396–406)

**Polarity Rules:**
| Instrument | Statement Debit (money out) | Statement Credit (money in) | Reconciliation Formula |
|---|---|---|---|
| BANK_ASSET | Ledger Debit (cash decreases) | Ledger Credit (cash increases) | opening + credits - debits = closing |
| CREDIT_CARD_LIABILITY | Ledger Debit (liability increases / charge) | Ledger Credit (liability decreases / payment) | opening + debits - credits = closing |

**Acceptance Criteria:**
- Canonical-to-Txn adapter correctly maps signed amounts to Debit/Credit columns based on instrument type
- Reconciliation formula switches based on instrument type
- Journal builder sets control account type (Asset vs Liability) based on instrument type
- Classification rules behave differently for liability instruments (credits are NOT income)

**Journeys:** J1 (step 1), J2 (step 4e, 4h)

---

### F-PAR-04: Parser Scaffolding Tool

**Description:** CLI tool to generate boilerplate plugin and parser files for a new institution.

**Source:**
- `tools/new_statement.py`

**Acceptance Criteria:**
- Given a plugin_id, parser_module name, and detection keywords, generates:
  - A plugin file in `ingestion_core/plugins/`
  - A parser file in `ingestion_core/parsers/`
- Generated files have the correct structure and imports
- Detection keywords are wired into the plugin's `detect()` method

**Journeys:** J1 (step 4)

---

## 3. Transaction Classification

### F-CLS-01: Sequenced Rule Engine

**Description:** Transactions are classified by a rule engine that applies rules in a defined sequence. Rules are grouped into tiers: deterministic (Green), pattern-based (Orange), fallback (Red), and catch-all. Each rule assigns a Category, Confidence level, RuleID, and AccountType. A rule only applies if the transaction has not already been classified by a higher-priority rule.

**Source:**
- `lbty_master_runner.py` → `apply_rules()` (lines 111–250)

**Acceptance Criteria:**
- Rules execute in defined sequence order (1 → 2 → 3 → 4)
- First match wins — once a transaction has a Category, subsequent rules skip it
- Every transaction gets classified (catch-all fallback: "For Accountant Review")
- Each classified transaction carries: Category, Confidence (Green/Orange/Red), RuleID, AccountType

**Journeys:** J2 (step 4f)

**See also:** `classification-rules.md` for the complete rule table and logic.

---

### F-CLS-02: Confidence Tier System

**Description:** Each classification carries a confidence level indicating how reliable the match is.

**Tiers:**
| Tier | Color | Meaning | Example |
|---|---|---|---|
| 1 | Green | Deterministic keyword match, high confidence | "ICBC" → Auto Insurance |
| 2 | Orange | Pattern-based or heuristic, medium confidence | Amount range + day-of-month → Rent |
| 3 | Red | Weak match or residual, needs review | Remaining credits → Income |
| 4 | Red | No match at all, requires manual classification | Fallback → For Accountant Review |

**Source:**
- `lbty_master_runner.py` → `apply_rules()`, `rule_register_df()`

**Acceptance Criteria:**
- Every classified transaction has a Confidence value (Green, Orange, or Red)
- The RULES tab in the output Excel documents the confidence tier for each rule
- Red-confidence transactions are flagged for accountant review

**Journeys:** J2 (step 4f), J3 (step 2)

---

### F-CLS-03: Moneris Revenue / GST Split

**Description:** Moneris settlement deposits are automatically split into two components: net revenue (Income) and GST Payable (5% of monthly Moneris gross). GST is computed on the monthly total and allocated per transaction, with the last transaction absorbing rounding remainder.

**Source:**
- `lbty_master_runner.py` → `split_gst_monthly()` (lines 253–312)
- Rule LBTY-010A stages Moneris credits, then the split function replaces them

**Acceptance Criteria:**
- Identify all Moneris settlement credits in a month
- Compute total monthly Moneris gross
- GST = 5% of monthly gross, rounded to 2 decimal places
- Allocate GST per transaction proportionally; last transaction gets rounding remainder
- Replace each Moneris row with two rows: Income (net) + GST Payable
- Original Moneris description preserved; GST row appended with " - GST 5%"
- Monthly GST totals are exact (no rounding drift)

**Journeys:** J2 (step 4g)

---

### F-CLS-04: Payroll Subset-Sum Matching

**Description:** Given a known payroll total (from overrides), find the combination of INTERAC-sent transfers in the $1,500–$2,500 range that sum exactly to the payroll total. Uses brute-force combinatorial search.

**Source:**
- `lbty_master_runner.py` → `apply_rules()` step 5 (lines 168–192)

**Acceptance Criteria:**
- Only applies when a payroll_total override is provided for the month
- Candidates: uncategorized debit transactions with "INTERACe-TransferSent" in description, amount between $1,500 and $2,500
- Find the smallest subset of candidates whose debits sum exactly to the payroll total
- If a solution exists, classify those transactions as "Payroll" (Orange, LBTY-012)
- If no exact match, leave candidates uncategorized for downstream rules

**Journeys:** J2 (step 4f)

---

### F-CLS-05: Rent Heuristic Detection

**Description:** Detect rent payments based on amount range and date. A single uncategorized debit of $3,000–$3,500 on day 1 or 2 of the month is classified as Rent.

**Source:**
- `lbty_master_runner.py` → `apply_rules()` step 6 (lines 195–204)

**Acceptance Criteria:**
- Candidate: uncategorized debit, amount $3,000–$3,500, posted on day 1 or 2 of the month
- Only applies if there is exactly ONE candidate (ambiguity → skip)
- Classified as "Rent" (Orange, LBTY-011)

**Journeys:** J2 (step 4f)

---

### F-CLS-06: Liability Instrument Credit Handling

**Description:** For credit card/LOC statements, credits are payments or refunds — NOT income. This rule must run before the "remaining credits → Income" fallback to prevent misclassification.

**Source:**
- `lbty_master_runner.py` → `apply_rules()` step 9 (lines 221–228)

**Acceptance Criteria:**
- Only applies to transactions from `CREDIT_CARD_LIABILITY` instruments
- Uncategorized credits are classified as the source account name (e.g., "Coast LOC 100616024137 (200)") with AccountType = Liability
- This prevents card payments from being classified as Income

**Journeys:** J2 (step 4f)

---

### F-CLS-07: Overrides System

**Description:** Per-month, per-account overrides allow injecting known values (e.g., payroll totals) that the parser cannot extract from the PDF.

**Source:**
- `overrides.csv`
- `lbty_master_runner.py` → auto-discovery payroll override lookup (lines 1286–1293)

**Acceptance Criteria:**
- CSV columns: `month, account_label, payroll_total`
- Matched by exact month (YYYY-MM) and account_label
- Override values feed into downstream classification (e.g., payroll subset-sum)
- File is optional; if absent, no overrides applied

**Journeys:** J2 (step 3)

---

## 4. Double-Entry Accounting

### F-ACC-01: Ledger-to-Journal Conversion

**Description:** Convert the classified ledger (single-entry with Category) into a proper double-entry journal. Each ledger transaction produces two journal lines: one for the control account (bank/card) and one for the counter account (classified category).

**Source:**
- `lbty_master_runner.py` → `build_journal_from_ledger()` (lines 367–509)

**Acceptance Criteria:**
- Every ledger debit produces: DR counter account (Category), CR control account (SourceAccount)
- Every ledger credit produces: DR control account, CR counter account
- Control account AccountType is determined by InstrumentType (BANK_ASSET → Asset, CREDIT_CARD_LIABILITY → Liability)
- Counter account AccountType comes from the classification rule
- Journal carries forward: Month, Date, SourceTxnID, Description, Confidence, RuleID, SourceAccount, CounterpartyAccount, Category
- Total journal Debits = Total journal Credits (always)

**Journeys:** J2 (step 5b)

---

### F-ACC-02: Opening Balance Journal (CSV-Based)

**Description:** Build opening balance journal entries from a manually maintained CSV file. Each opening balance produces a double-entry: the account itself and an offsetting equity entry. If the opening balances don't net to zero, an "Opening Balance Offset" equity line absorbs the difference.

**Source:**
- `lbty_master_runner.py` → `build_opening_journal_from_csv()` (lines 648–758)
- `opening_balances.csv`

**Acceptance Criteria:**
- CSV columns: `Account, AccountType, OpeningBalance`
- Asset: positive = DR; Liability/Equity: positive = CR
- Offsetting entry posted to "Opening Balance Offset" (Equity)
- If net of all opening entries ≠ 0, the offset line makes the journal balance
- Opening entries stamped with Month="OPENING", Date="1900-01-01"
- Comment lines (starting with `#`) in the CSV are allowed
- Opening balances are explicit — system never auto-derives them

**Journeys:** J2 (step 5c)

---

### F-ACC-03: Opening Balance Journal (Auto-Computed)

**Description:** Automatically compute opening balance journal entries from the first statement's opening balance for each source account. Posts a double-entry against "Retained Earnings (Opening)".

**Source:**
- `lbty_master_runner.py` → `build_opening_balance_journal()` (lines 511–619)

**Note:** In the current codebase, the CSV-based approach (F-ACC-02) is used in `main()`. This auto-computed version exists as a function but is not called in the main pipeline. It represents a design intent for automated opening balance derivation.

**Acceptance Criteria:**
- For each SourceAccount, find the earliest month in the ledger
- Look up that month's StatementOpeningBalance from bank reconciliation records
- BANK_ASSET: opening > 0 → DR Asset, CR Equity; opening < 0 → reverse
- CREDIT_CARD_LIABILITY: opening > 0 (owed) → DR Equity, CR Liability; opening < 0 → reverse
- Skip accounts with opening balance of 0.00

**Journeys:** J2 (step 5c, alternative path)

---

### F-ACC-04: Trial Balance

**Description:** Aggregate all journal entries by Account to produce a trial balance. Sum of all Debits must equal sum of all Credits.

**Source:**
- `lbty_master_runner.py` → `build_trial_balance()` (lines 622–646)

**Acceptance Criteria:**
- Group journal by Account, sum Debit and Credit columns
- Compute Net = Debit - Credit per account
- Append a TOTAL row with column sums
- TOTAL Debit must equal TOTAL Credit (invariant)
- Sort accounts alphabetically

**Journeys:** J2 (step 5d), J3 (step 4)

---

### F-ACC-05: Balance Sheet

**Description:** Build a balance sheet from the trial balance with proper sign conventions and a retained earnings roll-forward. No equity plug — if it doesn't balance, the Check line shows the imbalance.

**Source:**
- `lbty_master_runner.py` → `build_balance_sheet()` (lines 1089–1193)
- `compute_net_income_by_year()` (lines 988–1018)

**Acceptance Criteria:**
- Assets: positive balance = Debit - Credit
- Liabilities/Equity: positive balance = Credit - Debit
- Retained Earnings roll-forward:
  - Opening RE = sum of net income for all fiscal years before the latest
  - Current Net Income = net income for the latest fiscal year
  - Closing RE = Opening + Current
- Total Equity = existing equity accounts + Closing RE
- Check row: `Assets - Liabilities - Equity` must equal 0.00
- No synthetic equity plug or computed retained earnings hacks
- Sections: Assets, Liabilities, Equity (with roll-forward), Check

**Journeys:** J2 (step 5f), J3 (step 6)

---

### F-ACC-06: Reconciliation Control

**Description:** Per-statement reconciliation verifying that extracted transactions match the statement's opening and closing balances. Formula differs by instrument type.

**Source:**
- `lbty_master_runner.py` → per-statement reconciliation (lines 1368–1396)
- `build_reconciliation()` (lines 867–911) for year-level reconciliation

**Acceptance Criteria:**
- BANK_ASSET: `opening + credits - debits = closing`
- CREDIT_CARD_LIABILITY: `opening + debits - credits = closing`
- Diff = computed closing - statement closing, must be 0.00
- If diff ≠ 0.00, the system logs an error (currently commented-out hard stop)
- Year-level reconciliation aggregates monthly values and includes journal control totals
- Reconciliation tab shows: Statement Opening, Total Credits, Total Debits, Computed Closing, Statement Closing, Diff, plus Journal Debits/Credits/Diff

**Journeys:** J2 (step 4h, 5g), J3 (step 5)

---

## 5. Financial Reporting

### F-RPT-01: P&L by Fiscal Year

**Description:** Profit & Loss statement built from journal lines with AccountType = Income or Expense, grouped by fiscal year and month. Fiscal year start month is configurable (default: February, labeled by ending year).

**Source:**
- `lbty_master_runner.py` → `build_pl_matrix_from_journal()` (lines 914–986)
- `fiscal_year_for_month()` (lines 61–70)

**Acceptance Criteria:**
- Filter journal to Income and Expense account types only
- Income amount = Credit - Debit (per account per month)
- Expense amount = Debit - Credit (per account per month)
- Pivot: rows = accounts, columns = months + Total
- Sections: Revenue (detail + Total Revenue), Expenses (detail + Total Expenses), Net Income
- One P&L tab per fiscal year present in the data
- Fiscal year grouping: month ≥ start_month → next calendar year (e.g., Feb 2024 → FY2025)

**Journeys:** J2 (step 5e), J3 (step 7)

---

### F-RPT-02: Excel Workbook Output

**Description:** All outputs are written to a single Excel workbook with multiple tabs. Each tab has auto-fitted column widths and numeric formatting on money columns.

**Source:**
- `lbty_master_runner.py` → `write_workbook()` (lines 1198–1237)

**Acceptance Criteria:**
- Output path: `output/LBTY_Master_Ledger.xlsx`
- Tabs created in defined order (see excel-output-mapping.md for full list)
- Column widths auto-fit based on content (max 50 characters)
- Money columns formatted with 2 decimal places
- Sheet names truncated to 31 characters (Excel limit)

**Journeys:** J2 (step 6)

---

### F-RPT-03: Run Summary

**Description:** Per-statement intake and reconciliation summary showing the PDF filename, source account, balances, extracted totals, diff, and transaction count.

**Source:**
- `lbty_master_runner.py` → `build_run_summary()` (lines 760–865)

**Acceptance Criteria:**
- One row per ingested statement
- Columns: Month, PDF, SourceAccount, StatementOpeningBalance, TotalCreditsExtracted, TotalDebitsExtracted, ComputedClosingBalance, StatementClosingBalance, Diff, TxnCount
- Sorted by Month then SourceAccount then PDF
- Quick overview for spotting which statement has issues

**Journeys:** J2 (step 5k), J3 (step 10)

---

### F-RPT-04: Diagnostic Sheet

**Description:** Account-level diagnostic showing each account's net balance, AccountType, and whether it contributes to Net Income. Helps trace imbalance sources.

**Source:**
- `lbty_master_runner.py` → `build_diagnostic_sheet()` (lines 1027–1052)

**Acceptance Criteria:**
- Columns: Account, AccountType, Debit, Credit, Net, IncludedInNI
- AccountType derived from journal (mode/most common type for each account)
- IncludedInNI = True for Income/Expense accounts
- Sorted alphabetically by Account

**Journeys:** J2 (step 5h), J3 (implicit)

---

### F-RPT-05: Exception Report

**Description:** Flags Income/Expense accounts that have both debit AND credit activity, which may indicate refunds/reversals or misclassification.

**Source:**
- `lbty_master_runner.py` → `build_exceptions_journal()` (lines 1054–1087)

**Acceptance Criteria:**
- Filter journal to Income and Expense account types
- Find accounts where both sum(Debit) > 0 and sum(Credit) > 0
- Return the underlying journal lines for those flagged accounts
- Columns: Month, Date, AccountType, Account, Debit, Credit, Description, SourceAccount, CounterpartyAccount, SourceTxnID
- Empty result is valid (no exceptions found)

**Journeys:** J2 (step 5i), J3 (step 8)

---

## 6. Audit & Traceability

### F-AUD-01: Source Transaction ID (SourceTxnID)

**Description:** Each ledger transaction receives a stable hash-based ID derived from Month, Date, Description, Debit, and Credit. This ID carries through from ledger to journal for audit trail linkage.

**Source:**
- `lbty_master_runner.py` → `add_source_txn_id()` (lines 347–364)

**Acceptance Criteria:**
- ID = first 12 characters of SHA1 hash of `Month|Date|Description|Debit|Credit`
- Stable across re-runs if input data is unchanged
- Carried through: Ledger → Journal → Rule Hits → Exceptions
- Sufficient for demo + audit trail; upgradeable to persistent keys later

**Journeys:** J2 (step 5a)

---

### F-AUD-02: Rule Register

**Description:** A tabular register of all classification rules showing their ID, sequence, trigger pattern, assigned account, account type, and confidence tier.

**Source:**
- `lbty_master_runner.py` → `rule_register_df()` (lines 316–338)

**Acceptance Criteria:**
- Columns: RuleID, Sequence, Trigger, AccountAssigned, AccountType, Confidence
- One row per rule (20 rules in current implementation)
- Written to the RULES tab in the output Excel
- Serves as documentation of the classification logic

**Journeys:** J3 (step 9)

---

### F-AUD-03: Rule Hit Tracking

**Description:** For each ledger transaction that was classified by a rule, record which rule matched. Enables audit of classification decisions.

**Source:**
- `lbty_master_runner.py` → `build_rule_hits()` (lines 1020–1025)

**Acceptance Criteria:**
- Filter ledger to rows where RuleID is non-empty
- Columns: SourceTxnID, Month, Date, Description, Debit, Credit, RuleID, Category, Confidence
- Written to the RULE_HITS tab in the output Excel
- Every classified transaction should appear (all transactions get a rule, including fallback)

**Journeys:** J3 (step 9)

---

### F-AUD-04: Statement ID Hashing

**Description:** Each parsed statement and transaction receives a hash-based ID for deduplication and traceability.

**Source:**
- `ingestion_core/plugins/bmo_bank_v1.py` → `_hash16()` (used for statement_id and txn_id)

**Acceptance Criteria:**
- Statement ID: SHA256 hash of institution + account_id + period + source_file (first 16 chars)
- Transaction ID: SHA256 hash of source_file + period + date + description + amount + index (first 16 chars)
- IDs are deterministic given the same inputs

**Journeys:** J2 (step 4d)

---

### F-AUD-05: Layout Archive and Version History

**Description:** When analyzing a new statement type with pdfplumber, the extracted text is archived with a timestamp. This creates a permanent record of the PDF layout for regression testing and format change detection.

**Source:**
- `plumber_dump.py`
- `Samples_Library/` folder structure

**Acceptance Criteria:**
- Timestamped extraction file: `<filename>__plumber__<YYYY-MM-DD_HHMMSS>.txt`
- `latest__plumber.txt` overwritten each run (for active development)
- Auto-classified into `Bank-<Institution>/` or `Card-<Institution>/` folders
- Classification based on filename keywords (visa, mastercard, amex, credit, card → Card; else → Bank)
- Archive is never overwritten; provides historical layout record

**Journeys:** J1 (step 8)

---

## 7. Configuration

### F-CFG-01: Opening Balances Configuration

**Description:** Manual CSV file defining starting balances for each account, used to seed the journal before transaction activity.

**Source:**
- `opening_balances.csv`
- `lbty_master_runner.py` → `build_opening_journal_from_csv()`

**Acceptance Criteria:**
- CSV columns: `Account, AccountType, OpeningBalance`
- AccountType: asset, liability, or equity (case-insensitive)
- Account name must exactly match what appears in Journal
- Comment lines (starting with `#`) allowed
- System will NOT auto-derive opening balances
- If opening balances don't net to zero, an offset equity line is created

**Journeys:** J2 (step 2)

---

### F-CFG-02: Fiscal Year Configuration

**Description:** Configurable fiscal year start month. Determines how months are grouped into fiscal years for P&L and reconciliation reporting.

**Source:**
- `lbty_master_runner.py` → `FISCAL_YEAR_START_MONTH = 2` (line 81)
- `fiscal_year_for_month()` (lines 61–70)

**Acceptance Criteria:**
- Fiscal year start month is a single integer (1–12)
- Month ≥ start_month → belongs to next calendar year's fiscal year
- Example: start_month=2, Feb 2024 → FY2025; Jan 2025 → FY2025
- Opening balance entries (Month="OPENING") return fiscal year 0 (excluded from P&L)

**Journeys:** J2 (step 5e, 5g)

---

### F-CFG-03: Payroll Overrides Configuration

**Description:** CSV file providing known payroll totals per month per account, used by the payroll subset-sum matching rule.

**Source:**
- `overrides.csv`
- `lbty_master_runner.py` → override lookup (lines 1286–1293)

**Acceptance Criteria:**
- CSV columns: `month, account_label, payroll_total`
- Matched by exact `month` (YYYY-MM) and `account_label`
- If no match found, payroll subset-sum rule is skipped for that month
- File is optional

**Journeys:** J2 (step 3)

---

### F-CFG-04: Plugin Registration

**Description:** Plugins are registered by placing them in the `ingestion_core/plugins/` directory. A loader discovers and instantiates all plugins at startup.

**Source:**
- `ingestion_core/plugins/loader.py`
- Each plugin file exports a `PLUGIN` module-level variable

**Acceptance Criteria:**
- Loader scans `ingestion_core/plugins/` for Python files
- Each plugin file must export a `PLUGIN` variable that is a `StatementPlugin` instance
- All discovered plugins are available for statement detection
- Adding a new plugin requires only creating a new file — no registration code changes

**Journeys:** J1 (step 4, 6), J2 (step 4c)

---

## Feature Cross-Reference by Journey

| Feature ID | J1: Onboard | J2: Ingest | J3: Review |
|---|:---:|:---:|:---:|
| F-ING-01 | ✓ | ✓ | |
| F-ING-02 | ✓ | ✓ | |
| F-ING-03 | | ✓ | |
| F-ING-04 | | ✓ | |
| F-PAR-01 | ✓ | ✓ | |
| F-PAR-02 | ✓ | ✓ | |
| F-PAR-03 | ✓ | ✓ | |
| F-PAR-04 | ✓ | | |
| F-CLS-01 | | ✓ | |
| F-CLS-02 | | ✓ | ✓ |
| F-CLS-03 | | ✓ | |
| F-CLS-04 | | ✓ | |
| F-CLS-05 | | ✓ | |
| F-CLS-06 | | ✓ | |
| F-CLS-07 | | ✓ | |
| F-ACC-01 | | ✓ | |
| F-ACC-02 | | ✓ | |
| F-ACC-03 | | ✓ | |
| F-ACC-04 | | ✓ | ✓ |
| F-ACC-05 | | ✓ | ✓ |
| F-ACC-06 | | ✓ | ✓ |
| F-RPT-01 | | ✓ | ✓ |
| F-RPT-02 | | ✓ | |
| F-RPT-03 | | ✓ | ✓ |
| F-RPT-04 | | ✓ | ✓ |
| F-RPT-05 | | ✓ | ✓ |
| F-AUD-01 | | ✓ | |
| F-AUD-02 | | | ✓ |
| F-AUD-03 | | | ✓ |
| F-AUD-04 | | ✓ | |
| F-AUD-05 | ✓ | | |
| F-CFG-01 | | ✓ | |
| F-CFG-02 | | ✓ | |
| F-CFG-03 | | ✓ | |
| F-CFG-04 | ✓ | ✓ | |
