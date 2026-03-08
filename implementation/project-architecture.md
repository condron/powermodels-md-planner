# Project Architecture

Last reviewed: 2026-03-08

Generated: 2026-03-07 (seeded from pre-scanned context)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Layer Diagram

```
┌─────────────────────────────────────────────────────────────┐
│                    UI LAYER                                  │
│  PowerModels.App → PowerModels.WPF → PowerModels.UIBehavior │
│  PowerModels.Excel.Core                                     │
│  QBConnector                                                │
│       ↕ UiMsgs, SpreadSheetMsgs, AccountingSystemClientMsgs│
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│         ANTI-CORRUPTION LAYER                               │
│  SpreadsheetAdapter + Excel.Toolkit                         │
│    SpreadsheetContextService (orchestrator, 2600 lines)     │
│    SpreadsheetContext (translates UI gestures → domain cmds)│
│    ClientWorkspaceContext (workspace isolation)             │
│    ModelServerReadContext / ModelServerWriteContext          │
│       ↕ Domain Commands/Events                              │
└─────────────────────────────────────────────────────────────┘
                              ↕
┌─────────────────────────────────────────────────────────────┐
│                  DOMAIN LAYER                               │
│  ModelServer (aggregates, services, domain messages)        │
│  LocalDataStore (event store infrastructure)                │
│  TeamModelMgmt (team collaboration aggregates)              │
└─────────────────────────────────────────────────────────────┘
```

## Project Reference Graph

```
ModelServer           ← no dependencies (core domain)
LocalDataStore        ← no dependencies (infrastructure)

SpreadsheetAdapter    ← ModelServer, LocalDataStore
Excel.Toolkit         ← SpreadsheetAdapter

UIBehavior            ← SpreadsheetAdapter, Excel.Toolkit
WPF                   ← UIBehavior
App                   ← WPF, Excel.Toolkit

Excel.Core            ← SpreadsheetAdapter, Excel.Toolkit, UIBehavior, WPF

QBConnector.Core      ← SpreadsheetAdapter
QBConnector           ← QBConnector.Core, Excel.Toolkit, Excel.Core

TeamModelMgmt         ← SpreadsheetAdapter
```

## Dependency Rules

| Rule | Enforcement |
|------|-------------|
| UI never references ModelServer directly | Project references — UIBehavior depends on SpreadsheetAdapter, not ModelServer |
| Domain has no upstream dependencies | ModelServer.csproj has zero ProjectReferences |
| Only ModelServer + LocalDataStore reference ReactiveDomain | NuGet dependency scope — ReactiveDomain 0.13.0 only in these two |
| SpreadsheetAdapter is the sole bridge | All other projects reach domain through SpreadsheetAdapter |
| WPF contains no domain logic | Only references UIBehavior (XAML views only) |

## Package Distribution

| Project | ReactiveDomain | ReactiveUI | SemanticKernel | DynamicData |
|---------|---------------|------------|----------------|-------------|
| ModelServer | 0.13.0 | — | — | — |
| LocalDataStore | 0.13.0 | — | — | — |
| SpreadsheetAdapter | — (transitive) | — | 1.72.0 | 9.4.1 |
| UIBehavior | — | 22.3.1 | 1.72.0 | — |
| WPF | — | 22.3.1 (WPF) | 1.72.0 | — |
| App | — | 22.3.1 (WPF) | 1.72.0 | — |
| Excel.Core | — | 22.3.1 (WPF) | 1.72.0 | — |
| TeamModelMgmt | — | 22.3.1 (Blazor) | — | — |
