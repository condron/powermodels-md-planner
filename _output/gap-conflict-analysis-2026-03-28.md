# Gap & Conflict Analysis: PMA Master Plan vs Research Documents

Systematic comparison of the PMA master refactoring plan against the strategic options analysis, mitigation plan, accelerated mitigation plan, and unified going-forward plan — identifying overlaps, conflicts, gaps, and supersessions as input for a unified roadmap.

**Date:** 2026-03-28

---

## Input Documents

| Document | Date | Scope |
|----------|------|-------|
| **PMA Master Refactoring Plan** | 2026-03-28 | WPF isolation, pipeline factoring, Lisa-aligned features (Phases 0–3, 28–43.5 days) |
| **Strategic Options Analysis** | 2026-03-08 | Option A/B/C evaluation, investor lens, competitive moats, capital strategy |
| **Mitigation Plan** | 2026-03-08 | Technical debt: checkpoints, process managers, domain foundation, SFM extraction (Phases 0–4, 15–21 weeks solo) |
| **Accelerated Mitigation Plan** | 2026-03-08 | 6-week team compression with agent-assisted parallel execution |
| **Unified Going-Forward Plan** | 2026-03-19 | Merged mitigation + WPF MVP + trial balance P0; RD source findings |
| **Monday Demo Sprint Plan** | 2026-03-28 | Tactical sprint: P&L/CF monthly breakdown, fiscal year selector, Excel + PDF export ([monday-demo-plan-2026-03-28.md](monday-demo-plan-2026-03-28.md)) |
| **rd-agentic-flow repo** | 2026-03-28 | External R-D agentic tooling repo (`C:\Users\chris\source\repos\condron\rd-agentic-flow`) — contains two complete agent systems (samples/ + starter/), recipe-grade R-D skills, process manager instructions, read model review rules, and full feature-builder orchestration pipeline |

---

## Current Repo Status (Source Scan, 2026-03-28)

Status determined by scanning the implementation vault source code, not from prior plans.

### Already Implemented

| Item | Evidence | Mitigation Plan Assumed |
|------|----------|------------------------|
| **Journal aggregate** | `Journal.cs` (68 lines) — JournalCreated, AddEntry, RemoveEntry | Phase 2.2: "Create Journal Aggregate" — **DONE** |
| **JournalEntry aggregate** | `JournalEntry.cs` (290 lines) — balanced debits/credits, 5 categorization types (Default, Rule, ML, LLM, Human), recategorization, transaction association | Phase 2.2 dependency — **DONE, exceeds spec** |
| **Trial Balance report** | `TrialBalanceRm.cs` + unit tests + integration tests + FlaUI tests | Phase 3.4 partial — **DONE** |
| **Income Statement report** | `IncomeStatementRm.cs` + integration tests + FlaUI tests | Phase 3.4 partial — **DONE** |
| **Balance Sheet report** | `BalanceSheetRm.cs` + integration tests + FlaUI tests | Phase 3.4 partial — **DONE** |
| **Cash Flow Statement** | `CashFlowStatementRm.cs` | Not in mitigation plan — **BONUS** |
| **General Ledger report** | `GeneralLedgerRm.cs` | Not in mitigation plan — **BONUS** |
| **Income/Expense Summary** | `IncomeExpenseSummaryRm.cs` | Not in mitigation plan — **BONUS** |
| **Journal Report** | `JournalReportRm.cs` | Not in mitigation plan — **BONUS** |
| **Reports framework** | `AccountingReportBase.cs`, `ReportBuilder.cs`, `IAccountingReport.cs`, `AccountingReportsContext.cs` | Not in mitigation plan — **BONUS** |
| **Data ingestion pipeline** | `DataIngestionPipeline.cs` + 8 steps (CSV, ColumnMapping, PDF, TransactionNormalizer, ClassificationRule, AIClassification, AddOrUpdateDataSource, JournalPosting) | Assumed existing — correct |
| **29 aggregates** | All original aggregates present including all accounting system entities | Assumed 27 — actually 29 (Journal, JournalEntry added) |
| **~44 ReadModelBase RMs** | 12 in ModelServer, ~30 in SpreadsheetAdapter, 1 in TeamModelMgmt, 1 in UIBehavior | Assumed 46 — close enough |

### Not Implemented

| Item | Status | Which Plan References It |
|------|--------|--------------------------|
| ICheckpointStore / RM checkpoints | **Code exists in `process-manager/read-model-caching-reference/`** — `ICheckpointStore`, `PostgresCheckpointStore` (Dapper/Npgsql), `InMemoryCheckpointStore`, `IReadModelStore<TKey,TModel>`, `DapperReadModelStore`, `InMemoryReadModelStore`, `ProjectionSchema`. **Not wired into PowerModels** — 0/44 RMs checkpointed. Integration effort, not build. | Mitigation 1.1–1.3 |
| ProcessManager / ProcessAggregateBase | **Code exists in `process-manager/Infrastructure/`** — `ProcessAggregateBase<TState>` (state machine DSL, command accumulation, step timeouts), `ProcessManagerBase<TProcessAgg,TState>` (NullBus replay safety, LoadOrCreate, SaveAndTakeCommands, GoLive). Also: `CategoryStreamWarmup`, `EventVersioning`, `VersionedEventSerializer`. **Not wired into PowerModels.** Integration effort, not build. | Mitigation 1.4, 2.4 |
| CachingRepository for SFM | **Not in source** — exists in ReactiveDomain but not wired | Mitigation 1.5 |
| ClassificationRuleSet aggregate | **Not in source** — rules are ephemeral POCOs from `ChartOfAccountsRm.GenerateClassificationRules()` | PMA Phase 1 Change B |
| Excel export (ClosedXML) | **Not in source** — no ClosedXML dependency | PMA Phase 1 Change D |
| Fiscal year configuration | **Not in source** — no FiscalYear in any aggregate | Mitigation 2.1, PMA Phase 3 |
| ReconciliationState / PeriodClose | **Not in source** | Mitigation 3.1 |
| Entry Patterns Library | **Not in source** | Mitigation 3.2 |
| AccountingSystem extensions | **Minimal** — 29 lines, only `AccountingSystemCreated` event | Mitigation 2.1 |
| Industry CoA templates | **Not in source** | Mitigation 2.3 |
| SFM table-mapping extraction | **Not done** — SFM still god aggregate (142 refs in FinancialModelService) | Mitigation 3.5 |
| FinancialModelService split | **Not done** — still monolithic | Mitigation 4.4 |

