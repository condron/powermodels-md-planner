---
type: action-plan
source: feature-inventory.md, user-journeys.md, classification-rules.md, excel-output-mapping.md
target: PowerModels Accounting (PMA) — WPF desktop app
codebase_branch: joshkempner/journal-aggregate
generated: 2026-03-27
---

# Action Plan — Lisa Features → PMA

Gap analysis and delivery plan for implementing 35 extracted features from the Lisa General_Demo into the PowerModels WPF application.

---

## 1. Feature-by-Feature Gap Matrix

### Legend

| Coverage | Meaning |
|----------|---------|
| **Full** | PM already implements this capability end-to-end |
| **Partial** | PM has the infrastructure but is missing specific Lisa-equivalent logic |
| **None** | New build required |
| **N/A** | Not applicable — Lisa-specific tooling concept that doesn't map to PMA |

---

### 1.1 PDF Ingestion

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-ING-01** | PDF Text Extraction (pdfplumber) | `PDFReaderStep` — uses AI (GPT-4o vision) to extract transactions from PDF page images | **Partial** | PM uses AI vision instead of deterministic text extraction. PM sends page images to GPT-4o and gets structured JSON back. No raw text extraction layer exists. Lisa's pdfplumber approach is deterministic and cheaper but less flexible. |
| **F-ING-02** | Plugin-Based Statement Type Detection | `PreprocessorProvider` + JSON preprocessor files (chase.json, wells_fargo.json, etc.) | **Partial** | PM has a preprocessor registry (`PreprocessorProvider`) that loads JSON mapping templates from `%LocalAppData%\PowerModels\Preprocessors\`. These define column mappings for CSV files, not PDF detection. PM has no confidence-scored plugin detection for PDFs — the user manually selects the account type and institution. |
| **F-ING-03** | Auto-Discovery of Input PDFs | None | **None** | PM uses explicit file selection via UI dialog. No batch discovery or auto-ingestion of a folder of PDFs. |
| **F-ING-04** | Manual Run List (run_list.csv) | None | **N/A** | Lisa-specific batch mode. PM processes files one at a time through the UI. Batch import could be useful but is a different UX paradigm. |

### 1.2 Parsing

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-PAR-01** | Institution-Specific Deterministic Parsers | `PDFReaderStep` (AI-based) + `CsvReaderStep` + `ColumnMappingStep` (heuristic) | **Partial** | PM handles CSV parsing with auto-detected column mapping (`ColumnMappingStep` scores headers against field patterns). PDF parsing is entirely AI-driven (no deterministic parsers). PM has no institution-specific parser modules — the `ColumnMappingStep` is generic. Adding deterministic parsers would reduce AI costs and improve reliability for known formats. |
| **F-PAR-02** | Canonical Statement Model | `AccountTransaction` record + `DataIngestionContext` | **Partial** | PM's `AccountTransaction` serves a similar role to Lisa's `CanonicalStatement` but is flatter — it lacks: `statement_id`, `institution`, `parser_id`, `parser_confidence`, `instrument_type` at the statement level, `period`, `opening_balance`, `closing_balance`, `currency`. PM's model is transaction-level only; there's no statement-level envelope. |
| **F-PAR-03** | Multi-Instrument Type Support | `NormalBalanceSide` enum + `CashDirection` enum + polarity logic in `TransactionNormalizerStep` and `ClassificationRuleStep` | **Full** | PM already handles asset (debit-normal) vs liability (credit-normal) instruments correctly. `NormalBalanceSide`, `CashDirection`, and the normalization/classification logic all account for instrument polarity. |
| **F-PAR-04** | Parser Scaffolding Tool | None | **N/A** | Lisa-specific CLI dev tool. PM uses agent-based development workflow with GitHub skills. Not needed as a product feature. |

### 1.3 Transaction Classification

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-CLS-01** | Sequenced Rule Engine | `ClassificationRuleStep` — keyword-based rule matching with priority and confidence scoring | **Partial** | PM has a rule engine but it differs from Lisa's: PM uses keyword matching with weighted scoring (description 1.0, category 0.5, reference 0.3, txType 0.2) and priority-based ordering. Lisa uses sequenced deterministic rules with first-match-wins and tiered confidence. PM's model is more flexible (user-configurable keywords) but lacks Lisa's deterministic guarantee and tier system. PM also has `AIClassificationStep` as fallback for low-confidence results. |
| **F-CLS-02** | Confidence Tier System | `ClassificationResult.Confidence` (0.0–1.0 decimal) + `DataIngestionContext.ClassificationScoreThreshold` (0.8) | **Partial** | PM uses a continuous 0.0–1.0 confidence score with a threshold (0.8) to decide whether to escalate to AI. Lisa uses discrete tiers (Green/Orange/Red). PM's approach is functionally richer but lacks the human-readable tier labels and the "every transaction gets a rule" guarantee. |
| **F-CLS-03** | Moneris Revenue / GST Split | None | **None** | No transaction splitting logic exists in PM. This is a client-specific rule (LBTY-010A). The general capability — splitting a transaction into multiple journal lines (e.g., net + tax) — is partially supported by `JournalPostingStep.GenerateEntryWithFee()` which creates 3-line entries for fee separation. Extending this pattern to tax splitting would be the approach. |
| **F-CLS-04** | Payroll Subset-Sum Matching | None | **None** | No combinatorial matching logic in PM. This is a client-specific rule. The general capability — identifying a set of transactions that sum to a known total — does not exist. |
| **F-CLS-05** | Rent Heuristic Detection | None | **None** | No amount-range + date heuristic rules in PM. PM's `ClassificationRuleStep` only does keyword matching, not amount/date pattern matching. |
| **F-CLS-06** | Liability Credit Handling | `ClassificationRuleStep.DoesDirectionMatch()` + `GetMoneyDirection()` | **Partial** | PM's rule step already filters by `CashDirection` and handles liability polarity correctly via `NormalBalanceSide`. However, there's no explicit "liability credits are NOT income" guardrail rule that runs before fallback classification. The AI step handles this implicitly via prompts. |
| **F-CLS-07** | Overrides System | None | **None** | No per-month/per-account override mechanism. PM's classification is fully automatic (rules + AI). Adding overrides would require a new UI and storage mechanism. |

### 1.4 Double-Entry Accounting

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-ACC-01** | Ledger-to-Journal Conversion | `JournalPostingStep` — creates double-entry `JournalEntry` objects from classified `AccountTransaction`s | **Full** | PM's `JournalPostingStep` does exactly this: for each classified transaction, creates a debit line and credit line using `ClassificationResult.GetDebitAccountId()`/`GetCreditAccountId()`. Also handles fee separation (3-line entries). The candidate entries are then posted to Journal aggregates via `JournalService`. |
| **F-ACC-02** | Opening Balance Journal (CSV) | `ChartOfAccounts` aggregate with `AddAccountWithOpeningBalance` command + `NewAccountOpeningBalanceVm` | **Full** | PM handles opening balances as part of account creation in the ChartOfAccounts aggregate. The `NewAccountOpeningBalanceDialog.xaml` provides UI for setting opening balances on individual accounts. Business seeding also supports opening balances via `accounts.json` seed data. |
| **F-ACC-03** | Opening Balance Auto-Computed | None | **None** | PM does not auto-derive opening balances from first-statement data. Opening balances are always explicit (entered by user or via seed data). Lisa's auto-computed version isn't used in its own main pipeline either — noted as "design intent." Low priority. |
| **F-ACC-04** | Trial Balance | `TrialBalanceRm` (89 lines) — report RM in AccountingReportsContext | **Full** | PM has a complete TrialBalance report RM with full test coverage (`TrialBalanceRmTests.cs`, `TrialBalanceRmIntegrationTests.cs`). Uses the `ReportBuilder` DSL. |
| **F-ACC-05** | Balance Sheet | `BalanceSheetRm` (135 lines) — report RM in AccountingReportsContext | **Full** | PM has a complete BalanceSheet report RM with full test coverage. Uses `SideBySideReportBuilder` with left/right panels. |
| **F-ACC-06** | Reconciliation Control | Reconciliation folder in SpreadsheetAdapter + partial infrastructure | **Partial** | PM has reconciliation infrastructure (the entire `Reconciliation/` folder) but it's focused on the data ingestion pipeline flow, not a standalone reconciliation report. There's no dedicated per-statement reconciliation report (opening + txns = closing) or reconciliation tab equivalent to Lisa's. The accounting invariants are enforced at import time, not as a post-hoc verification report. |

### 1.5 Financial Reporting

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-RPT-01** | P&L by Fiscal Year | `IncomeStatementRm` (99 lines) + `IncomeExpenseSummaryRm` (163 lines) | **Partial** | PM has Income Statement and Income/Expense Summary reports but they use `ReportPeriod.DateRange` — user-selectable date ranges, not auto-grouped by fiscal year. PM lacks a fiscal year concept entirely (no `FiscalYearStartMonth` setting, no fiscal year grouping). The reports show totals for a selected period, not a monthly matrix pivoted by fiscal year. |
| **F-RPT-02** | Excel Workbook Output | None | **None** | PM displays reports in WPF views (`AccountingReport.xaml`, `ReportsContainer.xaml`). No Excel export capability exists. This is a significant gap for accounting firms that need Excel deliverables. |
| **F-RPT-03** | Run Summary | None | **None** | No import summary/dashboard showing per-statement intake results. PM processes files through the pipeline but doesn't aggregate results into a summary view. |
| **F-RPT-04** | Diagnostic Sheet | None | **None** | No account-level diagnostic report. The closest is the Trial Balance which shows per-account totals, but it lacks AccountType and IncludedInNI columns. |
| **F-RPT-05** | Exception Report | None | **None** | No exception flagging for accounts with contra-directional activity. This would be a new report RM in `AccountingReportsContext`. |

### 1.6 Audit & Traceability

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-AUD-01** | Source Transaction ID | `AccountTransaction.DataElementId` (Guid) + `AccountTransaction.Id` (row number) | **Partial** | PM uses Guids (`DataElementId`) for tracing through the event store and row numbers for pipeline ordering. Lisa uses hash-based IDs for cross-reference stability. PM's Guid approach is actually stronger for event-sourced systems but there's no human-readable stable hash for audit reports. |
| **F-AUD-02** | Rule Register | None | **None** | No tabular rule documentation output. PM's `ClassificationRule` model stores rules but there's no report RM or view that displays them as a reference table. |
| **F-AUD-03** | Rule Hit Tracking | None | **None** | PM's `ClassificationResult.Source` tracks "Rule" vs "AI" but doesn't record which specific rule matched. The `ClassificationRule.TimesUsed` and `LastUsed` fields exist but are infrastructure-level, not per-transaction audit trail. |
| **F-AUD-04** | Statement ID Hashing | None | **None** | PM has no statement-level identity concept. `DataIngestionContext.FileId` is the closest (Guid per import), but there's no hash-based deduplication or statement-level metadata tracking. |
| **F-AUD-05** | Layout Archive | None | **N/A** | Lisa-specific dev tool for PDF layout analysis. PM's AI-based approach doesn't need layout archives since it interprets page images directly. |

### 1.7 Configuration

| ID | Lisa Feature | PM Equivalent | Coverage | Gap |
|----|-------------|---------------|----------|-----|
| **F-CFG-01** | Opening Balances Config | `ChartOfAccounts` aggregate + `NewAccountOpeningBalanceVm` + seed data | **Full** | PM handles opening balances through the domain model (ChartOfAccounts commands) and provides UI for entry. |
| **F-CFG-02** | Fiscal Year Configuration | None | **None** | No fiscal year concept in PM. Reports use arbitrary date ranges. This is a prerequisite for P&L-by-fiscal-year reporting. Would need a new domain concept (likely on the ClientWorkspace aggregate or a new FiscalYearConfig value object). |
| **F-CFG-03** | Payroll Overrides Config | None | **None** | No override mechanism. Client-specific; low priority for general-purpose product. |
| **F-CFG-04** | Plugin Registration | `PreprocessorProvider` — discovers JSON mapping files from `%LocalAppData%\PowerModels\Preprocessors\` | **Partial** | PM has a file-discovery pattern for CSV preprocessors but not for PDF parsers/plugins. Extending this to a parser registry would be straightforward. |

---

## 2. Gap Summary

### Coverage Totals

| Coverage | Count | Features |
|----------|-------|----------|
| **Full** | 7 | F-PAR-03, F-ACC-01, F-ACC-02, F-ACC-04, F-ACC-05, F-CFG-01, F-ACC-01 |
| **Partial** | 12 | F-ING-01, F-ING-02, F-PAR-01, F-PAR-02, F-CLS-01, F-CLS-02, F-CLS-06, F-ACC-06, F-RPT-01, F-AUD-01, F-CFG-04 |
| **None** | 13 | F-ING-03, F-CLS-03, F-CLS-04, F-CLS-05, F-CLS-07, F-ACC-03, F-RPT-02, F-RPT-03, F-RPT-04, F-RPT-05, F-AUD-02, F-AUD-03, F-AUD-04, F-CFG-02, F-CFG-03 |
| **N/A** | 3 | F-ING-04, F-PAR-04, F-AUD-05 |

### Key Architectural Gaps

1. **No Fiscal Year Concept** — PM has no fiscal year start month, no fiscal year grouping. Prerequisite for P&L-by-FY and fiscal reconciliation.

2. **No Statement-Level Envelope** — PM's pipeline processes transactions individually. There's no `StatementImport` aggregate or statement-level metadata (institution, period, opening/closing balance, reconciliation status).

3. **No Deterministic PDF Parsers** — PM uses AI vision (GPT-4o) for all PDF extraction. Adding deterministic parsers for known formats would reduce cost and improve reliability.

4. **No Excel Export** — PM renders reports in WPF views only. Accounting firms expect Excel deliverables.

5. **No Rule Audit Trail** — PM's classification rules don't record which rule matched per transaction. The `ClassificationResult` only stores "Rule" vs "AI" as source.

6. **No Transaction Splitting** — PM creates 1:1 or 1:1+fee journal entries. Lisa supports N:M splitting (e.g., Moneris → Revenue + GST). The `JournalPostingStep.GenerateEntryWithFee()` pattern is close but not general-purpose.

7. **Classification Rules Are ACL-Layer Only** — PM's `ClassificationRule` is a POCO in `SpreadsheetAdapter.Reconciliation.Models`, not a domain aggregate. Rules aren't persisted in the event store. They live in the `DataIngestionContext` at pipeline execution time but have no permanent domain representation.

---

## 3. Architecture Mapping — ES-DDD Patterns for Gaps

### 3.1 Statement Import Aggregate (NEW)

**Addresses:** F-PAR-02 (canonical model), F-AUD-04 (statement ID), F-ACC-06 (reconciliation), F-RPT-03 (run summary)

A new `StatementImport` aggregate to represent an imported statement as a first-class domain concept:

```
Aggregate: StatementImport
Namespace: ModelServer.Domain.AccountingSystem

