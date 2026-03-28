---
type: classification-rules
source_app: Lisa General_Demo (CFO vibe-coded accounting app)
extracted_by: Windsurf/Claude
purpose: Claude-optimized context doc for planning tool ingestion
related_docs:
  - user-journeys.md
  - feature-inventory.md
  - excel-output-mapping.md
source_file: lbty_master_runner.py → apply_rules() (lines 111–250), split_gst_monthly() (lines 253–312), rule_register_df() (lines 316–338)
---

# Classification Rules — Transaction Rule Engine Reference

Complete reference for the sequenced rule engine that classifies bank and credit card transactions into accounting categories. Rules are hardcoded in `lbty_master_runner.py` and represent the CFO's domain knowledge about how transactions should be categorized for this specific client (LBTY).

---

## Engine Architecture

### Execution Model
- Rules execute in a **strict sequence** (steps 1–12)
- **First match wins**: once a transaction has a Category assigned, all subsequent rules skip it via the `set_if_empty()` guard
- Every transaction is guaranteed a classification (step 12 is a catch-all fallback)
- Rules receive the full transaction list + optional payroll total + instrument type

### Confidence Tiers

| Tier | Color | Sequence | Meaning | Accountant Action |
|---|---|---|---|---|
| Deterministic | Green | 1 | Exact keyword match, high confidence | Accept as-is |
| Pattern-based | Orange | 2 | Heuristic match (amount range, date, subset-sum) | Spot-check recommended |
| Weak/Residual | Red | 3 | Catch-bucket for remaining transactions | Review required |
| Fallback | Red | 4 | No rule matched at all | Manual classification required |

### Input Signature
```
apply_rules(
    txns: list[Txn],              # Ledger transactions for one statement
    payroll_total: Decimal | None, # Known payroll total from overrides.csv
    instrument_type: str           # "BANK_ASSET" or "CREDIT_CARD_LIABILITY"
) -> list[Txn]
```

### Instrument-Type Sensitivity
- Most rules apply to all instrument types
- **Step 9** (LIAB-001) applies ONLY to `CREDIT_CARD_LIABILITY` — it classifies credits as liability payments, NOT income
- Step 9 must execute BEFORE step 11 ("remaining credits → Income") to prevent misclassification

---

## Complete Rule Table

| RuleID | Seq | Step | Trigger Pattern | Category Assigned | AccountType | Confidence | Notes |
|---|---|---|---|---|---|---|---|
| LBTY-001 | 1 | 1 | Description contains "ICBC" | Auto Insurance | Expense | Green | BC auto insurance |
| LBTY-002 | 1 | 1 | Description contains "CO-OPERATORS" | Health Insurance | Expense | Green | Health insurance provider |
| LBTY-003 | 1 | 1 | Description contains "MSP/DIV" | Bank Fee | Expense | Green | BC Medical Services Plan division |
| LBTY-004 | 1 | 1 | Description contains "FREEDOMMOBILE" | Cell Phone | Expense | Green | Mobile phone bill |
| LBTY-005 | 1 | 1 | Description matches regex `\bCHEQ(?:UE)?\b.*\b(?:NO\.?\|#)?\s*597\b` | Royalties | Expense | Green | Cheque #597 in various formats |
| LBTY-006 | 1 | 1 | Description contains "CANACTBUS" OR ("ENT" AND "REM") | Payroll Remittances | Expense | Green | CRA payroll remittance |
| LBTY-008 | 1 | 1 | Description contains "INTERACE-TRANSFERFEE" OR ("INTERACE-TRANSFER" AND "FEE") | Bank Fee | Expense | Green | Interac e-Transfer fee |
| LBTY-018 | 1 | 1 | Description contains "PLANFEE" | Bank Fee | Expense | Green | Bank account plan fee |
| LBTY-019 | 1 | 1 | Description contains "BMOPAYMENT" AND "BPY/FAC" | BMO Loan Interest | Expense | Orange | BMO loan payment/interest |
| LBTY-007 | 2 | 2 | Description contains "PCMC" | Office Expense | Expense | Orange | PC MasterCard (office purchases) |
| LBTY-015 | 2 | 3 | Description contains "OnlineTransfer,TF0789#1990-918" | Intercompany Clearing | Liability | Orange | Known intercompany transfer by reference |
| LBTY-016 | 2 | 4 | Credit > 0 AND description contains "INTERACe-TransferReceived" AND amount = $500.00 | Intercompany Clearing | Liability | Orange | Known intercompany amount |
| LBTY-017 | 2 | 4 | Credit > 0 AND description contains "INTERACe-TransferReceived" AND amount ≠ $500.00 | Suspense | Liability | Red | Unknown inbound transfer |
| LBTY-012 | 2 | 5 | Subset-sum match: INTERAC-sent debits ($1,500–$2,500) summing to payroll_total | Payroll | Expense | Orange | Requires overrides.csv input |
| LBTY-011 | 2 | 6 | Debit $3,000–$3,500 AND posted day 1–2 AND only ONE candidate | Rent | Expense | Orange | Single-candidate heuristic |
| LBTY-013 | 2 | 7 | Debit > 0 AND description contains "INTERACe-TransferSent" (uncategorized) | Intercompany Clearing | Liability | Orange | Remaining outbound transfers |
| LBTY-014 | 2 | 8 | Debit > 0 AND description contains "Transfer,0005112420002550816" | Credit Card Payable | Liability | Orange | Known CC payment by transfer ref |
| LIAB-001 | — | 9 | instrument_type = CREDIT_CARD_LIABILITY AND Credit > 0 (uncategorized) | [SourceAccount name] | Liability | Orange | Card credits are payments, NOT income |
| LBTY-010A | — | 10 | Credit > 0 AND description contains "MonerisSettlement" (uncategorized) | Revenue Pending GST Split | Income | Red | Staged for GST split post-rules |
| LBTY-020 | 3 | 11 | Credit > 0 (uncategorized) | Income | Income | Red | Remaining deposits = income |
| LBTY-999 | 4 | 12 | Any uncategorized transaction | For Accountant Review | Expense | Red | Catch-all fallback |

