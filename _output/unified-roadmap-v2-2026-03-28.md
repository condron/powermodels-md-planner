# Unified Roadmap v2: Agent-Optimized Delivery with Weekly Visible Progress

**Date:** 2026-03-28
**Optimized for:** Total delivery speed · Weekly stakeholder-visible UI delivery · Agentic acceleration
**Constraint:** No hacking — proper ES-DDD patterns throughout. Every week ships UI-visible features.
**Supersedes:** [unified-roadmap-2026-03-28.md](unified-roadmap-2026-03-28.md) (v1 — phased without weekly delivery constraint)

---

## Core Principles

### 1. Human Work Creates Agent Capital

Every human-delivered item should produce at least one of:
- **A SKILL.md** — codified pattern an agent can follow
- **A seed data extension** — test fixture agents verify against
- **A reference implementation** — first instance of a pattern agents replicate
- **Infrastructure** — invisible plumbing agents consume without knowing

### 2. Every Week Ships Visible Features

No "infrastructure-only" weeks. Invisible work (checkpoints, pipeline integration, SKILLs) runs **parallel** to UI-visible deliverables, never instead of them.

### 3. Work Item Classification

| Tag | Meaning |
|-----|---------|
| 🧑 **Human** | Requires human judgment, pattern establishment, or architectural decisions |
| 🤖 **Agent** | Deliverable by agent using existing skills + pipeline |
| 🧑→🤖 **Human-then-Agent** | Human builds the first one (establishes pattern/SKILL), agents replicate |

---

## Current State (Starting Point)

| Report | RM | Status |
|--------|----|--------|
| Trial Balance | `TrialBalanceRm` | ✅ Shipping |
| Income Statement | `IncomeStatementRm` | ✅ Shipping |
| Balance Sheet | `BalanceSheetRm` | ✅ Shipping |
| Cash Flow Statement | `CashFlowStatementRm` | ✅ Shipping |
| General Ledger | `GeneralLedgerRm` | ✅ Shipping |
| Income/Expense Summary | `IncomeExpenseSummaryRm` | ✅ Shipping |
| Journal Report | `JournalReportRm` | ✅ Shipping |

**7 reports**, single-period only, no export, no year selection.

---

## Week 1 — Monday Demo: Reports Get Richer

**Demo headline:** _"P&L and Cash Flow now show monthly breakdowns. Any report exports to Excel or PDF. Fiscal year selector drives all reports."_

### UI-Visible Deliverables

| # | Item | Who | What Stakeholders See |
|---|------|-----|-----------------------|
| 1.1 | P&L monthly breakdown | 🧑→🤖 | Income Statement shows Revenue + Expenses per month with YTD grand total |
| 1.2 | Cash Flow monthly breakdown | 🤖 | Cash Flow Statement shows Operating/Investing/Financing per month |
| 1.3 | Fiscal year selector | 🧑 | Year dropdown in report header — switching year rebuilds all reports |
| 1.4 | Excel export (all 7 reports) | 🧑→🤖 | "Export to Excel" button → `.xlsx` with formatted sheets |
| 1.5 | PDF export (all 7 reports) | 🤖 | "Export to PDF" button → formatted `.pdf` |

### Invisible Work (Parallel)

| # | Item | Who | Agent Capital Created |
|---|------|-----|----------------------|
| 1.6 | Write Excel export SKILL.md | 🧑 | Agents can add export to future report types |

**Week 1 exit:** 7 reports with monthly P&L/CF, year selector, Excel + PDF export.
**Detail:** See [monday-demo-plan-2026-03-28.md](monday-demo-plan-2026-03-28.md)

---

## Week 2 — Infrastructure + First New Reports

**Demo headline:** _"4 new diagnostic reports. Application starts faster (checkpointed projections)."_

### UI-Visible Deliverables

