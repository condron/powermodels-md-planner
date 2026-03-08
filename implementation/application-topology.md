# Application Topology

Last reviewed: 2026-03-08

Generated: 2026-03-07 (seeded from pre-scanned context)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Application Hosts

| Host | Type | Entry Point | Storage | Purpose |
|------|------|-------------|---------|---------|
| **PowerModels.App** | WPF Desktop | App.xaml.cs → HostConfiguration | `%LocalAppData%\PowerModels` | Primary standalone accounting app |
| **PowerModels.Excel** | ExcelDNA XLL Add-in | ThisAddIn.cs | `%LocalAppData%\PowerModels` | Excel-integrated financial modeling |
| **TeamModelMgmt** | ASP.NET Blazor Server | Program.cs | SQL Server (PowerModelsContext) + LocalDataStore | Team collaboration, model management |
| **QBConnector** | Windows desktop | QBConnector add-in | `%LocalAppData%\PowerModels` | QuickBooks data sync |

## Database Topology

### Per-Business DataStore

```
%LocalAppData%\PowerModels\
├── Data/
│   ├── {WorkspaceName}              ← DataStore file (workspace-level events)
│   ├── {WorkspaceName}_dbs/
│   │   ├── {Business1}             ← DataStore file (business event stream)
│   │   ├── {Business2}             ← DataStore file (separate event stream)
│   │   └── Sandboxes/
│   │       └── {Sandbox1}          ← DataStore file (sandbox event stream)
│   └── ...
├── appsettings.json
├── PowerModels.lic
└── logs/
    ├── PowerModels.log              ← rolling daily
    └── PowerModels-err.log          ← errors only

%MyDocuments%\PowerModels\
└── {BusinessName}/
    ├── Models/                      ← .xlsx financial model files
    └── Imported Data/               ← CSV/PDF uploads
```

**Key:** Each business gets its own `DataStore` instance — an in-memory event store with optional disk persistence (via `PersistentAllStream`). Streams are isolated by business. No cross-business event access.

### DataStore Architecture

- **DataStore** (`LocalDataStore/DataStore.cs`) — In-memory event store implementing `IStreamStoreConnection`
- **LocalConnection** (`SpreadsheetAdapter/LocalConnection.cs`) — Wraps DataStore with file-based persistence, creates `IConfiguredConnection`
- **RemoteConnection** — SQL Server-backed connection (for TeamModelMgmt scenarios)
- **Stream organization per DataStore:**
  - Individual: `{AggregateType}-{Guid}` (e.g., `ChartOfAccounts-{guid}`)
  - By category: `$ce-{AggregateType}` (all events for aggregate type)
  - By event type: `$et-{EventType}` (all events of a given type)
  - Global: `$All` (all events in this business)

### Databases

| Database | Scope | Technology | Contents |
|----------|-------|------------|----------|
| **DataStore (per business)** | One per business | Custom in-memory + disk | All domain event streams for that business |
| **PowerModelsContext** | Firm-wide | SQL Server + EF Core | ASP.NET Identity (users, roles, auth) |
| **No shared "PowerModelsDb"** | — | — | Domain state is fully event-sourced per business, no shared relational DB |

## Runtime Composition

### WPF App Bootstrap

```
PowerModels.App startup:
  1. HostConfiguration.Host.StartAsync()
  2. Creates ContextService with two buses:
     InternalBus = SpreadsheetContextBus(localBus)       ← app-internal
     ExternalBus = SpreadsheetContextBus(serverBus)      ← to model server
  3. Creates SpreadsheetContextService (shared singleton)
  4. Creates read models (WorkspacesRm, ConnectionsRm, etc.)
  5. Publishes ClientWorkspaceContextMsgs.WorkspaceListRequested
```

### Business Context Creation

```
  1. User creates/opens workspace → LocalConnection created
  2. LocalConnection → DataStore.OnDisk(file)
  3. ClientWorkspaceContext wraps connection with:
     ├── ClientWorkspaceService (command handlers)
     ├── ClientWorkspaceContextRm (read model)
     ├── AccountStatementConfigService
     ├── TasklistService
     └── ModelServerContext[] (one per business in workspace)
         ├── ModelServerReadContext (wires read models to this business's events)
         └── ModelServerWriteContext (wires services to this business's connection)
```

### Presentation Hosts

```
WPF App:
  PowerModels.App
    → MainWindow.xaml + MainWindowVm
    → ContextService (workspace/business management)
    → UIBehavior ViewModels (ReactiveUI + SourceGenerators)
    → WPF Views (XAML, no domain logic)

Excel Add-in:
  ThisAddIn (ExcelDNA)
    → Static InternalBus / ExternalBus (SpreadsheetContextBus)
    → ModelContext (per open workbook)
    → BusinessContext (per active business)
    → SpreadsheetContext (per workbook, translates sheet gestures)

Blazor (TeamModelMgmt):
  Program.cs
    → PowerModelsContext (EF Core, SQL Server)
    → IDispatcher (singleton bus)
    → ManagedFinancialModelSvc, TeamSettingsSvc
    → Scans LocalConnection.DefaultDirectory for available businesses
```