---

## Item-by-Item Intersection

### Legend

| Symbol | Meaning |
|--------|---------|
| ✅ DONE | Already implemented in repo |
| 🔀 SUPERSEDED | PMA plan replaces mitigation item with a better approach |
| ⚡ CONFLICT | Plans disagree on approach or sequencing |
| 🕳️ GAP | Item in one plan but absent from the other |
| ➡️ INHERITED | PMA plan needs this but doesn't address it |

---

### Mitigation Phase 0: Safety Net

| Mitigation Item | PMA Plan | Status | Verdict |
|-----------------|----------|--------|---------|
| 0.1 SFM RestoreFromEvents test | Not referenced | N/A | **DROPPED** — excluded by decision |
| 0.2 Fix 4 missing source constructors | Not referenced | Unknown | 🕳️ **GAP** — trivial fix, PMA plan silent on it |
| 0.3 Growth aggregate RestoreFromEvents | Not referenced | N/A | **DROPPED** — excluded by decision |

### Mitigation Phase 1: Infrastructure Integration

| Mitigation Item | PMA Plan | Status | Verdict |
|-----------------|----------|--------|---------|
| 1.1 ICheckpointStore into ReadModelBase | Not referenced | Not implemented | 🕳️ **GAP** — PMA plan adds new RMs (DiagnosticRm, ExceptionReportRm, etc.) without addressing checkpoint infrastructure |
| 1.2 Checkpoint 9 SFM RMs | Not referenced | Not implemented | 🕳️ **GAP** — performance infrastructure, orthogonal to PMA |
| 1.3 IReadModelStore two-tier persistence | Not referenced | Not implemented | 🕳️ **GAP** — persistence infrastructure |
| 1.4 ProcessManagerBase integration | Phase 2 (observable pipeline may need PM patterns) | Not implemented | ➡️ **INHERITED** — PMA Phase 2 may depend on this but doesn't address it |
| 1.5 CachingRepository for SFM | Not referenced | Not implemented | 🕳️ **GAP** — performance optimization |

### Mitigation Phase 2: Domain Foundation

| Mitigation Item | PMA Plan | Status | Verdict |
|-----------------|----------|--------|---------|
| 2.1 Extend AccountingSystem (entity type, industry, fiscal year, close policy) | Phase 3 addresses fiscal year only | Minimal (29 lines, 1 event) | ⚡ **CONFLICT** — mitigation wants 4 extensions, PMA Phase 3 only adds fiscal year. Entity type, industry, close policy not in PMA. |
| 2.2 Create Journal Aggregate | Not referenced (already exists) | ✅ **DONE** — Journal + JournalEntry both implemented | ✅ Both plans satisfied |
| 2.3 Industry CoA Template Engine | Not referenced | Not implemented | 🕳️ **GAP** — PMA plan silent on CoA templates |
| 2.4 Reconciliation ProcessManager | Not referenced | Not implemented | 🕳️ **GAP** — different concern from PMA Phase 2's import pipeline observability |
| 2.5 SpreadsheetAdapter RM Audit | Phase 0 partially (pipeline factoring changes RM locations) | Not done | ⚡ **CONFLICT** — PMA Phase 0 restructures SA by factoring out pipeline, which changes where RMs live. Mitigation assumes current layout. PMA Phase 0 should run first. |

### Mitigation Phase 3: Feature Buildout

| Mitigation Item | PMA Plan | Status | Verdict |
|-----------------|----------|--------|---------|
| 3.1 ReconciliationState + PeriodClose aggregates | Not referenced | Not implemented | 🕳️ **GAP** — bank reconciliation and period close not in PMA |
| 3.2 Entry Patterns Library | Not referenced | Not implemented | 🕳️ **GAP** — not in PMA |
| 3.3 Classification Rules Enhancement | Phase 1 Change B (full aggregate rewrite) | Not implemented | 🔀 **SUPERSEDED** — PMA Change B goes far beyond mitigation 3.3: event-sourced ClassificationRuleSet aggregate with enhanced rule types (AmountRangeMatch, CompositeMatch, LiabilityCreditGuardrail) vs mitigation's "per-entity configurable rules" |
| 3.4 GL Report Generation (TB, IS, BS) | Phase 1 adds 2 reports, Phase 2 adds 2 more | ✅ **DONE** — TB + IS + BS + CashFlow + GL + IncomeExpense + JournalReport all exist | ✅ Mitigation item complete. PMA adds 4 new report types on top. |
| 3.5 SFM Table-Mapping Extraction | Not referenced | Not done | 🕳️ **GAP** — god aggregate reduction not in PMA |
| 3.6 Remaining RestoreFromEvents | Not referenced | N/A | **DROPPED** |

### Mitigation Phase 4: Consolidation