---

## Detailed Rule Logic

### Step 1: Deterministic Greens (LBTY-001 through LBTY-019)

Single-pass over all transactions. Each rule checks for a keyword in the uppercased description. These are the highest-confidence matches — exact vendor/payee identification.

```
for t in txns:
    d = t.Description.upper()
    if "ICBC" in d:           → Auto Insurance (LBTY-001)
    if "CO-OPERATORS" in d:   → Health Insurance (LBTY-002)
    if "MSP/DIV" in d:        → Bank Fee (LBTY-003)
    if "FREEDOMMOBILE" in d:  → Cell Phone (LBTY-004)
    if cheque597_regex(d):    → Royalties (LBTY-005)
    if "CANACTBUS" in d or ("ENT" in d and "REM" in d): → Payroll Remittances (LBTY-006)
    if interac_fee(d):        → Bank Fee (LBTY-008)
    if "PLANFEE" in d:        → Bank Fee (LBTY-018)
    if "BMOPAYMENT" in d and "BPY/FAC" in d: → BMO Loan Interest (LBTY-019)
```

**Key design choice:** Multiple keywords can match the same transaction in a single pass, but `set_if_empty()` ensures only the first match sticks. The order of `if` statements within step 1 defines implicit priority among greens.

---

### Step 2: PCMC → Office Expense (LBTY-007)

```
for t in txns:
    if "PCMC" in t.Description → Office Expense (Orange)
```

Separated from step 1 because it's a less precise keyword (could match other things in theory).

---

### Step 3: Known Intercompany Transfer by Reference (LBTY-015)

```
if "OnlineTransfer,TF0789#1990-918" in t.Description → Intercompany Clearing (Orange)
```

Exact transfer reference number match. Identifies a specific known intercompany flow.

---

### Step 4: Inbound Transfers — Amount Discrimination (LBTY-016, LBTY-017)

```
for credits with "INTERACe-TransferReceived":
    if amount == $500.00 → Intercompany Clearing (Orange, LBTY-016)
    else                 → Suspense (Red, LBTY-017)
```

**Design pattern:** Known recurring intercompany transfer at exactly $500.00 is classified automatically. All other inbound transfers are parked in Suspense for review.

---

### Step 5: Payroll Subset-Sum Matching (LBTY-012)

**Precondition:** `payroll_total` must be provided via `overrides.csv`

```
Candidates: uncategorized debits with "INTERACe-TransferSent", amount $1,500–$2,500
Algorithm: brute-force combinatorial search over all subsets
Goal: find smallest subset summing exactly to payroll_total
If found: classify all subset members as Payroll (Orange)
```

**Complexity:** O(2^n) where n = number of candidates. Acceptable for small candidate sets (typically 2–5 transfers per month).

**Design intent:** Payroll is paid via multiple INTERAC transfers to individual employees. The total is known from payroll records but the individual amounts aren't tagged on the statement.

---

### Step 6: Rent Heuristic (LBTY-011)

```
Candidates: uncategorized debits, $3,000–$3,500, posted day 1 or 2
If exactly ONE candidate → Rent (Orange)
If zero or multiple candidates → skip (ambiguous)
```

**Design intent:** Rent is paid on the 1st of the month. The amount range and single-candidate constraint provide reasonable confidence without exact matching.

---

