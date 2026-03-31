# Unified Roadmap v3: Three Boundaries — Accounting Core · Ingestion · Seeder

**Date:** 2026-03-28
**Supersedes:** v1 (phase-only), v2 (feature-count-driven)
**Key insight:** Reports are the strategic drive to a *real* source-of-truth accounting system. The accounting core — from chart of accounts through closing the books — is the product. Ingestion is a pluggable capability with multiple routes (PDF, QB, Plaid, CSV). The seeder/business loader is the third boundary: it drives accounting features forward without ingestion, loads real-world multi-company accountant scenarios, and provides the baseline for validating ingestion correctness. These three boundaries must be cleanly separable.

---

## Architecture: The Three Boundaries

```
┌─────────────────────────────────────────────────────────┐
│               SEEDER / BUSINESS LOADER                   │
│         (test driver + scenario builder)                  │
│                                                         │
│  Single-company seed ──┐                                │
│  Multi-company seed ───┼─→ CoA + Journals + Entries ─┐  │
│  Industry templates ───┘    (real-world scenarios)    │  │
│                                                       │  │
│  ● Drives accounting core without ingestion           │  │
│  ● Multi-company = real accountant workload           │  │
│  ● Provides known-good baseline for ingress testing   │  │
│  ● Agent-extendable: new scenarios as seed extensions │  │
└───────────────────────────────────────────────────────│──┘
                                                       │
                        Journal Commands (AddJournalEntry)
                                                       │
┌─────────────────────────────────────────────────────────┐
│                  INGESTION LAYER                         │
│              (pluggable, multiple routes)                │
│                                                         │
│  PDF Extraction ─┐                                   │  │
│  QB/Xero Import ─┤                                   │  │
│  Plaid Bank Feed ┼─→ Normalize → Classify → Review ──┤  │
│  CSV Import ─────┤       (85/15 boundary)            │  │
│  Manual Entry ───┘                                   │  │
│                                                      │  │
│  ClassificationRuleSet    HumanReviewGate             │  │
│  (persistent rules)       (accept/reject/edit)        │  │
└──────────────────────────────────────────────────────│──┘
                                                       │
                        Journal Commands (AddJournalEntry)
                                                       │
                                                       ▼
┌─────────────────────────────────────────────────────────┐
│                  ACCOUNTING CORE                         │
│          (system of record — the product)                │
│                                                         │
│  ChartOfAccounts → Journal → JournalEntry               │
│       ↓                                                 │
│  Trial Balance → Income Statement → Balance Sheet       │
│                  Cash Flow → General Ledger              │
│       ↓                                                 │
│  Period Close (ProcessManager)                          │
│  Closing Entries (adjusting + closing JEs)               │
│  End-of-Period Reports (post-close TB, retained earnings)│
│       ↓                                                 │
│  Fiscal Year Cycle (year-end close → new year open)     │
│       ↓                                                 │
│  Excel / PDF Export (all reports)                        │
└─────────────────────────────────────────────────────────┘
```

**Two boundaries, one target.** Both the seeder and every ingestion route produce `AddJournalEntry` commands. The accounting core never knows or cares where data came from.

**The seeder boundary enables:**
- **Accounting core velocity** — drive reports, period close, closing entries, fiscal year cycle forward without touching ingestion
- **Real-world scenarios** — multi-company seeding loads the workload a real accountant faces (10+ businesses, different CoA structures, mixed transaction patterns)
- **Ingress validation** — seed a known business, then re-ingest from PDF/QB/CSV and compare results. Same data, different route — the accounting core output must match.
- **Agent testing** — every agent-delivered feature verifies against seeder data. Deterministic inputs → deterministic assertions.

**The ingestion boundary enables:**
- Each ingestion route added independently
- The 85/15 boundary (classify → review) lives in the ingestion layer
- Adapters are independently testable against seeder baselines

**The accounting core is clean:**
- Period close, closing entries, and fiscal year cycle are pure accounting — no ingestion or seeder dependency

---

## The Three Tracks

### Track A: Accounting Core — "The Real Accounting System"

This is the strategic drive. The progression tells a story: **basic reports → temporal reports → period management → closing the books → fiscal year cycle.** Each step proves PowerModels is a real system of record, not a reporting tool.

