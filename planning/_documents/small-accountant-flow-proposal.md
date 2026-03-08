# Decomposition Proposal: small-accountant-flow

**Source deliverable**: `powermodels-md-project\_output\small-accountant-flow.md`
**Generated**: 2026-03-07 (dry-run -- no issues created)
**Suggested milestone**: Service Layer Buildout

---

## Gap Analysis Summary

### Existing GitHub Issues That Overlap With Deliverable Scope

| Issue # | Title | State | Overlapping Group |
|---------|-------|-------|-------------------|
| #2099 | PowerModels Accounting phase 2 | OPEN (Epic) | Multiple groups |
| #2109 | New Business creation stuck loading | OPEN | Group 1 |
| #2108 | Allow liability accounts for opening balances | OPEN | Group 1 |
| #2107 | Simplified UI for adding a cash account | OPEN | Group 1 |
| #2106 | New cash account with opening balance | OPEN | Group 1 |
| #2094 | Opening balances for external accounts during setup | OPEN | Group 1 |
| #2095 | Import of Coastal Capital Bank corrupts journal | OPEN | Group 4 |
| #2045 | Rendering of newly loaded Data in Journal incorrect | OPEN | Group 4 |
| #2011 | Resizing CoA Dialog display issue | OPEN | Group 2 |
| #2010 | Add Account dialog shows view model class name | OPEN | Group 2 |
| #1951 | Ingestion handles duplicate transactions | OPEN | Group 5 |
| #1961 | Improve Explore Journal Data (hide columns) | OPEN | Group 4 |

### Codebase Status Verification (vs CODEBASE_AUDIT.md)

Audit findings confirmed. Key updates from scan:

- **AccountingSystem aggregate** (`AccountingSystem.cs`): Only 28 lines. Contains creation event only -- no entity type, no industry context, no fiscal year, no close policy. Audit status PARTIAL confirmed for Group 1.
- **RuleStep** (`RuleStep.cs`): Contains hardcoded keyword-matching rules only (bank fees, interest, transfers). No user-configurable rule persistence, no per-entity rule management. Audit status PARTIAL confirmed.
- **JournalEntry**: Only exists as a reconciliation pipeline model (`JournalPostingStep.cs:10`) and a QBConnector API type -- not a domain aggregate. Audit status MISSING for Journal aggregate confirmed.
- **Period close / Fiscal year**: Zero matches in entire codebase. MISSING confirmed.
- **Reconciliation state aggregate**: Zero matches. MISSING confirmed.
- **Industry templates**: Zero matches. MISSING confirmed.
- **Financial statement generation (GL reports)**: Only found in QBConnector (import from QuickBooks), not as native generation. MISSING confirmed for native reporting.
- **Entry patterns library**: Zero matches for compensating entries, entry patterns, journal gestures. MISSING confirmed.
- **Confidence threshold enforcement**: Confidence is tracked in `ClassificationResult` and used in UI for display, but no explicit 85/15 auto-approve/review boundary enforcement found. PARTIAL confirmed.

---

## Epic 1: [CompanySetup] Client Onboarding and Company Configuration

**Source**: Group 1 -- Company Setup
**Codebase status**: PARTIAL (3 of 6 features exist)
**Priority**: HIGH -- foundational; all other groups depend on company context
**Dimension**: shoebox-offering (3+), reconciliation-elimination (2), proven-completeness (1)

### 1.1 Add Entity Type to AccountingSystem Aggregate

**Description**: Extend the `AccountingSystem` aggregate with an entity type property (Sole Prop, Single-Member LLC, Partnership, S-Corp, C-Corp). This determines equity behavior (owner draws vs distributions/dividends) and whether payroll accounts are required. Add a `SetEntityType` command and corresponding event.

**Acceptance Criteria**:
- [ ] `AccountingSystem` aggregate supports `SetEntityType` command with valid entity types
- [ ] Entity type is persisted via event sourcing (event raised, applied in `Apply()`)
- [ ] Entity type value constrains downstream CoA seeding (equity account structure)
- [ ] RestoreFromEvents() test covers entity type rehydration
- [ ] Invalid entity type values are rejected with a domain exception