### Step 7: Remaining Outbound Transfers (LBTY-013)

```
Uncategorized debits with "INTERACe-TransferSent" → Intercompany Clearing (Orange)
```

After payroll and rent are extracted, remaining outbound INTERAC transfers are assumed to be intercompany.

---

### Step 8: Known Credit Card Payment Transfer (LBTY-014)

```
Debit with "Transfer,0005112420002550816" → Credit Card Payable (Orange)
```

Exact transfer reference match for a known credit card payment.

---

### Step 9: Liability Credit Classification (LIAB-001)

**CRITICAL — instrument-type-specific rule**

```
if instrument_type == "CREDIT_CARD_LIABILITY":
    uncategorized credits → [SourceAccount name] (Liability, Orange)
```

**Why this matters:** On a credit card statement, credits are payments reducing the balance — they are NOT income. This rule must execute before step 11 ("remaining credits → Income") or card payments would be misclassified as revenue.

---

### Step 10: Moneris Revenue Staging (LBTY-010A)

```
Credits with "MonerisSettlement" → Revenue Pending GST Split (Income, Red)
```

This is a **staging** classification. After `apply_rules()` returns, the `split_gst_monthly()` function replaces these rows with two rows each: Income (net) + GST Payable (5%).

---

### Step 11: Remaining Credits → Income (LBTY-020)

```
Any uncategorized credit → Income (Red)
```

Catch-bucket for deposits that weren't matched by any specific rule. Red confidence = needs accountant review.

---

### Step 12: Fallback Catch-All (LBTY-999)

```
Any remaining uncategorized transaction → For Accountant Review (Expense, Red)
```

Guarantees every transaction gets a classification. Typically catches debits that didn't match any pattern.

---

## Post-Rules Processing: Moneris GST Split

**Source:** `split_gst_monthly()` (runs after `apply_rules()`)

After rules classify Moneris settlements as "Revenue Pending GST Split", this function:

1. Identifies all Moneris settlement credits in the month
2. Computes monthly Moneris gross total
3. Calculates GST = 5% of monthly gross (rounded to 2 decimal places)
4. Allocates GST per transaction proportionally
5. Last transaction absorbs rounding remainder (ensures exact totals)
6. Replaces each Moneris row with two rows:
   - **Income** row: Credit = gross - allocated GST, RuleID = LBTY-010
   - **GST Payable** row: Credit = allocated GST, Description += " - GST 5%", AccountType = Liability, RuleID = LBTY-010

---

## Rule Sequencing Invariants

These ordering constraints MUST be maintained:

1. **LIAB-001 (step 9) before LBTY-020 (step 11):** Liability credits must be classified before the "remaining credits → Income" catch-bucket
2. **LBTY-012 (step 5) before LBTY-013 (step 7):** Payroll transfers must be extracted before remaining transfers default to Intercompany
3. **LBTY-011 (step 6) before LBTY-013 (step 7):** Rent must be extracted before remaining transfers default to Intercompany
4. **LBTY-010A (step 10) before LBTY-020 (step 11):** Moneris must be staged before remaining credits default to Income
5. **All specific rules before LBTY-999 (step 12):** Catch-all must be last

---

## Generalizing for Other Clients

The current rules are **client-specific** (LBTY). To support multiple clients, the following abstractions are needed:

1. **Rule layers:** The README mentions separating rules into statement-specific / client-specific / firm-wide layers (not yet implemented)
2. **Configurable keywords:** Move vendor keyword mappings (ICBC, CO-OPERATORS, etc.) from hardcoded `if` statements to a configuration file or database
3. **Configurable amount thresholds:** Rent range ($3k–$3.5k), payroll range ($1.5k–$2.5k), intercompany amount ($500) should be per-client config
4. **Configurable transfer references:** Transfer numbers (TF0789#1990-918, 0005112420002550816) are account-specific
5. **GST rate:** Currently hardcoded at 5%; should be configurable per jurisdiction
6. **Rule priority/sequencing:** Should be declarative (config-driven) rather than procedural (code order)

---

## Rule Summary Statistics (from Sample Output)

Based on the 2-PDF demo run (BMO Oct 2025 + CoastCapital LOC Dec 2024):

- **Total transactions classified:** 85 (LEDGER_ALL has 86 rows including GST split additions)
- **Green (deterministic):** ~15 transactions (ICBC, CO-OPERATORS, MSP/DIV, FREEDOMMOBILE, cheque 597, payroll remittances, bank fees)
- **Orange (pattern-based):** ~50 transactions (payroll subset-sum, rent, intercompany transfers, PCMC, BMO loan, liability credits)
- **Red (weak/fallback):** ~20 transactions (Income catch-bucket, Moneris splits, suspense, accountant review)