| Stage | What It Proves | Exists Today |
|-------|---------------|--------------|
| Chart of Accounts + Journals | Ledger fundamentals | ✅ |
| Trial Balance, P&L, BS, CF, GL | Standard financial statements | ✅ (7 reports) |
| Monthly breakdowns + fiscal year selection | Temporal awareness | Roadmap Week 1 |
| Diagnostic + Exception reports | Accountant working tools | Roadmap Week 2 |
| Period close process | Controlled period boundaries | ❌ Not built |
| Adjusting journal entries | Pre-close corrections | ❌ Not built |
| Closing entries | Revenue/expense → retained earnings | ❌ Not built |
| Post-close trial balance | Clean opening position | ❌ Not built |
| Fiscal year end-to-end | Full accounting cycle complete | ❌ Not built |

### Track S: Seeder / Business Loader — "The Test Driver"

The seeder produces the same `AddJournalEntry` commands as ingestion, but deterministically and without external dependencies. It's the engine for both testing and feature development.

| Stage | What It Enables | Exists Today |
|-------|----------------|--------------|
| Single-company seed (basic CoA + transactions) | Accounting core development + unit tests | ✅ (BusinessLoader / seeding) |
| Multi-company seed (10+ businesses, varied CoA) | Real accountant workload simulation | ❌ Not built |
| Industry-specific templates (tax prep, bookkeeping, 1099) | Design partner scenario testing | ❌ Not built |
| Period-spanning seed (multi-month, multi-year) | Period close + closing entries + year-end testing | ❌ Not built |
| Known-good extraction baselines | Ingress validation: ingest same data via PDF/QB/CSV, compare to seed | ❌ Not built |

**Why this is a first-class track:**
- **Without multi-company seed data, agents can't test accounting features in realistic scenarios.** The seeder IS the test fixture.
- **Without period-spanning seeds, period close and year-end workflows can't be verified.** The seeder drives the accounting cycle.
- **Without extraction baselines, ingestion correctness is unverifiable.** The seeder provides the expected output for any ingestion route.

### Track B: Ingestion Layer — "Many Roads In"

Pluggable adapters that feed the accounting core. Currently coupled to SpreadsheetAdapter — needs extraction.

| Route | Status | Notes |
|-------|--------|-------|
| PDF extraction (Semantic Kernel) | ✅ Working | In SpreadsheetAdapter, needs extraction |
| QB/Xero import | ✅ Working | QBConnector project exists |
| CSV import | ✅ Working | CsvReaderStep in pipeline |
| Manual entry | ✅ Working | EntrySet aggregate |
| Plaid bank feeds | ❌ Not built | API integration |
| Classification rules (AI + rule-based) | ✅ Working | Ephemeral POCOs, needs aggregate |
| Human review gate | ❌ Not built | 85/15 boundary enforcement |

### Track C: Agent Infrastructure — "The Velocity Multiplier"

Skills, agents, and infrastructure that make Tracks A, S, and B agent-deliverable.

---

## Work Item Classification

| Tag | Meaning |
|-----|---------|
| 🧑 **Human** | Pattern establishment, architectural decisions, first-of-kind |
| 🤖 **Agent** | Follows existing skill/pattern — agent-deliverable |
| 🧑→🤖 **Human-then-Agent** | Human builds first instance, agents replicate |
| **A** | Accounting Core track |
| **S** | Seeder / Business Loader track |
| **B** | Ingestion Layer track |
| **C** | Agent Infrastructure track |

---

## Week 1 — Reports Get Richer + Demo

**Demo:** _"P&L and Cash Flow with monthly breakdowns. Fiscal year selector. All reports export to Excel and PDF."_

| # | Track | Item | Who | UI Visible |
|---|-------|------|-----|------------|
| 1.1 | A | P&L monthly breakdown | 🧑→🤖 | ✅ Monthly revenue + expense sections |
| 1.2 | A | Cash Flow monthly breakdown | 🤖 | ✅ Monthly operating/investing/financing |
| 1.3 | A | Fiscal year selector | 🧑 | ✅ Year dropdown drives all reports |
| 1.4 | A | Excel export (all 7 reports) | 🧑→🤖 | ✅ "Export to Excel" button |
| 1.5 | A | PDF export (all 7 reports) | 🤖 | ✅ "Export to PDF" button |

**Accounting Core state after Week 1:** 7 reports with temporal awareness + export. Fiscal year selection.

---

## Week 2 — New Reports + Agent Pipeline

**Demo:** _"4 new accounting reports. Faster startup."_

