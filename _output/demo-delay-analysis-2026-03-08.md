# From Initial Demo to MVP — Why It Took So Long and What Comes Next

Generated: 2026-03-08
Sources: vault-project-analysis-2026-03-08-v3.md, mitigation-plan-2026-03-08.md, accelerated-mitigation-plan-2026-03-08.md

---

## The Compound Cycle

The v3 analysis identifies a structural cycle that directly explains why delivering the initial demo took so long and why the path to MVP requires a different approach:

```
New accounting features (Journal, Reconciliation, PeriodClose)
  → New aggregates → new category streams → new events
    → New ReadModelBase RMs subscribing to those streams
      → Longer startup replay (no checkpoints)
        → New cross-aggregate sagas (implicit, no compensation)
          → More SpreadsheetAdapter RMs (defect funnel)
            → More defects at the ACL boundary
              → God Aggregate still coupled via table mapping
                → (cycle repeats)
```

**Every feature added to make the initial demo work made the underlying problems worse.** The initial demo has shipped, but the compound cycle explains why it was late and why moving to MVP requires breaking the cycle first.

---

## The Six Root Causes

### 1. Reconciliation Pipeline — 29% Defect Rate

The reconciliation pipeline (core to the initial demo and MVP) has the highest defect rate in the codebase. It is coordinated by implicit sagas — handler chains with no compensation logic, no timeout handling, and no workflow visibility. Bugs appear unpredictably and are difficult to reproduce.

- 5 bounded contexts involved
- 5 aggregates coupled
- 19 defects traced to this pipeline
- 0 explicit process managers

### 2. God Aggregate (ServerFinancialModel) — 3,557 Lines

67% of all commands (166 of 249) route through one handler service into one aggregate with 87 Apply methods and 31 Register calls. Any feature touching financial models risks side effects across the entire surface.

- 9 read models subscribe to `$ce-ServerFinancialModel` (hottest stream)
- 7 defects traced directly to SFM-subscribing read models
- 9 PRs in the last 9 months — all large (26–94 files each)
- Missing source constructor and RestoreFromEvents test

### 3. No Replay Safety Tests — 0/27

Zero aggregates have RestoreFromEvents tests. There is no automated way to verify that a bug fix or feature change doesn't break event replay. Issues surface only during demo or production use.

### 4. Startup Replay From Zero

46 ReadModelBase RMs replay their entire category stream from position 0 every time a business is opened. No checkpoints, no persisted state — all RM state is in-memory only and lost on restart.

With any meaningful data volume, this makes the demo slow and fragile. Adding new RMs for demo features makes it worse.

### 5. PDF Extraction — Text Parsing vs Image Parsing Switch

The initial demo pipeline assumed PDF text extraction for bank statements and source documents. In practice, PDFs from small financial institutions are malformed — embedded text is unreliable, garbled, or absent. This forced a mid-development switch from PDF text extraction to PDF image parsing (OCR-based), which changes the entire ingestion path:

- Different extraction library/service (OCR vs text layer parsing)
- Different error characteristics (OCR confidence thresholds vs text encoding issues)
- Different latency profile (image parsing is slower, requires GPU or API calls)
- Downstream classification and mapping steps must handle lower-fidelity input

This is not a bug — it's a **scope discovery** that only surfaces with real-world documents from real institutions. It compounded the delay of the initial demo because the fix touches the front of the pipeline, and every downstream step (mapping, classification, reconciliation) inherits the change.

### 6. SpreadsheetAdapter — The Defect Funnel

40 of 80 read models live in SpreadsheetAdapter (the ACL bridge between domain events and the UI). 13 of 16 defect-hit RMs are in this context. This is exactly where demo-visible bugs surface — the translation layer between what the domain produces and what the user sees.

---

## Why Fixing Bugs in the Initial Demo Won't Get Us to MVP

The standard response to a bug is to fix the specific issue. But each fix:

- Touches the god aggregate (side effects across 87 Apply methods)
- Has no replay safety net (no RestoreFromEvents test to catch regressions)
- May break implicit saga coordination (no process manager to enforce step ordering)
- Surfaces new bugs in SpreadsheetAdapter RMs (defect funnel)
- Doesn't reduce startup time (still replaying from position 0)

