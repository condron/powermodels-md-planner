---
type: user-journeys
source_app: Lisa General_Demo (CFO vibe-coded accounting app)
extracted_by: Windsurf/Claude
purpose: Claude-optimized context doc for planning tool ingestion
related_docs:
  - feature-inventory.md
  - classification-rules.md
  - excel-output-mapping.md
---

# User Journeys — Statement-to-Trial-Balance Workflow

This document captures three end-to-end user journeys extracted from Lisa's accounting demo app. The app ingests PDF bank/credit card statements, classifies transactions via a rule engine, produces double-entry journals, and outputs a multi-tab Excel workbook for accountant review.

---

## J1: Onboard a New Statement Type

**Actor:** Accountant / Power User  
**Goal:** Add support for a previously unseen bank or credit card statement format so it can be processed by the ingestion pipeline.  
**Preconditions:**
- A sample PDF of the new statement type is available
- The app environment is set up (Python, pdfplumber, etc.)

### Steps

1. **Classify the instrument type**
   - Determine whether the statement is `BANK_ASSET` or `CREDIT_CARD_LIABILITY`
   - This determines polarity logic downstream (bank credits = deposits; card credits = payments/refunds)

2. **Extract raw text for layout analysis**
   - Place exactly ONE sample PDF into the `Plumber_Sample/` folder
   - Run `pdfplumber.bat`
   - System extracts raw text (first 3 pages), auto-classifies as Bank or Card based on filename keywords, creates a standardized archive folder under `Samples_Library/`, and saves a timestamped extraction file plus `latest__plumber.txt`
   - **Decision point:** Is the text readable? Are transaction lines visible? No `(cid:xx)` garbage? If extraction fails, the PDF is likely image-scanned and cannot be processed.

3. **Analyze the text layout**
   - Open the `latest__plumber.txt` output
   - Identify: date format, amount column positions, balance column, wrapped lines, summary rows to ignore, opening/closing balance locations

4. **Generate parser scaffold**
   - Run: `python tools\new_statement.py <plugin_id> <parser_module> "Keyword1" "Keyword2"`
   - Example: `python tools\new_statement.py rbc_bank_v1 rbc_bank_parser "Royal Bank" "Account Statement"`
   - This creates a plugin file in `ingestion_core/plugins/` and a parser file in `ingestion_core/parsers/`

5. **Implement the parser**
   - In the parser module, implement extraction of:
     - `period` (YYYY-MM)
     - `account_id` (stable identifier)
     - `opening_balance`
     - `closing_balance`
     - Transaction rows (date, description, signed amount)
     - `instrument_type`
   - Return a `CanonicalStatement` object

6. **Implement detection keywords in the plugin**
   - In the plugin's `detect()` method, add keyword matching against the first ~250 lines of text
   - Return a confidence score (0.0–1.0) and reason string
   - Minimum confidence threshold for auto-detection is 0.60

7. **Test with a single statement**
   - Place the sample PDF in `inputs/`
   - Run `python lbty_master_runner.py`
   - Validate all accounting invariants (journal balances, trial balance balances, reconciliation diff = 0.00)
   - Do NOT modify journal logic unless structurally required

8. **Archive the sample**
   - The timestamped plumber output in `Samples_Library/` serves as a permanent layout record
   - If the bank changes their format later, this archive enables regression testing and debugging

### Outputs
- New plugin file: `ingestion_core/plugins/<institution>_v1.py`
- New parser file: `ingestion_core/parsers/<institution>_parser.py`
- Archived extraction text in `Samples_Library/<Bank|Card>-<Institution>/`
- Successfully ingested test statement in the Excel output

### Features Exercised
- `F-ING-01` PDF text extraction
- `F-ING-02` Plugin-based statement detection
- `F-PAR-01` Institution-specific parser
- `F-PAR-02` Canonical statement model
- `F-PAR-03` Multi-instrument type support
- `F-AUD-05` Layout archive and version history

---

