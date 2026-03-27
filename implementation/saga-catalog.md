# Saga Catalog

Last reviewed: 2026-03-27

Source: GitHub Issues & PRs (`event-systems/powermodels`)
Period: 2025-06-01 — 2026-03-27
Branch: `joshkempner/journal-aggregate` (active development branch)
Domain-crossing PRs analyzed: 38+ (PRs touching 2+ domain contexts: ModelServer, SpreadsheetAdapter, TeamModelMgmt, Connectors)

## Identified Workflow Chains

### 1. Reconciliation Workflow (Largest Saga)

**Epics:** #1851, #1869, #1905, #1946
**Milestone:** Shoebox RC4
**Status:** Closed (implemented)

**Flow:**
```
DataSource (upload CSV/PDF)
  → DataElement[] (parse rows into individual records)
    → ChartOfAccounts (match accounts)
      → SpreadsheetAdapter RMs (ChartOfAccountsRm, DataSourceRm, DataSourceListRm)
        → UIBehavior RMs (DataTableMapsRm, JournalCashTransactionsRm)
          → User categorizes/reconciles in WPF UI
            → AccountBalance (update balances)
```

**Contexts crossed:** ModelServer/AccountingSystem → ModelServer → SpreadsheetAdapter → UIBehavior → WPF
**Aggregates involved:** DataSource, DataElement, ChartOfAccounts, AccountBalance, CustomStatementTxType
**Key PRs:** #1617 (AI reconciliation), #1671 (CSV preprocessing), #1707 (reconcile uploaded files), #2064 (bug fixes for mapping/dates), #2093 (transaction extraction reliability)
**Coordination pattern:** Implicit — handler services in SpreadsheetAdapter coordinate between domain aggregates and UI read models. No dedicated process manager class.

### 2. Business Setup Workflow (PMA/PME Split)

**Epics:** #1526 (CoA), #2099 (Accounting phase 2)
**Milestone:** RC3–March Demo

**Two paths now exist:**

**PMA (WithoutModels) — active:**
```
ClientWorkspace (create workspace)
  → Business (create business with description)
    → AccountingSystem (create 1:1 with business)
      → ChartOfAccounts (create with standard/empty/imported list)
        → Account[] (add root + child accounts)
          → AccountBalance[] (opening balances — new step)
            → InitializeJournalDataSource
              → SpreadsheetAdapter (hydrate all read models)
                → UIBehavior (create business workflow RM)
```
Guided by `NewBusinessWorkflowWithoutModelsVm`: Settings → CoA → Opening Balances → Journal → Create.

**PME (WithModels) — on hold:**
```
Same steps 1-5, plus:
  → CreateModel (financial model)
    → Tasks setup
```
Guided by `NewBusinessWorkflowWithModelsVm`: Settings → CoA → Journal → Tasks → Create.

**Contexts crossed:** ModelServer → ModelServer/AccountingSystem → SpreadsheetAdapter → UIBehavior → WPF
**Aggregates involved:** ClientWorkspace, AccountingSystem, ChartOfAccounts, AccountBalance, Journal
**Key PRs:** #1700 (CoA setup options), #1755 (account subtypes), #2100 (opening balance UI), #2105 (opening balances during setup)
**Coordination pattern:** Implicit — ClientWorkspaceService and AccountingSystemService coordinate the multi-step creation. Business setup triggers ~50-80 stream creations.

### 3. Data Import Pipeline (Composable)

**Epics:** #1846 (import and view statements)
**Milestone:** Shoebox RC4 + journal-aggregate branch

**Flow (updated with composable pipeline):**
```
User uploads CSV/PDF
  → DataIngestionPipeline (orchestrator)
    → CsvReaderStep / PDFReaderStep (parse raw data)
      → ColumnMappingStep (map source columns)
        → TransactionNormalizerStep (normalize transactions)
          → ClassificationRuleStep / AIClassificationStep (classify accounts)
            → AddOrUpdateDataSourceStep (persist via DataSource aggregate)
              → JournalPostingStep (post to Journal aggregates — new)
                → SpreadsheetAdapter RMs (fan-out to read models)
```

**Contexts crossed:** ModelServer → ModelServer/AccountingSystem → SpreadsheetAdapter
**Aggregates involved:** FileStore, DataSource, DataElement, Journal, JournalEntry, DataTableDefinition, DataTableMap, ListDataTableMap, ServerFinancialModel
**Key PRs:** #1673 (boolean fields for LDTs/data sources), #1525 (RM threading simplification), #2031 (PDF validation UX), #2091 (expected balance on upload)
**Coordination pattern:** Implicit via composable `IDataIngestionStep` pipeline in SpreadsheetAdapter/Reconciliation/. Now includes journal posting step. Highest write volume saga (~1,001 events per CSV import).
**Data usage impact:** Single CSV upload generates ~1,001 events distributed across category streams. Events on `$ce-DataSource` fan out to 3 RMs; events on `$ce-DataElement` fan out to 1 RM. New `$ce-Journal` and `$ce-JournalEntry` streams add 1–2 RM subscribers each.

