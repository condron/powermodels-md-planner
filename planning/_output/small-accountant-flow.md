# Accounting Reference for a Small-Business Financial Preparation Platform

This reference is written to support two simultaneous deliverables: (a) a public accounting-domain application service layer (ACL) contract and (b) AI prompt engineering for transaction classification, chart-of-accounts (CoA) generation, and model/report generation. fileciteturn0file0L9-L18

The intended end-user is a solo practitioner or small accounting firm (typically 2–10 staff) doing bookkeeping and monthly close for 20–50+ small-business clients, where the real bottleneck is volume and context switching. fileciteturn0file0L19-L29

All domain mapping in this document is constrained to the existing taxonomy: **six account categories** (Income, Expense, Asset, Liability, Equity, Dividend/Owner’s Draw), the **existing ~37 account subtypes**, plus the existing **statement source types** (Checking, Savings, CreditCard, LineOfCredit), **cash position types**, and the **contra account flag** behavior. fileciteturn0file0L46-L83

## The accountant’s workflow in platform terms

Small-business bookkeeping in practice is a recurring pipeline: *ingest transactions → classify → resolve exceptions/splits → reconcile → adjust → report → close*. Your platform’s value is reducing the “manual interpretation” and “client follow-up” per transaction without breaking accounting invariants. fileciteturn0file0L9-L29

**Client onboarding**

Accountants typically start by collecting the minimum set of facts necessary to seed a well-formed CoA and effective classification:

- Entity type and reporting needs (sole prop / single-member LLC / partnership / S-corp / C-corp), because this determines whether “equity” behaves like owner draws vs distributions/dividends and whether payroll is present. fileciteturn0file0L19-L29  
- Industry context, because it changes which accounts are “core,” how to interpret payees, and whether things like inventory, sales tax, and job costing are required. fileciteturn0file0L173-L238  
- Accounting basis (cash vs accrual), because accrual requires A/R, A/P, deferral and accrual accounts, and drives adjusting entries at close. citeturn0search1  
- Bank/credit-card/line-of-credit accounts used for operations, because ingestion route and debit/credit orientation differ by statement source type. fileciteturn0file0L69-L83  
- Opening balances and “as-of” cutover date (first month in the platform), because reconciliation states and retained earnings roll-forward depend on accurate starting points. citeturn16search24  

A key operational difference between *first month* and *steady state* is rule learning: the first month has high uncertainty, a heavier “ask the client” load, and many one-time mappings; by later months, a material portion of transactions can be handled by recurring payee patterns and entity-specific rules. citeturn3search2turn13view0

**Transaction processing (daily/weekly in practice)**

Transactions arrive via bank feeds, statement uploads, payment processor deposits, and occasional manual entries (e.g., depreciation, accruals). fileciteturn0file0L114-L172 The accountant’s classification decision is typically anchored on “what is the counterparty and what does this represent economically?”—because bank/credit-card descriptions are messy and often not natural language. citeturn13view0turn15view0

In production systems for small-business accounting, recurring transactions are common enough that “nearest similar past transaction” is a strong baseline; newer patterns require generalized inference. In a large QuickBooks-scale study, transaction attributes commonly available for inferring meaning include the financial institution, the raw description, date/time, amount, and counterparty (often extracted/normalized from the description). citeturn13view0turn15view1

**Bank reconciliation (monthly for most clients)**

Bank reconciliation is not “optional hygiene” for small-business bookkeeping; it is the primary mechanism for verifying completeness and ensuring cash accounts match the bank’s view at period end. The workflow is: import statement/bank feed → match to books → identify unmatched items and timing differences → record corrections (bank fees, interest, NSF, missing checks/payments) → confirm ending bank balance equals book cash balance as-of the reconciliation date. citeturn0search3turn0search39turn16search0

Common reconciliation-driven adjustments include bank service charges (recorded as an expense reducing cash), bank interest (recorded as income increasing cash), and NSF/returned payments (reversing the cash deposit and restoring a receivable). citeturn0search3turn0search23turn0search39

**Monthly close**

Month-end close in small business settings usually means: ensure all feeds/statements are imported; reconcile bank and credit card accounts; review and clear uncategorized/exception items; post adjusting entries for accruals/deferrals/depreciation; run and review an (adjusted) trial balance; then produce P&L and balance sheet (and sometimes a simplified cash flow statement). citeturn1search18turn16search2turn16search7

Adjusting entry rules that matter for platform invariants:

- Adjusting entries move amounts between balance sheet and income statement accounts (deferrals/accruals) and typically **do not involve cash**. citeturn16search6turn16search2  
- Depreciation is an adjusting entry that increases expense and increases a contra-asset (accumulated depreciation). citeturn16search2turn0search14  

**Payroll, sales tax, quarterly operational cycles**

Payroll generally enters books as a compound entry: gross wages and employer payroll taxes are expenses; withholdings and employer taxes create liabilities; cash (or bank) is reduced when payroll is paid and when taxes are remitted. citeturn17search10turn17search3turn17search7

Sales tax collection is not revenue; it is a liability accrued when collected and reduced when remitted. citeturn17search0turn17search9turn17search4

For 1099 reporting, vendor payment tracking remains part of year-round bookkeeping. The threshold for reporting nonemployee compensation has historically been $600, and official guidance indicates a change to $2,000 for payments made after December 31, 2025 (i.e., starting in calendar year 2026). citeturn1search12turn1search2turn1search8

