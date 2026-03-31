# PowerModels Accelerated Mitigation Plan

Generated: 2026-03-08
Based on: mitigation-plan-2026-03-08.md, vault-project-analysis-2026-03-08-v3.md
Approach: Agent-assisted parallel execution with expert team

---

## Thesis

The full mitigation plan (Phases 0–4) estimates 15–21 weeks with single-developer pacing. This accelerated plan compresses to **5–6 weeks** by combining:

1. **Expert team** that eliminates learning-curve delays on both the patterns and the codebase
2. **AI agents** that eliminate mechanical work (test generation, RM migrations, aggregate scaffolding)
3. **Parallel execution** across 3 senior tracks + 3 support tracks running simultaneously

The critical path is not agent throughput — it's the sequence of **human design decisions** that gate dependent work. The team composition eliminates most of those gates.

---

## Team

| Role | Strengths | Primary Responsibility |
|------|-----------|----------------------|
| **James (ES Expert A)** | World-class event sourcing + large system design | Architect track — aggregate design, ProcessManager state machines, SFM extraction strategy |
| **Chris (ES Expert B)** | World-class event sourcing + large system design | Infrastructure track — ICheckpointStore, CachingRepository, ReadModelBase integration, PostgreSQL topology |
| **Josh (Principal Engineer)** | Knows the PowerModels codebase intimately | Integration + review — reviews all agent output, handles pipeline migration, catches domain edge cases |
| **Farouk (Support 1)** | Development | Test track — RestoreFromEvents tests, aggregate test suites |
| **Tom (Support 2)** | Development | Migration track — RM checkpoint migrations, SpreadsheetAdapter audit |
| **Maeve (Support 3)** | QA | Verification track — integration tests, regression, performance measurement |

### Agent Role

AI agents act as **force multipliers for the support team** and **scaffolding generators for the senior engineers**:

| Work Type | Agent Contribution | Human Contribution |
|-----------|-------------------|-------------------|
| RestoreFromEvents tests | Generate complete test from Apply method signatures | Support 1 validates + runs |
| RM checkpoint migrations | Generate before→after code for each of 46 RMs | Support 2 validates build + behavior |
| Source constructor fixes | Generate `:base(source)` additions | Principal batch-reviews |
| New aggregate scaffolding | Generate aggregate + commands + events + handler | ES Expert reviews design, adjusts invariants |
| ProcessManager state machines | Generate skeleton from state enum | ES Expert fills in transition logic |
| Entry patterns / classification rules | Generate from domain spec | Support validates accounting correctness |
| Vault rescan + doc regeneration | Fully automated via VaultTool | None required |

---

## Why This Team Compresses the Critical Path

### Design decisions that normally gate weeks → resolved in hours

| Decision | Normal Delay | This Team |
|----------|-------------|-----------|
| Journal aggregate design (balanced entries, period validation) | 1–2 weeks research + iteration | ES Expert A designs in hours — has built event-sourced ledgers at scale |
| ICheckpointStore integration into ReadModelBase | 1 week (learn the infra, test the pattern) | ES Expert B likely wrote the infrastructure — wiring their own code |
| SFM extraction dual-write strategy | 1–2 weeks (understand all 9 RM subscriptions) | Principal knows every RM subscription and handler route by heart |
| Reconciliation pipeline → ProcessManager migration | 2 weeks (map 12 implicit steps) | Principal has lived with those 19 defects — the PM state machine formalizes what's already in their head |
| PostgreSQL topology for checkpoint + RM persistence | 3–5 days ops decision | ES Expert B makes the call; `InMemoryCheckpointStore` is fallback |

### Mechanical work that normally fills weeks → agents batch in hours

| Work | Manual Estimate | Agent-Assisted |
|------|----------------|----------------|
| 27 RestoreFromEvents tests | 2–3 weeks | 2–3 days (agents generate, Support 1 validates) |
| 46 RM checkpoint migrations | 2–3 weeks | 3–4 days (agents apply template, Support 2 validates) |
| 4 source constructor fixes | 1 day | 1 hour |
| AccountingSystem extension (4 new commands) | 3–5 days | 1–2 days (agent scaffolds, ES Expert B reviews) |
| CoA template engine | 1 week | 2–3 days (agent generates, Support 1 validates) |
| Entry Patterns Library | 1 week | 2–3 days (agent generates patterns from accounting spec) |
| Classification rules enhancement | 1 week | 2–3 days |

---

## Week-by-Week Execution