Commands:
  - CreateStatementImport(importId, fileId, institution, accountId, instrumentType, period, openingBalance, closingBalance)
  - RecordExtractionResult(importId, transactionCount, extractionMethod)
  - RecordClassificationResult(importId, classifiedCount, unclassifiedCount)
  - RecordReconciliationResult(importId, computedClosing, diff, isReconciled)
  - MarkImportComplete(importId)
  - MarkImportFailed(importId, reason)

Events:
  - StatementImportCreated
  - ExtractionResultRecorded
  - ClassificationResultRecorded
  - ReconciliationResultRecorded
  - ImportCompleted
  - ImportFailed

State:
  - ImportId, FileId, Institution, AccountId, InstrumentType
  - Period (YYYY-MM), OpeningBalance, ClosingBalance
  - Status: Created → Extracted → Classified → Reconciled → Complete | Failed
  - TransactionCount, ClassifiedCount, UnclassifiedCount
  - ReconciliationDiff
```

**Read Models:**
- `StatementImportsRm` — list of all imports with status (for Run Summary view)
- Extend `AccountingReportsContext` with a Reconciliation report RM

### 3.2 Classification Rule Aggregate (NEW)

**Addresses:** F-CLS-01 (rule engine), F-CLS-02 (confidence tiers), F-AUD-02 (rule register), F-AUD-03 (rule hits)

Promote `ClassificationRule` from an ACL-layer POCO to a domain aggregate:

```
Aggregate: ClassificationRule
Namespace: ModelServer.Domain.AccountingSystem

