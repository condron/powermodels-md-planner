# PowerModels Mitigation Plan

Generated: 2026-03-07 (updated with process manager infrastructure)
Assessment: vault-project-analysis-2026-03-07-v2.md
Feature reference: small-accountant-flow-proposal.md
Process manager infrastructure: process-manager/Infrastructure/ (ProcessAggregateBase, ProcessManagerBase)
Approach: Integrate structural mitigation with the small-accountant-flow feature buildout — fix the foundation while building the replacement architecture. Use formal ProcessManager pattern for all new cross-aggregate coordination.

---

## Problem Statement

Three compounding risks identified in the v2 assessment feed each other in a cycle:

```
God Aggregate expansion → more events per stream → longer replay
  → more RM subscriptions → higher SpreadsheetAdapter complexity
    → more defects in ACL bridge → more implicit saga coordination
      → more God Aggregate coupling → (cycle repeats)
```

The small-accountant-flow proposal (Journal, ReconciliationState, PeriodClose aggregates) is the **constructive replacement** for the architecture that currently concentrates risk. This plan sequences the corrective and constructive work together so each phase strengthens the next.

Critically, `process-manager/Infrastructure/` provides formal process manager base classes ready for ReactiveDomain integration:
- **ProcessAggregateBase\<TState\>** — Event-sourced state machine with `InState().On<TEvent>()` DSL, command accumulation (`IssueCommand`/`TakeCommands`), step timeouts with staleness detection, terminal state definition
- **ProcessManagerBase\<TProcessAgg, TState\>** — Read model coordinator with NullBus replay safety, `LoadOrCreate`/`SaveAndTakeCommands`/`DispatchCommands` pattern, `StepTimedOut` routing, `GoLive()` recovery
- **CategoryStreamWarmup** — ESDB `$ce-*` stream warmup for process manager category subscriptions
- **EventVersioning + VersionedEventSerializer** — Event schema evolution with `[EventVersion]` attribute and upcaster chain

This eliminates the "all sagas are implicit" problem structurally — new workflows use ProcessManager rather than ad-hoc handler coordination.

---

## Phase 0: Foundation Fixes (Before Feature Work)

**Goal:** Establish a safe baseline before extending or creating aggregates.
**Duration:** 1–2 weeks
**Prerequisite for:** All subsequent phases

### 0.1 Fix 4 Missing Source Constructors

| Aggregate | Lines | Context | Status (9mo) |
|-----------|-------|---------|--------------|
| ServerFinancialModel | 3,557 | ModelServer | Active (9 PRs) |
| ReferenceDataSeries | 172 | ModelServer | Dormant (0 PRs) |
| DataSourceMapping | 61 | ModelServer | Dormant (0 PRs) |
| AccountingSystem | 29 | ModelServer/AS | Dormant (0 PRs) — **Extended in Phase 1** |

**Action:** Add `:base(source)` constructor to all 4. AccountingSystem is critical — Epic 1 extends it with 3 new commands.
**Evidence:** aggregates.md (missing pattern flag), scan-data.json (hasSourceCtor: false)
**Risk if skipped:** Correlation tracking fails silently. AccountingSystem extension in Phase 1 builds on a broken foundation.
**Effort:** 1 day

### 0.2 RestoreFromEvents Test for ServerFinancialModel

**Action:** Write RestoreFromEvents test for the God Aggregate — create instance, raise all 31 registered event types, restore from stream, assert state equality.
**Evidence:** test-coverage.md (0/27 aggregates tested), god-aggregate.md (87 Apply methods, 31 Register calls)
**Risk if skipped:** Event replay corruption is undetectable. Any Phase 1 change that affects financial model events could silently break replay.
**Effort:** 2–3 days (87 Apply methods to verify)

### 0.3 RestoreFromEvents Tests for Active Aggregates

**Action:** Write RestoreFromEvents tests for the 18 aggregates touched in the last 9 months (feature-provenance.md: non-stable aggregates).
**Evidence:** test-coverage.md (0/27), feature-provenance.md (18 aggregates with PRs)
**Priority order** (by PR count + defect exposure):
1. ChartOfAccounts (14 PRs, 3 RM defects)
2. TasklistItem (6 PRs)
3. DataTableMap, ListDataTableMap, ManualTableMap (4 PRs each, mapping RM defects)
4. CustomStatementTxType (4 PRs)
5. DataSource (3 PRs, 4 RM defects)
6. Remaining 11 active aggregates

**Risk if skipped:** New aggregates in Phase 1 establish the RestoreFromEvents pattern, but existing aggregates remain untested. Any event schema migration or replay change breaks silently.
**Effort:** 1–2 weeks

### 0.4 SpreadsheetAdapter Read Model Audit

**Action:** Catalog the 40 SpreadsheetAdapter RMs. Identify consolidation candidates (RMs subscribing to overlapping event sets), dead RMs (no UI consumer), and high-subscription RMs (>20 events).
**Evidence:** read-models.md (80 RMs total, 40 in SpreadsheetAdapter), defect-analysis.md (15% defect rate, 13/16 defect-hit RMs in SpreadsheetAdapter), bounded-contexts.md
**Purpose:** Establish the baseline before Phases 1-3 add 3-4 new RMs. Know which existing RMs can be consolidated or removed to offset the new ones.
**Effort:** 2–3 days (audit only, no changes)

### Phase 0 Exit Criteria

- [ ] All 4 source constructors fixed and tests green
- [ ] ServerFinancialModel RestoreFromEvents test passing
- [ ] ≥8 active aggregate RestoreFromEvents tests passing (top priority list)
- [ ] SpreadsheetAdapter RM audit document produced with consolidation recommendations

---

## Phase 1: Domain Foundation + God Aggregate Relief (Parallel Tracks)

**Goal:** Build the new aggregate foundation while reducing God Aggregate responsibility.
**Duration:** 3–4 weeks
**Depends on:** Phase 0