### Week 1: Safety Net + Infrastructure Start

```
ES Expert A     Phase 0.2 — SFM RestoreFromEvents test (87 Apply methods)
                ↓ if Apply bugs found → event versioning decisions (has EventVersioning infra)

ES Expert B     Phase 1.1 — ICheckpointStore integration into ReadModelBase
                (their own infrastructure — pattern validated by end of week)

Principal       Phase 0.1 — fix 4 source constructors (trivial)
                → reviews all agent-generated RestoreFromEvents tests
                → starts Phase 1.4 (wire ProcessManagerBase into solution)

Support 1       Phase 0.3 — RestoreFromEvents for 5 growth aggregates
                (agents generate each test, Support 1 validates + runs)
                Aggregates: ChartOfAccounts, AccountBalance, AccountingSystem,
                EntrySet, TasklistItem

Support 2       CI integration for new tests; build verification

Support 3       Baseline measurement: startup time at 1K, 5K, 15K events
                (before any changes — establishes the "before" number)
```

**Week 1 Exit:**
- 6/27 RestoreFromEvents tests passing (SFM + 5 growth targets)
- 4 source constructors fixed
- ICheckpointStore integration pattern validated
- ProcessManagerBase wired into solution
- Startup time baseline measured

### Week 2: Infrastructure Complete + Domain Start

```
ES Expert A     Phase 2.2 — Journal aggregate design + implementation
                (starts immediately — doesn't wait for Phase 1 to fully complete)
                Commands: PostEntry, ReverseEntry, CorrectEntry
                Design constraints: bus-only coordination, no cross-aggregate GetById
                Ledger RM placement: ModelServer, not SpreadsheetAdapter

ES Expert B     Phase 1.2 — checkpoint 9 SFM RMs (agents generate migrations)
                Phase 1.3 — two-tier IReadModelStore (InMemory + Dapper)
                Phase 1.5 — CachingRepository for SFM GetById

Principal       Phase 1.4 complete (ProcessManagerBase available)
                → reviews 9 RM checkpoint migrations
                → starts Phase 2.1 (AccountingSystem extension — agent scaffolds)

Support 1       Phase 1.2 support — validates each of 9 RM migrations as agents produce them

Support 2       Phase 2.5 — SpreadsheetAdapter RM audit (agent-assisted catalog of 40 RMs)

Support 3       Post-Phase-1.2 measurement: SFM stream startup time with checkpoints
                Target: >80% reduction in handler invocations
```

**Week 2 Exit:**
- All 9 SFM RMs checkpointed (hottest stream mitigated)
- Two-tier RM persistence operational
- CachingRepository wrapping SFM
- Journal aggregate designed and under implementation
- AccountingSystem extension in progress
- SpreadsheetAdapter RM audit complete
- Startup time measured with checkpoints (before/after comparison)

### Week 3: Domain Foundation Complete

```
ES Expert A     Phase 2.2 complete (Journal aggregate + tests)
                → Phase 2.4 — Reconciliation ProcessManager design
                  ReconciliationProcess : ProcessAggregateBase<ReconState>
                  8 states, timeout handling, compensation logic

ES Expert B     Phase 2.1 complete (AccountingSystem: entity type, industry, fiscal year)
                → Phase 2.3 (CoA template engine — agent scaffolds, expert reviews)
                → starts Phase 3.5 planning (SFM extraction dual-write strategy)

Principal       Phase 2.4b — reconciliation pipeline migration plan
                Maps each of 12 Pipeline.cs steps to PM state transitions
                Identifies which SpreadsheetAdapter RMs need re-subscription
                Feature flag design for gradual cutover

Support 1       Phase 2.3 support (CoA templates — agent generates, support validates)
                → starts Phase 3.6 (remaining RestoreFromEvents — agents batch-generate)

Support 2       Continue RM checkpoint batch (remaining 37 ReadModelBase RMs)
                Phase 4.1 pulled forward — agents apply same template as SFM RMs

Support 3       Journal aggregate integration tests
                AccountingSystem extension integration tests
                Reconciliation PM test harness setup
```

**Week 3 Exit:**
- Journal aggregate complete with RestoreFromEvents + full test suite
- AccountingSystem extended (5+ events)
- CoA template seeding working
- Reconciliation ProcessManager designed with full state machine
- Pipeline migration plan documented (12 steps → 8 PM states)
- ~20/46 RMs checkpointed (SFM 9 + batch progress)

### Week 4: Process Managers + Feature Buildout Start