| # | Track | Item | Who | UI Visible |
|---|-------|------|-----|------------|
| 2.1 | A | DiagnosticRm | 🤖 | ✅ TB + AccountType + NI flag |
| 2.2 | A | ExceptionsRm | 🤖 | ✅ Accounts with both DR+CR activity |
| 2.3 | A | LedgerAllRm (flat transaction view) | 🤖 | ✅ Flat classified transaction listing |
| 2.4 | A | OpeningJournalRm | 🤖 | ✅ Opening balance entries |
| 2.5 | A | Faster startup (checkpoint pilot) | 🧑 | ✅ Visible perf improvement |
| 2.6 | C | Integrate `ICheckpointStore` + `DapperReadModelStore` | 🧑 | — |
| 2.7 | C | Integrate `ProcessAggregateBase` + `ProcessManagerBase` | 🧑 | — |
| 2.8 | C | Copy `starter/` agents + skills from rd-agentic-flow | 🧑 | — |
| 2.9 | C | Adapt `greylock-aggregate` recipe skill for PowerModels | 🧑 | — |
| 2.10 | C | Integrate review instructions + update testing skill | 🧑 | — |
| 2.11 | C | Write pipeline step SKILL.md | 🧑 | — |
| 2.12 | S | **Period-spanning seed** (12 months of transactions across 2 fiscal years) | 🧑→🤖 | — |
| 2.13 | S | Write seeder extension SKILL.md | 🧑 | — |

**Accounting Core state after Week 2:** 11 reports. Checkpoint infrastructure integrated. Agent pipeline operational. Period-spanning seed data enables Week 3 period close work.

---

## Week 3 — Period Close + Classification Rules

**Demo:** _"Period close workflow — accountant can close a month. Classification rules are persistent and auditable. Rules reference and audit trail reports."_

| # | Track | Item | Who | UI Visible |
|---|-------|------|-----|------------|
| 3.1 | A | **PeriodClose ProcessManager** | 🧑 | ✅ "Close Period" button — locks a period from further posting |
| 3.2 | A | **Adjusting journal entries** (pre-close corrections) | 🤖 | ✅ Adjusting JE type in journal UI |
| 3.3 | A | **Post-close Trial Balance** | 🤖 | ✅ New report: TB after closing entries |
| 3.4 | B | ClassificationRuleSet aggregate (persistent rules) | 🤖 | ✅ Rules visible in rules panel |
| 3.5 | B | RulesReferenceRm | 🤖 | ✅ New report: all active rules |
| 3.6 | B | RuleHitsRm | 🤖 | ✅ New report: which rule classified each transaction |
| 3.7 | A | Checkpoint remaining accounting RMs | 🤖 | — |
| 3.8 | A | Checkpoint SFM-related RMs (9 hottest) | 🤖 | — |
| 3.9 | B | Classification rules seed data | 🧑→🤖 | — |
| 3.10 | A | FiscalYearConfig on AccountingSystem | 🤖 | — |
| 3.11 | S | **Multi-company seed** (3+ businesses, varied CoA structures) | 🤖 | — |
| 3.12 | S | Industry template: tax prep scenario (Liberty Tax model) | 🤖 | — |

**Accounting Core state after Week 3:** 13 reports. **Period close is operational.** Adjusting entries. Post-close TB. Classification rules persistent. Multi-company seed proves accountant workload.

---

## Week 4 — Closing the Books + Ingestion Separation

**Demo:** _"Full closing-the-books workflow: adjusting entries → closing entries → post-close TB → retained earnings roll forward. Human review gate for classifications. Reconciliation."_

| # | Track | Item | Who | UI Visible |
|---|-------|------|-----|------------|
| 4.1 | A | **Closing entries** (revenue/expense → retained earnings) | 🧑→🤖 | ✅ Auto-generated closing JEs visible in journal |
| 4.2 | A | **Retained earnings roll-forward** | 🤖 | ✅ Balance Sheet shows correct retained earnings post-close |
| 4.3 | A | ReconciliationRm | 🤖 | ✅ New report: statement vs extracted totals |
| 4.4 | A | RunSummaryRm | 🤖 | ✅ New report: per-file intake overview |
| 4.5 | B | **Human review gate** (accept/reject/edit classifications) | 🧑→🤖 | ✅ Review UI — the 85/15 boundary in action |
| 4.6 | B | Enhanced rule types (regex, amount range, compound) | 🤖 | ✅ New rule type options |
| 4.7 | B | **Extract ingestion pipeline from SpreadsheetAdapter** | 🧑 | — |
| 4.8 | B | StatementImport aggregate | 🤖 | — |
| 4.9 | B | TransactionSplittingStep | 🤖 | — |