### Track A: AccountingSystem Extension (Epic 1.1–1.3)

| Item | What | Aggregate | New Commands | New Events |
|------|------|-----------|-------------|------------|
| 1.1 | Entity type | AccountingSystem | SetEntityType | EntityTypeSet |
| 1.2 | Industry context | AccountingSystem | SetIndustryContext | IndustryContextSet |
| 1.3 | Fiscal year + close policy | AccountingSystem | SetFiscalYear, SetClosePolicy | FiscalYearSet, ClosePolicySet |

**Mitigation integration:**
- Source ctor already fixed in Phase 0.3
- RestoreFromEvents test written for AccountingSystem as part of this work (covers new events)
- AccountingSystem stays as **configuration-only decision state** — entity type, industry, and fiscal year constrain downstream aggregates but don't own operational data
- Close policy may belong on PeriodClose (Phase 3) rather than AccountingSystem — review during implementation

**New streams:** 0 (extends existing AccountingSystem stream)
**New RMs:** 0 (existing SpreadsheetAdapter RMs already subscribe to AccountingSystem events — add new event handlers to existing RMs)

### Track B: Journal Aggregate (Epic 3.1)

| Item | What | Aggregate | New Commands | New Events |
|------|------|-----------|-------------|------------|
| 3.1 | Journal aggregate | Journal (NEW) | PostEntry, CorrectEntry, RemoveEntry | EntryPosted, EntryCorrected, EntryRemoved |

**Mitigation integration:**
- **God Aggregate relief:** Journal absorbs posting responsibility that currently leaks into ServerFinancialModel's event stream via the reconciliation pipeline. New accounting features (entries, corrections, adjustments) route through Journal, not ServerFinancialModel.
- **Saga foundation:** Journal is the coordination anchor for Phases 2-3. ReconciliationState posts adjustments through Journal. PeriodClose checks Journal for open entries. Classification posts through Journal.
- **Pattern establishment:** Built from day one with `:base(source)`, RestoreFromEvents test, private Apply methods, Register calls.

**Design constraint — bus-only coordination:**
- Other contexts interact with Journal via **commands on the bus**, not direct `GetById`
- Classification pipeline sends `PostEntry` command; reconciliation sends `PostEntry` for adjustments
- No handler should call `GetById` on Journal AND another aggregate in the same handler method
- This prevents creating implicit saga #8

**New streams:** 1 per business (Journal stream)
**New RMs:** Ledger RM (3.4, Phase 2) — place in **ModelServer**, not SpreadsheetAdapter. SpreadsheetAdapter projection only when UI requires it.
**Data usage impact:** Moderate — journal entries are lower volume than data imports (~50-200 entries/month vs ~1,001 events per CSV import)

### Track C: God Aggregate DataTable Mapping Extraction (P1 #4)

| What | From | To | Events Moved |
|------|------|----|-------------|
| DataTable mapping events | ServerFinancialModel | DataTableMap, ListDataTableMap, ManualTableMap (existing) | ~20 of 31 registered events |

**Evidence:** god-aggregate.md lists 31 registered events; ~20 are DataTable/ListDataTable/ManualTable mapping events. Dedicated mapping aggregates already exist (DataTableMap: 370 lines, ListDataTableMap: 369 lines, ManualTableMap: 306 lines).

**Action:**
1. Move mapping event handlers from ServerFinancialModel into the existing mapping aggregates
2. ServerFinancialModel retains only: ModelCreated, ModelDeleted, ModelArchived, ModelApproved, ModelReviewed, PencilEditApplied, ChangesCompleted, UserDefinedWorksheets events (~11 events)
3. FinancialModelService reduced from 166 commands — mapping commands route to dedicated mapping services
4. Update all SpreadsheetAdapter RMs that subscribe to mapping events (identified in Phase 0.4 audit)

**Impact:**
- ServerFinancialModel drops from ~3,557 to ~1,500-2,000 lines (estimate)
- FinancialModelService drops from 166 to ~80-90 commands
- Average PR size for model-related features drops from 26-94 files to ~10-20 (feature-provenance.md evidence)
- Read amplification per mapping change decreases — mapping events now on their own streams, not replayed with every model operation

**Risk:** This is the highest-risk change in the plan. Moving events changes stream structure. Requires:
- Migration strategy for existing event streams — leverage **VersionedEventSerializer** and **EventUpcasterRegistry** from `process-manager/Infrastructure/EventVersioning.cs` for schema evolution
- All mapping RMs updated to subscribe from correct streams
- Full RestoreFromEvents re-verification after migration

**Effort:** 2–3 weeks

### Phase 1 Exit Criteria

- [ ] AccountingSystem extended with 3 new commands + RestoreFromEvents test
- [ ] Journal aggregate created with PostEntry/CorrectEntry/RemoveEntry + RestoreFromEvents test
- [ ] God Aggregate reduced by ~20 events (mapping extraction complete)
- [ ] FinancialModelService reduced from 166 to ~80-90 commands
- [ ] No new implicit sagas created — all cross-aggregate interaction via bus commands

---

## Phase 1.5: Two-Layer Snapshot/Caching (Read Explosion Mitigation)

**Goal:** Eliminate per-category-stream replay on startup and reduce aggregate `GetById` replay cost. Address the startup amplification at the infrastructure level before adding new RMs in Phase 2.
**Duration:** 2–3 weeks (parallel with late Phase 1 work)
**Depends on:** Phase 0.4 (SpreadsheetAdapter RM audit — need to know which RMs to prioritize)
**Infrastructure:**
- R-D's `CachingRepository` (aggregate layer — exists, needs persisted tier)
- `read-model-caching-reference/` (read model layer — `IReadModelStore`, `ICheckpointStore`, `DapperReadModelStore`, `ProjectionSchema`)

### Why This Phase Exists

**Corrected subscription picture** (from vault scan with `Start<TAgg>()` extraction):