**Year-end**

Year-end close includes final adjustments, closing temporary accounts, and producing a tax-prep package (trial balance, financial statements, schedules). Closing entries reset temporary accounts (revenue, expenses, dividends/distributions) into retained earnings (or owner’s equity equivalents, depending on entity structure). citeturn16search3turn16search24

**Transaction volume calibration (what can be supported with available data)**

Publicly available sources rarely publish “bank-statement transaction lines per month by industry” as a standardized metric; however, multiple bookkeeping-pricing and operational guides indicate that transaction volume is a primary cost driver, and commonly used operational tiers cluster around **~50–100 (low), ~100–300 (medium), and 300+ (high)** transactions per month. citeturn8search28turn8search8

The spec’s baseline calibration table is consistent with those tiers, but it should be treated as an initial hypothesis to validate in-product by measuring: (a) statements imported per client, (b) line count per statement source type, and (c) split-rate and exception-rate by industry. fileciteturn0file0L30-L44 citeturn8search28

**Domain model validation notes (workflow-driven)**

- The workflow implies that “bank reconciliation state” and “period close state” must be first-class domain concepts (not merely UI flags), because they constrain posting and drive reporting stability. citeturn16search0turn16search24turn1search18  
- Reconciliation adjustments (bank fees, interest, NSF) should be supported as explicit patterns (not only free-form journal entries), because they are frequent and high-impact to cash correctness. citeturn0search3turn0search39  

**Public operations implied**

Mapped to the ACL grouping model (company context implicit, name-based operations). fileciteturn0file1L9-L40

- **Group 1 (Company Setup):** create company; update company metadata; set accounting basis; set fiscal year and period calendar; set close policy. fileciteturn0file1L9-L22  
- **Group 2 (Chart of Accounts):** seed CoA from industry/entity template; create/update accounts (including subtype, cash position type, contra flag); manage group sets for reporting sections. fileciteturn0file1L23-L40 fileciteturn0file0L46-L83  
- **Group 4 (Journal & Ledger):** post balanced journal entries; correct/remove entries via compensating entries; upload and classify bank transactions; post classification results into the ledger. fileciteturn0file1L64-L83  
- **Group 5 (Classification & Rules):** upload statement and run classification pipeline; review; override classification; create/manage per-entity rules. fileciteturn0file1L87-L106  
- **Group 14 (Task Management):** create recurring monthly-close checklists per company (reconcile, review, deliverables), enabling multi-client throughput. fileciteturn0file1L242-L269  

**AI knowledge required**

- How accountants interpret bank transaction attributes (description, extracted counterparty, amount, date/time, account source) to decide the economic meaning and the correct counterpart account. citeturn13view0turn15view0  
- The difference between “cash movement” vs “expense/income” (e.g., transfers, credit card payments, loan proceeds) to avoid common false positives. fileciteturn0file0L414-L424  
- How reconciliation works, what items create differences (fees, interest, NSF), and what adjusting entries are required. citeturn0search3turn0search39turn0search23  
- How and when to generate adjusting entries (accruals/deferrals/depreciation) and why these typically do not include cash. citeturn16search2turn16search6  

**Expected outcomes**

- After ingestion + classification, the system produces a set of proposed postings that are balanced and mapped to the client’s CoA; any that cannot be confidently mapped remain in an explicit “needs review” state. fileciteturn0file0L114-L172 fileciteturn0file1L87-L106  
- After reconciliation, statement-ending balances match period-ending book balances for the reconciled accounts; reconciliation exceptions are either resolved via matches or posted adjustments. citeturn0search3turn16search0  
- After close, financial statements generated from the adjusted trial balance remain stable unless the period is re-opened through an explicit operation. citeturn16search7turn16search24  

## Chart of accounts structure mapped to the existing taxonomy

A useful small-business CoA is not “maximally detailed”; it is *stable, scannable, and aligned with how transactions actually appear*. This means: (a) keep a limited number of accounts at day one, (b) expand only when the accountant is forced to split or explain meaningfully different economic behavior, and (c) ensure every account has a correct normal balance orientation for validation and AI correctness. citeturn0search14turn16search13

### Category and subtype mapping rules

Your platform’s taxonomy constraints are:

- **Account categories (6):** Income, Expense, Asset, Liability, Equity, Dividend/Owner’s Draw. fileciteturn0file0L50-L67  
- **Account subtypes (~37):** used for validation, AI guidance, and report grouping (via group sets). fileciteturn0file0L58-L67  
- **Cash position type:** indicates whether an account is Checking/Savings/LineOfCredit/CreditCard/Cash for operational cash workflows. fileciteturn0file0L78-L79  
- **Contra flag:** reverses the “normal” debit/credit behavior of the parent category (e.g., accumulated depreciation). fileciteturn0file0L80-L83  

In accounting terms, normal balances follow a widely taught convention: assets and expenses are typically debit-normal; liabilities, equity, and revenues are credit-normal; dividends/distributions are debit-normal. citeturn0search14turn16search13

### Numbering and hierarchy conventions

Small-business CoAs commonly use a numeric range scheme that clusters accounts by report section (assets, liabilities, equity, income, expenses), supporting consistent ordering and faster review. Common conventions include: assets (1000–1999), liabilities (2000–2999), equity (3000–3999), income (4000–4999), and expense/COGS (5000+), with variations by system and firm preference. citeturn1search17turn1search3turn1search13