Commands:
  - CreateClassificationRule(ruleId, accountingSystemId, name, priority, keywords[], direction, accountId, confidenceTier)
  - UpdateClassificationRule(ruleId, ...)
  - DeactivateClassificationRule(ruleId)
  - RecordRuleHit(ruleId, transactionId, importId)

Events:
  - ClassificationRuleCreated
  - ClassificationRuleUpdated
  - ClassificationRuleDeactivated
  - RuleHitRecorded

State:
  - RuleId, Name, Priority, Keywords, Direction, AccountId
  - ConfidenceTier (Green/Orange/Red)
  - IsActive
  - HitCount, LastHitDate
```

**Read Models:**
- `ClassificationRulesRm` — domain-layer projection for pipeline consumption (replaces the current POCO list)
- `RuleRegisterRm` — report RM for audit (rule table display)
- `RuleHitsRm` — per-transaction audit trail of which rule matched

**Migration:** The existing `ClassificationRuleStep` would read from `ClassificationRulesRm` instead of receiving rules via `DataIngestionContext.ClassificationRules`. The step itself remains in the ACL layer.

### 3.3 Fiscal Year Value Object (NEW)

**Addresses:** F-CFG-02 (fiscal year config), F-RPT-01 (P&L by FY)

Add fiscal year configuration to the `ClientWorkspace` aggregate (or `ChartOfAccounts`):

```
Value Object: FiscalYearConfig
  - StartMonth (1-12, default 1 = calendar year)

