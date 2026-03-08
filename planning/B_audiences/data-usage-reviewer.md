---
type: audience
id: data-usage-reviewer
label: Data Usage Reviewer
concerns:
  - stream growth projections and storage scaling
  - read amplification and subscriber fan-out impact
  - startup replay cost as businesses mature
  - snapshot and compaction strategy gaps
  - per-business isolation guarantees
  - God Aggregate stream size and replay frequency
  - disk persistence format efficiency
  - three-database boundary clarity
vocab_map:
  data-usage-and-storage:
    - read amplification
    - stream growth
    - startup replay cost
    - no snapshots
    - append-only persistence
    - storage scaling
    - Protobuf3 persistence
    - PersistentAllStream
    - subscriber fan-out
    - $All replay
  event-sourced-architecture:
    - immutable event history
    - state replay
    - single source of truth
    - deterministic replay
  application-topology-pattern:
    - per-business data isolation
    - DataStore architecture
    - local data persistence
    - multi-host architecture
  isolation-acl-pattern:
    - context hierarchy
    - read write separation
    - ModelServerReadContext wiring
    - ModelServerWriteContext wiring
proof_preferences:
  - stream count and event count metrics per business maturity tier
  - startup time measurements at different event counts
  - disk size growth curves over time
  - read amplification validation against subscriber count
risk_tolerance: low
status: active
---

# Data Usage Reviewer

## Description

Reviews the data usage, storage, and performance characteristics of the PowerModels event-sourced architecture. Focused on understanding how streams grow, how reads amplify, and where the current no-snapshot, no-compaction design creates scaling risks or operational constraints.

## Primary Concerns

- **Stream growth trajectory**: How fast do streams grow per business? What's the upper bound?
- **Read amplification**: Is per-category-stream fan-out (max 9× on `$ce-ServerFinancialModel`) sustainable as subscriber count grows?
- **Startup cost**: How does $All replay time scale with business maturity?
- **Snapshot gap**: What's the cost of full-stream replay on every GetById?
- **God Aggregate impact**: ServerFinancialModel as the largest and most-replayed stream
- **Storage efficiency**: Protobuf3 single-file persistence — is it adequate at scale?
- **Database boundary clarity**: Is the DataStore / SQL / file system split well-understood?

## Communication Preferences

- **Language**: Quantitative — event counts, stream counts, replay times, disk sizes, amplification ratios
- **Proof points**: Measured metrics at different business maturity tiers, growth projections
- **Format**: Tables, diagrams, and trend data preferred over narrative
- **Cadence**: Per-release review, or when new aggregate types or read models are added

## Decision Factors

- **Scaling headroom**: Does the current architecture support the next order of magnitude?
- **Operational risk**: What breaks first as businesses grow?
- **Mitigation options**: Snapshots, compaction, incremental loading — what's the cost/benefit?
- **Cross-concern impact**: How do data usage characteristics affect cloud migration, multi-tenancy, startup time?