### 4. Counterparty/Entity Management

**Epics:** (no dedicated epic)
**Milestone:** Shoebox RC3–RC4

**Flow:**
```
Counterparty (create as org or individual)
  → Role assignment (Customer/Vendor/Employee/Contractor flags)
    → Per-role entity creation (Customer, Vendor, Employee, Contractor aggregates)
      → Product (income/expense mappings)
        → SpreadsheetAdapter RMs (CounterpartiesRm, CustomerListRm, VendorListRm, etc.)
          → UIBehavior RMs (CounterpartiesRm SourceCache)
```

**Contexts crossed:** ModelServer/AccountingSystem → ModelServer → SpreadsheetAdapter → UIBehavior
**Aggregates involved:** Counterparty, Customer, Vendor, Employee, Contractor, Product
**Key PRs:** #1732 (counterparties domain), #1733 (counterparties UI), #1628 (item numbers), #1632 (customer/vendor UI), #1640 (products)
**Coordination pattern:** Implicit — AccountingSystemService handles all counterparty and role-entity creation in a single handler with 34 commands.

### 5. Financial Model Table Mapping

**Epics:** (no dedicated epic — part of core model operations)
**Milestone:** Shoebox RC3

**Flow:**
```
DataTableDefinition (define structure)
  → DataTableMap / ListDataTableMap / ManualTableMap (create mapping)
    → ServerFinancialModel (register mapping via events)
      → SpreadsheetAdapter RMs (FinancialModelRm subscribes to 49 events)
        → UIBehavior RMs (FinancialModelTablesRm subscribes to 54 events)
          → Excel (InputTablesRm, ManualTableListRm)
```

**Contexts crossed:** ModelServer → SpreadsheetAdapter → UIBehavior → Excel
**Aggregates involved:** DataTableDefinition, DataTableMap, ListDataTableMap, ManualTableMap, ManualTable, ServerFinancialModel
**Key PRs:** #1557 (cumulative mappings), #1565 (delete mappings), #1567 (single-value tables), #1571 (update mappings), #1573 (remove local accounts), #1748 (single-line tables), #1753 (semantic formulas)
**Coordination pattern:** Implicit — mapping services (DataSourceModelMapService, DataTableModelMappingService, etc.) coordinate between mapping aggregates and ServerFinancialModel. **This is where God Aggregate coupling is most visible** — every mapping operation raises events in both the mapping aggregate AND ServerFinancialModel.

### 6. Task Management Workflow

**Epics:** #1793 (schedules/tasks)
**Milestone:** Shoebox RC4–RC5

**Flow:**
```
TasklistItem (create task)
  → Schedule (optional recurrence)
    → SpreadsheetAdapter RMs (SingleClientTasklistRm: 21 events, WorkspaceTasklistRm: 21 events)
      → UIBehavior RMs (SingleUserTasklistRm, SingleWorkspaceTasklistRm)
        → TeamModelMgmt RMs (TaskMetricsRm, SingleModelTaskMetricsRm)
```

**Contexts crossed:** ModelServer → SpreadsheetAdapter → UIBehavior → TeamModelMgmt
**Aggregates involved:** TasklistItem, ManagedFinancialModel, TeamSettings
**Key PRs:** #1725 (task lists), #1776 (recurring tasks), #1786 (semi-annual recurrence), #2024 (cross-workspace tasks), #2049 (workspace tasks in standalone app)
**Coordination pattern:** Implicit — TasklistService (20 commands) coordinates with TeamModelMgmt for task metrics. **Only saga that crosses into TeamModelMgmt.**

### 7. Journal Data Lifecycle (New)

**Epics:** #2099 (Accounting phase 2)
**Milestone:** journal-aggregate branch
**Status:** In progress

**Flow:**
```
DataIngestionPipeline (CSV/PDF import)
  → Journal (create journal container per accounting system)
    → JournalEntry[] (create individual transactions)
      → JournalEntry categorization (assign accounts)
        → JournalEntry recategorization (correct assignments)
          → JournalEntry counterparty association
            → JournalsRm + JournalEntriesRm (domain-layer read models)
              → AccountingReportsContext RMs (BalanceSheetRm, IncomeStatementRm, etc.)
```