| Category | Count | Subscription | Startup Cost |
|----------|-------|-------------|-------------|
| ReadModelBase | 46 | Category streams (`$ce-*`) — replays from position 0 per stream | Per-stream replay |
| TransientSubscriber | 34 | Bus (live only) — no stream replay | None |
| **Total** | **80** | | |

ReadModelBase RMs subscribe to **specific aggregate category streams** via `Start<TAgg>()`, not `$All`. TransientSubscribers are short-lived bus listeners (UI-scoped) with zero startup cost.

The hottest category stream is `$ce-ServerFinancialModel` with **9 subscribing RMs**. Most other streams have 1–3 subscribers. The amplification is per-category-stream, not per-event-globally.

Phase 0.4 audits the surface; Phase 1C reduces per-stream cost by splitting mapping events off ServerFinancialModel. But neither addresses the **startup replay** problem (46 ReadModelBase RMs replaying their category streams from position 0) or the **aggregate replay** problem (ServerFinancialModel replaying 3,557+ events per `GetById`).

Without this phase, every subsequent phase makes the problem worse — more aggregates, more events, more RMs.

### Layer 1: Aggregate Snapshots — Expand R-D's CachingRepository

**Current state:** R-D's `CachingRepository` wraps `IRepository` and caches `GetById` results in memory. After first load, repeated loads return the cached instance. Cache is lost on restart.

**Expansion — two-tier CachingRepository:**

| Tier | Storage | Behavior |
|------|---------|----------|
| **Hot (in-memory)** | `ConcurrentDictionary<Guid, AggregateRoot>` | Current `CachingRepository` behavior — zero-latency `GetById` after first load |
| **Persisted (relational DB)** | Snapshot table in PostgreSQL | Periodically persist aggregate state + stream version. On restart, load snapshot + replay only events since snapshot version |

**Aggregate snapshot flow:**
```
GetById(id):
  1. Check in-memory cache → hit? return cached
  2. Check PostgreSQL snapshot → found? load snapshot, replay events since snapshot version, cache, return
  3. Neither → full stream replay (existing behavior), cache, persist snapshot
  
Save(aggregate):
  1. Append events to ESDB (existing)
  2. Update in-memory cache (existing CachingRepository behavior)
  3. If events_since_last_snapshot > threshold → persist new snapshot to PostgreSQL
```

**Priority targets** (by replay cost):

| Aggregate | Events in Stream | `GetById` Cost | Snapshot Benefit |
|-----------|-----------------|----------------|-----------------|
| ServerFinancialModel | 3,557+ | 87 Apply methods replayed | 3,557 → ~50-100 events per load |
| ChartOfAccounts | ~540 lines, 14 PRs/9mo | High churn, frequent loads | Moderate reduction |
| DataTableMap/ListDataTableMap | ~370 each | Moderate | Moderate — but extracted from God Agg in Phase 1C, streams now smaller |

**Implementation approach:**
1. Extend R-D's `CachingRepository` with a `ISnapshotStore` interface
2. `ISnapshotStore.LoadSnapshot<T>(Guid id)` → returns `(T aggregate, long version)` or null
3. `ISnapshotStore.SaveSnapshot<T>(Guid id, T aggregate, long version)`
4. PostgreSQL implementation using Dapper (consistent with `DapperReadModelStore` patterns)
5. Snapshot frequency configurable per aggregate type (e.g., ServerFinancialModel every 100 events, smaller aggregates every 500)

**Effort:** 1–2 weeks (CachingRepository expansion + snapshot store implementation)

### Layer 2: Read Model Checkpoints + Persistent Store

**Current state:** 46 ReadModelBase RMs subscribe to category streams (`$ce-*`) from position 0, keep state in memory only. Every restart replays each category stream from the beginning. The 34 TransientSubscriber RMs are bus-only and have zero startup cost.

**Target state:** ReadModelBase RMs use `ICheckpointStore` to resume from last processed position per category stream, and `IReadModelStore` for two-tier persistence.

**Migration pattern for existing RMs:**

```csharp
// BEFORE: No persistence, no checkpoints
public class ModelTemplateRm : ReadModelBase, IHandle<DataTableMapped>, ... {
    private readonly Dictionary<Guid, ModelTemplate> _templates = new();
    
    public ModelTemplateRm(IConfiguredConnection connection)
        : base("ModelTemplateRm", connection) {
        Start<ServerFinancialModel>(); // replays $ce-ServerFinancialModel from position 0
    }
}

// AFTER: Two-tier storage + checkpoint resume
public class ModelTemplateRm : ReadModelBase, IHandle<DataTableMapped>, ... {
    private readonly IReadModelStore<Guid, ModelTemplateState> _store;
    
    public ModelTemplateRm(
        IConfiguredConnection connection,
        IReadModelStore<Guid, ModelTemplateState> store,
        ICheckpointStore checkpoints)
        : base("ModelTemplateRm", connection, checkpoints) {
        _store = store;
        // ReadModelBase uses checkpoint to resume $ce-ServerFinancialModel from last position
    }
    
    public void Handle(DataTableMapped @event) {
        _store.Upsert(@event.Id, new ModelTemplateState { ... });
        // Checkpoint saved by ReadModelBase infrastructure
    }
}
```

**Two-tier `IReadModelStore` composition:**

| Operation | Hot (InMemoryReadModelStore) | Persisted (DapperReadModelStore) |
|-----------|----------------------------|----------------------------------|
| `Upsert` | ✅ write | ✅ write |
| `GetById` | ✅ read (primary) | fallback if cache miss |
| `GetAll` | ✅ read (primary) | fallback |
| `GetWhere(sql)` | N/A | ✅ SQL-level filtering |
| Startup | Warm from PostgreSQL | Already persisted |

**PostgreSQL schema required:**
- `ProjectionSchema.InitializeAsync()` creates tables per RM + `projection_checkpoints` table
- Use `[Table]`/`[Column]` attributes on POCO state classes (pattern from `ReadModelState.cs`)
- Indexes on foreign keys and query-path columns