## J2: Monthly Statement Ingestion

**Actor:** Accountant  
**Goal:** Process one or more monthly PDF statements into a classified, reconciled Excel workbook.  
**Preconditions:**
- Parsers exist for all statement types being ingested
- `opening_balances.csv` is up to date for any new accounts
- `overrides.csv` has payroll totals if payroll subset-sum matching is needed

### Steps

1. **Prepare input PDFs**
   - Place PDF statements into `inputs/` folder
   - Recommended naming: `<Institution>_<YYYY>.<MM>.<DD>.pdf` (e.g., `BMO_2024.01.31.pdf`)
   - System determines instrument type automatically — do NOT prefix with "Bank-"
   - Ensure only the statements you want processed are in the folder

2. **Set opening balances (if new accounts)**
   - Edit `opening_balances.csv` with columns: `Account, AccountType, OpeningBalance`
   - Account name must exactly match what appears in the Journal
   - Asset: positive = debit balance; Liability: positive = amount owed; Equity: positive = credit balance
   - Comment lines starting with `#` are allowed
   - **Decision point:** Is this the first statement for a new account? If yes, add the opening balance. System will NOT auto-derive opening balances.

3. **Set overrides (if needed)**
   - Edit `overrides.csv` with columns: `month, account_label, payroll_total`
   - This provides the known payroll total for subset-sum matching against INTERAC transfers

4. **Run ingestion**
   - Execute `python lbty_master_runner.py` (or `Run_LBTY.bat`)
   - System performs the following pipeline per PDF:
     - a. **Auto-discover** PDFs in `inputs/` (or read `run_list.csv` if present)
     - b. **Extract text** via pdfplumber
     - c. **Detect statement type** by running all plugins and selecting highest confidence match (≥0.60)
     - d. **Parse** into `CanonicalStatement` (period, opening/closing balance, transactions)
     - e. **Convert** canonical transactions to ledger `Txn` objects with debit/credit columns
     - f. **Apply classification rules** (sequenced: deterministic → pattern → fallback → catch-all)
     - g. **Split Moneris deposits** into Income + GST Payable (5% of monthly gross)
     - h. **Reconciliation control** per statement: verify `opening + credits - debits = closing` (bank) or `opening + debits - credits = closing` (card). Diff must be 0.00.

5. **Build accounting outputs**
   - System automatically builds from the classified ledger:
     - a. **Add SourceTxnID** (SHA1-based hash for audit trail)
     - b. **Double-entry journal** from ledger (control account = source account, counter account = classified category)
     - c. **Opening balance journal** from `opening_balances.csv` with offsetting equity entries
     - d. **Trial balance** (debits must equal credits)
     - e. **P&L by fiscal year** (fiscal year starts in month configured, default February)
     - f. **Balance sheet** with retained earnings roll-forward (no equity plug)
     - g. **Reconciliation** per fiscal year
     - h. **Diagnostics** (account-level net balances with AccountType)
     - i. **Exceptions** (Income/Expense accounts with both debit and credit activity)
     - j. **Rule audit trail** (which rule matched each transaction)
     - k. **Run summary** (per-statement reconciliation overview)

6. **Receive output**
   - Excel workbook written to `output/LBTY_Master_Ledger.xlsx`
   - Contains 12+ tabs depending on fiscal years present

### Outputs
- `output/LBTY_Master_Ledger.xlsx` with tabs: LEDGER_ALL, OPENING_JOURNAL, TRIAL_BALANCE, JOURNAL, PL_{year}, BALANCE_SHEET, RECONCILIATION_{year}, RUN_SUMMARY, DIAGNOSTIC, EXCEPTIONS, RULES, RULE_HITS

### Features Exercised
- `F-ING-01` through `F-ING-04` (all ingestion features)
- `F-PAR-01` through `F-PAR-03` (all parsing features)
- `F-CLS-01` through `F-CLS-07` (all classification features)
- `F-ACC-01` through `F-ACC-06` (all accounting features)
- `F-RPT-01` through `F-RPT-05` (all reporting features)
- `F-AUD-01` through `F-AUD-04` (audit features)
- `F-CFG-01` through `F-CFG-04` (configuration features)