From a platform perspective: numbering is mostly a *presentation and export concern*, while correct category/subtype/contra behavior is a *validation and AI concern*. citeturn0search14turn16search0

### Industry-specific minimum viable CoAs (day-one templates)

Below, each “account name” is shown with its **required mapping** to *(Category → Subtype → contra? → cash position type?)* in your domain model. Where the taxonomy lacks a standard subtype, the mapping uses **Custom** and the gap is called out explicitly.

#### Schedule C sole proprietor

**Operating profile:** often cash basis or modified cash; typically low volume; owner draws common; payroll may be absent unless there are employees. citeturn0search1turn1search15

**Minimum viable CoA**

- Checking: Asset → Checking → (cash position: Checking) fileciteturn0file0L58-L83  
- Owner’s equity: Equity → OwnersEquity  
- Owner draws: Dividend/Owner’s Draw → Dividend **or** Equity → OwnersDraws (see “taxonomy conflict” note below) fileciteturn0file0L50-L67  
- Revenue: Income → Revenue  
- Core expenses: Expense → Rent, Utilities, Insurance, OfficeExpenses, ProfessionalFees, Travel, Meals, Advertising, OtherExpense fileciteturn0file0L58-L67  

**Tax-aware mapping note:** common Schedule C buckets align naturally to your expense subtypes (e.g., advertising, rent, utilities, legal/professional, supplies/office), though some Schedule C lines (car/truck, contract labor, repairs, supplies) may need Custom accounts under Expense if the firm wants that level of specificity. citeturn1search15turn1search1

#### Restaurant

**Operating profile:** high-frequency spend; food/beverage COGS; tips and payroll complexity; multiple deposits (cash + card); high split-rate. citeturn2search5turn8search28

**Minimum viable CoA**

- Operating checking: Asset → Checking → (cash position: Checking) fileciteturn0file0L58-L83  
- Credit card payable (if merchant processor batches are modeled separately): Liability → CreditCard (or OtherCurrentLiabilities if used as “processor clearing”)  
- Food cost: Expense → CostOfGoodsSold  
- Beverage cost: Expense → CostOfGoodsSold  
- Payroll: Expense → Payroll; PayrollTax; PayrollBenefits fileciteturn0file0L58-L67  
- Sales tax payable: Liability → SalesTaxPayable fileciteturn0file0L58-L67  
- Tips payable (gap): Liability → OtherCurrentLiabilities (taxonomy lacks a specific subtype) citeturn2search8turn2search5  
- Depreciation and accumulated depreciation: Expense → Depreciation; Asset → AccumulatedDepreciation (contra flag = true) fileciteturn0file0L58-L67 citeturn16search2  

**Unique/industry-specific accounts**

- Tip float / tip clearing (if used operationally): Asset → OtherCurrentAssets (or Asset → Checking if the restaurant uses a dedicated bank account). citeturn2search26turn2search8  
- “Comped meals / promotions” often needs tracking; taxonomy gap suggests mapping to Expense → Advertising or Expense → OtherExpense, depending on policy.  

#### Retail

**Operating profile:** inventory and COGS; purchase timing differences; returns/refunds; sales tax; possible shrinkage. citeturn0search1turn17search0

**Minimum viable CoA**

- Inventory: Asset → Inventory fileciteturn0file0L58-L67  
- COGS: Expense → CostOfGoodsSold  
- Sales tax payable: Liability → SalesTaxPayable citeturn17search0  
- A/P (if accrual): Liability → AccountsPayable citeturn0search1  
- Merchant fees (gap): Expense → OtherExpense (or ProfessionalFees by firm convention)  
- Returns/allowances (contra-income): Income → Revenue with **contra flag** = true (taxonomy lacks explicit “Returns” subtype but contra behavior supports it). fileciteturn0file0L80-L83  

#### Construction

**Operating profile:** job costing; retainage; equipment; subcontractors; progress billing; potentially higher A/R complexity. citeturn11search16turn2search17turn2search0

**Minimum viable CoA**

- Operating checking: Asset → Checking → (cash position: Checking)  
- Accounts receivable: Asset → AccountsReceivable  
- Retainage receivable (gap): Asset → AccountsReceivable **or** Asset → OtherCurrentAssets (policy: treat as a sub-ledger of A/R). citeturn2search0turn2search4  
- Accounts payable: Liability → AccountsPayable  
- Retainage payable (gap): Liability → AccountsPayable **or** Liability → OtherCurrentLiabilities. citeturn2search0turn2search4  
- Job costs: Expense → CostOfGoodsSold (used as “job costs” in many contractor CoAs; detailed job cost codes can sit under this subtype). citeturn11search16turn11search24  
- Equipment: Asset → FixedAssets; Expense → Depreciation; Asset → AccumulatedDepreciation (contra) citeturn16search2  

#### Professional services firm

**Operating profile:** often low COGS; payroll; billable expenses; may require client trust accounting in legal contexts. citeturn3search0turn3search13

**Minimum viable CoA**

- Revenue: Income → Revenue  
- Payroll: Expense → Payroll; PayrollTax; PayrollBenefits citeturn17search10turn17search3  
- Professional fees (outsourced): Expense → ProfessionalFees  
- Travel/meals: Expense → Travel; Meals  

**Trust account variant (legal/IOLTA)**

