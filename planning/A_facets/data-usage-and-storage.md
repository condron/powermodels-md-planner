---
type: facet
id: data-usage-and-storage
label: Data Usage and Storage
synonyms:
  - stream creation patterns
  - read amplification
  - event store growth
  - per-business data isolation
  - Protobuf persistence
  - append-only storage
  - no compaction no snapshots
  - startup replay cost
  - write volume estimates
links_to_facets:
  - event-sourced-architecture
  - application-topology-pattern
  - isolation-acl-pattern
claims:
  - Every AppendToStream auto-creates $ce and $et projection streams synchronously
  - Repository replays full aggregate stream from position 0 on every GetById with no snapshots
  - Read amplification is per-category-stream, not global — max 9x on `$ce-ServerFinancialModel` (hottest stream). 46 ReadModelBase RMs subscribe to category streams; 34 TransientSubscriber RMs are bus-only with zero startup cost
  - Stream growth is unbounded with no compaction pruning or archival mechanism
  - Mature business accumulates 15K-30K events in 3-6 MB with 200-400 streams
  - Opening a business replays entire $All stream across all subscribers
  - PowerModelsContext contains no domain data only ASP.NET Identity tables
  - DataStore uses Protobuf3 with PersistentAllStream writing all events to a single file per business
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Data Usage and Storage

## Description

PowerModels uses a custom in-memory event store (DataStore) with Protobuf3 disk persistence, one per business. Every event written triggers synchronous projection stream creation and fan-out to 60+ read model subscribers. There are no snapshots, no compaction, and no incremental loading — the full event history is replayed on every business open and every aggregate load. This architecture trades storage efficiency and startup speed for simplicity and guaranteed consistency.

## Key Points

- **Stream auto-creation**: Each `AppendToStream()` creates the instance stream plus `$ce-{category}`, `$et-{eventType}`, and `$All` projection entries synchronously
- **Write volumes**: Business setup generates 25-45 events; a 1,000-row CSV import generates ~1,001 events; a 12-month bank statement ~722 events
- **Stream growth**: A mature business (2-3 years) accumulates 15K-30K events in 3-6 MB across 200-400 streams, with no upper bound
- **Three databases**: DataStore (per-business event store), PowerModelsContext (SQL — auth only, no domain data), file system (model workbooks, imported files)
- **No compaction**: Streams grow monotonically — no pruning, archival, or snapshot mechanism exists
- **Per-business isolation**: Each business has its own DataStore file; no cross-business event access

## Performance Characteristics

- **Read amplification (per-category-stream)**: Each event is appended to PersistentAllStream, category and event-type projection streams, replayed by the aggregate repository on GetById, and distributed to ReadModelBase RMs subscribed to the matching `$ce-*` stream (max 9 RMs on `$ce-ServerFinancialModel`). TransientSubscribers receive events on the bus only when active.
- **Startup cost**: Opening a business reads the full PersistentAllStream into memory (fixed cost), then 46 ReadModelBase RMs each replay their category streams from position 0. 34 TransientSubscriber RMs have zero startup cost (bus-only).
- **Command latency**: Every `GetById()` replays the full aggregate stream from position 0 — no caching between handler invocations
- **Memory**: Entire DataStore held in memory; scales linearly with total event count
- **God aggregate risk**: ServerFinancialModel (3,556 lines, 100+ commands) is the largest stream and most frequent replay target

## Implementation Reference

Detailed data topology in `implementation/data-usage-and-storage.md` and `implementation/scan-data.json`.