---

## J3: Review & Validate Output

**Actor:** Accountant / Reviewer  
**Goal:** Verify the generated Excel workbook is correct before using it for client work.  
**Preconditions:**
- Ingestion has completed successfully
- Excel workbook exists in `output/`

### Steps

1. **Open the Excel workbook**
   - Open `output/LBTY_Master_Ledger.xlsx`

2. **Review LEDGER_ALL**
   - Verify all transactions are present
   - Check correct debit/credit polarity
   - Check correct instrument type assignment
   - Check each transaction has a Category, Confidence level, and RuleID
   - **Decision point:** Are there transactions with `Confidence = Red` or `Category = For Accountant Review`? These need manual review.

3. **Review JOURNAL**
   - Confirm double-entry is complete (every transaction has a debit line and a credit line)
   - Confirm total Debits = total Credits
   - **Stop immediately** if journal does not balance

4. **Review TRIAL_BALANCE**
   - Confirm Debits = Credits
   - Opening balances are included
   - Liability balances positive = amount owed
   - No unexplained clearing balances

5. **Review RECONCILIATION (per year)**
   - For each statement: `Opening + Activity = Closing`
   - Difference must be 0.00
   - **Decision point:** If reconciliation fails, investigate:
     - Parser logic (missing or duplicate transactions)
     - Opening balance mismatch
     - Sign inversion errors
     - Polarity errors (bank vs. card logic)

6. **Review BALANCE_SHEET**
   - Confirm: `Assets = Liabilities + Equity`
   - Check row: `Check (Assets - Liabilities - Equity)` must be 0.00
   - If not balanced, trace back: Trial Balance → Journal → Ledger → Canonical layer
   - There is NO automatic equity plug — if it doesn't balance, something is wrong upstream

7. **Review P&L (per fiscal year)**
   - Revenue and expense classification reasonable
   - Fiscal year grouping correct (default: Feb start → labeled by ending year)
   - No liability or asset accounts appearing in P&L

8. **Review EXCEPTIONS**
   - Income/Expense accounts with both debit and credit activity are flagged
   - These may be refunds/reversals (legitimate) or misclassification (needs rule fix)

9. **Review RULES and RULE_HITS**
   - RULES tab shows the full rule register with confidence tiers
   - RULE_HITS shows which rule matched each transaction
   - Use this to identify rules that need tuning

10. **Review RUN_SUMMARY**
    - Per-statement overview: PDF filename, source account, opening/closing balances, extracted totals, diff, transaction count
    - Quick way to spot which statement has issues

11. **Adjust rules if needed**
    - If classification is incorrect, add or modify rules at the correct layer
    - Re-run ingestion and re-validate

12. **Add opening balance if new account appeared**
    - Update `opening_balances.csv`
    - Re-run ingestion

### Outputs
- Validated Excel workbook ready for client work
- Identified rule adjustments or opening balance corrections needed

### Features Exercised
- `F-RPT-01` through `F-RPT-05` (all reporting features)
- `F-AUD-01` through `F-AUD-04` (audit features)
- `F-ACC-04` Trial balance invariant
- `F-ACC-05` Balance sheet invariant
- `F-ACC-06` Reconciliation invariant

---

## Journey Interaction Map

```
J1 (Onboard) ──────► J2 (Ingest) ──────► J3 (Review)
      │                    │                    │
      │                    ▼                    │
      │              Classification             │
      │              Rules Engine               │
      │                    │                    │
      └────── Parser ◄────┘                    │
              fixes if                          │
              reconciliation ◄─────────────────┘
              fails
```

- **J1** is a one-time setup per statement type
- **J2** is the recurring monthly workflow
- **J3** may loop back to J2 (re-run after fixes) or J1 (parser issues for new formats)
