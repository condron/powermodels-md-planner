---
type: facet
id: isolation-acl-pattern
label: Isolation & Anti-Corruption Layer
synonyms:
  - context hierarchy
  - read write separation
  - workspace isolation
  - model isolation
  - SpreadsheetContextService
  - metadata routing
  - replay safety
  - hexagonal adapter
links_to_facets:
  - event-sourced-architecture
  - project-dependency-architecture
  - message-bus-architecture
  - domain-model-structure
claims:
  - SpreadsheetContextService is the central orchestrator with 130+ handler interfaces
  - Context hierarchy provides nested isolation from workspace to model to spreadsheet level
  - ModelServerReadContext and ModelServerWriteContext enforce strict read/write separation
  - Workspace isolation uses ConcurrentDictionary keyed by workspaceId
  - SpreadsheetContextMetadata propagates routing info without exposing domain internals
  - NullBus prevents event re-publishing during aggregate replay in reconciliation
  - No UI code can bypass the ACL — enforced by project references and bus routing
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Isolation & Anti-Corruption Layer

## Description

The PowerModels ACL enforces strict boundaries between UI and domain through a context hierarchy (SpreadsheetContextService → ClientWorkspaceContext → ModelServerContext → SpreadsheetContext) and read/write separation. Every command and event flows through metadata-routed handlers — no direct aggregate access from UI.

## Key Points

- **SpreadsheetContextService**: Singleton orchestrator, 130+ handler interfaces, routes by metadata
- **ClientWorkspaceContext**: Per-workspace isolation, manages business and sandbox lifecycle
- **ModelServerReadContext**: Wires read models (FinancialModelListRm, AccountBalancesRm, CounterpartiesRm, etc.)
- **ModelServerWriteContext**: Wires services (AccountingSystemService, FinancialModelService, DataSourceService, etc.)
- **SpreadsheetContext**: Per-workbook, translates UI gestures into domain commands

## Isolation Guarantees

| Guarantee | Mechanism |
|-----------|-----------|
| UI cannot access aggregates | No ProjectReference from UIBehavior → ModelServer |
| UI cannot bypass ACL | All commands routed through SpreadsheetContextService |
| Workspace isolation | ConcurrentDictionary<Guid, ClientWorkspaceContext> |
| Model isolation | Dictionary<Guid, SpreadsheetContext> |
| Metadata propagation | SpreadsheetContextMetadata on every message |
| Replay safety | NullBus during ReconciliationBootstrap |

## Design Rationale

The ACL exists because the domain model uses ReactiveDomain primitives (AggregateRoot, Command, Event) that should not leak into UI code. The adapter translates between UI-level gestures (sheet changes, form submissions) and domain-level commands (CreateChartOfAccounts, UpdateAccount). This enables the domain to evolve independently of the UI.

## Implementation Reference

Full context hierarchy and wiring details in `implementation/isolation-acl.md`.