| # | Item | Who | What Stakeholders See |
|---|------|-----|-----------------------|
| 2.1 | DiagnosticRm | 🤖 | New report: TB + AccountType + NI flag table |
| 2.2 | ExceptionsRm | 🤖 | New report: Income/Expense accounts with both DR+CR activity |
| 2.3 | LedgerAllRm (flat transaction view) | 🤖 | New report: Flat classified transaction listing |
| 2.4 | OpeningJournalRm | 🤖 | New report: Opening balance entries |
| 2.5 | Faster app startup (visible perf improvement) | 🧑 | App launches noticeably faster — checkpointed RMs skip replay |

### Invisible Work (Parallel)

| # | Item | Who | Agent Capital Created |
|---|------|-----|----------------------|
| 2.6 | Integrate `ICheckpointStore` + `PostgresCheckpointStore` | 🧑 | All RMs can checkpoint |
| 2.7 | Integrate `IReadModelStore` + `DapperReadModelStore` | 🧑 | RMs persist state across restarts |
| 2.8 | Pilot checkpoint on 2 accounting RMs | 🧑→🤖 | Reference impl for RM checkpointing |
| 2.9 | Copy `starter/` agents + skills from rd-agentic-flow | 🧑 | 15 agents, 9 skills, feature-builder pipeline |
| 2.10 | Integrate `ProcessAggregateBase` + `ProcessManagerBase` | 🧑 | Process managers buildable by agents |
| 2.11 | Adapt `greylock-aggregate` recipe skill for PowerModels | 🧑 | Recipe-grade ES-DDD SKILL.md |
| 2.12 | Integrate review instructions (aggregate, RM, process) | 🧑 | Copilot path-based review rules |
| 2.13 | Update testing skill — drop RestoreFromEvents as REQUIRED | 🧑 | Agents stop writing unnecessary tests |
| 2.14 | Write pipeline step SKILL.md | 🧑 | Agents can add pipeline steps |

**Week 2 exit:** 11 reports shipping. Agent pipeline operational. Infrastructure integrated. 6 new/updated SKILLs.

---

## Week 3 — Domain Aggregates + Classification

**Demo headline:** _"Classification rules are now persistent and auditable. Human review gate for AI classifications. 14 projections checkpointed — startup is fast."_

### UI-Visible Deliverables

| # | Item | Who | What Stakeholders See |
|---|------|-----|-----------------------|
| 3.1 | ClassificationRuleSet — rules visible in UI | 🤖 | Classification rules persisted as aggregate, viewable in rules panel |
| 3.2 | RulesReferenceRm | 🤖 | New report: Static rule reference table showing all active rules |
| 3.3 | RuleHitsRm | 🤖 | New report: Audit trail — which rule classified each transaction |
| 3.4 | Import review UI (human review gate) | 🧑→🤖 | New UI: Review AI classifications before posting, accept/reject per line |

### Invisible Work (Parallel)

| # | Item | Who | Agent Capital Created |
|---|------|-----|----------------------|
| 3.5 | ClassificationRuleSet aggregate | 🤖 | Persisted rules (commands, events, handler, tests) |
| 3.6 | CreateImportReviewStep (pipeline step) | 🤖 | Human review gate in ingestion pipeline |
| 3.7 | Checkpoint remaining accounting RMs (5 more) | 🤖 | 7 accounting RMs checkpointed |
| 3.8 | Checkpoint SFM-related RMs (9 hottest) | 🤖 | 16 total RMs checkpointed |
| 3.9 | Classification rules seed data | 🧑→🤖 | Agents can test rule application against known-good data |
| 3.10 | FiscalYearConfig on AccountingSystem aggregate | 🤖 | Configurable fiscal year start month |

**Week 3 exit:** 13 reports. Classification rules persistent + auditable. Review gate operational. 16 RMs checkpointed.

---

## Week 4 — Reconciliation + Statement Import

**Demo headline:** _"Bank reconciliation report matches imported statements to extracted transactions. Run summary shows per-file intake status. Enhanced rule types (regex, amount range)."_

### UI-Visible Deliverables