**You are fighting the compound cycle, not individual bugs.**

---

## The Path from Initial Demo to MVP

The initial demo is shipped. The mitigation plan doesn't add features to it — it **puts the floor under the features that already exist** to produce an MVP stable enough for design partner trials. The key insight from the v3 analysis:

> *"Breaking the cycle requires integrating the checkpoint and process manager infrastructure BEFORE building the accountant workflow features."*

### Week-by-Week Path to MVP

| Week | What Gets Fixed | MVP Impact |
|------|----------------|-------------|
| **1** | RestoreFromEvents tests (SFM + 5 growth aggregates), startup time baseline measured | Know what's actually broken; safety net for all subsequent changes |
| **2** | 9 SFM RM checkpoints, CachingRepository for SFM, two-tier RM persistence, ProcessManagerBase wired in | Startup goes from full-replay to checkpoint-resume; SFM GetById becomes fast |
| **3** | Journal aggregate, AccountingSystem extension, reconciliation ProcessManager designed, pipeline migration plan | Accounting domain foundation built correctly with explicit saga coordination |
| **4** | Reconciliation ProcessManager operational (feature flag on, old pipeline as fallback), SFM extraction begins | **Reconciliation defect rate drops from 29% → <10%**; pipeline bugs become visible and traceable |
| **5** | PeriodClose PM, GL reports from Journal events, SFM extraction complete (87 → ~67 Apply), 46/46 RMs checkpointed | Full accountant workflow MVP-ready; startup <2s at 15K events |
| **6** | Consolidation, architectural review, performance baselines, verification | **MVP** — stable, measurable, ready for design partner trials |

### What Changes: Initial Demo → MVP

| Metric | Initial Demo (Now) | MVP (Week 6) |
|--------|-------------|----------------|
| RestoreFromEvents coverage | 0/27 | 30/30 |
| Reconciliation defect rate | 29% | <10% |
| SFM Apply methods | 87 | ~67 |
| SFM registered events | 31 | ~11 |
| SFM lines | 3,557 | ~1,500–2,000 |
| RM checkpoint coverage | 0/46 | 46/46 |
| Startup time (15K events) | Full replay | <2s (checkpoint resume) |
| Explicit process managers | 0 | 2 (Reconciliation, PeriodClose) |
| Implicit sagas | 7 | ≤4 |
| SpreadsheetAdapter defect rate | 15% | <10% |

---

## The Real Answer

The initial demo was delayed because **the team was building features on top of infrastructure that doesn't exist yet** — checkpoints, process managers, replay safety, and aggregate decomposition. The initial demo shipped, but continuing to patch it will not produce an MVP. The compound cycle ensures that each bug fix risks creating new ones.

The mitigation plan is not "more time to build features." It is a specific, sequenced, 5–6 week plan to turn the initial demo into an MVP by stabilizing the infrastructure underneath it. The infrastructure to do this (ProcessManagerBase, ICheckpointStore, CachingRepository, EventVersioning) is already designed and ready for integration.

The difference:
- **Without mitigation:** Patching the initial demo fights the same compound cycle. Bug fixes create new bugs. MVP timeline is unpredictable.
- **With mitigation:** The cycle is broken at its root. The MVP is stable because the infrastructure underneath it is stable. Timeline is 5–6 weeks with the current team + AI agents.

### Initial Demo vs MVP

| | Initial Demo (Shipped) | MVP (Week 6) |
|---|---|---|
| **Status** | Delivered | 5–6 weeks out |
| **Stability** | Fragile — compound cycle active | Stable — cycle broken at root |
| **Replay safety** | 0/27 tests | 30/30 tests |
| **Reconciliation** | 29% defect rate, implicit sagas | <10% defect rate, explicit ProcessManagers |
| **Startup** | Full replay from position 0 | <2s checkpoint resume |
| **PDF ingestion** | Text → image parsing switch in progress | Image parsing integrated, pipeline stable |
| **Ready for** | Showcase / conversation starter | Design partner trials / investor proof point |