Command on ClientWorkspace:
  - SetFiscalYearConfig(workspaceId, startMonth)

Event:
  - FiscalYearConfigSet(workspaceId, startMonth)
```

Reports would consume this via a small `FiscalYearConfigRm` or by adding the field to an existing workspace RM.

### 3.4 Enhanced Classification Step (MODIFY)

**Addresses:** F-CLS-05 (rent heuristic), F-CLS-06 (liability guardrail)

Extend `ClassificationRuleStep` to support rule types beyond keyword matching:

```
New rule types (on ClassificationRule aggregate):
  - KeywordMatch (existing)
  - AmountRangeMatch (new — min, max, dayOfMonth range)
  - CompositeMatch (new — combine keyword + amount + date conditions)

New pre-classification guardrail:
  - LiabilityCreditGuardrail — before fallback, mark unclassified liability credits as the source account
```

### 3.5 Transaction Splitting in JournalPostingStep (MODIFY)

**Addresses:** F-CLS-03 (GST split)

Generalize `JournalPostingStep.GenerateEntryWithFee()` to support N-way splits:

```
New concept: SplitRule
  - Condition: matches transaction by ClassificationRule or description pattern
  - Splits: list of (percentage, accountId, description suffix)
  
Example: Moneris GST Split
  - Condition: RuleId = "moneris-settlement"
  - Split 1: 95.238% → Income account (net)
  - Split 2: 4.762% → GST Payable (5% of gross)
  - Rounding: last split absorbs remainder