**Audience alignment**: Accounting firm partners need entity type to drive correct equity and tax treatment per client. Bookkeepers need clarity on which draw/distribution accounts to use.
**Gaps closed**: shoebox-offering -- messy client automation; proven-completeness -- completeness contract
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: None
**Affected components**: `src/ModelServer/ModelServer/Domain/AccountingSystem/AccountingSystem.cs`, `AccountingSystemMsgs`

### 1.2 Add Industry Context to AccountingSystem Aggregate

**Description**: Add an industry context property to `AccountingSystem` (e.g., Restaurant, Retail, Construction, Professional Services, E-commerce, Healthcare, Nonprofit, Property Management, Real Estate, General). This drives CoA template selection and classification hint weighting.

**Acceptance Criteria**:
- [ ] `SetIndustryContext` command and event added to AccountingSystem
- [ ] Industry values are an enum or constrained value set
- [ ] Industry context is available as input to CoA template seeding (Group 2 dependency)
- [ ] RestoreFromEvents() test covers industry rehydration

**Audience alignment**: Firm partners need per-client industry context for correct classification and reporting. Bookkeepers benefit from industry-appropriate default accounts.
**Gaps closed**: shoebox-offering -- messy client automation; ai-empowerment-strategy -- capacity expansion without hiring
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: None
**Affected components**: `AccountingSystem.cs`, `AccountingSystemMsgs`

### 1.3 Add Fiscal Year and Period Calendar to AccountingSystem

**Description**: Add fiscal year start month, period calendar (monthly), and close policy (soft close / hard close) to the `AccountingSystem` aggregate. These constrain when journal entries can be posted and when period close is enforced.

**Acceptance Criteria**:
- [ ] `SetFiscalYear` command sets fiscal year start month (1-12)
- [ ] `SetClosePolicy` command sets close policy enum (SoftClose, HardClose)
- [ ] Period calendar is derivable from fiscal year start (12 monthly periods)
- [ ] Journal posting validation can query whether a target period is open/closed
- [ ] RestoreFromEvents() tests for both commands

**Audience alignment**: Firm partners need period control to prevent post-close edits. Bookkeepers need clarity on which periods accept entries.
**Gaps closed**: proven-completeness -- defensible output; reconciliation-elimination -- structural consistency
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: None
**Affected components**: `AccountingSystem.cs`, `AccountingSystemMsgs`

### 1.4 Company Setup UI -- Entity Type, Industry, Fiscal Year

**Description**: Extend the business creation / setup UI to collect entity type, industry context, and fiscal year start. Wire to the new commands on `AccountingSystem`.

**Acceptance Criteria**:
- [ ] Business creation wizard includes entity type selection
- [ ] Business creation wizard includes industry selection
- [ ] Fiscal year start month defaults to January, editable
- [ ] Close policy defaults to SoftClose, editable
- [ ] Selecting values dispatches corresponding commands

**Audience alignment**: Bookkeepers need a streamlined onboarding flow. Firm partners need to quickly set up new clients.
**Gaps closed**: shoebox-offering -- no more manual data entry
**Labels**: `ai-agent`, `decomposed`, `ui`
**Depends-on**: 1.1, 1.2, 1.3
**Affected components**: `PowerModels.UIBehavior`, `PowerModels.App`

---

## Epic 2: [CoA] Industry-Specific Chart of Accounts Templates

**Source**: Group 2 -- Chart of Accounts
**Codebase status**: MOSTLY COMPLETE (5 of 7 features exist)
**Priority**: HIGH -- enables correct classification and reporting from day one
**Dimension**: shoebox-offering (3+), ai-empowerment-strategy (2)

### 2.1 Industry CoA Template Engine

**Description**: Create an industry template registry that maps (entity type + industry + accounting basis) to a minimum viable CoA. The deliverable specifies 9 industry templates (Schedule C sole proprietor, Restaurant, Retail, Construction, Professional Services, Real Estate, E-commerce, Healthcare, Nonprofit, Property Management) with exact category/subtype/contra/cash-position mappings.

**Acceptance Criteria**:
- [ ] Template registry returns a list of account definitions per (industry, entity type, basis)
- [ ] Each account definition includes: name, category, subtype, contra flag, cash position type
- [ ] At minimum, Schedule C sole proprietor and Restaurant templates are implemented
- [ ] Templates use only existing taxonomy values (6 categories, ~37 subtypes)
- [ ] Accounts that require `Custom` subtype are documented as taxonomy gaps