| # | Item | Who | What Stakeholders See |
|---|------|-----|-----------------------|
| 4.1 | ReconciliationRm | 🤖 | New report: Statement totals vs extracted totals with variance |
| 4.2 | RunSummaryRm | 🤖 | New report: Per-PDF/file intake overview (counts, status, errors) |
| 4.3 | Enhanced rule types (regex, amount range, compound) | 🤖 | Rules panel shows new rule type options |
| 4.4 | Transaction splitting in UI | 🤖 | Split transactions visible in journal (e.g., tax component extraction) |

### Invisible Work (Parallel)

| # | Item | Who | Agent Capital Created |
|---|------|-----|----------------------|
| 4.5 | StatementImport aggregate | 🤖 | Tracks PDF/statement ingestion lifecycle |
| 4.6 | TransactionSplittingStep (pipeline step) | 🤖 | Split mixed transactions during ingestion |
| 4.7 | Statement import seed data | 🤖 | Test fixtures for reconciliation scenarios |
| 4.8 | WPF isolation — begin SA pipeline extraction | 🧑 | Clean project boundaries (parallel track) |

**Week 4 exit:** 15 reports (matches Lisa's 12 + 3 bonus). Reconciliation + statement import operational. Enhanced classification.

---

## Week 5 — Polish + Structural Refactoring

**Demo headline:** _"All 15 reports export to Excel and PDF. Application starts in under 3 seconds. Full end-to-end demo from import to reports."_

### UI-Visible Deliverables

| # | Item | Who | What Stakeholders See |
|---|------|-----|-----------------------|
| 5.1 | Verify Excel/PDF export for all 15 reports | 🤖 | Every report produces valid export files |
| 5.2 | CachingRepository for SFM — fast startup | 🧑 | App startup noticeably faster (SFM replay eliminated) |
| 5.3 | Full integration test — seed → ingest → classify → review → post → reports → export | 🧑 | Complete end-to-end workflow demonstrated |
| 5.4 | UI polish pass — consistent formatting, empty-state handling | 🤖 | Professional appearance across all reports |

### Invisible Work (Parallel)

| # | Item | Who | Agent Capital Created |
|---|------|-----|----------------------|
| 5.5 | WPF isolation — move reports to UIBehavior | 🧑 | All report RMs in UIBehavior project |
| 5.6 | Pipeline factoring — `DataIngestionPipeline` as standalone project | 🧑 | Pipeline testable without SA dependency |
| 5.7 | SFM table-mapping extraction (begin) | 🧑 | Start breaking god aggregate |

**Week 5 exit:** Production-ready MVP. 15 reports with export. Fast startup. Clean end-to-end demo.

---

## Week 6+ — Structural Completion (Ongoing)

| # | Item | Who | Notes |
|---|------|-----|-------|
| 6.1 | SFM table-mapping extraction (complete) | 🧑 | Break 142-ref god aggregate |
| 6.2 | FinancialModelService split | 🧑 | Split monolithic service |
| 6.3 | Headless API surface | 🧑 | WPF-free entry point for reports + export |
| 6.4 | Industry CoA templates | 🤖 | Seed data for multiple industries |
| 6.5 | Entry Patterns Library | 🤖 | Common journal entry templates |

---

## Weekly Demo Scorecard

| Week | New Reports | Total Reports | UI Features Added | Agent Items | Human Items |
|------|-------------|---------------|-------------------|-------------|-------------|
| 0 (now) | — | 7 | — | — | — |
| **1** | 0 | 7 | Monthly breakdown, year selector, Excel/PDF export | 2 | 4 |
| **2** | +4 | 11 | 4 new reports, faster startup | 4 | 9 |
| **3** | +2 | 13 | Rules reports, review gate, persistent rules | 6 | 2 |
| **4** | +2 | 15 | Reconciliation, run summary, enhanced rules, splitting | 4 | 1 |
| **5** | 0 | 15 | Full export coverage, fast startup, end-to-end demo | 2 | 3 |

**Cumulative:** 18 agent-delivered items, 19 human-delivered items (49% agent). But human work is front-loaded (Weeks 1–2), agent work accelerates (Weeks 3–4).

---

## Agent Readiness Progression

| After Week | What Agents Can Now Do | Why |
|------------|----------------------|-----|
| **1** | Replicate monthly breakdowns, build PDF export variant | P&L + Excel patterns established |
| **2** | Build any report from spec, checkpoint any RM | Recipe-grade SKILLs + infrastructure + agent pipeline installed |
| **3** | Build aggregates, pipeline steps, rule extensions, seed data | ClassificationRuleSet reference impl + pipeline SKILL |
| **4** | Build reports dependent on new aggregates | StatementImport + reconciliation patterns exist |
| **5** | — | Everything mechanical is already agent-delivered |

---

## Dependencies & Critical Path

```
WEEK 1 ─────────────────────────────────────────────────────────
  1.1 P&L monthly (🧑→🤖) ──→ 1.2 CF monthly (🤖)
  1.3 Fiscal year (🧑)
  1.4 Excel export (🧑→🤖) ──→ 1.5 PDF export (🤖)
  1.6 Excel SKILL (🧑)
                                    MONDAY DEMO ✓

WEEK 2 ─────────────────────────────────────────────────────────
  2.1–2.4 New reports (🤖) ─────── no blockers
  2.6–2.7 Checkpoint infra (🧑) ─→ 2.8 Pilot RMs (🧑→🤖) → 2.5 Faster startup
  2.9–2.14 Agent pipeline (🧑) ─── no blockers
                                    WEEK 2 DEMO ✓

WEEK 3 ─────────────────────────────────────────────────────────
  2.11 Recipe skill ──→ 3.5 ClassificationRuleSet (🤖) ──→ 3.2 RulesRef (🤖)
                                                          ──→ 3.3 RuleHits (🤖)
  2.14 Pipeline SKILL ──→ 3.6 ImportReview step (🤖) ──→ 3.4 Review UI (🧑→🤖)
  2.8 Pilot ──→ 3.7 Checkpoint accounting RMs (🤖)
             ──→ 3.8 Checkpoint SFM RMs (🤖)
                                    WEEK 3 DEMO ✓

WEEK 4 ─────────────────────────────────────────────────────────
  3.5 ClassificationRuleSet ──→ 4.3 Enhanced rules (🤖)
  2.11 Recipe skill ──→ 4.5 StatementImport (🤖) ──→ 4.1 Reconciliation (🤖)
                                                   ──→ 4.2 RunSummary (🤖)
  2.14 Pipeline SKILL ──→ 4.6 Splitting step (🤖) → 4.4 Splitting in UI (🤖)
                                    WEEK 4 DEMO ✓

WEEK 5 ─────────────────────────────────────────────────────────
  All features ──→ 5.1 Export verification (🤖)
  5.2 CachingRepository (🧑)
  5.3 Integration test (🧑)
  4.8 WPF isolation ──→ 5.5–5.7 Structural refactoring (🧑)
                                    PRODUCTION MVP DEMO ✓
```

---

## Success Metrics

| Metric | Target | Measured By |
|--------|--------|-------------|
| Weekly visible features | Every week has UI-demonstrable progress | Demo recording/screenshots |
| Reports shipping | 15 (Lisa's 12 + 3 bonus) | Report list in UI |
| All reports export | Excel + PDF for all 15 | Export produces valid files |
| RMs checkpointed | 16+ (accounting + SFM) | `projection_checkpoints` table rows |
| Agent-delivered items | 18 of 37 (49%) | Git history — agent commits |
| Time to agent pipeline | End of Week 2 | Agent can build aggregate from spec |
| Time to production MVP | 5 weeks | Full demo script passes end-to-end |
| SKILLs available | 6 new/updated | `.github/skills/` file count |

---

## Related Documents

- [Monday Demo Sprint Plan](monday-demo-plan-2026-03-28.md) — Week 1 detail
- [Gap & Conflict Analysis](gap-conflict-analysis-2026-03-28.md) — source analysis + rd-agentic-flow integration
- [PMA Master Refactoring Plan](pma-master-refactoring-plan-2026-03-28.md) — structural refactoring detail
- [Unified Roadmap v1](unified-roadmap-2026-03-28.md) — phase-organized predecessor
