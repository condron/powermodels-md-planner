# Unified Roadmap: Agent-Optimized Delivery Sequence

**Date:** 2026-03-28
**Optimized for:** Total delivery speed · Stakeholder execution visibility · Agentic acceleration
**Constraint:** No hacking — proper ES-DDD patterns throughout

---

## Core Principle: Human Work Creates Agent Capital

Every human-delivered item should produce at least one of:
1. **A SKILL.md** — codified pattern an agent can follow
2. **A seed data extension** — test fixture agents verify against
3. **A reference implementation** — first instance of a pattern agents replicate
4. **Infrastructure** — invisible plumbing agents consume without knowing

Work items are classified:

| Tag | Meaning |
|-----|---------|
| 🧑 **Human** | Requires human judgment, pattern establishment, or architectural decisions |
| 🤖 **Agent** | Deliverable by agent using existing skills + pipeline |
| 🧑→🤖 **Human-then-Agent** | Human builds the first one (establishes pattern/SKILL), agents replicate |

---

## Phase 0: Monday Demo Sprint (Days 1–3)

**Goal:** Visible feature progress for stakeholder demo. Each item also establishes a reusable pattern.

| # | Item | Who | Deliverable | Reusable Capital Created |
|---|------|-----|-------------|--------------------------|
| 0.1 | P&L monthly breakdown (section-per-month) | 🧑→🤖 | Modified `IncomeStatementRm` with monthly sections | **Pattern:** monthly breakdown via `Section()` loop — Cash Flow replicates this |
| 0.2 | Cash Flow monthly breakdown | 🤖 | Modified `CashFlowStatementRm` with monthly sections | Follows pattern from 0.1 |
| 0.3 | Fiscal year selector | 🧑 | `ReportsContainerVm` year dropdown, `DateRange` period | **Pattern:** report period selection wiring (VM ↔ XAML ↔ RM) |
| 0.4 | Generic Excel export (ClosedXML) | 🧑→🤖 | `ReportExcelExporter` — `IAccountingReport → .xlsx` | **SKILL.md candidate:** Excel export pattern. **Reference impl** for PDF export. |
| 0.5 | Generic PDF export | 🤖 | `ReportPdfExporter` — `IAccountingReport → .pdf` | Follows export pattern from 0.4 |

**Phase 0 output:** 7 reports with monthly breakdown + fiscal year selection + Excel/PDF export.
**Agent capital produced:** Monthly breakdown pattern, export pattern, period wiring pattern.

**Detail:** See [monday-demo-plan-2026-03-28.md](monday-demo-plan-2026-03-28.md)

---

## Phase 1: Agent Pipeline Integration (Days 4–8)

**Goal:** Install the full agentic development pipeline so all subsequent feature work can be agent-delivered. This is the highest-leverage human work in the entire roadmap.

| # | Item | Who | Deliverable | Reusable Capital Created |
|---|------|-----|-------------|--------------------------|
| 1.1 | Integrate `ICheckpointStore` + `PostgresCheckpointStore` | 🧑 | Namespace adaptation, NuGet deps (Dapper, Npgsql), `projection_checkpoints` table DDL | **Infrastructure:** All RMs can checkpoint without per-RM work |
| 1.2 | Integrate `IReadModelStore` + `DapperReadModelStore` | 🧑 | Namespace adaptation, `InMemoryReadModelStore` available for tests | **Infrastructure + test tooling:** RMs persist state, tests use in-memory store |
| 1.3 | Pilot checkpoint on 1–2 accounting RMs | 🧑→🤖 | `TrialBalanceRm` or `IncomeStatementRm` checkpointed | **Reference impl:** Shows how to wire checkpoint into existing RM. SKILL.md update. |
| 1.4 | Integrate `ProcessAggregateBase` + `ProcessManagerBase` | 🧑 | Namespace adaptation, resolve `StepTimedOut`/`CommandsIssued` events, `LaterService` integration | **Infrastructure:** Process managers can be built by agents |
| 1.5 | Copy `starter/` agents + skills into PowerModels `.github/` | 🧑 | 15 agents, 9 skills, CLAUDE.md | **Agent pipeline:** feature-builder orchestration pipeline operational |
| 1.6 | Adapt `greylock-aggregate` recipe skill for PowerModels | 🧑 | Recipe-grade ES-DDD skill with PowerModels conventions (`:base(source)`, `ICorrelatedRepository`, `TrackingMetadata`) | **SKILL.md:** Agents can build aggregates from specs |
| 1.7 | Integrate `process.instructions.md` + `readmodel.instructions.md` review rules | 🧑 | Path-specific Copilot review rules for `*Agg.cs`, `*RM.cs`, `**/Processes/*.cs` | **Review automation:** Code review catches R-D anti-patterns automatically |
| 1.8 | Update testing skill — drop RestoreFromEvents as REQUIRED | 🧑 | Updated `csharp-xunit-reactive-domain/SKILL.md` in both repos | **Skill fix:** Agents stop writing unnecessary RestoreFromEvents tests |
| 1.9 | Write Excel export SKILL.md (from Phase 0 reference impl) | 🧑 | SKILL.md covering `IAccountingReport → ClosedXML` pattern | **SKILL.md:** Agents can add export to new report types |
| 1.10 | Write pipeline step SKILL.md | 🧑 | SKILL.md covering `IDataIngestionStep`, context extensions, pipeline builder tests | **SKILL.md:** Agents can add pipeline steps |