```

This could live as a property on the `ClassificationRule` aggregate or as a separate `SplitTemplate` value object.

### 3.6 Excel Export Service (NEW)

**Addresses:** F-RPT-02 (Excel output)

New service in SpreadsheetAdapter that takes an `IAccountingReport` and writes it to Excel:

```
Service: ReportExportService
Namespace: SpreadsheetAdapter.Services

Method: ExportToExcel(IEnumerable<IAccountingReport> reports, string outputPath)

Dependencies: ClosedXML or EPPlus (NuGet)

Behavior:
  - One sheet per report (using IAccountingReport.SheetName)
  - Columns from IReportPanel.Columns
  - Sections with subtotals from IReportSection
  - Grand total from IReportPanel.TotalRow
  - Auto-fit columns, currency formatting
```

The existing `ReportBuilder` DSL already produces structured `IAccountingReport` objects with panels, sections, columns, and totals — making this a clean export layer.

### 3.7 New Report RMs (NEW)

**Addresses:** F-RPT-03, F-RPT-04, F-RPT-05

| Report | Base Class | Pattern | Key Data |
|--------|-----------|---------|----------|
| **RunSummaryRm** | `AccountingReportBase` | Vertical | Per-import: file, account, balances, txn count, diff |
| **DiagnosticRm** | `AccountingReportBase` | Vertical | Per-account: type, debit, credit, net, included-in-NI |
| **ExceptionReportRm** | `AccountingReportBase` | Vertical | Journal lines where Income/Expense account has contra-directional activity |
| **ReconciliationReportRm** | `AccountingReportBase` | Vertical | Per-statement: opening, credits, debits, computed closing, diff |

All follow the established `AccountingReportBase` + `ReportBuilder` DSL pattern with full test coverage.

---

## 4. Dependency Graph

```
                      F-CFG-02 Fiscal Year Config ──────────────────────┐
                                                                        │