- Trust checking (IOLTA): Asset → Checking (cash position: Checking)  
- Client trust liability (gap): Liability → OtherCurrentLiabilities  
Rationale: IOLTA funds must be segregated from operating funds and represent client-owned money until earned/disbursed. citeturn3search0turn3search13turn3search17

#### Real estate (rental property owner/operator)

**Operating profile:** rental income; mortgage interest; depreciation; security deposits; property-level tracking (classes/tags). citeturn2search15turn0search1

**Minimum viable CoA**

- Rental income: Income → Revenue  
- Repairs/maintenance (gap): Expense → OtherExpense (or Custom if firm wants separation)  
- Insurance: Expense → Insurance  
- Interest: Expense → Interest fileciteturn0file0L58-L67  
- Fixed assets and depreciation: Asset → FixedAssets; Expense → Depreciation; Asset → AccumulatedDepreciation (contra) citeturn16search2  
- Tenant security deposits: Liability → OtherCurrentLiabilities (taxonomy gap for a dedicated deposits subtype). Security deposits are generally treated as liabilities until refunded or applied. citeturn2search6turn2search15  

#### E-commerce

**Operating profile:** high transaction count; payment processor net deposits, refunds, chargebacks, marketplace fees; multi-state sales tax; inventory for product sellers. citeturn3search28turn17search8turn8search28

**Minimum viable CoA**

- Revenue: Income → Revenue  
- Refunds/returns (contra-income): Income → Revenue with contra flag = true fileciteturn0file0L80-L83  
- Merchant fees: Expense → OtherExpense (gap; common dedicated account “Processing Fees”)  
- Sales tax payable: Liability → SalesTaxPayable citeturn17search0  
- Inventory/COGS (if applicable): Asset → Inventory; Expense → CostOfGoodsSold fileciteturn0file0L58-L67  
- Marketplace fees (gap): Expense → OtherExpense  

**Critical classification nuance:** payment processors often deposit *net of fees/refunds/disputes*, so AI must be able to reconstruct gross revenue and fee expense (either via clearing-account patterns or a split entry). Stripe reporting/reconciliation explicitly distinguishes gross, fees, adjustments, refunds, and net settlement totals. citeturn3search28turn3search4turn3search11

#### Healthcare practice

**Operating profile:** significant accounts receivable (especially insurer lag); write-offs/adjustments; HIPAA-related vendor/compliance constraints for bookkeeping vendors/tools. citeturn3search18turn3search10

**Minimum viable CoA**

- Patient service revenue: Income → Revenue  
- Insurance A/R: Asset → AccountsReceivable (use classes/tags for insurer vs patient tracking if needed) citeturn3search18  
- Medical supplies (gap): Expense → OtherExpense (or Custom if separating clinical supplies vs office supplies)  
- Payroll and benefits: Expense → Payroll; PayrollTax; PayrollBenefits fileciteturn0file0L58-L67  
- Write-offs/bad debt (gap): Expense → OtherExpense (taxonomy lacks “BadDebt” subtype)  

#### Nonprofit

**Operating profile:** contributions/grants with donor restrictions; program vs management expense presentation; tracking net assets “with” vs “without” donor restrictions. citeturn2search3turn2search24

**Minimum viable CoA (mapped to your taxonomy)**

- Contributions/grants: Income → OtherIncome (or Income → Revenue by policy; taxonomy does not distinguish “contribution” natively)  
- Restricted net assets (gap): Equity → Custom (e.g., “Net Assets With Donor Restrictions”) citeturn2search3turn2search24  
- Unrestricted net assets (gap): Equity → Custom (e.g., “Net Assets Without Donor Restrictions”) citeturn2search3  
- Program expenses: Expense → OtherExpense (and/or Custom sub-accounts under Expense with group sets to produce “Program vs Management”)  
- Fundraising/management: Expense → OtherExpense (or Custom)  

**Taxonomy gap note:** donor-restriction reporting is a first-order nonprofit requirement, but your subtype list does not include nonprofit-specific equity constructs; implementing this will rely on Equity → Custom plus reporting group sets. citeturn2search3turn2search24 fileciteturn0file0L58-L67

#### Property management

**Operating profile:** per-property P&L; security deposits and trust handling; three-way reconciliation (bank + books + property/tenant ledgers). citeturn11search8turn2search27turn2search9

**Minimum viable CoA**

- Rent income: Income → Revenue  
- Management fee income: Income → Revenue (separate account name)  
- Maintenance/repairs: Expense → OtherExpense (or Custom)  
- Tenant deposits payable: Liability → OtherCurrentLiabilities (commonly named “Tenant Security Deposits Payable”) citeturn2search9turn2search27  
- Separate trust cash (if required): Asset → Checking (cash position: Checking), paired with liability sub-accounts by property/owner if using trust accounting patterns. citeturn2search27turn11search8  

### Cash basis vs accrual basis (platform implications)

The IRS distinguishes cash vs accrual methods in terms of when income and expenses are recognized: under cash, generally when received/paid; under accrual, generally when earned/incurred. citeturn0search1

Platform implications:

- Cash-basis clients can often run with a minimal accrual structure (no A/R, no A/P), though they may still need sales tax payable, payroll tax payable, and loan liabilities. citeturn17search0turn17search3  
- Accrual clients require A/R, A/P, prepaid expenses, accrued liabilities, and routine adjusting entries at close. citeturn16search2turn16search6turn0search1  

### Domain model validation notes (CoA-driven)

