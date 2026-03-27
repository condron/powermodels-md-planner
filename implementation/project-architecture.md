# Project Architecture

Last reviewed: 2026-03-27

Generated: 2026-03-27 (from `joshkempner/journal-aggregate` branch)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Layer Diagram

```
┌──────────────────────────────────────────────────────────────┐
│                    UI LAYER                                    │
│  PMA: PowerModels.App → PowerModels.WPF → PowerModels.UIBeh. │
│  PME: PowerModels.Excel.Core (on hold)                        │
│  QBConnector                                                  │
│       ↕ UiMsgs, SpreadSheetMsgs, AccountingSystemClientMsgs  │
└──────────────────────────────────────────────────────────────┘
                              ↕
┌──────────────────────────────────────────────────────────────┐
│  ANTI-CORRUPTION LAYER (shared PMA/PME — separation planned)  │
│  SpreadsheetAdapter + Excel.Toolkit                           │
│    SpreadsheetContextService (orchestrator, ~1,973 lines)     │
│    SpreadsheetContext (translates UI gestures → domain cmds)  │
│    ClientWorkspaceContext (workspace isolation, ~1,183 lines) │
│    ModelServerReadContext / ModelServerWriteContext            │
│    DataIngestionPipeline (CSV/PDF → domain aggregates)        │
│    BusinessLoader / Seeding (headless business creation)      │
│       ↕ Domain Commands/Events                                │
└──────────────────────────────────────────────────────────────┘
                              ↕
┌──────────────────────────────────────────────────────────────┐
│                  DOMAIN LAYER                                  │
│  ModelServer (aggregates, services, domain messages, RMs)     │
│  LocalDataStore (event store infrastructure)                  │
│  TeamModelMgmt (team collaboration aggregates)                │
└──────────────────────────────────────────────────────────────┘
                              ↕
┌──────────────────────────────────────────────────────────────┐
│                  UTILITIES                                     │
│  SeedTestBusiness (Business Loader CLI)                       │
│  DataValidator (CLI data validation)                          │
│  UITestRunner (FlaUI-based UI test runner)                    │
│  DataStoreRepl, PLicense, PModel, QModel                     │
└──────────────────────────────────────────────────────────────┘
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

Excel.Core            ← SpreadsheetAdapter, Excel.Toolkit, UIBehavior, WPF  (PME — on hold)

QBConnector.Core      ← SpreadsheetAdapter
QBConnector           ← QBConnector.Core, Excel.Toolkit, Excel.Core

TeamModelMgmt         ← SpreadsheetAdapter

SeedTestBusiness      ← SpreadsheetAdapter  (Business Loader CLI)
```

## Dependency Rules

| Rule | Enforcement |
|------|-------------|
| UI never references ModelServer directly | Project references — UIBehavior depends on SpreadsheetAdapter, not ModelServer |
| Domain has no upstream dependencies | ModelServer.csproj has zero ProjectReferences |
| Only ModelServer + LocalDataStore reference ReactiveDomain | NuGet dependency scope — ReactiveDomain 0.14.0 only in these two |
| SpreadsheetAdapter is the sole bridge | All other projects reach domain through SpreadsheetAdapter |
| WPF contains no domain logic | Only references UIBehavior (XAML views only) |

## Package Distribution

| Project | ReactiveDomain | ReactiveUI | SemanticKernel | DynamicData |
|---------|---------------|------------|----------------|-------------|
| ModelServer | 0.14.0 | — | — | — |
| LocalDataStore | 0.14.0 | — | — | — |
| SpreadsheetAdapter | — (transitive) | — | 1.73.0 | 9.4.1 |
| UIBehavior | — | 22.3.1 | 1.73.0 | — |
| WPF | — | 22.3.1 (WPF) | 1.73.0 | — |
| App | — | 22.3.1 (WPF) | 1.73.0 | — |
| Excel.Core | — | 22.3.1 (WPF) | 1.73.0 | — |
| TeamModelMgmt | — | 22.3.1 (Blazor) | — | — |

**Breaking change in R-D 0.14.0:** `NullBus` and `CapturingBus` have been removed. Replay safety is handled differently — aggregates no longer need NullBus wrapping during rehydration.