| Mitigation Item | PMA Plan | Status | Verdict |
|-----------------|----------|--------|---------|
| 4.1 Checkpoint all remaining RMs | Not referenced | 0/44 done | 🕳️ **GAP** |
| 4.2 Business Setup ProcessManager | Not referenced | Not implemented | 🕳️ **GAP** |
| 4.3 ModelTemplateRm audit (82 subscriptions) | Not referenced | Not done | 🕳️ **GAP** |
| 4.4 Split FinancialModelService | Not referenced | Not done | 🕳️ **GAP** |
| 4.5 Measure and baseline | Not referenced | Not done | 🕳️ **GAP** |

---

### PMA Plan Items Not In Mitigation

| PMA Item | Mitigation Plan | Verdict |
|----------|----------------|---------|
| **Phase 0: WPF isolation + pipeline factoring** | Not addressed at all | 🕳️ **GAP** — entirely new scope |
| **Phase 1 Change B: ClassificationRuleSet aggregate** | Mitigation 3.3 is weaker version | 🔀 PMA supersedes |
| **Phase 1 Change C: Classification Review Workflow** | Not addressed | 🕳️ **GAP** — review gate between classification and posting |
| **Phase 1 Change D: Excel Export** | Not addressed | 🕳️ **GAP** — ClosedXML report export |
| **Phase 1: Enhanced rule types** (AmountRange, Composite, LiabilityCreditGuardrail) | Not addressed | 🕳️ **GAP** |
| **Phase 1: DiagnosticRm + ExceptionReportRm** | Not addressed | 🕳️ **GAP** — new reports |
| **Phase 1.5: Transaction Splitting** | Not addressed | 🕳️ **GAP** — GST/tax decomposition |
| **Phase 2: StatementImport aggregate** | Not addressed | 🕳️ **GAP** — import pipeline observability |
| **Phase 2: RunSummaryRm + ReconciliationReportRm** | Not addressed | 🕳️ **GAP** — new reports |
| **Phase 3: IncomeStatementRm fiscal year matrix** | Mitigation 2.1 includes fiscal year | ⚡ **PARTIAL OVERLAP** |

---

## Conflict Detail

### Conflict 1: AccountingSystem Extension Scope

- **Mitigation 2.1** wants 4 extensions: entity type, industry, fiscal year, close policy
- **PMA Phase 3** adds only `FiscalYearStartMonth`
- **Resolution needed:** Should the unified roadmap include entity type, industry, and close policy? Or are those deferred beyond PMA scope?

### Conflict 2: SpreadsheetAdapter RM Layout

- **Mitigation 2.5** audits SA's 30 RMs assuming current project structure
- **PMA Phase 0** restructures SA by factoring pipeline into `DataIngestion` and duplicating UI-facing types
- **Resolution:** PMA Phase 0 must run before any SA RM audit. The audit should target the post-isolation layout.

### Conflict 3: Report Data Source

- **Mitigation 3.4** assumed GL reports would come from a new Journal aggregate's events
- **Reality:** Reports already exist and project from `JournalEntry` aggregate events (which already exist with balanced debits/credits and categorization)
- **Resolution:** No conflict — the repo has already resolved this by building Journal + JournalEntry + reports together.

### Conflict 4: Classification Rules Approach

- **Mitigation 3.3** proposes per-entity configurable rules with 85/15 threshold
- **PMA Phase 1 Change B** proposes full event-sourced `ClassificationRuleSet` aggregate with seeding from COA, user editability, audit trail, enhanced match types
- **Resolution:** PMA supersedes. Mitigation 3.3 is subsumed.

---

## Gap Summary

### Infrastructure Gaps (Mitigation items PMA inherits but doesn't address)

These items improve system reliability and performance. PMA adds new features on top of the current infrastructure without improving it.

| Gap | Impact on PMA | Priority for Unified Roadmap |
|-----|---------------|------------------------------|
| **RM checkpoints** (Mitigation 1.1–1.3, 4.1) | PMA adds 4–6 new RMs without checkpoints. Every new RM increases startup cost. | HIGH — should precede or parallel PMA feature work |
| **ProcessManager infrastructure** (Mitigation 1.4) | PMA Phase 2 (observable pipeline) may need PM patterns for reliable coordination | MEDIUM — needed before PMA Phase 2 |
| **CachingRepository for SFM** (Mitigation 1.5) | Independent of PMA. SFM remains hot. | MEDIUM — pure performance |
| **Source constructor fixes** (Mitigation 0.2) | Trivial (4 fixes). No PMA dependency. | LOW — opportunistic |

### Domain Gaps (Mitigation items with no PMA equivalent)

| Gap | Business Value | Priority for Unified Roadmap |
|-----|---------------|------------------------------|
| **AccountingSystem extensions** (entity type, industry, close policy — beyond fiscal year) | Needed for multi-entity and period close workflows | MEDIUM — not needed for Lisa demo alignment |
| **Industry CoA Template Engine** (Mitigation 2.3) | Accelerates onboarding for new businesses | LOW — not in Lisa demo, not in PMA |
| **Reconciliation ProcessManager** (Mitigation 2.4) | Addresses #1 defect generator (29% of defects) | HIGH — structural fix for reliability |
| **ReconciliationState + PeriodClose** (Mitigation 3.1) | Required for month-end close workflow | MEDIUM — needed for full accountant workflow |
| **Entry Patterns Library** (Mitigation 3.2) | Reduces manual entry effort | LOW — nice-to-have |
| **SFM Extraction** (Mitigation 3.5) | Reduces god aggregate from 31→~11 events | MEDIUM — reduces defect risk |
| **FinancialModelService Split** (Mitigation 4.4) | Reduces service complexity from 166→~100 commands | MEDIUM — maintainability |
| **ModelTemplateRm Audit** (Mitigation 4.3) | 82 event subscriptions — possible over-subscription | LOW — investigation |
| **Business Setup PM** (Mitigation 4.2) | 3 defects from implicit saga | LOW — lower priority than reconciliation PM |

### Feature Gaps (PMA items with no mitigation equivalent)