**Audience alignment**: Firm partners save time onboarding clients. Bookkeepers get correct starting accounts without manual configuration.
**Gaps closed**: shoebox-offering -- messy client automation; ai-empowerment-strategy -- capacity expansion without hiring
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 1.2 (industry context)
**Affected components**: New service/class in `ModelServer`, references `ChartOfAccounts.cs`, `StandardChartOfAccounts.cs`

### 2.2 Seed CoA From Industry Template Command

**Description**: Add a command to `ChartOfAccounts` aggregate (or a domain service) that seeds the CoA from an industry template. Should be idempotent (no-op if accounts already exist).

**Acceptance Criteria**:
- [ ] `SeedFromTemplate` command creates all accounts defined in the template
- [ ] Existing accounts are not duplicated
- [ ] Group sets for industry-specific reporting sections are created (e.g., Food vs Labor vs Occupancy for restaurants)
- [ ] Command is callable from the business setup flow after entity type and industry are set

**Audience alignment**: Firm partners need one-click CoA setup per client.
**Gaps closed**: shoebox-offering -- no more manual data entry
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 2.1
**Affected components**: `ChartOfAccounts.cs`, potentially new domain service

### 2.3 Contra Account Workflow in UI

**Description**: The `IsContraAccount` field exists on accounts but has no UI workflow for setting or managing it. Add UI support for marking accounts as contra and displaying contra behavior (reversed normal balance) in account lists and validation feedback.

**Acceptance Criteria**:
- [ ] Configure CoA UI shows contra flag as a toggle/checkbox when editing an account
- [ ] Contra accounts display their effective normal balance direction (reversed)
- [ ] Validation engine respects contra flag when checking debit/credit direction

**Audience alignment**: Bookkeepers need to correctly set up accumulated depreciation, contra-revenue (refunds/returns) accounts.
**Gaps closed**: proven-completeness -- completeness contract
**Labels**: `ai-agent`, `decomposed`, `ui`
**Depends-on**: None
**Affected components**: `ConfigureChartOfAccountsVm.cs`, related views

---

## Epic 3: [Journal] Journal Aggregate and Ledger Infrastructure

**Source**: Group 4 -- Journal & Ledger
**Codebase status**: PARTIAL (6 of 10 features exist)
**Priority**: CRITICAL -- this is the core accounting engine; classification, reconciliation, and reporting all depend on it
**Dimension**: reconciliation-elimination (2), future-ledger (2), proven-completeness (1)

### 3.1 Create Journal Aggregate

**Description**: Create a top-level `Journal` aggregate that owns journal entries as first-class domain objects. Currently `JournalEntry` is only a pipeline model, not an event-sourced aggregate. The Journal aggregate should support posting balanced entries, corrections via compensating entries, and period-aware validation.

**Acceptance Criteria**:
- [ ] `Journal` aggregate extends `AggregateRoot` following project conventions (`:base(source)`, `RegisterEvents()`)
- [ ] `PostEntry` command creates a balanced journal entry (debits = credits)
- [ ] `CorrectEntry` command posts a compensating (reversing) entry linked to the original
- [ ] `RemoveEntry` command posts a compensating entry that zeroes out the original
- [ ] Entry validation rejects unbalanced entries, entries to closed periods, entries with invalid accounts
- [ ] Entries preserve provenance (source: manual, classification, reconciliation adjustment)
- [ ] RestoreFromEvents() test verifies full rehydration
- [ ] All Apply() methods are private; state set only in Apply()

**Audience alignment**: Firm partners need an audit trail that proves completeness. Bookkeepers need corrections that do not silently edit history.
**Gaps closed**: reconciliation-elimination -- no rework, structural consistency; proven-completeness -- defensible output; future-ledger -- committed future events
**Labels**: `ai-agent`, `decomposed`, `domain`, `critical`
**Depends-on**: 1.3 (fiscal year/period for period-aware validation)
**Affected components**: New aggregate in `src/ModelServer/ModelServer/Domain/AccountingSystem/`

### 3.2 Entry Patterns Library (Journal Gestures)

**Description**: Create a library of pre-defined journal entry patterns (gestures) for common accounting scenarios. The deliverable specifies: purchase (cash and credit card), revenue (with sales tax), A/P bill pay, payroll, depreciation, owner equity movements, inter-account transfers, credit card payments, loan payments, and payment processor net deposits.