**Phase 1 output:** Full agent pipeline operational, infrastructure integrated, 6 new/updated SKILLs.
**Agent capital produced:** Every subsequent feature has a recipe.

---

## Phase 2: Agent-Delivered Features (Days 9–18)

**Goal:** Ship features using the agent pipeline. Human role shifts to spec writing and review.

### Reports (all 🤖 Agent — follows accounting-reports SKILL)

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 2.1 | DiagnosticRm | 🤖 | — | Trial Balance + AccountType + NI flag. Small RM. |
| 2.2 | ExceptionsRm | 🤖 | — | Filter Income/Expense with both DR+CR activity. |
| 2.3 | LedgerAllRm (flat transaction view) | 🤖 | — | Flat projection of classified transactions. |
| 2.4 | OpeningJournalRm | 🤖 | — | Filter JournalReportRm for opening entries. |

### Domain Aggregates (all 🤖 Agent — follows recipe-grade ES-DDD SKILL from 1.6)

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 2.5 | ClassificationRuleSet aggregate | 🤖 | 1.6 (recipe skill) | Persists rules that are currently ephemeral POCOs. Commands: `CreateRuleSet`, `AddRule`, `UpdateRule`. Events: `RuleSetCreated`, `RuleAdded`, etc. |
| 2.6 | StatementImport aggregate | 🤖 | 1.6 | Tracks PDF/statement ingestion. Needed for Reconciliation + RunSummary reports. |
| 2.7 | FiscalYearConfig on AccountingSystem aggregate | 🤖 | 1.6 | Extend `AccountingSystem` with fiscal year start month. Small change. |

### Pipeline Steps (all 🤖 Agent — follows pipeline step SKILL from 1.10)

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 2.8 | CreateImportReviewStep | 🤖 | 1.10 (pipeline SKILL), 2.5 (ClassificationRuleSet) | Human review gate for AI classifications. |
| 2.9 | TransactionSplittingStep | 🤖 | 1.10 | Split mixed transactions (e.g., tax component extraction). |

### Infrastructure Rollout (🤖 Agent — follows checkpoint reference impl from 1.3)

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 2.10 | Checkpoint remaining accounting RMs (5 more) | 🤖 | 1.3 (reference impl) | Mechanical — follow the pattern from pilot RM. |
| 2.11 | Checkpoint SFM-related RMs (9 hottest) | 🤖 | 1.1, 1.3 | Same pattern, higher impact (SFM stream has 9 subscribers). |

### Seed Data Extensions (🧑→🤖 first set human, then agent)

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 2.12 | Classification rules seed data | 🧑→🤖 | 2.5 | First seed extension is human (establishes pattern), subsequent are agent. |
| 2.13 | Statement import seed data | 🤖 | 2.6, 2.12 pattern | Follows seed extension pattern. |

**Phase 2 output:** 4 new reports (→11 total), 3 new aggregates, 2 pipeline steps, 14 checkpointed RMs, extended seed data.

---

## Phase 3: Reports from New Aggregates + Polish (Days 19–25)