**Priority migration order** (by stream fan-out and event count):

The `$ce-ServerFinancialModel` stream has **9 subscribing RMs** — the highest fan-out. Migrating these first gives the biggest startup time reduction.

| RM | Stream | Events | Priority |
|----|--------|--------|----------|
| ModelTemplateRm | `$ce-ServerFinancialModel` | 82 | 🔴 First — highest event count, on hottest stream |
| FinancialModelRm | `$ce-ServerFinancialModel` | 49 | 🔴 Second — on hottest stream, 4 defects |
| FinancialModelListRm | `$ce-ServerFinancialModel` | 17 | � Third — on hottest stream |
| Remaining 6 on `$ce-ServerFinancialModel` | `$ce-ServerFinancialModel` | varies | 🟡 Batch — complete the hottest stream |
| ManualTableRm, ManualTableMapRm | `$ce-ManualTable`, `$ce-ManualTableMap` | 14 each | 🟡 Next — second-tier streams |
| Remaining 31 ReadModelBase RMs | various | varies | 🟢 Last — most streams have 1-2 subscribers |

**Effort:** 2–3 weeks (schema + migrate top 3 RMs + batch remaining)

### Phase 1.5 Exit Criteria

- [ ] `CachingRepository` expanded with `ISnapshotStore` interface (hot + persisted tiers)
- [ ] PostgreSQL snapshot store implementation for aggregates (Dapper-based)
- [ ] ServerFinancialModel snapshot working — `GetById` replays <100 events after first load
- [ ] `projection_checkpoints` table created via `ProjectionSchema`
- [ ] All 9 RMs on `$ce-ServerFinancialModel` migrated to `IReadModelStore` + `ICheckpointStore`
- [ ] Startup replay of `$ce-ServerFinancialModel` reduced from full to incremental catch-up
- [ ] Migration template established for remaining 37 ReadModelBase RMs (batch in Phase 2)
- [ ] Note: 34 TransientSubscriber RMs need no migration (bus-only, zero startup cost)

### Impact on Subsequent Phases

| Phase | Before 1.5 | After 1.5 |
|-------|-----------|-----------|
| Phase 2: New Ledger RM | Built without checkpoints → adds to startup replay | Built WITH `ICheckpointStore` + `DapperReadModelStore` from day one |
| Phase 3: ProcessManagers | ProcessManagerBase already uses NullBus replay safety | ProcessManagers benefit from `CachingRepository` for `LoadOrCreate` — snapshot-backed aggregate loads |
| Phase 3D: Pipeline migration | Existing pipeline RMs replay from 0 | Migrated RMs resume from checkpoint — safer rollback window |

---

## Phase 2: Feature Buildout (Parallel Tracks)

**Goal:** Build the feature surface from the small-accountant-flow proposal on top of the corrected foundation.
**Duration:** 3–4 weeks
**Depends on:** Phase 1 Track A (for 2.1, 2.2), Track B (for 3.4), Phase 1.5 (for RM patterns)

### Track D: CoA Templates + UI (Epic 1.4, 2.1–2.3)

| Item | What | Depends On |
|------|------|-----------|
| 1.4 | Company Setup UI (entity type, industry, fiscal year) | 1.1-1.3 |
| 2.1 | Industry CoA Template Engine | 1.2 |
| 2.2 | Seed CoA From Template command | 2.1 |
| 2.3 | Contra Account Workflow in UI | None |

**Mitigation integration:**
- ChartOfAccounts (14 PRs in 9 months, most active aggregate) gets the SeedFromTemplate command. This is additive — no structural risk.
- Industry template engine is a new **service**, not a new aggregate — no stream or RM impact.
- Contra UI is purely presentation — no domain impact.

**New streams:** 0
**New RMs:** 0 (extends existing ChartOfAccountsRm)

### Track E: Ledger Read Model + Classification Rules (Epic 3.4, 4.1–4.2)

| Item | What | Depends On |
|------|------|-----------|
| 3.4 | Ledger RM (journal entry materialization) | 3.1 |
| 4.1 | Per-entity configurable classification rules | None |
| 4.2 | 85/15 confidence threshold enforcement | None |

**Mitigation integration:**
- **Ledger RM placement:** Build in **ModelServer**, not SpreadsheetAdapter. This is a read model that materializes general ledger balances from Journal events. SpreadsheetAdapter gets a thin projection only when the UI requires account balance display.
- **Classification rules** (4.1): New aggregate or child of AccountingSystem for rule persistence. If new aggregate → new stream, new RM. Design as a small focused aggregate. If child of AccountingSystem → evaluate whether it overloads AccountingSystem's configuration responsibility.
- **Confidence threshold** (4.2): Configuration change to existing pipeline steps. No new streams or RMs.

**New streams:** 0-1 (Ledger RM is read-only; classification rules may be 1 new aggregate)
**New RMs:** 1 (Ledger RM in ModelServer)
**SpreadsheetAdapter impact:** Minimal — Ledger RM is in ModelServer. Classification rule changes affect existing pipeline steps.

### Phase 2 Exit Criteria

- [ ] Business creation wizard collects entity type, industry, fiscal year
- [ ] Industry CoA seeding works for ≥2 templates
- [ ] Ledger RM materializes balances from Journal events (in ModelServer)
- [ ] Classification rules are configurable per entity (replacing hardcoded RuleStep)
- [ ] Confidence threshold boundary enforced in classification pipeline
- [ ] No new SpreadsheetAdapter RMs created (Ledger RM in ModelServer)

---

## Phase 3: Explicit Process Managers + Accounting Completion