**Acceptance Criteria**:
- [ ] Each pattern defines: template name, line structure (account roles, debit/credit direction), required parameters
- [ ] Patterns are usable by both manual entry UI and AI classification output
- [ ] At minimum: purchase, revenue, transfer, credit card payment, depreciation, owner draw patterns
- [ ] Pattern application produces a valid balanced entry through the Journal aggregate
- [ ] Patterns handle compound/split entries (e.g., payroll, processor deposits)

**Audience alignment**: Bookkeepers get guided entry creation for common scenarios. AI classification uses patterns to produce structurally correct output.
**Gaps closed**: ai-empowerment-strategy -- staff efficiency; reconciliation-elimination -- fewer corrections
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 3.1
**Affected components**: New service in `ModelServer`

### 3.3 General Ledger Report Generation

**Description**: Implement native GL report generation: trial balance, income statement (P&L), and balance sheet. Currently these only exist as QuickBooks connector imports. Reports are derived from the adjusted trial balance, which is derived from the general ledger, which is derived from balanced entries.

**Acceptance Criteria**:
- [ ] Trial balance report: lists all accounts with debit/credit balances; total debits = total credits
- [ ] Income statement: revenue - COGS = gross profit; - operating expenses = operating income; +/- other = net income
- [ ] Balance sheet: Assets = Liabilities + Equity
- [ ] Reports are period-aware (filter by fiscal period)
- [ ] Reports respect group sets for industry-specific presentation
- [ ] Contra accounts are reported correctly (reduce parent category totals)

**Audience alignment**: Firm partners need financials for client deliverables. Tax preparers need trial balance for tax prep packages.
**Gaps closed**: proven-completeness -- defensible output, acceptance criteria; future-ledger -- forward-looking financials
**Labels**: `ai-agent`, `decomposed`, `reporting`
**Depends-on**: 3.1
**Affected components**: New reporting service in `ModelServer`, UI views

### 3.4 Ledger Read Model

**Description**: Create a read model that materializes the general ledger from journal entry events. The ledger is computed from events, not persisted as a separate aggregate. Provides account-level balance queries for reporting and validation.

**Acceptance Criteria**:
- [ ] Read model subscribes to journal entry events and maintains running balances per account
- [ ] Supports balance-as-of-date queries for period-end reporting
- [ ] Supports account-level transaction history (all entries affecting an account)
- [ ] Handles compensating entries correctly (nets against original)

**Audience alignment**: Bookkeepers need to see account balances. Reporting depends on ledger balances.
**Gaps closed**: reconciliation-elimination -- single financial record
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 3.1
**Affected components**: New read model in `ModelServer`

---

## Epic 4: [Classification] Rule Management and Confidence Boundaries

**Source**: Group 5 -- Classification & Rules
**Codebase status**: MOSTLY COMPLETE (6 of 8 features exist)
**Priority**: HIGH -- directly impacts daily accountant productivity
**Dimension**: ai-empowerment-strategy (2), professional-judgment-preservation (2), shoebox-offering (3+)

### 4.1 Per-Entity Configurable Classification Rules

**Description**: Replace the hardcoded keyword rules in `RuleStep.cs` with a configurable, per-entity rule system. Accountants should be able to create, edit, prioritize, and delete classification rules. Rules should be generated from overrides (correction-driven learning).

**Acceptance Criteria**:
- [ ] Classification rules are persisted as domain objects (new aggregate or child of AccountingSystem)
- [ ] Rules support conditions: description contains, payee matches, amount range, source account type
- [ ] Rules have priority ordering (higher priority rules evaluated first)
- [ ] Rules specify: target account, vendor/counterparty, confidence level
- [ ] Override-to-rule: when an accountant corrects a classification, system suggests creating a rule
- [ ] `RuleStep` loads per-entity rules instead of hardcoded patterns
- [ ] Rules are replayable across transaction backlog

**Audience alignment**: Firm partners need rule learning to reduce per-client effort over time. Bookkeepers need to codify their expertise into reusable rules.
**Gaps closed**: professional-judgment-preservation -- CPA retains control; ai-empowerment-strategy -- capacity expansion without hiring; reconciliation-elimination -- fewer corrections
**Labels**: `ai-agent`, `decomposed`, `domain`, `classification`
**Depends-on**: None
**Affected components**: `RuleStep.cs`, new aggregate/service, UI for rule management

