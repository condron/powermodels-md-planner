---
type: facet
id: event-sourced-architecture
label: Event-Sourced Financial Architecture
synonyms:
  - time-ordered financial record
  - immutable event history
  - rewind refresh replay
  - deterministic financial record
  - single source of truth
  - data physics
  - history of intent
  - state replay
  - architectural rigidity moat
links_to_facets:
  - reconciliation-elimination
  - professional-judgment-preservation
claims:
  - PowerModels maintains a single time-ordered double-entry financial record
  - Financial outputs are views over the record not independent artifacts
  - Corrections propagate deterministically across accounting reporting and forecasting
  - System preserves full event sequence rather than overwriting balances
  - Rebuilds are replaced with refresh
evidence_refs:
  - technical-codebase-review
  - accounting-podcast-ai-bookkeeping-sep2024
  - strategy-overview-feb2026
owner: technical-team
status: active
---

# Event-Sourced Financial Architecture

## Description

PowerModels uses event sourcing as its core architectural principle. Instead of storing current account balances that get overwritten with each transaction, the system maintains an immutable, time-ordered sequence of all financial events. Current state is derived by replaying events, and any point in time can be reconstructed by replaying events up to that moment.

## Key Points

- **Immutable event log**: Every transaction, adjustment, and correction is appended to the event stream, never deleted or overwritten
- **Deterministic replay**: Financial outputs (balances, reports, forecasts) are computed by replaying the event stream
- **Point-in-time reconstruction**: Can rewind to any prior date, make corrections, and refresh forward
- **No drift between systems**: Accounting, reporting, and forecasting all derive from the same event stream
- **Audit trail by design**: Complete history of what happened and when, with no phantom states
- **Refresh vs rebuild**: Changes propagate automatically through refresh, not manual Excel rebuilds

## Technical Benefits

- **Reconciliation becomes validation**: Check for completeness, not repair inconsistencies
- **Corrections don't break downstream**: Adjustments flow deterministically through all outputs
- **No version conflicts**: Single source of truth eliminates "which version is right?"
- **Scalable complexity**: Adding entities or reporting dimensions doesn't multiply reconciliation burden

## Differentiation

Most accounting systems store balances in tables that get updated in place. PowerModels stores the *events* that produce those balances. This architectural difference enables:
- Rewind/refresh instead of rebuild
- Guaranteed consistency across accounting, reporting, forecasting
- Complete auditability without special logging infrastructure
- Professional-grade preparation without sacrificing transparency

## Related Evidence

This architecture directly addresses the core problem: fragmented financial systems that drift over time and require constant reconciliation.