```
ES Expert A     Phase 2.4 — ReconciliationProcess implementation
                State machine, timeout handling, compensation
                + ReconciliationProcessManager (LoadOrCreate, SaveAndTakeCommands)
                CategoryStreamWarmup for $ce-reconProcessAgg

ES Expert B     Phase 3.5 — SFM table-mapping extraction begins
                Dual-write: events on both old (SFM) and new (dedicated) streams
                20 events moving to DataTableMap, ListDataTableMap, ManualTableMap

Principal       Phase 2.4b — pipeline migration execution
                Feature flag on; step-by-step wiring through PM
                Old pipeline as fallback until verified

Support 1       Phase 3.2 (Entry Patterns Library — agent generates patterns)
                Phase 3.3 (Classification Rules — agent scaffolds per-entity config)

Support 2       Continue RM checkpoint batch (~35/46 done)
                Phase 4.3 (ModelTemplateRm audit — does it need 82 events?)

Support 3       Reconciliation PM integration tests
                Pipeline migration regression tests (old vs new path comparison)
                Defect rate tracking setup
```

**Week 4 Exit:**
- Reconciliation ProcessManager operational (feature flag on, old pipeline as fallback)
- SFM extraction in progress (dual-write active)
- Entry Patterns Library operational
- Classification rules configurable per entity with 85/15 threshold
- ~35/46 RMs checkpointed

### Week 5: Feature Completion + Extraction

```
ES Expert A     Phase 3.1 — PeriodClose ProcessManager
                PeriodCloseProcess : ProcessAggregateBase<CloseState>
                5 states, SoftClose/HardClose/Reopen, checklist coordination
                + ReconciliationState aggregate

ES Expert B     Phase 3.5 complete — SFM extraction verification
                All 9 RMs re-subscribed to correct streams
                SFM reduced from 31 → ~11 registered events
                → Phase 4.4 (FinancialModelService split: ~100 core + ~60 mapping)

Principal       Phase 3.4 — GL Report Generation (Trial Balance, P&L, Balance Sheet)
                Read model projections from Journal events
                All with ICheckpointStore
                → verifies reconciliation pipeline migration (flag fully on)

Support 1       Phase 3.6 — remaining RestoreFromEvents tests
                Agents generate all 21 remaining; Support 1 batch-validates
                Target: 27/27 + 3 new aggregates

Support 2       RM checkpoint batch complete (46/46)
                Helps with GL report RM implementation

Support 3       Full regression suite
                Reconciliation defect rate measurement (before/after)
                Startup benchmarks across all business sizes
                SFM extraction verification tests
```

**Week 5 Exit:**
- PeriodClose ProcessManager operational
- ReconciliationState aggregate created
- GL reports rendering from Journal events
- SFM extraction complete (31 → ~11 events, 87 → ~67 Apply methods)
- FinancialModelService split in progress
- 27/27 RestoreFromEvents tests + new aggregates
- 46/46 RMs checkpointed
- Reconciliation pipeline fully on ProcessManager (old path decommissioned)

### Week 6: Consolidation + Sign-Off

```
ES Expert A     Architectural review of all Phase 2-3 output
                Verify: no new implicit sagas, all coordination via bus commands
                Sign-off on ProcessManager patterns

ES Expert B     FinancialModelService split complete
                Verify: all checkpoint + persistence coverage
                PostgreSQL operational review

Principal       Final integration review
                Verify: no regressions, feature flag cleanup
                Pipeline migration: old path decommission confirmed

Support 1       Final RestoreFromEvents verification (all pass)
                New aggregate test compliance check

Support 2       Documentation updates
                Vault rescan (VaultTool project-refresh)

Support 3       Performance baselining (Phase 4.5):
                - Startup time at 1K, 5K, 15K, 50K events
                - GetById latency for SFM at various stream sizes
                - Reconciliation defect rate comparison
                - RM replay invocation count (before/after)
```

**Week 6 Exit = Plan Complete:**
All Phase 0–4 deliverables met.

---

## Gantt Summary