### 4.2 85/15 Confidence Threshold Enforcement

**Description**: Implement explicit confidence threshold boundaries for classification auto-approval vs review. The deliverable specifies an 85% boundary: transactions above threshold can be auto-posted (if policy allows); below threshold requires accountant review. Threshold should be configurable per firm/company.

**Acceptance Criteria**:
- [ ] Classification pipeline respects a configurable confidence threshold (default 0.85)
- [ ] Transactions above threshold are marked auto-approvable
- [ ] Transactions below threshold are routed to "needs review" state
- [ ] Threshold is configurable per company (stored on AccountingSystem or a policy aggregate)
- [ ] UI clearly indicates which transactions were auto-classified vs need review

**Audience alignment**: Firm partners need the 85/15 boundary to preserve professional judgment. Bookkeepers need clear indication of what the system did vs what they need to review.
**Gaps closed**: professional-judgment-preservation -- 85/15 boundary, judgment not automation; reconciliation-elimination -- clarity of what system did vs what they need to do
**Labels**: `ai-agent`, `decomposed`, `classification`
**Depends-on**: None
**Affected components**: `AIStep.cs`, `ClassificationResult`, `TransactionsRm.cs`, UI view models

---

## Epic 5: [Reconciliation] Reconciliation State and Period Close

**Source**: Group 14 -- Task Management (reconciliation and close workflows)
**Codebase status**: PARTIAL (5 of 7 features exist)
**Priority**: HIGH -- reconciliation is the primary mechanism for verifying completeness
**Dimension**: reconciliation-elimination (2), proven-completeness (1)

### 5.1 Reconciliation State Aggregate

**Description**: Create a `ReconciliationState` aggregate that tracks bank reconciliation status per account per period. Reconciliation is not optional hygiene -- it is the primary mechanism for verifying completeness and ensuring cash accounts match the bank's view at period end.

**Acceptance Criteria**:
- [ ] `ReconciliationState` aggregate tracks: account, period, status (NotStarted, InProgress, Reconciled), statement ending balance, book ending balance, difference
- [ ] `StartReconciliation` command initializes state for an account/period
- [ ] `CompleteReconciliation` command validates statement balance = book balance (within tolerance) and marks reconciled
- [ ] Reconciliation adjustments (bank fees, interest, NSF) are posted as journal entries through the Journal aggregate
- [ ] RestoreFromEvents() test

**Audience alignment**: Firm partners need reconciliation status per client for quality control. Bookkeepers need to track which accounts are reconciled each month.
**Gaps closed**: reconciliation-elimination -- no more reconciliation, single financial record, end of drift; proven-completeness -- completeness contract
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 3.1 (Journal aggregate for posting adjustments)
**Affected components**: New aggregate in `src/ModelServer/ModelServer/Domain/AccountingSystem/`

### 5.2 Period Close Aggregate

**Description**: Create a `PeriodClose` aggregate that tracks monthly close status and enforces period locking. Close workflow: ensure all feeds imported, all accounts reconciled, exceptions cleared, adjusting entries posted, trial balance reviewed, financials produced.

**Acceptance Criteria**:
- [ ] `PeriodClose` aggregate tracks: period, status (Open, SoftClosed, HardClosed), checklist items
- [ ] `SoftClose` command marks period as soft-closed (warnings on new entries but not blocked)
- [ ] `HardClose` command marks period as hard-closed (new entries blocked)
- [ ] `ReopenPeriod` command explicitly reopens a closed period with audit trail
- [ ] Journal posting validation checks period close status before accepting entries
- [ ] Close status integrates with task management (auto-generate close checklist tasks)

**Audience alignment**: Firm partners need period control for reporting stability. Bookkeepers need a clear close workflow to follow.
**Gaps closed**: reconciliation-elimination -- structural consistency, reporting confidence; proven-completeness -- defensible output
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 1.3 (fiscal year/periods), 3.1 (journal validation)
**Affected components**: New aggregate in `src/ModelServer/ModelServer/Domain/AccountingSystem/`

### 5.3 Reconciliation Adjustment Patterns