- **Owner’s Draw taxonomy conflict:** your model includes a distinct category “Dividend / Owner’s Draw” and also includes “OwnersDraws” under Equity subtypes. This will require an explicit policy: either (a) treat Dividend category as “distributions” broadly (including owner draws) or (b) reserve Dividend category for corporations and keep owner draws under Equity for sole props/partnerships. fileciteturn0file0L50-L67  
- **Common missing expense subtypes for small business:** “bank/merchant fees,” “repairs & maintenance,” “bad debt/write-offs,” and “entertainment” often appear as recurring classification needs; without them, accountants will overuse OtherExpense/Custom, reducing AI learnability and cross-client template quality. citeturn3search28turn17search8turn3search18  

**Public operations implied**

- **Group 2 (Chart of Accounts):** seed CoA templates by industry; add/update accounts with category/subtype/cash position/contra; manage hierarchy and reporting group sets. fileciteturn0file1L23-L40  
- **Group 3 (Counterparties):** create unified counterparties with roles (vendor/customer/employee/contractor) to support classification hints and A/R/A/P workflows. fileciteturn0file1L43-L62  
- **Group 13 (Templates & Solutions):** register and apply industry CoA templates and mappings as reusable blueprints. fileciteturn0file1L201-L214  

**AI knowledge required**

- How to propose a day-one CoA based on industry + entity + accounting basis, constrained to the allowed categories/subtypes and using Custom only when necessary. fileciteturn0file0L46-L83  
- How contra accounts work for common real-world needs (refunds/returns as contra-income; accumulated depreciation as contra-asset). fileciteturn0file0L80-L83 citeturn16search2  
- How to map specialized industries to the same underlying taxonomy without inventing new categories (e.g., “Tips payable” → Liability/OtherCurrentLiabilities). citeturn2search8turn2search9  

**Expected outcomes**

- For each new company, the platform can generate a minimal, valid CoA whose accounts all have correct category/subtype/cash position/contra configuration and can immediately accept journal postings. citeturn0search14turn16search13  
- Industry templates produce consistent reporting groups (e.g., food vs labor vs occupancy for restaurants) via group sets, even when underlying subtypes are shared (e.g., multiple COGS accounts). fileciteturn0file1L23-L40  

## Double-entry rules as system invariants

Double-entry correctness is the foundation for (1) a journal/ledger API that can be validated mechanically, and (2) AI classification output that is testable and safe. fileciteturn0file0L240-L345

### Balanced entry requirement

Every journal entry must have total debits equal total credits, with at least two lines (a debit and a credit), and compound entries are common. citeturn16search13turn16search21

### Debit/credit behavior by category, including contra

Normal balance rules:

- **Assets:** debit increases, credit decreases. citeturn0search14  
- **Liabilities:** credit increases, debit decreases. citeturn0search14  
- **Equity:** credit increases, debit decreases. citeturn0search14  
- **Income/Revenue:** credit increases, debit decreases. citeturn0search14  
- **Expenses:** debit increases, credit decreases. citeturn0search14  
- **Dividends/Distributions:** debit-normal (they reduce equity, but are not expenses). citeturn0search14turn16search3  

Contra accounts reverse the normal balance behavior of their parent category (e.g., accumulated depreciation is a contra-asset with credit-normal behavior). fileciteturn0file0L80-L83

### Statement-source orientation (critical for bank-feed classification)

Because your system ingests bank/credit-card/LOC statements, the AI and ACL must correctly orient the **statement account line** for each transaction:

- **Checking/Savings (Asset):**
  - Money out: **Credit** the bank account (asset decreases) → AI must choose the **debit** counterpart (expense/asset/liability/draw).  
  - Money in: **Debit** the bank account (asset increases) → AI must choose the **credit** counterpart (income/liability/equity/asset).  

- **Credit card / Line of credit (Liability):**
  - Charge/advance: **Credit** the card/LOC account (liability increases) → AI must choose the **debit** counterpart (expense/asset).  
  - Payment: **Debit** the card/LOC account (liability decreases) → AI must choose the **credit** counterpart (cash/bank asset).  

These follow directly from normal balance rules for assets vs liabilities. citeturn0search14turn16search13

### Common entry patterns the platform should treat as “gestures”

Each pattern below is a reusable template for (a) ACL operations (e.g., “create split payment entry”) and (b) AI output structure (e.g., “this bank line is likely a credit card payment—post as liability reduction, not expense”).

**Purchases and operating expenses**

- Purchase with cash/checking: Debit Expense; Credit Checking. citeturn16search13turn0search14  
- Purchase on credit card: Debit Expense; Credit CreditCard (liability). citeturn16search13turn0search14  

**Revenue and collections**

- Cash sale with sales tax: Debit Cash; Credit Revenue; Credit SalesTaxPayable. citeturn17search0turn17search4  
- Customer payment on invoice: Debit Cash; Credit AccountsReceivable. citeturn16search13turn16search2  

**A/P and bill pay (accrual clients)**

- Record vendor bill: Debit Expense (or Inventory); Credit AccountsPayable. citeturn16search13turn0search1  
- Pay vendor bill: Debit AccountsPayable; Credit Cash. citeturn16search13turn16search0  

**Payroll (simplified but structurally correct)**

- Accrue payroll (earned but not yet paid): Debit Payroll Expense; Credit Accrued Liabilities (or a payroll payable account). citeturn17search10turn16search2  
- Run payroll (at pay date): Debit Payroll Expense (+ employer PayrollTax/Benefits); Credit Cash; Credit PayrollTaxPayable (withholdings + employer liabilities). citeturn17search10turn17search3turn17search7  