| Gap | Notes |
|-----|-------|
| **WPF isolation** (PMA Phase 0) | Entirely new — enables WPF and headless API to share domain code |
| **Excel export** (PMA Phase 1 Change D) | New capability for Lisa-aligned workflow |
| **Classification review workflow** (PMA Phase 1 Change C) | Two-phase import with review gate |
| **Enhanced rule types** (PMA Phase 1) | AmountRangeMatch, CompositeMatch, LiabilityCreditGuardrail |
| **Transaction splitting** (PMA Phase 1.5) | GST/tax decomposition |
| **StatementImport aggregate** (PMA Phase 2) | Import pipeline observability |
| **4 new reports** (PMA Phases 1–2) | Diagnostic, Exception, RunSummary, Reconciliation |

---

## Strategic Narrative Impact

The Strategic Options Analysis (2026-03-08) recommended Option A ("Fix What We Have") with a 5–6 week timeline to April 2026 cloud launch. The key investor arguments were:

1. **Time to market: 6–8 weeks** — Mitigation + cloud launch
2. **3 design partners waiting** for working demo
3. **Capital efficiency** — $500K angel round

### How the PMA Plan Affects the Narrative

| Strategic Claim | Mitigation Plan Alone | With PMA Plan Added | Impact |
|-----------------|----------------------|---------------------|--------|
| Time to market | 5–6 weeks (accelerated) | +28–43.5 days for PMA features | ⚠️ **Extends timeline** — unless PMA work is parallelized with remaining mitigation items |
| Demo readiness | Fixes defects, adds journal | Adds classification rules, review workflow, Excel export, new reports | ✅ **Strengthens demo** — Lisa-aligned features make the demo far more compelling |
| Capital efficiency | $500K covers mitigation + launch | Same $500K must also cover PMA features | ⚠️ **Scope risk** — more work for same capital unless prioritized |
| Competitive moat | ES architecture + domain model | + Classification engine, review workflow, report suite | ✅ **Deepens moat** — harder to replicate |
| 85/15 boundary | AI classification exists | + Rule engine + review gate + confidence tiers | ✅ **Demonstrates the boundary** — investors can see where AI stops and human judgment begins |

### Net Assessment

The PMA plan **strengthens the investor narrative** by adding demonstrable accounting workflow features that make the 85/15 boundary visible and concrete. However, it **extends the timeline** unless the unified roadmap can parallelize infrastructure work (checkpoints, PMs) with feature work (classification rules, review, export).

The key question for the unified roadmap: **Which items are demo-critical (must ship for design partner validation) vs infrastructure-critical (must ship for reliability) vs nice-to-have?**

---

## Recommendations for Unified Roadmap

### 1. Sequence PMA Phase 0 First

WPF isolation + pipeline factoring changes project structure. All other work (mitigation RM checkpoints, PMA features) should target the post-isolation layout. Running Phase 0 first avoids rework.

### 2. Parallelize Infrastructure and Features

| Track A: Infrastructure (from mitigation) | Track B: Features (from PMA) |
|-------------------------------------------|------------------------------|
| RM checkpoint infrastructure (1.1) | Classification rules aggregate (Change B) |
| SFM RM checkpoints (1.2) | Classification review workflow (Change C) |
| ProcessManager wiring (1.4) | Excel export (Change D) |
| CachingRepository (1.5) | New reports (Diagnostic, Exception) |

These tracks have minimal dependencies between them.

### 3. Resolve the AccountingSystem Scope Question

Decide whether entity type, industry, and close policy (mitigation 2.1) ship with fiscal year (PMA Phase 3) or are deferred.

### 4. Defer Low-Priority Mitigation Items

The following can be deferred past the initial unified roadmap without blocking demo or reliability:
- Entry Patterns Library (3.2)
- Industry CoA Templates (2.3)
- Business Setup PM (4.2)
- ModelTemplateRm Audit (4.3)
- FinancialModelService Split (4.4) — can happen after SFM extraction

### 5. Keep Reconciliation PM High Priority

The reconciliation pipeline's 29% defect rate is the #1 reliability problem. The Reconciliation ProcessManager (mitigation 2.4) should be in the unified roadmap even though it's not in PMA, because:
- PMA Phase 2 (observable pipeline) operates in the same area
- Both touch `JournalService` and the data ingestion pipeline
- They can share the ProcessManager infrastructure

### 6. SFM Extraction Is Independent

God aggregate reduction (mitigation 3.5) can run on its own track at any point. It doesn't depend on PMA work or vice versa. Schedule it based on team availability.

---

## Cross-Reference Matrix

| Work Item | Mitigation | PMA | Status | Unified Priority |
|-----------|-----------|-----|--------|-----------------|
| WPF isolation + pipeline factoring | — | Phase 0 | Not started | **P0** — do first |
| RM checkpoint infrastructure | 1.1–1.3 | — | Not started | **P1** — parallel with features |
| ProcessManager infrastructure | 1.4 | Phase 2 dep | Not started | **P1** — before Phase 2 |
| CachingRepository for SFM | 1.5 | — | Not started | **P2** — performance |
| ClassificationRuleSet aggregate | 3.3 (weak) | Phase 1 Change B | Not started | **P1** — demo-critical |
| Classification review workflow | — | Phase 1 Change C | Not started | **P1** — demo-critical |
| Excel export | — | Phase 1 Change D | Not started | **P1** — demo-critical |
| New reports (Diagnostic, Exception) | — | Phase 1 | Not started | **P1** — demo value |
| Transaction splitting | — | Phase 1.5 | Not started | **P2** — nice-to-have |
| Reconciliation ProcessManager | 2.4 | — | Not started | **P1** — reliability-critical |
| AccountingSystem extensions | 2.1 | Phase 3 (partial) | Minimal | **P2** — needed for close workflow |
| Observable pipeline / StatementImport | — | Phase 2 | Not started | **P2** — depends on PM infra |
| Fiscal year config | 2.1 (partial) | Phase 3 | Not started | **P2** |
| SFM extraction (20 events) | 3.5 | — | Not started | **P2** — independent track |
| ReconciliationState + PeriodClose | 3.1 | — | Not started | **P2** — needed for close |
| FinancialModelService split | 4.4 | — | Not started | **P3** — after SFM extraction |
| Entry Patterns Library | 3.2 | — | Not started | **P3** — deferred |
| Industry CoA Templates | 2.3 | — | Not started | **P3** — deferred |
| Journal aggregate | 2.2 | — | ✅ DONE | — |
| GL Reports (TB, IS, BS) | 3.4 | — | ✅ DONE | — |
| 4 bonus reports (CashFlow, GL, IncExp, JournalRpt) | — | — | ✅ DONE | — |