**Goal:** Make the reconciliation saga explicit using the formal ProcessManager pattern and complete the accounting workflow.
**Duration:** 4–5 weeks
**Depends on:** Phase 1 Track B (Journal), Phase 2 Track E (Ledger RM)
**Infrastructure:** `process-manager/Infrastructure/ProcessAggregateBase.cs`, `ProcessManagerBase.cs`, `CategoryStreamWarmup.cs`

### 3A: Entry Patterns + GL Reports (Epic 3.2–3.3)

| Item | What | Depends On |
|------|------|------------|
| 3.2 | Entry Patterns Library (journal gestures) | 3.1 |
| 3.3 | GL Report Generation (trial balance, P&L, balance sheet) | 3.1, 3.4 |

**Mitigation integration:**
- Entry patterns formalize what are currently **implicit data transformations** in the reconciliation pipeline. Making them explicit services that produce Journal commands reduces the coordination buried in handler code.
- GL reports read from Ledger RM only — no cross-aggregate coordination.

### 3B: Reconciliation Process Manager (Epic 5.1) — First Formal ProcessManager

**This is the single most important mitigation action.** Evidence:
- saga-catalog.md: Reconciliation is implicit, 5 contexts, 5 aggregates
- defect-analysis.md: 19 defects (29% of total), #1 defect pattern
- feature-provenance.md: 4 epics dedicated to reconciliation rework

#### Process Aggregate: `ReconciliationProcess : ProcessAggregateBase<ReconState>`

**State machine:**
```
enum ReconState {
    NotStarted,        // default — process not yet initiated
    Importing,         // statement data being parsed/validated
    Matching,          // transactions being matched to journal entries
    ReviewPending,     // unmatched items awaiting accountant review
    AdjustmentsPending,// reconciliation adjustments queued for posting
    Completing,        // final balance comparison in progress
    Reconciled,        // terminal — statement balance = book balance
    Failed             // terminal — unrecoverable error or user abort
}
```

**State handlers (registered in `RegisterStateHandlers()`):**
```csharp
protected override IReadOnlyList<ReconState> TerminalStates => [ReconState.Reconciled, ReconState.Failed];

protected override void RegisterStateHandlers() {
    InState(ReconState.NotStarted)
        .On<ReconciliationStarted>(e => {
            // Transition to Importing, issue command to parse statement
            Raise(new ReconStateChanged(Id, ReconState.Importing));
            IssueCommand(new ParseStatementData(e.AccountId, e.StatementId));
        });

    InState(ReconState.Importing)
        .On<StatementDataParsed>(e => {
            Raise(new ReconStateChanged(Id, ReconState.Matching));
            IssueCommand(new MatchTransactions(Id, e.AccountId));
        })
        .On<StepTimedOut>(_ => {
            Raise(new ReconStateChanged(Id, ReconState.Failed));
            Raise(new ReconciliationFailed(Id, "Statement import timed out"));
        });

    InState(ReconState.Matching)
        .On<TransactionsMatched>(e => {
            if (e.UnmatchedCount > 0) {
                Raise(new ReconStateChanged(Id, ReconState.ReviewPending));
            } else {
                Raise(new ReconStateChanged(Id, ReconState.Completing));
                IssueCommand(new CompareBalances(Id, e.AccountId, e.PeriodEnd));
            }
        });

    InState(ReconState.ReviewPending)
        .On<ReviewCompleted>(e => {
            if (e.HasAdjustments) {
                Raise(new ReconStateChanged(Id, ReconState.AdjustmentsPending));
                // Issue PostEntry commands for each adjustment via Entry Patterns
                foreach (var adj in e.Adjustments)
                    IssueCommand(new PostEntry(adj.JournalId, adj.Pattern, adj.Lines));
            } else {
                Raise(new ReconStateChanged(Id, ReconState.Completing));
                IssueCommand(new CompareBalances(Id, e.AccountId, e.PeriodEnd));
            }
        });

    InState(ReconState.AdjustmentsPending)
        .On<AdjustmentsPosted>(_ => {
            Raise(new ReconStateChanged(Id, ReconState.Completing));
            IssueCommand(new CompareBalances(Id, ...));
        });

    InState(ReconState.Completing)
        .On<BalancesCompared>(e => {
            if (e.IsBalanced) {
                Raise(new ReconciliationCompleted(Id, e.AccountId, e.Period, e.EndingBalance));
                Raise(new ReconStateChanged(Id, ReconState.Reconciled));
            } else {
                Raise(new ReconStateChanged(Id, ReconState.ReviewPending));
                Raise(new BalanceMismatchDetected(Id, e.Difference));
            }
        });
}
```

**What this gets you vs. the implicit approach:**

