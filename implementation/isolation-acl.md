# Isolation & Anti-Corruption Layer

Last reviewed: 2026-03-27

Generated: 2026-03-27 (from `joshkempner/journal-aggregate` branch)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Context Hierarchy

```
SpreadsheetContextService (singleton, 130+ handler interfaces)
    ↓ routes by SpreadsheetContextMetadata
ClientWorkspaceContextService
    ↓ manages per-workspace
ClientWorkspaceContext (per workspace)
    ├── ModelServerContext[] (per business/sandbox)
    │   ├── ModelServerReadContext (read models: FinancialModelListRm, AccountBalancesRm, etc.)
    │   └── ModelServerWriteContext (services: AccountingSystemService, FinancialModelService, JournalAggregatesService, etc.)
    └── SpreadsheetContext[] (per model/workbook)
        └── Translates UI gestures → domain commands
```

## Read/Write Separation

### ModelServerReadContext

UI queries domain state only through read models wired here:
- FinancialModelListRm
- AccountBalancesRm
- JournalCashAccountsRm
- JournalDataRm
- DataSourceListRm
- CounterpartiesRm
- FileStoreRm
- AccountingSystemRm
- JournalsRm (domain-layer, Journal aggregate)
- JournalEntriesRm (domain-layer, JournalEntry aggregate)

### ModelServerWriteContext

All domain changes through services wired here:
- AccountingSystemService
- AccountStatementConfigService (custom statement transaction types)
- ClientWorkspaceService (workspace + business lifecycle, 9 commands)
- FinancialModelService
- DataSourceService
- JournalAggregatesService (Journal + JournalEntry commands)
- ManualTableService
- DataTableDefinitionService
- EntrySetService
- FileStoreService
- ReferenceDataService
- UserDefinedWorksheetsService
- DataSourceModelMapService
- TasklistService (work item tracking, 20 commands)

### SpreadsheetAdapter Services

Bridge-layer services that translate between UI and domain:
- DataMappingService (data source → model column mappings)
- DataTableModelMappingService (data table → model row mappings)
- ListDataTableModelMappingService (list-style data table mappings)
- ManualTableModelMappingService (manual table → model mappings)

### TeamModelMgmt Services

Team collaboration services (Blazor server):
- ManagedFinancialModelSvc (10 commands — model sharing, permissions)
- TeamSettingsSvc (7 commands — team configuration)

### Connectors

- QuickbooksDataSourceService (QBConnector — QuickBooks data sync)

### UIBehavior Read Models

UI-layer read models (not wired through ModelServerReadContext):
- ImportFilesRm (file import workflow state)
- TransactionsRm (transaction workflow state)

## Isolation Guarantees

| Guarantee | Mechanism |
|-----------|-----------|
| UI cannot access aggregates | No ProjectReference from UIBehavior → ModelServer |
| UI cannot bypass ACL | All commands routed through SpreadsheetContextService |
| Workspace isolation | `ConcurrentDictionary<Guid, ClientWorkspaceContext>` keyed by workspaceId |
| Model isolation | `Dictionary<Guid, SpreadsheetContext>` keyed by workbookContextId |
| Metadata propagation | SpreadsheetContextMetadata attached to every message — routing without exposing internals |
| Replay safety | Handled internally by ReactiveDomain 0.14.0 (NullBus removed) |

## Accounting Reports Context (Parallel Read-Side)

The `AccountingReportsContext` is the first read-side interface that bypasses the SpreadsheetAdapter ACL entirely. Report read models subscribe directly to domain event streams:
- BalanceSheetRm, IncomeStatementRm, CashFlowStatementRm
- GeneralLedgerRm, TrialBalanceRm, IncomeExpenseSummaryRm, JournalReportRm
- Wired in UIBehavior, not through ModelServerReadContext
- Establishes the migration path away from SpreadsheetAdapter for PMA features

## Key ACL Files

| File | Lines | Role |
|------|-------|------|
| SpreadsheetContextService.cs | ~1,973 | Main orchestrator, 130+ handlers |
| SpreadsheetContext.cs | ~1,500+ | UI gesture → domain command translation |
| ClientWorkspaceContext.cs | ~1,183 | Workspace isolation boundary |
| ClientWorkspaceContextService.cs | ~300+ | Workspace lifecycle management |
| ModelServerReadContext.cs | — | Read model wiring |
| ModelServerWriteContext.cs | — | Service wiring (incl. JournalAggregatesService) |
| AccountingReportsContext.cs | ~44 | Report RM wiring (parallel to ACL) |
| SpreadsheetContextBus.cs | ~100 | Metadata decorator for buses |
| SpreadsheetContextMetadata.cs | ~10 | Record(ClientId, WorkspaceId?, WorkbookContextId?) |