```
         Week 1        Week 2        Week 3        Week 4        Week 5        Week 6
Expert A ██ P0.2 SFM  ████ P2.2     ███ P2.4      ████ P2.4     ███ P3.1      █ Review
         RestoreFrom  Journal Agg   Recon PM      Recon PM      PeriodClose
         Events       Design+Impl   Design        Implementation PM + ReconState

Expert B ████ P1.1    ████ P1.2/3/5 ██ P2.1+2.3   ████ P3.5     ███ P3.5      █ P4.4
         Checkpoint   9 RM migrate  AcctSys+CoA   SFM Extract   SFM Complete   FMS Split
         Store Integ  Two-tier+Cache               Dual-write    Verify

Princpl  █ P0.1       ██ P1.4       ███ P2.4b     ████ P2.4b    ███ P3.4      █ Final
         Source ctors  PM wiring     Pipeline      Pipeline      GL Reports     Review
         + reviews    + reviews     migration plan migration exec

Supp 1   ████ P0.3    ██ P1.2 help  ██ P2.3+P3.6  ██ P3.2+3.3   ████ P3.6     █ Verify
         RestoreFrom  RM migration  CoA+tests     Entry+Rules   Batch tests
         6 aggregates validation    batch start

Supp 2   █ CI setup   ██ P2.5       ████ P4.1     ████ P4.1+4.3 ██ P4.1       █ Docs
                      SA RM audit   RM checkpoint  RM batch+     RM complete    Vault
                                    batch          TmplRm audit  46/46          rescan

Supp 3   ██ Baseline  ██ Measure    ██ Integ      ████ Recon    ████ Full      ██ Final
         startup time checkpoint    tests          PM tests      regression    baselines
                      impact                       pipeline regr  + benchmarks
```

---

## Deliverable Tracking

### Phase 0 Deliverables (Week 1)

| # | Deliverable | Owner | Agent Role | Exit Criteria |
|---|-------------|-------|-----------|---------------|
| 0.1 | 4 source constructors fixed | Principal | Generate fixes | CI green |
| 0.2 | SFM RestoreFromEvents test | ES Expert A | Scaffold from Apply methods | Test passes; any Apply bugs documented |
| 0.3 | 5 growth aggregate RestoreFromEvents | Support 1 | Generate complete tests | All 5 pass |
| — | Startup time baseline | Support 3 | — | Measured at 1K, 5K, 15K events |

### Phase 1 Deliverables (Weeks 1–2)

| # | Deliverable | Owner | Agent Role | Exit Criteria |
|---|-------------|-------|-----------|---------------|
| 1.1 | ICheckpointStore in ReadModelBase | ES Expert B | Generate migration template | Optional param, null default, opt-in |
| 1.2 | 9 SFM RMs checkpointed | ES Expert B + Support 1 | Generate 9 migrations | >80% startup invocation reduction |
| 1.3 | Two-tier IReadModelStore | ES Expert B | Generate Dapper store config | InMemory (hot) + PostgreSQL (persisted) |
| 1.4 | ProcessManagerBase in solution | Principal | Namespace adaptation | Available as base class |
| 1.5 | CachingRepository for SFM | ES Expert B | — | GetById cached within session |

### Phase 2 Deliverables (Weeks 2–4)

| # | Deliverable | Owner | Agent Role | Exit Criteria |
|---|-------------|-------|-----------|---------------|
| 2.1 | AccountingSystem extended | ES Expert B | Scaffold commands + events | Entity type, industry, fiscal year, close policy |
| 2.2 | Journal aggregate | ES Expert A | Scaffold aggregate | PostEntry, ReverseEntry, CorrectEntry + RestoreFromEvents |
| 2.3 | CoA template engine | Support 1 | Generate template + seeding | ≥2 industry templates working |
| 2.4 | Reconciliation ProcessManager | ES Expert A | Scaffold state machine | 8 states, timeouts, compensation, RestoreFromEvents |
| 2.4b | Pipeline migration | Principal | — | Feature flag on, old pipeline as fallback |
| 2.5 | SpreadsheetAdapter RM audit | Support 2 | Catalog 40 RMs | Consolidation candidates identified |

### Phase 3 Deliverables (Weeks 4–5)

| # | Deliverable | Owner | Agent Role | Exit Criteria |
|---|-------------|-------|-----------|---------------|
| 3.1 | PeriodClose PM + ReconciliationState | ES Expert A | Scaffold aggregates | 5 states, SoftClose/HardClose/Reopen |
| 3.2 | Entry Patterns Library | Support 1 | Generate patterns | ≥6 accounting scenarios |
| 3.3 | Classification rules enhancement | Support 1 | Scaffold config model | Per-entity rules, 85/15 threshold |
| 3.4 | GL Report Generation | Principal | Generate RM projections | Trial balance, P&L, balance sheet |
| 3.5 | SFM table-mapping extraction | ES Expert B | — | 31→~11 events, 87→~67 Apply, dual-write verified |
| 3.6 | Remaining RestoreFromEvents (21) | Support 1 | Generate all 21 tests | 27/27 passing |