**Accounting Core state after Week 4:** 15 reports. **Closing the books works end-to-end.** Retained earnings correct. Reconciliation. Ingestion pipeline extraction begun.

---

## Week 5 — Fiscal Year Cycle + Polish

**Demo:** _"Complete fiscal year cycle: work through year → close periods → closing entries → year-end close → new year opens with correct balances. All 15 reports with Excel/PDF export. Full end-to-end demo."_

| # | Track | Item | Who | UI Visible |
|---|-------|------|-----|------------|
| 5.1 | A | **Year-end close process** (all periods closed → year finalized) | 🧑 | ✅ Year-end workflow in UI |
| 5.2 | A | **Opening balances for new fiscal year** (auto-generated from prior year close) | 🤖 | ✅ New year starts with correct balances |
| 5.3 | A | Verify Excel/PDF export for all 15 reports | 🤖 | ✅ All reports export correctly |
| 5.4 | A | CachingRepository for SFM | 🧑 | ✅ Fast startup |
| 5.5 | A | Full integration test — seed → post → close → report → export | 🧑 | ✅ End-to-end demo |
| 5.6 | B | Complete pipeline extraction — `DataIngestionPipeline` as standalone | 🧑 | — |
| 5.7 | B | Ingestion route test: seed data via CSV adapter | 🤖 | ✅ Demo CSV import path |
| 5.8 | S | **Extraction baseline test** — ingest seeded business via CSV, compare to seed output | 🤖 | ✅ Proves ingestion correctness against known-good data |
| 5.9 | S | 10+ company seed for scale/performance testing | 🤖 | — |

**Accounting Core state after Week 5:** Complete accounting cycle. Period close → closing entries → year-end → new year. 15 reports. All export. Ingestion validated against seeder baselines. **This is a real accounting system.**

---

## The Strategic Story (Week by Week)

| Week | What Stakeholders See | What It Proves |
|------|----------------------|----------------|
| **1** | Monthly financial statements, fiscal year selection, Excel/PDF export | PowerModels produces professional-grade financial output |
| **2** | 4 new diagnostic/working reports, faster startup | PowerModels has accountant-grade tools, not just statements |
| **3** | Period close, adjusting entries, post-close TB, classification rules with audit trail | **PowerModels enforces accounting period boundaries** — this is a real system |
| **4** | Closing the books, retained earnings, reconciliation, human review gate | **PowerModels completes the accounting cycle** — revenue/expense close to retained earnings |
| **5** | Full fiscal year cycle, all reports export, end-to-end demo | **PowerModels is a complete accounting system of record** |

This progression tells a story that investors and design partners understand: **not "we added more reports" but "we built a complete accounting cycle."**

---

## Why the Three Boundaries Matter

### For the Product

The accounting core works with ANY data source. A firm can:
- Import from QuickBooks for existing clients
- Use PDF extraction for shoebox clients
- Connect Plaid for bank-feed-driven bookkeeping
- Upload CSV exports from any system
- Manually enter adjusting entries

**All routes produce journal entries. The accounting core doesn't care.**

### For the Demo

The accounting core can be demo'd with **seeder data only** — no PDF extraction, no QB connection, no Plaid. The complete closing-the-books workflow is provable without touching the ingestion layer.

Ingestion routes are **additive demos**: "And here's how data gets IN — from PDF, from QuickBooks, from a bank feed."

Multi-company seeding shows the **real accountant experience**: managing 10+ businesses with different CoA structures, different industries, different fiscal years — all in one session.

### For Testing and Verification

The seeder creates a **three-layer verification model**:

1. **Seeder → Accounting Core** — deterministic: seed a business, assert report outputs match expected values. No external dependencies. Fast. Agent-runnable.
2. **Ingestion Route → Accounting Core** — same business data, ingested via PDF/QB/CSV instead of seeded. Output must match the seeder baseline.
3. **Cross-route comparison** — ingest the same source data via two different routes (e.g., CSV and PDF). Outputs must match each other AND the seeder baseline.

This makes ingestion correctness **provable against known-good data** rather than manually inspected.

### For the Architecture

The ingestion pipeline (`DataIngestionPipeline` + `IDataIngestionStep[]`) already exists in SpreadsheetAdapter. Extracting it to a standalone project (Week 4–5) creates a clean boundary:

```
PowerModels.Ingestion/
  ├── DataIngestionPipeline.cs
  ├── Steps/
  │    ├── CsvReaderStep.cs
  │    ├── PDFReaderStep.cs
  │    ├── ColumnMappingStep.cs
  │    ├── ClassificationRuleStep.cs
  │    ├── AIClassificationStep.cs
  │    └── JournalPostingStep.cs      ← produces Journal commands
  ├── Adapters/
  │    ├── QuickBooksAdapter.cs
  │    ├── PlaidAdapter.cs (future)
  │    └── CsvAdapter.cs
  └── Review/
       └── HumanReviewGate.cs          ← 85/15 boundary
```

Each adapter is independently testable, independently deployable, independently demo-able — and verifiable against seeder baselines.

### For Agents

- **Accounting Core track:** Agents follow the recipe-grade ES-DDD skill to build aggregates (PeriodClose, ClosingEntries), the accounting-reports skill to build reports, and the process manager skill for the period close workflow.
- **Seeder track:** Agents follow the seeder extension SKILL to add new multi-company scenarios, industry templates, and period-spanning test data. Every new accounting feature gets a corresponding seed extension.
- **Ingestion track:** Agents follow the pipeline step SKILL to add new ingestion steps or adapters.
- **The tracks don't interfere.** An agent working on a new report doesn't need to know about PDF extraction. An agent adding a Plaid adapter doesn't need to know about period close. An agent extending seed data doesn't need to know about either.

---

## Agent Readiness by Track

| After Week | Track A (Accounting Core) | Track S (Seeder) | Track B (Ingestion) |
|------------|--------------------------|------------------|---------------------|
| **1** | Monthly breakdowns, export variants | — | — |
| **2** | Any report from spec, any aggregate from spec, RM checkpointing | New seed scenarios (follows seeder SKILL) | Pipeline steps |
| **3** | Period-aware reports, closing-related workflows | Multi-company seeds, industry templates | Classification rule extensions |
| **4** | Year-end reports, retained earnings logic | Year-spanning seeds, extraction baselines | New ingestion adapters, review gate variants |
| **5** | Full accounting cycle features | Scale seeds, cross-route validation | New ingestion routes (Plaid, etc.) |

---

## Work Item Summary

| Track | Total | 🧑 Human | 🤖 Agent | 🧑→🤖 |
|-------|-------|----------|----------|--------|
| **A — Accounting Core** | 22 | 7 | 13 | 2 |
| **S — Seeder** | 7 | 1 | 4 | 2 |
| **B — Ingestion Layer** | 10 | 2 | 6 | 2 |
| **C — Agent Infrastructure** | 6 | 6 | 0 | 0 |
| **Total** | **45** | **16** | **23** | **6** |

Agent-delivered: 29 of 45 items (64%). Human work concentrated in Weeks 1–2 (patterns + infrastructure + first seed). Agent work dominates Weeks 3–5 — including seed scenario extensions, which compound testing coverage.

---

## Success Metrics

| Metric | Target | Track |
|--------|--------|-------|
| **Accounting cycle complete** | Period close → closing entries → year-end → new year | A |
| Reports shipping | 15 (including post-close TB) | A |
| All reports export | Excel + PDF for all 15 | A |
| Period close operational | Close/reopen periods, posting lockout | A |
| Closing entries auto-generate | Revenue/expense → retained earnings | A |
| Seeded companies | 10+ businesses with varied CoA | S |
| Period-spanning seed | 2+ fiscal years of transactions | S |
| Industry templates | Tax prep scenario (Liberty Tax model) | S |
| Extraction baseline test | CSV ingestion matches seeder output | S |
| Ingestion routes working | PDF, QB, CSV (3 of 5) | B |
| Pipeline extracted | Standalone `DataIngestionPipeline` project | B |
| Human review gate | Accept/reject/edit workflow | B |
| RMs checkpointed | 16+ | C |
| Agent pipeline operational | End of Week 2 | C |
| Agent-delivered items | 29 of 45 (64%) | C |
| Weekly visible progress | Every week has UI-demonstrable features | All |

---

## Related Documents

- [Monday Demo Plan](monday-demo-plan-2026-03-28.md) — Week 1 detail
- [Gap & Conflict Analysis](gap-conflict-analysis-2026-03-28.md) — technical gaps + rd-agentic-flow
- [Strategic Critique](strategic-critique-roadmap-2026-03-28.md) — gap analysis of v2 approach
- [Unified Roadmap v2](unified-roadmap-v2-2026-03-28.md) — predecessor (feature-count-driven)
- [Unified Roadmap v1](unified-roadmap-2026-03-28.md) — predecessor (phase-only)