---

---

## Part 2: Strategic Reframe Through Business Imperatives

The gap analysis above compares plans mechanically. This section reframes everything through three business imperatives:

1. **Total delivery speed to production-ready MVP** — fastest clean path, no hacking
2. **Demonstrating execution ability to non-technical stakeholders** — visible progress
3. **Betting the farm on agentic acceleration** — tooling and environment optimized for agent-delivered features

---

### Existing Agentic Acceleration Surface (Repo Scan)

The codebase already has substantial infrastructure for agent-driven development that neither the mitigation plan nor PMA plan acknowledged as strategic assets:

| Asset | What It Does | Agent Leverage |
|-------|-------------|----------------|
| **SeedTestBusiness CLI** | Creates reproducible business states from JSON datasets (adventure-services: 30 accounts, 615 journal entries, 30 counterparties) | Agents can test their work against known-good state |
| **UITestRunner + FlaUI** | Single-command end-to-end verification: seed → launch → navigate → extract → validate | Agent output is machine-verifiable — no human in the loop |
| **DataValidator** | Cross-layer assertion: seed data → event store → RM → report → UI totals match | Agents get pass/fail feedback on numerical correctness |
| **11 PowerModels Skills** | `accounting-reports` (490 lines, recipe-grade), `reactive-domain-es-ddd` (212 lines, pattern reference), `csharp-xunit-reactive-domain` (196 lines), `reactive-domain-process-manager` (92 lines, marked FUTURE), `business-loader`, `csharp-developer`, `dotnet-cli-reference`, `md-planner-decomposition`, `proposal-reading`, `skill-creator`, `test-review` | Agents have pattern knowledge for ES-DDD aggregates, testing, reports, seed data, and process managers |
| **Feature Builder agent** | Full workflow coordinator: GitHub Issue → branch → plan → architecture review → implement → 4-way review → merge | Agent-to-agent coordination without human orchestration |
| **16 PowerModels agents** | Specialized agents for planning, implementation, review, testing, architecture | Parallel agent work on independent tasks |
| **rd-agentic-flow: samples/ (Greylock variant)** | 8 agents (domain-builder, implementer, architecture/correctness/implementation-reviewer, diagram-designer, doc-orchestrator, tests) + 6 skills (greylock-aggregate **recipe-grade**, greylock-testing, greylock-csharp, doc-orchestration 5-file suite, excalidraw-diagram, mermaid-diagram) + 5 path-specific review instructions (aggregate, events, process, readmodel, tests) | **Recipe-grade ES-DDD skill with full checklist, file paths, anti-patterns.** Doc orchestration + diagram skills. Path-based code review rules for Copilot. |
| **rd-agentic-flow: starter/ (PowerModels-compatible)** | 15 agents (feature-builder, proposal-decomposer, work-decomposer, planner, plan-architect, implementer, dependency-tracker, agent-tester, test-refactor, test-reviewer, 4 parallel reviewers: architecture/correctness/code-quality/security) + 9 skills (reactive-domain-es-ddd, csharp-xunit-reactive-domain, reactive-domain-process-manager, csharp-developer, dotnet-cli-reference, md-planner-decomposition, proposal-reading, skill-creator, test-review) + CLAUDE.md project guide | **Full orchestration pipeline:** issue → dependency check → plan → architecture review → implement → acceptance checklist → 4-way parallel review → fix → smoke test → format → commit → rebase → merge. Epic branch strategy with Draft PRs. |
| **rd-agentic-flow: infrastructure code** | `process-manager/Infrastructure/` — ProcessAggregateBase, ProcessManagerBase, CategoryStreamWarmup, EventVersioning. `process-manager/read-model-caching-reference/` — ICheckpointStore, PostgresCheckpointStore, DapperReadModelStore, IReadModelStore, InMemoryReadModelStore, ProjectionSchema | Ready-to-integrate R-D infrastructure for checkpointing and process managers |
| **Reports framework** | AccountingReportBase + ReportBuilder DSL + 7 working reports + integration tests | Adding a report is mechanical — the framework does the heavy lifting |
| **JournalEntry categorization** | Already tracks Rule/ML/LLM/Human/Default with ruleId + ruleVersion | Audit trail for classification rules exists without new aggregate work |

### The Fundamental Insight

**The refactoring IS the speed investment.** Clean boundaries don't just satisfy architecture aesthetics — they reduce the context window an agent needs to do useful work. Every coupling point that requires an agent to understand SpreadsheetAdapter's 30 RMs to add a pipeline step is a velocity tax.

The question isn't "refactor OR ship features." It's "what's the minimum refactoring that maximizes agent throughput on features?"

---

### What's Missing for Full Agentic Acceleration

