# Message Bus Topology

Last reviewed: 2026-03-27

Generated: 2026-03-27 (from `joshkempner/journal-aggregate` branch)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Bus Types

| Bus | Type | Purpose |
|-----|------|---------|
| LocalBus | `Dispatcher` | App-internal communication (created in App.xaml.cs) |
| ExternalBus | `Dispatcher` | Server communication (SpreadsheetContextService.Bus) |
| SpreadsheetContextBus | Decorator | Wraps either bus, auto-attaches SpreadsheetContextMetadata (clientId, workspaceId, workbookContextId) |
| ~~NullBus~~ | Removed in R-D 0.14.0 | Previously used for replay safety; now handled internally by ReactiveDomain |
| InMemoryBus | Test-only | Used in test fixtures |

## Two-Bus Architecture

Created in `ContextService` (App/Context/ContextService.cs):

```
InternalBus = SpreadsheetContextBus(localBus, ...)      ← app-to-app
ExternalBus = SpreadsheetContextBus(serverBus, ...)     ← app-to-server
```

The `SpreadsheetContextBus` decorator auto-attaches `SpreadsheetContextMetadata` to every message flowing through it. This metadata carries `ClientId`, `WorkspaceId?`, and `WorkbookContextId?` — enabling routing without exposing domain internals to the UI layer.

## Message Flow: UI → Domain → UI

```
1. UI ACTION
   ViewModel sends SpreadSheetMsgs.* command via ExternalBus
       ↓
2. ACL ROUTING
   SpreadsheetContextService receives message
   Routes by SpreadsheetContextMetadata to correct SpreadsheetContext
       ↓
3. ACL TRANSLATION
   SpreadsheetContext translates UI gesture into domain command
   e.g., SheetChange → FinancialModelMsgs.UpdateCompositeRowCustomFormula
   Sends domain command via ModelServerBus
       ↓
4. DOMAIN HANDLING
   QueuedSubscriber service (e.g., FinancialModelService) handles command
   Loads aggregate via ICorrelatedRepository.GetById(id, command)
   Calls aggregate method → aggregate raises domain event via Apply()
   Repository.Save() persists events and publishes to bus
       ↓
5. READ MODEL PROJECTION (two paths)
   a. SpreadsheetAdapter read models (e.g., FinancialModelRm) catch domain events
      Project into denormalized views
      Publish client-facing messages (AccountingSystemClientMsgs.*, SpreadSheetMsgs.*)
   b. AccountingReportsContext read models (e.g., BalanceSheetRm) catch domain events directly
      Project into report views (first parallel read-side interface, bypasses ACL)
       ↓
6. UI UPDATE
   UIBehavior read models (e.g., CounterpartiesRm) catch client messages
   Update SourceCache/observable state
   ViewModels bound via ReactiveUI update automatically
```

## Message Namespaces by Layer

| Layer | Namespace | Examples | Direction |
|-------|-----------|----------|-----------|
| **Domain** | `ModelServer.Messages` | ChartOfAccountsMsgs, FinancialModelMsgs, DataSourceCommands/Events, JournalMsgs, JournalEntryMsgs | Internal to domain |
| **ACL** | `SpreadsheetAdapter.Messages` | SpreadSheetMsgs, ConnectionMsgs, ClientWorkspaceContextMsgs, DataMapMsgs, AccountingSystemClientMsgs, PredictiveForecastingMsgs | Bridge layer |
| **UI** | `PowerModels.UIBehavior.Messages` | UiMsgs | UI-to-ACL only |

## Envelope Commands

SpreadsheetAdapter uses envelope commands to abstract workspace/model routing:
- `SpreadSheetMsgs.WorkspaceCommandEnvelope(Command WorkspaceCommand)` — wraps workspace-scoped commands
- `SpreadSheetMsgs.ModelCommandEnvelope(string ModelServerName, Command ModelCommand)` — wraps model-scoped commands

These envelopes allow the UI to send commands without knowing which workspace or model context is active — the ACL unwraps and routes to the correct handler.