### Phase 4 Deliverables (Weeks 3–6, pulled forward)

| # | Deliverable | Owner | Agent Role | Exit Criteria |
|---|-------------|-------|-----------|---------------|
| 4.1 | All 46 RMs checkpointed | Support 2 | Generate 37 migrations | 46/46 checkpoint-based |
| 4.3 | ModelTemplateRm audit | Support 2 | Analyze 82 subscriptions | Recommendations documented |
| 4.4 | FinancialModelService split | ES Expert B | — | Core (~100 cmds) + TableMapping (~60 cmds) |
| 4.5 | Performance baselines | Support 3 | — | Startup <2s at 15K events; GetById <100ms at 5K |

---

## Success Metrics (Week 6 Targets)

| Metric | Before | Target | Measured By |
|--------|--------|--------|-------------|
| Calendar time to complete | — | ≤6 weeks | Calendar |
| Startup time (15K events) | Baseline (Week 1) | <2s | Support 3 benchmarks |
| SFM GetById replay | Full stream (3,557+ events) | <100 events (snapshot) | CachingRepository metrics |
| RestoreFromEvents coverage | 0/27 | 30/30 (27 existing + 3 new) | test-coverage.md |
| Source constructor coverage | 23/27 | 30/30 | aggregates.md |
| Explicit process managers | 0 | 2 (Reconciliation, PeriodClose) | saga-catalog.md |
| RM checkpoint coverage | 0/46 | 46/46 | read-models.md |
| SFM registered events | 31 | ~11 | god-aggregate.md |
| SFM Apply methods | 87 | ~67 | scan-data.json |
| SFM lines | 3,557 | ~1,500-2,000 | scan-data.json |
| FinancialModelService commands | 166 | ~80-90 | message-map.md |
| Reconciliation defect rate | 29% | <10% | defect-analysis.md |
| SpreadsheetAdapter defect rate | 15% | <10% | defect-analysis.md |
| Implicit sagas | 7 | ≤4 | saga-catalog.md |
| Accountant workflow epics | 0/5 | 5/5 | GitHub issues |
| Missing aggregates | 3 | 0 | scan-data.json |

---

## Risk Factors That Could Extend to 7–8 Weeks

| Risk | Likelihood | Impact | Trigger | Mitigation |
|------|-----------|--------|---------|------------|
| SFM RestoreFromEvents reveals >5 Apply bugs | HIGH | +1 week | Week 1 test results | EventVersioning + VersionedEventSerializer ready. ES Expert A handles upcaster design. |
| PostgreSQL ops setup delays | MEDIUM | +3–5 days | Week 2 checkpoint deployment | `InMemoryCheckpointStore` as fallback; PostgreSQL deferred to Week 3. |
| Journal aggregate design iteration | LOW (expert team) | +1 week | Week 2 design review | ES Expert A has built event-sourced ledgers before. Start minimal (debit/credit/amount/date), extend via Entry Patterns. |
| Reconciliation pipeline migration regression | MEDIUM | +3–5 days | Week 4 feature flag activation | Old pipeline as fallback. Support 3 runs regression suite before flag-on. |
| SFM extraction breaks RM subscriptions | MEDIUM | +3–5 days | Week 4–5 dual-write phase | Principal knows all 9 RM subscriptions. Migrate one RM at a time. Full RestoreFromEvents re-verification. |

---

## The Acceleration Formula

```
Traditional (1 developer):     15–21 weeks
  → Limited by: sequential phases, learning curves, mechanical work

Agents only (no team context):  7–10 weeks
  → Limited by: human review cycles, design decisions waiting for expertise

This team + agents:             5–6 weeks
  → Limited by: sequential dependencies only (Journal before GL reports,
    CheckpointStore before RM migrations, ProcessManagerBase before PM implementation)
```

**What the agents eliminate:** All mechanical work — test generation, RM migrations, aggregate scaffolding, source constructor fixes, documentation, vault rescans.

**What the experts eliminate:** Design decision delays — Journal aggregate design, ProcessManager state machines, SFM extraction strategy, PostgreSQL topology, pipeline migration mapping.

**What the principal eliminates:** Review bottlenecks — knows every file, every RM subscription, every handler route. Agent output reviewed in minutes, not hours.

**What remains on the critical path:** The irreducible sequence of dependencies where each phase's output gates the next phase's input. With this team, those gates open in days, not weeks.