F-ING-01 PDF Extraction ──┐                                            │
F-ING-02 Detection ───────┤                                            │
F-PAR-01 Parsers ─────────┼──▶ F-PAR-02 Canonical Model ──┐           │
F-PAR-03 Multi-Instrument ┘    (StatementImport agg)      │           │
                                                            ▼           ▼
                           F-CLS-01 Rule Engine ──────▶ F-ACC-01 ──▶ F-RPT-01 P&L by FY
                           F-CLS-02 Confidence Tiers      Journal     F-RPT-02 Excel Export
                           F-CLS-05 Rent Heuristic        Posting     F-RPT-03 Run Summary
                           F-CLS-06 Liability Guard                   F-RPT-04 Diagnostic
                                     │                                F-RPT-05 Exceptions
                                     ▼                                     │
                           F-CLS-03 GST Split ──────────────────────────┘
                           F-AUD-02 Rule Register                       │
                           F-AUD-03 Rule Hits ──────────────────────────┘
                                                                        │
                           F-ACC-04 Trial Balance ◄─────────────────────┘
                           F-ACC-05 Balance Sheet    (already built)
                           F-ACC-06 Reconciliation ◄── StatementImport agg
```

**Critical Path:** StatementImport aggregate → Enhanced ClassificationRule aggregate → New report RMs → Excel export

---

## 5. Delivery Increments

### Increment 0: Foundation — Domain Model Extensions
**Features:** F-PAR-02, F-AUD-04, F-CFG-02  
**Effort:** Medium  
**Rationale:** These are prerequisites for subsequent increments.

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `StatementImport` aggregate | New | ModelServer | Statement-level metadata + lifecycle |
| `StatementImportCommands.cs` | New | ModelServer/Messages | Commands for StatementImport |
| `StatementImportEvents.cs` | New | ModelServer/Messages | Events for StatementImport |
| `StatementImportService.cs` | New | ModelServer/Services | Command handler |
| `StatementImportsRm.cs` | New | ModelServer/Services | Domain read model |
| `FiscalYearConfig` | Modify | ModelServer | Value object + command on ClientWorkspace |
| `StatementImportTests.cs` | New | ModelServer.Tests | Aggregate + service tests |

### Increment 1: Classification Engine Enhancement
**Features:** F-CLS-01, F-CLS-02, F-CLS-05, F-CLS-06, F-AUD-02, F-AUD-03  
**Effort:** Large  
**Rationale:** Promotes classification rules to domain aggregates, adds rule types, audit trail.

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `ClassificationRule` aggregate | New | ModelServer | Domain aggregate replacing POCO |
| `ClassificationRuleCommands.cs` | New | ModelServer/Messages | Commands |
| `ClassificationRuleEvents.cs` | New | ModelServer/Messages | Events |
| `ClassificationRuleService.cs` | New | ModelServer/Services | Command handler |
| `ClassificationRulesRm.cs` | New | ModelServer/Services | Domain RM for pipeline |
| `RuleHitsRm.cs` | New | ModelServer/Services | Per-transaction audit trail |
| `ClassificationRuleStep.cs` | Modify | SpreadsheetAdapter | Read from RM, support new rule types, record hits |
| `AmountRangeMatch` | New | SpreadsheetAdapter | New rule type for amount+date patterns |
| `LiabilityCreditGuardrail` | New | SpreadsheetAdapter | Pre-fallback guardrail |
| Tests | New | ModelServer.Tests | Aggregate, service, RM tests |
| Tests | New | SpreadsheetAdapter.Tests | Enhanced step tests |

### Increment 2: Reporting — New Reports + Fiscal Year
**Features:** F-RPT-01 (enhance), F-RPT-03, F-RPT-04, F-RPT-05, F-ACC-06  
**Effort:** Medium  
**Depends on:** Increment 0 (StatementImport, FiscalYearConfig)

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `IncomeStatementRm.cs` | Modify | UIBehavior/Reports | Add fiscal year period grouping |
| `RunSummaryRm.cs` | New | UIBehavior/Reports | Import summary report |
| `DiagnosticRm.cs` | New | UIBehavior/Reports | Account-level diagnostic |
| `ExceptionReportRm.cs` | New | UIBehavior/Reports | Contra-directional activity flags |
| `ReconciliationReportRm.cs` | New | UIBehavior/Reports | Per-statement reconciliation |
| `AccountingReportsContext.cs` | Modify | UIBehavior/Reports | Register new RMs |
| WPF Views | Modify | PowerModels.WPF | Add report navigation for new reports |
| FlaUI tests | New | Utilities/UITestRunner | UI tests for new reports |
| RM tests | New | UIBehavior.Tests | Full test coverage per report |

### Increment 3: Excel Export
**Features:** F-RPT-02  
**Effort:** Medium  
**Depends on:** Increment 2 (all reports built)

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `ReportExportService.cs` | New | SpreadsheetAdapter | IAccountingReport → Excel workbook |
| NuGet: ClosedXML | New | SpreadsheetAdapter | Excel generation library |
| Export button in `ReportsContainerVm` | Modify | UIBehavior | "Export to Excel" command |
| Export dialog in `ReportsContainer.xaml` | Modify | WPF | Save dialog + progress |
| `ReportExportServiceTests.cs` | New | SpreadsheetAdapter.Tests | Export verification |

### Increment 4: Transaction Splitting
**Features:** F-CLS-03  
**Effort:** Small  
**Depends on:** Increment 1 (ClassificationRule aggregate)

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `SplitTemplate` value object | New | ModelServer | Split rules (percentage, account, label) |
| `ClassificationRule` aggregate | Modify | ModelServer | Add SplitTemplate property |
| `JournalPostingStep.cs` | Modify | SpreadsheetAdapter | Generalize fee pattern to N-way splits |
| Tests | New | SpreadsheetAdapter.Tests | Split posting tests |

### Increment 5: Deterministic PDF Parsers (Optional)
**Features:** F-ING-01 (enhance), F-ING-02 (enhance), F-PAR-01  
**Effort:** Large (per institution)  
**Depends on:** Increment 0 (StatementImport)

| Component | Type | Layer | Description |
|-----------|------|-------|-------------|
| `IStatementParser` interface | New | SpreadsheetAdapter | Parser contract |
| `ParserRegistry` | New | SpreadsheetAdapter | Plugin discovery + confidence scoring |
| Per-institution parsers | New | SpreadsheetAdapter | Deterministic regex-based parsers |
| `PDFReaderStep.cs` | Modify | SpreadsheetAdapter | Try deterministic parser first, fall back to AI |
| `PreprocessorProvider.cs` | Modify | SpreadsheetAdapter | Extend to manage parser configs |
| Tests | New | SpreadsheetAdapter.Tests | Parser tests per institution |

**Note:** This increment is optional because PM's AI-based PDF extraction already works. Deterministic parsers add cost reduction and reliability for high-volume known formats but are not a correctness gap.

---

## 6. Features Excluded from Plan

| Feature | Reason |
|---------|--------|
| **F-ING-03** Auto-Discovery | Batch processing paradigm doesn't fit PMA's interactive UI model |
| **F-ING-04** Run List CSV | Lisa-specific batch mode |
| **F-PAR-04** Parser Scaffolding | Dev tooling, not product feature |
| **F-CLS-04** Payroll Subset-Sum | Client-specific rule; too narrow for general product |
| **F-CLS-07** Overrides System | Adds configuration complexity; could be revisited later |
| **F-ACC-03** Auto-Computed Opening Balances | Not used in Lisa's own pipeline; explicit balances are better practice |
| **F-AUD-05** Layout Archive | Dev tooling for PDF analysis; PM uses AI, doesn't need layout archives |
| **F-CFG-03** Payroll Overrides | Paired with F-CLS-04 |

---

## 7. Open Questions

1. **ClassificationRule persistence model** — Should rules be per-business (on ChartOfAccounts/AccountingSystem) or global? Lisa's rules are hardcoded for one client. PM should support per-business rules with optional global templates.

2. **Excel export scope** — Should "Export to Excel" export a single report or all reports as a multi-tab workbook (like Lisa)? Multi-tab aligns better with accounting firm workflow.

3. **Deterministic parser priority** — Is there a near-term need for deterministic PDF parsers, or is the AI approach sufficient? This affects whether Increment 5 is built.

4. **Fiscal year on which aggregate** — `ClientWorkspace` (workspace-level setting) or `ChartOfAccounts` (accounting system-level)? Fiscal year is an accounting concept, so ChartOfAccounts may be more appropriate.

5. **Rule hit granularity** — Should `RuleHitRecorded` events go on the `ClassificationRule` aggregate (one event per hit on the rule) or on the `JournalEntry` aggregate (one event per classified entry)? The former creates hot streams for popular rules; the latter distributes evenly.

---

## 8. Risk Register

| Risk | Impact | Mitigation |
|------|--------|------------|
| **StatementImport aggregate adds stream growth** | Each PDF import creates a new aggregate instance with 4-6 events | Low per-business volume (12-50 statements/year); acceptable |
| **ClassificationRule as aggregate creates event overhead** | Each rule hit records an event | Consider recording hits as batch events or only on JournalEntry, not on ClassificationRule |
| **Excel export library dependency** | ClosedXML or EPPlus adds a NuGet dependency to SpreadsheetAdapter | Both are mature, MIT/LGPL licensed; ClosedXML preferred (MIT) |
| **Fiscal year migration** | Adding FiscalYearConfig to ClientWorkspace requires handling existing businesses without it | Default to calendar year (StartMonth=1) for existing businesses |
| **God aggregate risk** | Any temptation to add fiscal year, classification rules, or statement imports to ServerFinancialModel | Strictly separate aggregates per design rules |
| **Report RM proliferation** | 4 new report RMs add to startup replay cost | All subscribe to `$ce-ChartOfAccounts` + `$ce-DataElement` (same streams as existing reports); incremental cost is minimal since data is already being replayed |