**Description**: Support explicit reconciliation adjustment patterns (bank fees, interest, NSF/returned payments) as guided entry creation workflows. These are frequent, high-impact to cash correctness, and should not require free-form journal entry knowledge.

**Acceptance Criteria**:
- [ ] Bank fee adjustment: creates Debit Expense (Bank Fees); Credit Cash
- [ ] Interest adjustment: creates Debit Cash; Credit Interest Income (or Debit Interest Expense; Credit Cash)
- [ ] NSF/returned payment: reverses cash deposit, restores receivable
- [ ] Each adjustment is posted through the Journal aggregate as a proper balanced entry
- [ ] Adjustments are linked to the reconciliation they belong to

**Audience alignment**: Bookkeepers need guided patterns for common reconciliation adjustments instead of manual journal entries.
**Gaps closed**: reconciliation-elimination -- fewer corrections, clean books
**Labels**: `ai-agent`, `decomposed`, `domain`
**Depends-on**: 3.1, 3.2, 5.1
**Affected components**: New service, UI integration with reconciliation views

---

## Priority Reasoning

| Epic | Priority | Rationale |
|------|----------|-----------|
| 3. Journal Aggregate & Ledger | CRITICAL | Every other group depends on a proper journal/ledger. Classification posts to it, reconciliation validates against it, reporting reads from it, period close locks it. This is the foundation. |
| 1. Company Setup | HIGH | Entity type and industry drive CoA templates, classification hints, and equity behavior. Must be in place before onboarding works correctly. Partially blocked by existing open issues (#2109, #2106, #2094). |
| 2. CoA Templates | HIGH | Industry templates enable correct classification from day one. Depends on company setup (industry context). High intersection dimension with shoebox-offering (3+). |
| 4. Classification Rules | HIGH | Directly impacts daily productivity. Existing rule system is hardcoded. Highest audience impact for bookkeepers. |
| 5. Reconciliation & Close | HIGH | Required for production use but can be developed in parallel with Journal aggregate. Period close depends on Journal + Fiscal Year. |

### Sequencing

```
Phase 1 (foundational):
  1.1, 1.2, 1.3  (Company Setup domain)  -- no dependencies
  3.1             (Journal Aggregate)     -- depends on 1.3 for period validation

Phase 2 (parallel):
  1.4  (Company Setup UI)     -- depends on 1.1-1.3
  2.1, 2.2  (CoA Templates)   -- depends on 1.2
  2.3  (Contra UI)            -- no dependencies
  3.4  (Ledger Read Model)    -- depends on 3.1
  4.1  (Rule Management)      -- no dependencies
  4.2  (Confidence Threshold) -- no dependencies

Phase 3 (dependent):
  3.2  (Entry Patterns)       -- depends on 3.1
  3.3  (GL Reports)           -- depends on 3.1, 3.4
  5.1  (Reconciliation State) -- depends on 3.1
  5.2  (Period Close)         -- depends on 1.3, 3.1
  5.3  (Recon Adjustments)    -- depends on 3.1, 3.2, 5.1
  2.2  (Seed CoA Command)     -- depends on 2.1
```

### Items NOT Proposed (EXISTS / Out of Scope)

| Capability | Reason |
|-----------|--------|
| Group 3: Counterparties | COMPLETE -- full CQRS exists for Counterparty, Vendor, Customer with role flags |
| Accounting basis (cash/accrual) | EXISTS -- `AccountingMethod.cs` |
| Opening balances | EXISTS -- `OpeningBalanceVm.cs`, being actively improved (#2094, #2106) |
| AI classification pipeline | EXISTS -- `AIStep.cs` (300+ lines), only threshold enforcement is PARTIAL |
| Review workflow (accept/reject) | EXISTS -- NeedsReview flag, full accept/reject flow |
| Task aggregate | EXISTS -- `TasklistItem.cs` with recurring tasks, association, UI |
| ChartOfAccounts aggregate | EXISTS -- 540 lines, hierarchy, group sets, standard chart |
| Reconciliation pipeline | EXISTS -- `Pipeline.cs` with 12+ steps |
| Double-entry validation | EXISTS -- `ValidationEngine.cs` |

---

_This proposal was generated by the work-decomposer agent from the small-accountant-flow deliverable. No GitHub Issues have been created. Pending human review and approval._
