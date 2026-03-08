---
type: facet
id: application-topology-pattern
label: Application Topology
synonyms:
  - multi-host architecture
  - per-business data isolation
  - event store per business
  - WPF Excel Blazor hosts
  - DataStore architecture
  - runtime composition
  - local data persistence
links_to_facets:
  - event-sourced-architecture
  - wpf-excel-interface-strategy
  - cloud-migration-strategy
  - isolation-acl-pattern
claims:
  - Four application hosts serve different user contexts with shared domain infrastructure
  - Each business gets its own DataStore instance with complete event stream isolation
  - No shared relational database for domain data — domain state is fully event-sourced per business
  - WPF app uses HostConfiguration with two-bus architecture for internal and external communication
  - Excel add-in uses static buses with per-workbook SpreadsheetContext
  - Blazor host uses SQL Server for auth only and scans local directory for business DataStores
  - File system layout enforces workspace and business isolation at the OS level
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Application Topology

## Description

PowerModels runs across four application hosts (WPF desktop, Excel add-in, Blazor server, QuickBooks connector) that share the same domain infrastructure but serve different user contexts. Domain data is fully event-sourced with one DataStore per business — no shared relational database.

## Key Points

- **WPF App**: Primary standalone accounting application (App.xaml.cs → HostConfiguration → ContextService → buses → read models)
- **Excel Add-in**: ExcelDNA XLL with static buses and per-workbook context management
- **Blazor Server**: ASP.NET team collaboration with SQL Server for auth only, local DataStore for domain
- **QBConnector**: QuickBooks data sync adapter

## Per-Business Isolation

- Each business creates its own `DataStore` — an in-memory event store with `PersistentAllStream` disk persistence
- Stream naming: `{AggregateType}-{Guid}` (individual), `$ce-{AggregateType}` (category), `$et-{EventType}` (by type), `$All` (global)
- File system layout: `%LocalAppData%\PowerModels\Data\{Workspace}_dbs\{Business}`
- No cross-business event access — isolation is physical

## Runtime Composition

- ContextService creates InternalBus + ExternalBus (both wrapped in SpreadsheetContextBus)
- Opening a workspace creates LocalConnection → DataStore.OnDisk → ClientWorkspaceContext
- Each business in a workspace gets ModelServerReadContext (read models) + ModelServerWriteContext (services)

## Implementation Reference

Detailed topology in `implementation/application-topology.md` and `implementation/scan-data.json`.