**Contexts crossed:** SpreadsheetAdapter (ingestion) → ModelServer (journal aggregates + domain RMs) → UIBehavior (AccountingReportsContext)
**Aggregates involved:** Journal, JournalEntry, ChartOfAccounts (for account lookup)
**Coordination pattern:** Implicit — JournalAggregatesService handles Journal + JournalEntry commands. Report RMs subscribe to journal events via AccountingReportsContext (bypasses SpreadsheetAdapter ACL).
**Significance:** First workflow where read models bypass SpreadsheetAdapter entirely, establishing the migration pattern for PMA features.

### 8. Accounting Reports (New)

**Epics:** #2099 (Accounting phase 2)
**Milestone:** journal-aggregate branch
**Status:** In progress

**Flow:**
```
Journal/JournalEntry events + ChartOfAccounts events
  → AccountingReportsContext (wired in UIBehavior, parallel to ACL)
    → BalanceSheetRm, IncomeStatementRm, CashFlowStatementRm
    → GeneralLedgerRm, TrialBalanceRm
    → IncomeExpenseSummaryRm, JournalReportRm
      → UIBehavior ViewModels (report views)
```

**Contexts crossed:** ModelServer (events) → UIBehavior (AccountingReportsContext) — bypasses SpreadsheetAdapter
**Aggregates consumed:** Journal, JournalEntry, ChartOfAccounts (read-only via events)
**Coordination pattern:** Direct event subscription — no ACL translation. First parallel read-side interface.

### 9. Standalone App Integration

**Epics:** #2017 (standalone desktop app)
**Milestone:** Shoebox RC5

**Flow:**
```
App host (new WPF standalone, separate from Excel add-in)
  → Shares UIBehavior components with Excel
    → Shares SpreadsheetAdapter read models
      → Same ModelServer domain
```

**Contexts crossed:** App → UIBehavior → WPF → Excel → SpreadsheetAdapter
**Key PRs:** #2022 (app outline), #2028 (upload view), #2037 (reconcile transactions), #2040 (categorize), #2049 (workspace tasks), #2065 (business setup UIs)
**Coordination pattern:** Host-level — not a domain saga. The standalone app reuses existing domain infrastructure. 57 PRs touched App context.

## Implicit vs. Explicit Sagas

| Pattern | Count | Examples |
|---------|-------|---------|
| **Implicit (in handler services)** | 8 | All identified sagas — coordination buried in services |
| **Explicit (dedicated process manager)** | 0 | No dedicated saga/process manager classes found in PowerModels codebase |

**All coordination is implicit.** There are no dedicated process manager or saga classes. Multi-aggregate workflows are coordinated by handler services that call `GetById` on multiple aggregates and send commands within the same handler invocation. This means:
- No timeout/compensation logic for multi-step operations
- No explicit failure recovery — if step 3 of a workflow fails, steps 1-2 are already committed
- No visibility into workflow state — you can't ask "what step is this import on?"

### Available Infrastructure (Not Yet Integrated)

Formal process manager base classes exist in `process-manager/Infrastructure/` ready for ReactiveDomain integration:

| Class | Role |
|-------|------|
| `ProcessAggregateBase<TState>` | Event-sourced state machine — `InState().On<TEvent>()` DSL, `IssueCommand()`, `CreateStepTimeout()` with staleness detection |
| `ProcessManagerBase<TProcessAgg, TState>` | Read model coordinator — NullBus replay safety, `LoadOrCreate`/`SaveAndTakeCommands`/`DispatchCommands`, `GoLive()` recovery |
| `CategoryStreamWarmup` | ESDB `$ce-*` stream warmup for process manager category subscriptions |
| `EventVersioning` + `VersionedEventSerializer` | Event schema evolution with `[EventVersion]` attribute and upcaster chain |

**Mitigation plan** (`_output/mitigation-plan-2026-03-07.md`) targets Reconciliation and Period Close as the first two ProcessManager implementations in Phase 3.

## Saga Risk Matrix

| Saga | Complexity | Aggregates | Contexts | Failure Impact | Priority |
|------|-----------|-----------|----------|---------------|----------|
| Reconciliation | High | 5 | 5 | Data corruption (journal entries) | 🔴 P0 |
| Data Import | High | 9 | 3 | ~1,001 events committed with no rollback | 🔴 P0 |
| Journal Data Lifecycle | High | 3 | 3 | Journal entry corruption, report inconsistency | 🔴 P0 |
| Financial Model Mapping | High | 6 | 4 | God Aggregate coupling | 🟡 P1 |
| Business Setup | Medium | 5 | 4 | 50-80 streams created atomically | 🟡 P1 |
| Accounting Reports | Medium | 0 (read-only) | 2 | Report projection errors | 🟡 P1 |
| Counterparty Management | Medium | 6 | 4 | Role consistency across entities | 🟢 P2 |
| Task Management | Low | 3 | 4 | Low impact — task state only | 🟢 P2 |
| Standalone App | Low | 0 | 5 | Host-level only, no domain coordination | 🟢 P3 |