| Gap | Impact | Fix |
|-----|--------|-----|
| **ES-DDD skill exists but lacks recipe format — SOLVED in rd-agentic-flow** | PowerModels' `reactive-domain-es-ddd` (212 lines) is pattern-reference only. **But** `rd-agentic-flow/samples/.github/skills/greylock-aggregate/SKILL.md` IS recipe-grade with full checklist, file paths, anti-patterns, and the Greylock variant uses `:this()` + `Source = source` + `IRepository` + `IReadModelStore`. The `starter/` variant uses PowerModels patterns (`:base(source)` + `ICorrelatedRepository` + `QueuedSubscriber`). | **Adapt** greylock-aggregate recipe to PowerModels conventions (or use starter/ variant directly). Key delta: PowerModels uses `:base(source)`, `ICorrelatedRepository`, `TrackingMetadata`, `QueuedSubscriber`. |
| **No "pipeline step" SKILL.md** | Adding CreateImportReviewStep has no codified pattern. No skill exists for this area at all. | Write a SKILL.md covering IDataIngestionStep, context extensions, step registration, pipeline builder tests |
| **Testing skill has stale RestoreFromEvents requirement** | Both PowerModels and rd-agentic-flow testing skills list RestoreFromEvents as "CRITICAL" and "REQUIRED" — agents following either skill will write RestoreFromEvents tests for every aggregate, conflicting with dropped requirement | Update testing skills in both repos: keep RestoreFromEvents as optional/recommended, remove CRITICAL/REQUIRED labels |
| **SA coupling defeats agent isolation** | Agent working on classification rules needs to understand SA dependency graph | WPF isolation (PMA Phase 0) creates clean project boundaries |
| **No classification rules seed data** | Agent can't test ClassificationRuleSet against known-good rule application | Extend adventure-services dataset with classification rules JSON |
| **Feature specs aren't agent-consumable** | Prose requirements require human interpretation | Feature definitions should include: commands, events, seed data, expected output, verification steps |
| **No "Excel export" SKILL.md** | ClosedXML integration pattern not codified | Write a SKILL.md after first export is built, then agents replicate for all reports |

### Reframed Priority: Runway Before Features

Instead of "mitigation phases" and "PMA phases," organize by **what enables what**:

#### Layer 0: Build the Runway (enables everything below)

| Work Item | Why It's Runway | Agent Impact |
|-----------|----------------|--------------|
| **WPF isolation + pipeline factoring** | Clean project boundaries | Agents work on domain/pipeline features without SA coupling context |
| **Domain aggregate SKILL.md** | Codified pattern for agent consumption | Agents build aggregates from spec — ClassificationRuleSet, StatementImport, etc. |
| **Pipeline step SKILL.md** | Codified pattern for agent consumption | Agents add pipeline steps (review gate, splitting, etc.) |
| **Seed data extensions** | Classification rules, import review scenarios | Agents can test every new feature against seed data |
| **Feature definition template** | Commands + events + seed data + expected output + verification | Human writes spec, agent delivers implementation |

#### Layer 1: Fly the Plane (agent-delivered features)

Once the runway exists, these features become agent-deliverable via existing tooling:

| Feature | Delivery Method | Verification |
|---------|----------------|-------------|
| **New reports** (Diagnostic, Exception, RunSummary, Reconciliation) | Agent follows accounting-reports SKILL.md | UITestRunner extracts + validates |
| **ClassificationRuleSet aggregate** | Agent follows new domain-aggregate SKILL.md | Unit tests + seed data verification |
| **Classification review workflow** | Agent follows new pipeline-step SKILL.md | Integration test with seed data |
| **Excel export** | Agent builds first one, then SKILL.md for replication | DataValidator compares export to report output |
| **Enhanced rule types** | Agent extends ClassificationRuleSet per spec | Seed data with rule scenarios + unit tests |
| **Transaction splitting** | Agent adds pipeline step per SKILL.md | Balanced-entry validation in DataValidator |

#### Layer 2: Invisible Infrastructure (enables reliability without feature agents knowing)

| Work Item | Why Invisible | Timing |
|-----------|--------------|--------|
| **RM checkpoints** | **Code already exists** in `process-manager/read-model-caching-reference/` — `ICheckpointStore`, `PostgresCheckpointStore`, `DapperReadModelStore`, `InMemoryReadModelStore`. Integration, not build. Feature agents adding new reports don't need to know — base class handles it. | Parallel with Layer 0, before Layer 1 adds new RMs |
| **Process Manager infrastructure** | **Code already exists** in `process-manager/Infrastructure/` — `ProcessAggregateBase<TState>`, `ProcessManagerBase<TProcessAgg,TState>`, `CategoryStreamWarmup`, `EventVersioning`. Integration, not build. Enables Reconciliation PM (fixes 29% defect rate). | Parallel with Layer 0 |
| **CachingRepository for SFM** | Performance — prevents embarrassing startup delays | Before demo |

---

### Speed Optimization: Critical Path Analysis

**What blocks the demo?**

```
                    ┌─────────────────────────────────────────┐
                    │ Non-technical stakeholder sees:          │
                    │  • App launches fast                     │
                    │  • Upload statement → classified → posted│
                    │  • Reports render with real numbers      │
                    │  • Export to Excel                       │
                    │  • "Here's what the AI did vs human"     │
                    └─────────────┬───────────────────────────┘
                                  │ requires
                    ┌─────────────┴───────────────────────────┐
                    │ Demo-critical features:                   │
                    │  • Classification rules (visible AI/human│
                    │    boundary)                              │
                    │  • Review workflow (human approves AI)    │
                    │  • Excel export (accountant deliverable)  │
                    │  • 7 existing reports + new reports       │
                    └─────────────┬───────────────────────────┘
                                  │ requires
                    ┌─────────────┴───────────────────────────┐
                    │ Runway (minimum viable):                  │
                    │  • Clean project boundaries (Phase 0)    │
                    │  • Domain aggregate pattern (SKILL.md)   │
                    │  • Seed data for rules + review          │
                    └─────────────┬───────────────────────────┘
                                  │ parallel with
                    ┌─────────────┴───────────────────────────┐
                    │ Invisible infrastructure:                 │
                    │  • RM checkpoints (fast startup)         │
                    │  • Recon PM (no crashes during demo)     │
                    └─────────────────────────────────────────┘
```