**Depreciation**

- Monthly depreciation: Debit Depreciation Expense; Credit Accumulated Depreciation (contra-asset). citeturn16search2turn0search14  

**Owner equity movements**

- Owner contribution: Debit Cash; Credit Owner’s Equity. citeturn16search13turn0search14  
- Owner draw/distribution: Debit Owner’s Draw (or Dividend/Distribution); Credit Cash. citeturn16search3turn0search14  

**Transfers and debt (high AI-misclassification risk)**

- Transfer between bank accounts: Debit Bank Account A; Credit Bank Account B (both assets; not revenue/expense). fileciteturn0file0L414-L424  
- Credit card payment: Debit CreditCard (liability decreases); Credit Cash (asset decreases). Not an expense. fileciteturn0file0L414-L424  
- Loan proceeds: Debit Cash; Credit NotesPayable/LongTermDebt (liability increases). Not revenue. fileciteturn0file0L414-L424  
- Loan payment with split principal/interest: Debit Loan Liability (principal); Debit Interest Expense; Credit Cash. citeturn16search13turn0search14  

**Payment processor net deposit (e-commerce and many services firms)**

When the bank shows a **net** deposit, reconstruct it as a split entry:

- Debit Cash (net deposit)  
- Debit Fees expense (merchant fees)  
- Debit contra-income (refunds/chargebacks) as needed  
- Credit Revenue (gross)  

Stripe reconciliation/reporting explicitly distinguishes fees and net totals, enabling this split when the platform has processor reports. citeturn3search28turn3search4turn3search11

### Validation rules (ACL invariants + AI output gating)

At minimum:

- Entry balances (sum debits = sum credits). citeturn16search13turn16search0  
- Accounts exist and match expected category/subtype constraints. fileciteturn0file0L46-L83  
- Amounts are positive; direction is represented by debit/credit, not by sign. citeturn16search21  
- Date must be within an open period; closed periods require explicit re-open/correction workflow. citeturn16search24  
- Contra flag changes “what is normal,” so the validator must evaluate direction against the effective normal balance. fileciteturn0file0L80-L83  

**Domain model validation notes (double-entry driven)**

- You will need a strict rule: **statement ingestion must always create postings whose first leg is the statement account**, oriented correctly by statement source type (asset vs liability). This is the single largest driver of classification correctness for bank feeds. citeturn13view0turn15view0  
- The contra flag is not optional metadata; it must participate in validation (“is this debit to a revenue account expected?”) and in AI training signals. fileciteturn0file0L80-L83  

**Public operations implied**

- **Group 4 (Journal & Ledger):** post balanced journal entry; post compound entries; reverse entries; correct via compensating entries; enforce period open/close. fileciteturn0file1L64-L83  
- **Group 5 (Classification & Rules):** classification output must be representable as a balanced entry template, not only as a category label. fileciteturn0file1L87-L106 citeturn13view0  

**AI knowledge required**

- Normal balances and how to orient debits/credits based on account category and statement source type. citeturn0search14turn16search13  
- How to detect non-income/expense movements (transfers, debt proceeds, credit card payments) that visually resemble income/expense in bank descriptions. fileciteturn0file0L414-L424  
- How to generate split/compound entries for common patterns (sales tax, payroll, loan payments, processor deposits) using only allowed taxonomies. citeturn17search0turn17search10turn3search28  

**Expected outcomes**

- The ACL rejects or quarantines any AI-produced posting that violates balance, period, or direction constraints; AI can still propose but cannot finalize without meeting invariants. citeturn16search0turn16search24  
- For a bank feed item, the system can always explain “which side is fixed” (statement account) and “which side is inferred” (counterpart account(s)), enabling deterministic review UI. citeturn13view0turn15view0  

## Financial statements as reporting contracts

Your Zone 3 contract is effectively: **financial statements are derived from the (adjusted) trial balance, which is derived from the general ledger, which is derived from balanced entries**. citeturn16search0turn16search7

### Income statement (P&L)

A standard P&L structure is: revenue minus COGS equals gross profit; minus operating expenses equals operating income; plus/minus other income/expenses equals net income. fileciteturn0file0L302-L332

For the persona, the P&L is reviewed primarily for: margin sanity (gross margin especially), unusual expense spikes, vendor/payment anomalies, and seasonality comparisons. citeturn1search18turn1search10

### Balance sheet

Balance sheets are organized into current vs non-current assets/liabilities, and must satisfy the accounting equation **Assets = Liabilities + Equity**. citeturn0search6turn16search13

### Cash flow statement (indirect method)

Small businesses often want a simplified cash flow perspective (“where did cash go?”). Under the indirect method, cash from operations starts with net income and adjusts for non-cash expenses (depreciation) and changes in working capital accounts, then adds investing and financing cash flows. citeturn16search1turn16search14

### Trial balance

The trial balance is a listing of all ledger accounts with debit/credit balances; total debits must equal total credits, and it is used as a checkpoint before producing financials. citeturn16search0turn16search24

### Ratios

For small business, baseline ratios frequently include current ratio, quick ratio, gross margin, net margin, and debt-to-equity, though industry-specific variants matter (e.g., food cost percentage for restaurants). fileciteturn0file0L302-L332

**Domain model validation notes (reporting-driven)**