**Goal:** Build reports that depend on Phase 2 aggregates. Prepare for production demo.

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 3.1 | ReconciliationRm | 🤖 | 2.6 (StatementImport) | Statement vs extracted totals proof. |
| 3.2 | RunSummaryRm | 🤖 | 2.6 | Per-PDF intake overview. |
| 3.3 | RulesReferenceRm | 🤖 | 2.5 (ClassificationRuleSet) | Static rule reference table. |
| 3.4 | RuleHitsRm | 🤖 | 2.5 | Audit trail of which rule classified each transaction. |
| 3.5 | Enhanced rule types (regex, amount range, compound) | 🤖 | 2.5 | Extend ClassificationRuleSet per spec. |
| 3.6 | Excel export SKILL.md for new reports | 🤖 | 1.9, 2.1–2.4 | Verify export works for all new report types. |
| 3.7 | CachingRepository for SFM | 🧑 | — | Performance — wraps `IRepository` with in-memory + persisted snapshot tier. |
| 3.8 | Integration testing — full demo flow | 🧑 | All above | Seed → reports → export → verify end-to-end. |

**Phase 3 output:** 4 more reports (→15 total, matching Lisa's 12 + 3 bonus), enhanced rules, SFM performance fix, production-ready demo.

---

## Phase 4: WPF Isolation + Structural Refactoring (Days 19–30, parallel with Phase 3)

**Goal:** Clean project boundaries for headless API future. Runs parallel to Phase 3.

| # | Item | Who | Blocked By | Notes |
|---|------|-----|-----------|-------|
| 4.1 | WPF isolation — extract pipeline from SA | 🧑 | — | Break SA → ModelServer dependency. See PMA Phase 0. |
| 4.2 | WPF isolation — move reports to UIBehavior | 🧑 | 4.1 | Already partially done (7 RMs in UIBehavior). |
| 4.3 | Pipeline factoring — `DataIngestionPipeline` as standalone project | 🧑 | 4.1 | Clean project boundary for pipeline-only testing. |
| 4.4 | SFM table-mapping extraction | 🧑 | 4.1 | Begin breaking up the god aggregate (142 refs). |
| 4.5 | FinancialModelService split | 🧑 | 4.4 | Split monolithic service. |

**Phase 4 output:** Clean project boundaries, headless-API-ready architecture, SFM extraction begun.

---

## Delivery Timeline

```
Week 1 (Mon–Fri):  Phase 0 (demo sprint)
                    ├── 🧑 P&L monthly, fiscal year, Excel export (patterns)
                    ├── 🤖 Cash Flow monthly, PDF export (follows patterns)
                    └── MONDAY DEMO ✓

Week 2 (Mon–Fri):  Phase 1 (agent pipeline integration)
                    ├── 🧑 Infrastructure code integration (checkpoints, PM, stores)
                    ├── 🧑 Agent + skill installation from rd-agentic-flow
                    ├── 🧑 SKILL.md authoring (aggregate recipe, pipeline step, export)
                    └── Agent pipeline operational ✓

Week 3–4:          Phase 2 (agent-delivered features)
                    ├── 🤖 4 new reports (Diagnostic, Exceptions, LedgerAll, OpeningJournal)
                    ├── 🤖 3 aggregates (ClassificationRuleSet, StatementImport, FiscalYear)
                    ├── 🤖 2 pipeline steps (review gate, splitting)
                    ├── 🤖 14 checkpointed RMs
                    └── Feature velocity demo ✓

Week 4–5:          Phase 3 + 4 (parallel)
                    ├── 🤖 4 more reports from new aggregates
                    ├── 🤖 Enhanced rule types
                    ├── 🧑 SFM performance (CachingRepository)
                    ├── 🧑 WPF isolation + pipeline factoring
                    └── PRODUCTION MVP DEMO ✓
```

---

## Agent Readiness Matrix

Shows which work items become agent-deliverable after each phase:

| After Phase | Newly Agent-Deliverable | Why |
|-------------|------------------------|-----|
| **Phase 0** | Monthly breakdown reports, export variants | Patterns established in P&L + Excel export |
| **Phase 1** | Aggregates, pipeline steps, RM checkpoints, reports | Recipe-grade SKILLs + infrastructure + agent pipeline installed |
| **Phase 2** | Reports depending on new aggregates, seed data extensions | Aggregates exist, seed data pattern established |
| **Phase 3** | — (polish phase) | Everything mechanical is already agent-delivered |

**The compounding effect:**
- Phase 0: 2 of 5 items agent-delivered (40%)
- Phase 1: 0 of 10 items agent-delivered (0% — this is investment)
- Phase 2: 12 of 13 items agent-delivered (92%)
- Phase 3: 6 of 8 items agent-delivered (75%)

**Cumulative: 20 of 36 items (56%) agent-delivered.** The human-delivered items are concentrated in Phases 0–1 (pattern establishment), and agent-delivered items dominate Phases 2–3 (replication).

---

## Work Item Summary by Executor

### 🧑 Human Work (16 items — concentrated in Weeks 1–2)

| Phase | Items | Theme |
|-------|-------|-------|
| 0 | P&L monthly, fiscal year selector, Excel export | Pattern establishment |
| 1 | Infrastructure integration (4), agent pipeline install (2), SKILL authoring (4) | Runway construction |
| 2 | First seed data extension | Pattern establishment |
| 3 | CachingRepository, integration testing | Performance + verification |
| 4 | WPF isolation, pipeline factoring, SFM extraction | Structural refactoring |

### 🤖 Agent Work (20 items — concentrated in Weeks 3–5)

| Phase | Items | Theme |
|-------|-------|-------|
| 0 | Cash Flow monthly, PDF export | Pattern replication |
| 2 | 4 reports, 3 aggregates, 2 pipeline steps, 2 RM checkpoint batches, 1 seed extension | Feature velocity |
| 3 | 4 reports, enhanced rules, export verification | Feature completion |

---

## Dependencies & Critical Path

```
Phase 0 ──────────────────────────────┐
  0.1 P&L monthly (🧑→🤖)            │
  0.2 CF monthly (🤖, follows 0.1)   │
  0.3 Fiscal year (🧑)               ├──→ MONDAY DEMO
  0.4 Excel export (🧑→🤖)           │
  0.5 PDF export (🤖, follows 0.4)   │
                                      │
Phase 1 ──────────────────────────────┤ (starts after demo, or parallel)
  1.1–1.4 Infrastructure (🧑)        │
  1.5–1.7 Agent pipeline (🧑)        ├──→ AGENT PIPELINE READY
  1.8–1.10 SKILLs (🧑)               │
                                      │
Phase 2 ──────────────────────────────┤ (starts after Phase 1)
  2.1–2.4 Reports (🤖)        ─────── no blockers, immediate
  2.5 ClassificationRuleSet (🤖) ──── needs 1.6 (recipe skill)
  2.6 StatementImport (🤖)     ────── needs 1.6
  2.7 FiscalYear config (🤖)   ────── needs 1.6
  2.8 ImportReview step (🤖)   ────── needs 1.10 + 2.5
  2.9 Splitting step (🤖)      ────── needs 1.10
  2.10–2.11 RM checkpoints (🤖) ──── needs 1.3
  2.12–2.13 Seed data (🧑→🤖) ────── needs 2.5, 2.6
                                      │
Phase 3 ──────────────────────────────┤ (starts after relevant Phase 2 items)
  3.1–3.2 Recon/Summary (🤖)  ────── needs 2.6
  3.3–3.4 Rules reports (🤖)   ────── needs 2.5
  3.5 Enhanced rules (🤖)      ────── needs 2.5
                                      │
Phase 4 (parallel with 3) ───────────┤
  4.1–4.5 WPF isolation (🧑)         ├──→ PRODUCTION MVP
```

---

## Success Metrics

| Metric | Target | Measured By |
|--------|--------|-------------|
| Reports shipping | 15 (Lisa's 12 + 3 bonus) | Report list in UI |
| Reports with Excel export | All 15 | Export produces valid .xlsx |
| Reports with PDF export | All 15 | Export produces valid .pdf |
| RMs checkpointed | 14+ (all accounting + 9 SFM) | `projection_checkpoints` table rows |
| Agent-delivered items | 20 of 36 (56%) | Git history — agent commits |
| Time to agent pipeline | 8 days (end of Phase 1) | Agent can build aggregate from spec |
| Time to production MVP | 25–30 days | Full demo script passes |
| SKILLs available | 6 new/updated | `.github/skills/` file count |

---

## Related Documents

- [Monday Demo Sprint Plan](monday-demo-plan-2026-03-28.md)
- [Gap & Conflict Analysis](gap-conflict-analysis-2026-03-28.md)
- [PMA Master Refactoring Plan](pma-master-refactoring-plan-2026-03-28.md)