**What demonstrates execution to stakeholders?**

Non-technical stakeholders can't see checkpoints or aggregate boundaries. They see:
- **Features shipping weekly** — visible in demo, release notes, GitHub milestones
- **End-to-end verification videos** — UITestRunner produces screenshots and structured output
- **Before/after comparisons** — "Last month 0 reports, this month 7 reports + Excel export"
- **The 85/15 boundary** — "Here's what the AI classified, here's what a human reviewed"

The agentic tooling directly serves execution visibility: agent delivers feature → UITestRunner verifies → screenshot proves it works → stakeholder sees progress. This is a continuous demo machine.

**What's the fastest clean path?**

| Week | Runway Track | Feature Track (agent-delivered) | Infrastructure Track | Stakeholder-Visible Output |
|------|-------------|-------------------------------|---------------------|---------------------------|
| 1 | WPF isolation + pipeline factoring | — | Integrate checkpoint + PM code from `process-manager/` | "Restructured for headless API" |
| 2 | SKILL.md files + seed data extensions | New reports (4) via accounting-reports SKILL | Wire checkpoints to 9 SFM RMs + Reconciliation PM | "4 new reports with Excel-ready data" |
| 3 | Feature definition templates | ClassificationRuleSet aggregate | Wire checkpoints to remaining RMs | "Classification engine with audit trail" |
| 4 | — | Review workflow + Excel export | CachingRepository | "Human review gate + Excel export for accountants" |
| 5 | — | Enhanced rule types + transaction splitting | SFM extraction begins | "Full classification suite + tax splitting" |
| 6 | — | Polish, integration testing, demo prep | SFM extraction complete | **Production-ready MVP demo** |

**Key properties of this schedule:**
- **Runway is 1–2 weeks**, not a separate phase — it runs concurrently with infrastructure
- **Features start in Week 2**, delivered by agents using the runway
- **Every week has stakeholder-visible output** — demonstrating execution
- **Infrastructure is invisible but protects the demo** — no crashes, fast startup
- **Clean architecture throughout** — no hacking, proper ES-DDD patterns

---

### The Betting-the-Farm Implication

If agentic acceleration is the strategic bet, then the **skills, seed data, and feature definitions are first-class deliverables** — not support artifacts. They are the reusable capital that compounds:

- **First report** takes a human to build the SKILL.md. **Reports 2–11** are agent-delivered in hours.
- **First aggregate** takes a human to build the domain-aggregate SKILL.md. **Subsequent aggregates** are agent-delivered from specs.
- **First pipeline step** codifies the pattern. **Subsequent steps** are mechanical.

The ROI model is:

```
Week 1: Build skills (human investment) → 0 features
Week 2: Skills exist → 4 features (agent-delivered)
Week 3: Skills refined → 3 features (agent-delivered, faster)
Week 4: Skills mature → 3 features (agent-delivered, faster still)
...
```

vs the traditional model:

```
Week 1: 1 feature (human-delivered)
Week 2: 1 feature (human-delivered)
Week 3: 1 feature (human-delivered)
...
```

The breakeven is Week 2. By Week 6, the agentic model has delivered 2–3x more features with higher consistency (every feature machine-verified).

---

## rd-agentic-flow Integration Priority (Added 2026-03-28)

The `rd-agentic-flow` repo (`C:\Users\chris\source\repos\condron\rd-agentic-flow`) fundamentally changes the integration picture. Three categories of assets are ready to integrate:

### Category 1: Infrastructure Code (Highest Priority)

These are production-ready implementations that close the two biggest infrastructure gaps identified in the mitigation plan — and they're **integration work, not build work.**

| Asset | Source | Closes Gap | Integration Effort |
|-------|--------|-----------|-------------------|
| `ICheckpointStore` + `PostgresCheckpointStore` | `process-manager/read-model-caching-reference/` | Mitigation 1.1–1.3 (RM checkpointing) | **2–3 days** — namespace change (`Greylock.Publish.Infrastructure` → PowerModels), wire into `ReadModelBase` constructor, add NuGet deps (Dapper, Npgsql), create `projection_checkpoints` table, pilot on 1–2 accounting RMs |
| `IReadModelStore` + `DapperReadModelStore` + `InMemoryReadModelStore` | `process-manager/read-model-caching-reference/` | Mitigation 1.2 (RM persistence) | **1–2 days** — namespace change, POCO state classes for accounting RMs, table DDL. `InMemoryReadModelStore` already ideal for tests. |
| `ProcessAggregateBase<TState>` + `ProcessManagerBase<TProcessAgg,TState>` | `process-manager/Infrastructure/` | Mitigation 1.4, 2.4 (process manager infrastructure) | **2–3 days** — namespace change (`Greylock.Domain.Infrastructure` → PowerModels), resolve `StepTimedOut` / `CommandsIssued` event references, `LaterService` integration, `CategoryStreamWarmup` |
| `ProjectionSchema` pattern | `process-manager/read-model-caching-reference/` | DDL management for read model tables | **0.5 day** — adapt template for PowerModels accounting tables |

**Total: ~6–8 days** to close infrastructure gaps that the mitigation plan estimated at 4–6 weeks.

### Category 2: Agent System (High Priority — Enables Agentic Acceleration Bet)

Two agent systems exist with different patterns:

| Variant | Agents | Skills | Pattern Compatibility | Best Use |
|---------|--------|--------|----------------------|----------|
| **samples/** (Greylock) | 8 agents | 6 skills (greylock-aggregate **recipe-grade**, greylock-testing, greylock-csharp, doc-orchestration, excalidraw, mermaid) + 5 path-specific review instructions | `:this()` + `Source = source`, `IRepository`, plain `AppSvc`, `IReadModelStore` | **Reference for recipe-grade skill format.** Doc orchestration and diagram skills are pattern-agnostic. Review instructions adaptable. |
| **starter/** (PowerModels) | 15 agents | 9 skills + CLAUDE.md | `:base(source)`, `ICorrelatedRepository`, `QueuedSubscriber`, `TransientSubscriber` + `SourceCache`, `TrackingMetadata` | **Direct integration** into PowerModels `.github/` — agents and skills match current conventions. |

**Key pattern conflict:** The samples/ Greylock variant uses `IReadModelStore` + `ICheckpointStore` for read models (the modern pattern from the infrastructure code). The starter/ PowerModels variant still uses `TransientSubscriber` + `SourceCache`. When infrastructure code is integrated into PowerModels, the starter/ skills should be updated to reflect the new patterns.

**Integration approach:**
1. Copy `starter/` agents and skills into PowerModels `.github/` as baseline
2. Adapt `samples/greylock-aggregate/SKILL.md` recipe format for PowerModels conventions
3. Integrate `samples/.github/instructions/*.instructions.md` review rules (adapt for PowerModels patterns)
4. Add `samples/.github/skills/doc-orchestration/` for documentation generation
5. Update testing skill to drop RestoreFromEvents as REQUIRED (both repos)
6. Update `starter/` read model skill after checkpoint/store integration

**Integration effort:** 2–3 days for full agent system integration.

### Category 3: Orchestration Pipeline (Medium Priority — Force Multiplier)

The `starter/` feature-builder workflow is a complete orchestration pipeline:

```
Human opens issue → feature-builder agent:
  → dependency-tracker (check blockers)
  → planner (break into tasks)
  → plan-architect (validate against codebase)
  → implementer (write code per skills)
  → 4 parallel reviewers (architecture, correctness, code-quality, security)
  → fix loop (implementer fixes review issues)
  → acceptance checklist (map criteria to files/tests)
  → smoke test (UI changes only)
  → dotnet format → commit → rebase → merge into Epic branch
```

This pipeline + the Epic branching strategy (Draft PRs, issue branches, ff-merge) is directly usable once agents and skills are integrated.

**Integration effort:** 1 day — mostly documentation and CLAUDE.md setup.

### Revised Gap Assessment After rd-agentic-flow

| Original Gap | Status After rd-agentic-flow |
|-------------|------------------------------|
| ES-DDD skill lacks recipe format | **CLOSED** — greylock-aggregate is recipe-grade; adapt for PowerModels |
| No pipeline step SKILL.md | **Still open** — no pipeline skill in either repo |
| Testing skill stale RestoreFromEvents | **Still open** — needs update in both repos |
| RM checkpoint infrastructure | **CLOSED** — ICheckpointStore + PostgresCheckpointStore ready to integrate |
| Process Manager infrastructure | **CLOSED** — ProcessAggregateBase + ProcessManagerBase ready to integrate |
| SA coupling defeats agent isolation | **Still open** — WPF isolation needed |
| No classification rules seed data | **Still open** |
| Feature specs not agent-consumable | **Partially closed** — feature-builder + planner agents handle spec → implementation, but feature definition template still needed |
| No Excel export SKILL.md | **Still open** |

**5 of 9 gaps closed or partially closed** by integrating rd-agentic-flow assets.

### Impact on Roadmap Timeline

The infrastructure code integration compresses the "invisible infrastructure" track dramatically:

| Track | Before rd-agentic-flow | After rd-agentic-flow |
|-------|----------------------|----------------------|
| RM checkpoints | 4–6 weeks (build from scratch) | **6–8 days** (integrate + pilot) |
| Process managers | 3–4 weeks (build from scratch) | **2–3 days** (integrate) |
| Agent system upgrade | 2–3 weeks (build recipe skills) | **2–3 days** (copy + adapt) |
| **Total infrastructure** | **9–13 weeks** | **~2 weeks** |

This means the "runway" phase compresses from 2 weeks to potentially **1 week** if infrastructure integration runs parallel to WPF isolation.

---

## Summary

- **5 mitigation items already done** (Journal, JournalEntry, TB, IS, BS + 4 bonus reports)
- **1 item superseded** by PMA (classification rules)
- **4 conflicts** identified with resolutions
- **9 infrastructure/domain gaps** in PMA plan (inherited from mitigation but not addressed) — **5 now closed by rd-agentic-flow assets**
- **7 feature gaps** in mitigation plan (PMA-only items)

### Reframed Through Business Imperatives

- **Speed:** Infrastructure integration (not build) compresses runway to ~1 week. Features start Week 2.
- **Execution visibility:** Every week produces stakeholder-visible output. UITestRunner is a continuous demo machine. Monday demo plan targets P&L monthly breakdown + Excel export.
- **Agentic acceleration:** rd-agentic-flow provides a complete orchestration pipeline (15 agents, 9 skills, Epic branching, 4-way review). Skills, seed data, and feature definitions are first-class deliverables. The accounting-reports SKILL.md pattern is proven — recipe-grade aggregate skill already exists in rd-agentic-flow.
- **No hacking:** WPF isolation, proper aggregates, event-sourced patterns throughout. The refactoring enables the speed, not inhibits it.
- **Core recommendation:** Drop the "mitigation vs PMA" framing. Organize as Runway → Features → Infrastructure, with all three tracks running in parallel after Week 1.