- Group sets (CoA reporting groups) are a critical bridge between raw subtypes and statement presentation; they must support per-industry layouts without changing taxonomy. fileciteturn0file1L23-L40  
- Contra accounts must be reported correctly (accumulated depreciation reduces fixed assets; contra-revenue reduces revenue) or statements will mislead. citeturn16search2turn0search14  

**Public operations implied**

- **Group 2 (Chart of Accounts):** manage report group sets and membership (mapping accounts to statement sections). fileciteturn0file1L23-L40  
- **Group 4 (Journal & Ledger):** run trial balance; run adjusted trial balance; produce period financial statements; lock periods at close. fileciteturn0file1L64-L83  
- **Group 9 (Data Integration):** connect external subledgers (payroll/provider reports, payment processor reports) so that statement outputs reconcile to source totals. citeturn3search28turn17search10 fileciteturn0file1L154-L191  

**AI knowledge required**

- How to map the platform CoA taxonomy to statement sections (Revenue, COGS, Operating Expenses, Other Income/Expense; Current vs Non-Current). citeturn16search0turn16search14  
- How to interpret and generate a cash flow statement using the indirect method from balance sheet and income statement data. citeturn16search1  
- How to surface accountant-facing “review cues” (unusual variances, margin breaks) without making tax or audit determinations. citeturn1search18turn1search10  

**Expected outcomes**

- Generating a financial statement for a closed period is deterministic and traceable to ledger balances; any later correction either creates a new period adjustment or requires explicit re-open + audit trail. citeturn16search24  
- Trial balance always balances unless there is corruption; failures are treated as top-severity invariants. citeturn16search0  

## The classification problem as an AI and rules system

Transaction classification is hard largely because bank transactions are *underspecified*: they rarely include line items, and descriptions vary across financial institutions and routes from POS to the issuing bank. citeturn13view0turn15view0

### What accountants actually use to classify

A robust model mirrors “real accountant heuristics”:

- Counterparty/payee signal is dominant, but extracting it reliably is nontrivial due to messy descriptions. citeturn13view0  
- Amount and recurrence are strong secondary signals, enabling “nearest match” approaches for repeated transactions. citeturn15view1turn15view0  
- Account source (checking vs credit card) changes the interpretation because the fixed leg differs (asset vs liability orientation). fileciteturn0file0L69-L83  

### What makes it hard in practice

- Merchant description variability is extreme; one large-scale study observed ~10 million unique counterparties across ~1 billion transactions and ~100 million unique normalized descriptions, with a single grocery merchant appearing in 300+ formats. citeturn13view0  
- A significant share of transactions can involve “new” counterparties for a business; the same study reports that historically, **about 50% of transactions are with new counterparties on average**, which limits purely company-history-only classifiers. citeturn13view0  
- “Looks like income/expense but isn’t” is a dominant error class: transfers, credit card payments, debt proceeds, and owner draws. fileciteturn0file0L414-L424  

### How rules compound (and why the platform should model it explicitly)

Rules systems in mainstream products are explicit: accountants can define rules based on “money in vs money out,” the source account, description/bank text, and amount conditions; and they can prioritize rules. citeturn3search2turn3search5 This strongly matches the mental model: exact match → partial match → amount + keyword → fallback to model suggestions.

Your platform should treat per-entity rule learning as a first-class domain artifact (not just an ML byproduct). This matches the ACL’s intended public surface for per-entity rules. fileciteturn0file1L87-L106

### Confidence calibration and “top-N” suggestions

Modern transaction categorization systems commonly use confidence scoring and “top-N suggestions” to reduce review time while preserving trust. In an Intuit relational deep-learning study, a hybrid model achieved **Top-1 accuracy ~68.67% and Top-5 accuracy ~88.04%** in a “few-shot” setting using company context, and the paper explicitly motivates offering Top-5 categories because the preferred category is often in the top set, improving user trust. citeturn15view0turn14view0

### Implications for “month 3 / 6 / 12 auto-classifiable rates” and “unique payees per business”

Published, industry-specific small-business benchmarks for “auto-classifiable by month N” and “unique payees per business” are not standardized in public accounting guidance. What can be justified from large-scale categorization research is:

- Description normalization and counterparty extraction are necessary because raw descriptions contain many variants per merchant. citeturn13view0  
- A large fraction of transactions may involve counterparties a business hasn’t seen before, implying that rule learning must be combined with generalized inference. citeturn13view0turn15view0  

From a platform design standpoint, the correct approach is to **measure these metrics per client and per industry in-product** (and then use them for AI calibration), rather than encoding a single “universal” expectation. fileciteturn0file0L346-L393

**Domain model validation notes (classification-driven)**

- Your existing “statement source types” and “cash position types” are essential to correct classification orientation and should be input features for both rules and AI. fileciteturn0file0L69-L83  
- The ACL spec includes confidence thresholds (e.g., validation vs auto-approve); these should be surfaced as policy knobs per firm and perhaps per company. fileciteturn0file1L87-L106  

**Public operations implied**

- **Group 5 (Classification & Rules):** upload statement (CSV/PDF); run classification pipeline; present suggested category + confidence + explanation; accept overrides; persist per-entity rules; replay rules across backlog. fileciteturn0file1L87-L106  
- **Group 4 (Journal & Ledger):** post classification results as balanced entries; correct misclassifications via compensating entries; link bank items to postings for reconciliation. fileciteturn0file1L64-L83  
- **Group 3 (Counterparties):** manage counterparties as shared reference data for payee normalization and 1099 tracking. fileciteturn0file1L43-L62 citeturn1search2  