| Capability | Before (implicit) | With ProcessAggregateBase |
|-----------|-------------------|---------------------------|
| State visibility | None — poll SpreadsheetAdapter RMs | `State` property on aggregate, persisted via events |
| Timeout handling | None — imports hang indefinitely (issue #2068) | `CreateStepTimeout()` with auto-staleness detection |
| Compensation | None — ~1,001 events committed with no rollback | `IssueCommand` for compensating entries on failure transition |
| Retry | None — restart from scratch | `Failed` state preserves context; re-send `StartReconciliation` |
| Audit trail | Scattered across handler logs | `CommandsIssued` event serializes all dispatched commands |
| Replay safety | N/A | Event-sourced aggregate replays correctly via `Register<T>(Apply)` |

#### Process Manager: `ReconciliationProcessManager : ProcessManagerBase<ReconciliationProcess, ReconState>`

**Responsibilities:**
- Subscribes to domain events from DataSource, Journal, Classification pipeline via ESDB **category streams** (`$ce-reconProcessAgg`)
- On each event: `LoadOrCreate(processId)` → `aggregate.When(event)` → `SaveAndTakeCommands()` → `DispatchCommands()`
- Routes `StepTimedOut` events to the correct process aggregate via `ProcessTypeName` matching
- NullBus during replay prevents duplicate command dispatch on restart
- `GoLive()` switches to live bus after catch-up; `OnGoLive()` reschedules timeouts for in-progress reconciliations

**Startup sequence:**
```csharp
public ReconciliationProcessManager(
    IConfiguredConnection connection,
    IRepository repository,
    IBus bus)
    : base("ReconciliationPM", connection, repository, bus) {

    // Category stream warmup ensures $ce-reconProcessAgg exists
    // (handled in composition root via CategoryStreamWarmup.Warmup())

    // Subscribe to domain events that drive the process
    EventStream.Subscribe<StatementDataParsed>(this);
    EventStream.Subscribe<TransactionsMatched>(this);
    EventStream.Subscribe<ReviewCompleted>(this);
    EventStream.Subscribe<AdjustmentsPosted>(this);
    EventStream.Subscribe<BalancesCompared>(this);

    Start<ReconciliationProcess>(blockUntilLive: true);
    GoLive();
}
```

**Event handler pattern (repeated for each event):**
```csharp
public void Handle(StatementDataParsed @event) {
    var (agg, _) = LoadOrCreate(@event.ProcessId);
    agg.Source = NewCorrelationSource();
    agg.When(@event);
    var commands = SaveAndTakeCommands(agg);
    DispatchCommands(commands);
}
```

**New streams:** 1 per reconciliation instance (ReconciliationProcess stream)
**New RMs:** 0 — the ProcessManager IS the read model (extends ReadModelBase)
**Data usage impact:** Low — small state transition events, ~6-8 per reconciliation cycle
**Category stream:** `$ce-reconProcessAgg` (requires CategoryStreamWarmup on first deploy)

### 3C: Period Close Process Manager (Epic 5.2) + Recon Adjustments (Epic 5.3)

#### Process Aggregate: `PeriodCloseProcess : ProcessAggregateBase<CloseState>`

**State machine:**
```
enum CloseState {
    Open,              // default — period accepts entries
    ChecklistPending,  // close initiated, checklist items being verified
    SoftClosed,        // warnings on new entries but not blocked
    HardClosed,        // new entries blocked
    Reopened           // terminal (reverts to Open on next cycle)
}
```

**Coordination via commands:**
- `SoftClose` → `IssueCommand(new GenerateCloseChecklist(...))` → TasklistItem aggregate creates checklist tasks
- `HardClose` → validates all reconciliations complete (queries ReconciliationProcess RM), all checklist items done
- `ReopenPeriod` → `IssueCommand(new PostEntry(reopeningAdjustment))` if needed
- Period close status queryable by Journal aggregate's validation layer to reject entries to closed periods

**Recon Adjustments (5.3):**
- Reconciliation adjustment patterns (bank fees, interest, NSF) are Entry Patterns (3.2) that produce `PostEntry` commands
- ReconciliationProcess issues these commands during `AdjustmentsPending` state
- Adjustments flow: ReconciliationProcess → `IssueCommand(PostEntry)` → Journal aggregate → Ledger RM updates
- No handler calls `GetById` on both Journal and ReconciliationProcess — command dispatch is the only coordination

**New streams:** 1 per business per period (PeriodCloseProcess stream)
**New RMs:** 0 — ProcessManager IS the read model
**Category stream:** `$ce-periodCloseAgg` (requires CategoryStreamWarmup)

### 3D: Migrate Existing Implicit Reconciliation → Process Manager

**Action:** Wire the existing reconciliation pipeline (Pipeline.cs, 12+ steps) through the new ReconciliationProcessManager. The pipeline currently coordinates DataSource → DataElement → ChartOfAccounts → AccountBalance implicitly through handler services in SpreadsheetAdapter.

**Migration steps:**
1. Add ProcessManager infrastructure to PowerModels (copy `ProcessAggregateBase`, `ProcessManagerBase`, `CategoryStreamWarmup` from `process-manager/Infrastructure/`)
2. Add `StepTimedOut` and `CommandsIssued` events to `Greylock.Domain.Msgs.Evts`
3. Create `ReconciliationProcess` aggregate + `ReconciliationProcessManager`
4. Add `CategoryStreamWarmup.Warmup(connection, ["reconProcessAgg", "periodCloseAgg"])` to composition root
5. Pipeline.start → sends command that creates/starts `ReconciliationProcess`
6. Each pipeline step outcome → domain event → ProcessManager routes to aggregate → state transition + next command
7. Pipeline.complete → `ReconciliationCompleted` event from aggregate
8. SpreadsheetAdapter RM subscribers that currently track reconciliation state → subscribe to `ReconciliationProcess` events instead
9. Feature flag: route through ProcessManager only when flag is on; keep old pipeline as fallback

**This is where the 19 reconciliation defects get addressed structurally** — not by fixing each bug individually, but by replacing the implicit coordination with a formal state machine that has timeout handling, compensation, and visibility.

### Phase 3 Exit Criteria

- [ ] `ReconciliationProcess : ProcessAggregateBase<ReconState>` with 7 states, timeout handling, RestoreFromEvents
- [ ] `ReconciliationProcessManager : ProcessManagerBase<ReconciliationProcess, ReconState>` with GoLive recovery
- [ ] `PeriodCloseProcess : ProcessAggregateBase<CloseState>` with SoftClose/HardClose/Reopen, RestoreFromEvents
- [ ] `PeriodCloseProcessManager : ProcessManagerBase<PeriodCloseProcess, CloseState>` with GoLive recovery
- [ ] CategoryStreamWarmup integrated for both process aggregate stream prefixes
- [ ] Entry patterns library producing Journal commands for ≥6 accounting scenarios
- [ ] GL reports (trial balance, P&L, balance sheet) reading from Ledger RM
- [ ] Existing reconciliation pipeline wired through ReconciliationProcessManager (behind feature flag)
- [ ] All cross-aggregate coordination via `IssueCommand` → `DispatchCommands` (no `GetById` across aggregates)
- [ ] Zero new implicit sagas created — all new coordination uses ProcessManager pattern

---

## Cumulative Impact Projection

### God Aggregate

| Metric | Before | After Phase 1 | After Phase 1.5 | After Phase 3 |
|--------|--------|---------------|-----------------|---------------|
| Lines | 3,557 | ~1,500-2,000 | ~1,500-2,000 | ~1,500-2,000 |
| Registered events | 31 | ~11 | ~11 | ~11 |
| FinancialModelService commands | 166 | ~80-90 | ~80-90 | ~80-90 |
| `GetById` replay cost | 3,557+ events | ~1,500+ events | **<100 events** (snapshot) | <100 events |
| Avg PR file count (model features) | 26-94 | ~10-20 | ~10-20 | ~10-20 |

### Saga Explicitness

| Saga | Before | After Phase 3 |
|------|--------|---------------|
| Reconciliation | Implicit (19 defects) | **ProcessManager** — `ReconciliationProcess : ProcessAggregateBase<ReconState>` with 7 states, timeouts, compensation |
| Period Close | Doesn't exist | **ProcessManager** — `PeriodCloseProcess : ProcessAggregateBase<CloseState>` coordinating close workflow |
| Data Import Pipeline | Implicit (8 defects) | Implicit (Phase 4 candidate — natural fit for ProcessManager with timeout on import step) |
| Business Setup | Implicit (3 defects) | Partially explicit — AccountingSystem coordinates setup |
| Financial Model Mapping | Implicit (coupling) | Reduced — mapping extracted from God Aggregate |
| **Journal → Recon → Close** | **Doesn't exist** | **Explicit from day one** — ProcessManager → IssueCommand → DispatchCommands |

### Read Explosion / Startup Performance

| Metric | Before | After Phase 1.5 | After Phase 3 |
|--------|--------|-----------------|---------------|
| Startup replay | 46 ReadModelBase RMs replay category streams (`$ce-*`) from position 0; 34 TransientSubscriber RMs have zero startup cost | 9 RMs on `$ce-ServerFinancialModel` (hottest stream) resume from checkpoint; remaining 37 templated | All ReadModelBase RMs checkpoint-based |
| Aggregate `GetById` replay | Full stream per load | Snapshot + recent events (ServerFinancialModel <100) | Same |
| RM state persistence | In-memory only, lost on restart | PostgreSQL via `DapperReadModelStore` + `InMemoryReadModelStore` hot cache | Same |
| `$ce-ServerFinancialModel` fan-out | 9 RMs replay same stream | 9 RMs resume from checkpoint — replay once, not 9× | Same |

### SpreadsheetAdapter

| Metric | Before | After Phase 3 |
|--------|--------|---------------|
| Read models | 40 | 40 + consolidation savings from 0.4 audit |
| Defect rate | 15% | Projected reduction — reconciliation state no longer tracked via RM polling |
| New RMs added | — | 0 in SpreadsheetAdapter (new RMs in ModelServer) |

### Test Coverage

| Metric | Before | After Phase 3 |
|--------|--------|---------------|
| RestoreFromEvents tests | 0/27 | ≥12/27 (8 existing + Journal + 2 ProcessAggregates + AccountingSystem) |
| Source constructors | 23/27 | 27/27 |
| New aggregates with full test pattern | 0 | 3 (Journal, ReconciliationProcess, PeriodCloseProcess) |
| ProcessManager replay tests | 0 | 2 (verify NullBus→GoLive transition, no duplicate commands on restart) |

### New Aggregate Summary

| Aggregate | Base Class | Phase | Context | Streams | Coordinator |
|-----------|-----------|-------|---------|---------|-------------|
| Journal | AggregateRoot | 1B | ModelServer/AS | 1/business | JournalService (standard handler) |
| ReconciliationProcess | ProcessAggregateBase\<ReconState\> | 3B | ModelServer/AS | 1/recon instance | ReconciliationProcessManager (ReadModelBase) |
| PeriodCloseProcess | ProcessAggregateBase\<CloseState\> | 3C | ModelServer/AS | 1/business/period | PeriodCloseProcessManager (ReadModelBase) |

**Key difference from plain aggregates:** ProcessAggregates are coordinated by ProcessManagers (which extend ReadModelBase), not by handler services. The ProcessManager IS the read model — no separate RM needed. Category stream warmup required on first deploy.

All new read-side projections placed in ModelServer — SpreadsheetAdapter projections created only when UI demands.

---

## Dependencies and Risks

### External Dependencies

| Dependency | Impact | Mitigation |
|-----------|--------|-----------|
| March demo deadline (#2099, #2106-2109) | Phase 0 competes with active demo work | Phase 0 source ctor + RestoreFromEvents tests don't touch demo-critical code paths |
| joshkempner availability (69% of PRs) | Primary implementer for all phases | Phase 2 tracks are parallelizable across 2+ developers |
| Existing reconciliation pipeline stability | Phase 3D rewires a working pipeline | Keep old pipeline as fallback until new path is verified end-to-end |

### Technical Risks

| Risk | Likelihood | Impact | Mitigation |
|------|-----------|--------|-----------|
| God Aggregate event migration breaks existing streams | Medium | High | Use `VersionedEventSerializer` + `EventUpcasterRegistry` from process-manager infrastructure for schema evolution; run old+new in parallel before cutover |
| New Journal aggregate creates performance bottleneck | Low | Medium | Journal is lower volume than data imports; monitor stream growth |
| ReconciliationProcessManager migration breaks existing reconciliation | Medium | High | Feature flag: route through ProcessManager only when flag is on; NullBus replay safety prevents duplicate commands on restart |
| AccountingSystem grows into configuration God Aggregate | Low | Medium | Review at Phase 1 exit — if >200 lines, extract close policy to PeriodClose |
| Snapshot deserialization breaks on aggregate schema change | Medium | Medium | Invalidate snapshots on version mismatch — fall back to full replay. Snapshot version tracked alongside stream version |
| PostgreSQL RM state diverges from ESDB truth | Low | High | `ICheckpointStore` ensures exactly-once processing position; rebuild from $All is always available as fallback. `ExecuteTransactional()` for atomic state+checkpoint saves |

---

## Success Metrics

| Metric | Target | Measured By |
|--------|--------|-------------|
| God Aggregate lines | <2,000 | scan-data.json after Phase 1 |
| God Aggregate `GetById` replay | <100 events | Snapshot store metrics after Phase 1.5 |
| RestoreFromEvents coverage | ≥12/27 aggregates | test-coverage.md after Phase 3 |
| Source ctor coverage | 27/27 | scan-data.json after Phase 0 |
| Startup replay elimination | Top 3 RMs checkpoint-based | `projection_checkpoints` table after Phase 1.5 |
| RM persistence coverage | ≥3 RMs on `IReadModelStore` | Phase 1.5 exit; remaining batch in Phase 2 |
| Reconciliation defect rate | <15% of new defects | defect-analysis.md refresh after Phase 3 |
| SpreadsheetAdapter defect rate | <10% | defect-analysis.md refresh after Phase 3 |
| Implicit sagas | ≤4 (from 7) | saga-catalog.md refresh after Phase 3 |
| New aggregate test pattern compliance | 100% | All 3 new aggregates have RestoreFromEvents + source ctor |
| ProcessManager pattern compliance | 100% of new sagas | All cross-aggregate workflows use ProcessAggregateBase + ProcessManagerBase |
| Cross-aggregate coordination via bus | 100% of new interactions | Code review — all coordination via `IssueCommand` → `DispatchCommands`, no `GetById` across aggregate boundaries |

---

## Vault Integration

### Documents Updated by This Plan

| Doc | Update Trigger |
|-----|---------------|
| scan-data.json | After each phase (new aggregates, moved events) |
| aggregates.md | After Phase 0 (source ctors), Phase 1 (Journal), Phase 3 (ReconciliationState, PeriodClose) |
| god-aggregate.md | After Phase 1C (event extraction) |
| message-map.md | After Phase 1 (new commands), Phase 3 (new commands) |
| read-models.md | After Phase 2 (Ledger RM), Phase 3 (ReconciliationState RM, PeriodClose RM) |
| bounded-contexts.md | After Phase 1 (Journal in ModelServer/AS), Phase 3 (new aggregates) |
| test-coverage.md | After Phase 0 (RestoreFromEvents), Phase 1-3 (new aggregate tests) |
| feature-provenance.md | After each milestone completion (GitHub re-extraction) |
| saga-catalog.md | After Phase 3 (explicit reconciliation saga, new workflow chain) |
| defect-analysis.md | After each milestone completion (GitHub re-extraction) |

### Drift Detection

Run after each phase:
```powershell
dotnet run -- project-refresh --config ../../projects/PowerModels/graph-vault.json
```

Expected drift signals:
- Phase 0: No drift (tests + ctor fixes don't change architecture docs)
- Phase 1: Drift in aggregates.md, message-map.md, god-aggregate.md (extraction changes)
- Phase 1.5: Drift in read-models.md (checkpoint + store changes), application-topology.md (PostgreSQL dependency added)
- Phase 2: Drift in read-models.md (Ledger RM)
- Phase 3: Drift in aggregates.md, bounded-contexts.md, saga-catalog.md (new ProcessAggregates + explicit ProcessManagers)

---

## Appendix: ProcessManager Infrastructure Reference

**Source:** `process-manager/Infrastructure/`

### Pattern: Domain Event → ProcessManager → ProcessAggregate → Command Dispatch

```
Domain event (e.g., StatementDataParsed)
  → ProcessManagerBase.Handle(event)  [subscribed via EventStream or bus]
    → LoadOrCreate(processId)         [loads ProcessAggregate from repository]
      → aggregate.When(event)          [dispatches to state-specific handler]
        → handler transitions state via Raise(new StateChanged(...))
        → handler accumulates commands via IssueCommand(new PostEntry(...))
      → SaveAndTakeCommands(aggregate) [persists aggregate + returns commands]
    → DispatchCommands(commands)       [sends to ActiveBus]
```

### Key Infrastructure Classes

| Class | Extends | Role |
|-------|---------|------|
| `ProcessAggregateBase<TState>` | `AggregateRoot` | State machine aggregate — state enum, `InState().On<TEvent>()` DSL, `IssueCommand()`, `CreateStepTimeout()` |
| `ProcessManagerBase<TProcessAgg, TState>` | `ReadModelBase` | Coordinator — NullBus replay safety, `LoadOrCreate`, `SaveAndTakeCommands` → `DispatchCommands`, `GoLive()` |
| `CategoryStreamWarmup` | static | ESDB `$ce-*` stream warmup — required before ProcessManager subscribes to category streams |
| `VersionedEventSerializer` | `IEventSerializer` | Event schema evolution — `[EventVersion]` attribute, `IEventUpcaster` chain |
| `EventUpcasterRegistry` | — | Upcaster chain management — sequential version-to-version transformation |

### Timeout Handling

```csharp
// In ProcessAggregate state handler:
InState(ReconState.Importing)
    .On<StatementDataParsed>(HandleParsed)
    .On<StepTimedOut>(HandleImportTimeout);  // auto-filtered for staleness

// Schedule timeout:
var timeout = CreateStepTimeout(TimeSpan.FromMinutes(5));
IssueCommand(timeout);  // sent via DelaySendEnvelope/LaterService

// Staleness: if state or version changed before timeout fires, it's auto-ignored.
// No need for manual cancellation.
```

### NullBus Replay Safety

On ProcessManager startup, `ActiveBus` is a `NullBus` that discards all publishes. After `Start<TProcessAgg>(blockUntilLive: true)` catches up the event stream, `GoLive()` switches to the live bus. This prevents duplicate command dispatch when replaying historical events on restart.