**AI knowledge required**

- Feature understanding: which transaction attributes are predictive and how to normalize descriptions/counterparties. citeturn13view0turn15view0  
- How to propose top-N outputs with confidence and when to abstain (flag for accountant/client questions). citeturn14view0turn15view0  
- How to avoid the highest-cost error classes (transfers, debt proceeds, credit card payments, sales tax). fileciteturn0file0L414-L424 citeturn17search0  

**Expected outcomes**

- Overrides generate durable learning artifacts (rules and/or training signals) and reduce future review load without causing brittle overfitting. fileciteturn0file1L87-L106  
- Classification outputs are always representable as postings that satisfy double-entry validation, or they remain “unposted proposals.” citeturn16search13turn16search0  

## Common scenarios and expected outcomes

These scenarios are the “messy middle” that will break both ledger correctness and AI accuracy if not explicitly handled. fileciteturn0file0L394-L435

### Splits and compound transactions

**Scenario: one transaction allocated across multiple expense accounts**

This is common in accounting tools that support splitting bank transactions into multiple categories (amounts must sum to difference = $0). citeturn18search2turn18search1

**Expected outcomes**

- A single statement line maps to **one** journal entry with **one fixed-leg line** (the statement account) and **multiple counterpart lines**, balancing exactly.  
- The UI/ACL must preserve the linkage from the statement line to all journal lines for reconciliation traceability.  
- The AI output format must support multi-line splits, not just a single label.

### Refunds and returns

**Scenario: customer refund (cash out), reversing revenue**

For cash-basis bank reconciliation, a refund may appear as a bank outflow. Correct treatment is generally a reduction of revenue (contra-income) rather than an “expense,” and it must be linked to the original sale when possible. citeturn3search28turn13view0

**Expected outcomes**

- Post as: Debit contra-income (Income/Revenue with contra flag) and Credit Cash (or, for processor-based cases, incorporate into the processor clearing split). fileciteturn0file0L80-L83  
- If the original sale is known, store a relationship for auditability and better future classification (“refund of X”).  

### Transfers and non-income/expense movements

**Scenario: transfer between accounts**

These commonly create AI mistakes because the bank line looks like a payment or deposit. fileciteturn0file0L414-L424

**Expected outcomes**

- The classifier must recognize “internal transfer candidates” using a combination of amount symmetry, short timing windows, and known account pairs; it should propose a transfer entry (asset-to-asset or liability-to-asset) and not hit the P&L.  
- Posting as revenue/expense should be blocked or flagged as “highly unusual” by validation rules.

**Scenario: credit card payment**

This is another frequent misclassification; it is liability reduction, not an expense. fileciteturn0file0L414-L424

**Expected outcomes**

- Post: Debit CreditCard (liability decreases); Credit Checking (asset decreases).  
- If the bank line is net of multiple card payments or includes fees, require a split with explicit fee lines.

### Recurring transactions and rule automation

**Scenario: rent/subscription repeats monthly**

Rules-based categorization systems explicitly support recurring patterns with priority ordering. citeturn3search2turn3search5

**Expected outcomes**

- System should auto-apply a high-confidence rule and either auto-post (if policy allows) or queue for low-friction review.  
- Rule creation should be triggered directly from accountant corrections (override → rule suggestion), consistent with the intended public surface. fileciteturn0file1L87-L106  

### Corrections and adjusting workflows

**Scenario: reclassify a transaction already posted**

Accounting systems often implement this as (a) reversal/compensation and (b) new posting, preserving audit trail. fileciteturn0file1L64-L83

**Expected outcomes**

- The ACL exposes “correct entry” and “remove entry” as explicit gestures implemented via compensating entries, not silent edits. fileciteturn0file1L64-L83  
- The classification learning loop links the correction to the original features (payee normalization, amounts, description tokens) so future matches improve. citeturn13view0turn3search2  

**Domain model validation notes (scenario-driven)**

- Splits require a stable domain representation: one source transaction → many journal lines, with balancing enforced and provenance preserved. citeturn18search2turn16search13  
- Reversal/correction must be modeled as additive events (compensating entries) rather than destructive edits, or reconciliation and period-close semantics become unreliable. fileciteturn0file1L64-L83  

**Public operations implied**

- **Group 4 (Journal & Ledger):** post split entries; link statement items to entries; reverse/correct entries using compensating entries; void and repost. fileciteturn0file1L64-L83  
- **Group 5 (Classification & Rules):** allow split proposals; learn from corrections; manage rule hierarchy and priority. citeturn3search2turn18search2 fileciteturn0file1L87-L106  

**AI knowledge required**

- How to decide when a split is required (processor deposits, loan payments, mixed purchases) and how to produce balanced multi-line outputs. citeturn3search28turn16search13  
- How to recognize and correctly classify non-P&L cash movements as transfers/liability reductions/equity movements. fileciteturn0file0L414-L424  

**Expected outcomes**

- Every scenario yields a deterministic ledger outcome that satisfies double-entry validation and preserves traceability from statement line → classification decision → posted journal entry. citeturn16search13turn16search0  
- The system produces explicit events suitable for testing: “statement uploaded,” “classified,” “override applied,” “rule created,” “entry posted,” “reconciliation completed,” “period closed.” fileciteturn0file0L436-L446