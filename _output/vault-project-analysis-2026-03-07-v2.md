# PowerModels Project Analysis v2 — Full Vault + GitHub History

Generated: 2026-03-07
Sources:
- Implementation vault: scan-data.json, 9 architecture docs, 6 artifact docs
- Planning vault: 253 intersections (23 facets × 11 audiences)
- GitHub: 317 issues, 240 merged PRs, 9-month window (event-systems/powermodels)

---

## 1. Codebase Health Summary

| Metric | Value | Assessment |
|--------|-------|------------|
| Aggregates | 27 production | Appropriate for domain complexity |
| Commands | 249 | High — 67% route through one handler |
| Events | 443 | 1.78× command count (healthy ratio) |
| Handler Services | 22 | Well-distributed across contexts |
| Read Models | 80 (46 ReadModelBase + 34 TransientSubscriber) | Per-category-stream fan-out: max 9× on `$ce-ServerFinancialModel`; TransientSubscribers are bus-only, zero startup cost |
| Test Files | 154 | Coverage breadth is good, depth is not |
| Issues (9mo) | 317 (229 feature, 66 defect, 16 epic) | Active development, healthy ratio |
| Merged PRs (9mo) | 240 (79% linked to issues) | Strong traceability |
| Milestones | 10 in window | Shoebox RC1→RC5 is the primary arc |

## 2. The Three Compounding Risks

The vault data, GitHub history, and Q-analysis converge on three risks that compound each other. Each one makes the others worse.

### Risk A: God Aggregate — ServerFinancialModel

| Metric | Value | Source |
|--------|-------|--------|
| Lines | 3,557 | scan-data.json |
| Apply methods | 87 (vs 31 Register calls) | scan-data.json |
| Commands via handler | 166 of 249 (67%) | message-map.md |
| PRs touching it (9mo) | 9 — all large (26-94 files each) | feature-provenance.md |
| Features that expanded it | Semantic formulas, single-line tables, trial balance, mapping ops | feature-provenance.md |
| Defects (direct) | 0 | defect-analysis.md |
| Defects (via read models) | 7 (FinancialModelRm: 4, ModelTemplateRm: 2, FinancialModelService: 1) | defect-analysis.md |
| Missing source ctor | Yes | aggregates.md |
| RestoreFromEvents test | No | test-coverage.md |

**Why it compounds:** Every new feature that touches ServerFinancialModel adds events to a stream that is already the largest per business, replayed most frequently, and subscribed to by the most read models. The 9 PRs that touched it in 9 months each changed 26-94 files — showing that God Aggregate changes cascade across the entire codebase.

### Risk B: All Sagas Are Implicit

| Metric | Value | Source |
|--------|-------|--------|
| Workflow chains identified | 7 | saga-catalog.md |
| Explicit process managers | 0 | saga-catalog.md |
| Domain-crossing PRs | 38 | PR file path analysis |
| Highest-risk saga | Reconciliation (5 contexts, 5 aggregates, 19 related defects) | saga-catalog.md + defect-analysis.md |

**Why it compounds:** Without explicit sagas, there's no compensation logic. When the Data Import Pipeline commits ~1,001 events and something fails downstream, there's no rollback. When the Business Setup creates 50-80 streams and gets stuck (issue #2109), there's no recovery path. Implicit sagas also mean no visibility — you can't answer "what step is this import on?" without tracing through handler code.

### Risk C: SpreadsheetAdapter is the Defect Funnel

| Metric | Value | Source |
|--------|-------|--------|
| Read models in SpreadsheetAdapter | 40 | bounded-contexts.md |
| Defect rate | 15% (highest of all contexts) | defect-analysis.md |
| Defect-affected read models | 13 of 16 are in SpreadsheetAdapter | defect-analysis.md |
| Top defect RMs | DataSourceRm (4), FinancialModelRm (4), DataSourceListRm (3), ChartOfAccountsRm (3) | defect-analysis.md |
| Role | ACL bridge: domain events → UI-consumable projections | bounded-contexts.md |

**Why it compounds:** SpreadsheetAdapter is where all 7 sagas surface their state to the user. Every saga workflow terminates in SpreadsheetAdapter read models. This is why defects cluster here — it's the translation boundary where domain event semantics meet UI expectations. Adding more sagas or more read models makes this worse.

### The Compound Effect

```
God Aggregate expansion
  → More events per stream
    → Longer replay on every command
      → More read model subscriptions needed
        → Higher SpreadsheetAdapter complexity
          → More defects in the ACL bridge
            → More implicit saga coordination to manage
              → More God Aggregate coupling
                → (cycle repeats)
```

## 3. What GitHub History Proves

### The Reconciliation Saga is the #1 Defect Generator

| Evidence | Value |
|----------|-------|
| Reconciliation-related defects | **19 of 66 total (29%)** |
| Reconciliation epics | 4 (#1846, #1851, #1869, #1905, #1946) |
| Affected milestones | RC1 through RC5 + March Demo |
| Root causes | PDF/CSV parsing edge cases, date formats, amount sign interpretation, account mapping |

The reconciliation workflow spans 5 bounded contexts, touches 5 aggregates, has 4 dedicated epics, and generates nearly a third of all defects. It's also the most complex implicit saga with no compensation logic.

### Stable Aggregates Are Actually Stable

9 aggregates have had **zero PRs in 9 months**:

| Aggregate | Lines | Context | Stability Signal |
|-----------|-------|---------|-----------------|
| ManualTable | 365 | ModelServer | ✅ Mature — 11 Apply methods, well-tested |
| ClientWorkspace | 247 | ModelServer | ✅ Mature — setup-only, rarely changed |
| ReferenceDataSeries | 172 | ModelServer | ⚠️ Missing source ctor — dormant risk |
| EntrySet | 169 | ModelServer | ✅ Mature |
| ManagedFinancialModel | 125 | TeamModelMgmt | ✅ Mature |
| TeamSettings | 105 | TeamModelMgmt | ✅ Mature |
| UserDefinedWorksheets | 69 | ModelServer | ✅ Mature |
| DataSourceMapping | 61 | ModelServer | ⚠️ Missing source ctor — dormant risk |
| AccountingSystem | 29 | ModelServer/AS | ⚠️ Missing source ctor — dormant risk |

Stability validates that these aggregates have settled into their final form. The three with missing source ctors are dormant risks — they'll fail silently if their correlation tracking is ever needed.

### ChartOfAccounts is the Most Active Aggregate

14 PRs touched ChartOfAccounts in 9 months — more than ServerFinancialModel (9). Features include CoA setup, account subtypes, filtering, reconciliation prep. This suggests **ChartOfAccounts is the current growth vector** while ServerFinancialModel is stabilizing after the table mapping phase.

### Feature Velocity Shows a Clear Arc

| Phase | Period | Issues | Character |
|-------|--------|--------|-----------|
| Beta stabilization | Jun 2025 | 65 | Backlog seeded, controlled beta bugs fixed |
| Shoebox buildout | Jul–Nov 2025 | 139 | RC1→RC3, reconciliation, counterparties, file import |
| Reconciliation rework | Dec 2025–Feb 2026 | 113 | RC4 (55 features), RC5, standalone app |
| March demo | Mar 2026 | 11 | Accounting phase 2, opening balances |

The project is transitioning from **data import/reconciliation** (Shoebox) to **accounting workflows** (opening balances, accrual accounting). This means:
- ChartOfAccounts and AccountBalance are the next growth aggregates
- New sagas will emerge around journal entry creation and accounting period management
- The reconciliation saga should stabilize as focus shifts

## 4. Q-Analysis Enriched by GitHub Data

### Strongest Facet-Audience Connections (Dim 3+)

| Facet | Dim-3+ Count | GitHub Validation |
|-------|-------------|-------------------|
| **unit-economics** | 9 of 11 audiences | Universal — but no direct GitHub features map to it. This is a *communication* facet, not an *implementation* facet. |
| **reconciliation-elimination** | 4 audiences | **Directly validated**: RC4's 55-feature reconciliation rework, 19 related defects, 4 epics. Most active implementation area. |
| **shoebox-offering** | 4 audiences | **Directly validated**: 12 Shoebox-labeled features, entire RC1→RC5 milestone arc is Shoebox-driven. |
| **competitive-positioning** | 4 audiences | Indirectly validated — Shoebox + standalone app differentiate from competitors. |
| **event-sourced-architecture** | 2 audiences | **Risk validated**: God Aggregate, 0 RestoreFromEvents tests, implicit sagas. Architecture is real but undertested. |
| **drift-metric** | 2 audiences | New metric category — could be enriched with defect rate trends and feature velocity data. |

### The Communication Gap Is Real

Technical facets (event-sourced-architecture, application-topology, data-usage-and-storage, domain-model-structure, message-bus-architecture, isolation-acl) show Dim-0 against business audiences. GitHub data confirms why: **none of the 229 features in 9 months were described in architectural terms.** Features are described as user stories ("A user can reconcile transactions"), not architecture stories.

This means the vault's Q-analysis is correctly identifying that the architecture narrative needs translation for non-technical audiences. The bridge: connect architectural improvements to feature velocity — "extracting the God Aggregate will reduce average PR size from 50 files to 15 files for model-related features."

## 5. Defect Hotspot Map

### By Root Cause Category (cross-referenced)

| Pattern | Defects | Saga | Context | Root Cause |
|---------|---------|------|---------|-----------|
| Transaction import corruption | 8 | Data Import Pipeline | SpreadsheetAdapter | PDF/CSV parsing: date formats, amount signs, numeric tokens in descriptions |
| Read model state sync | 6 | All sagas | SpreadsheetAdapter | RM not updating — race conditions, missing event handlers |
| UI component state | 5 | N/A | UIBehavior/WPF | WPF component state out of sync with RM |
| Account mapping errors | 4 | Reconciliation | SpreadsheetAdapter | Incorrect account resolution during categorization |
| Business setup workflow | 3 | Business Setup | ModelServer→SpreadsheetAdapter | Multi-step creation stuck or greyed out |
| **Total classified** | **26 of 66** | | | 40 defects are miscellaneous/unclassified |

### Defect Rate by Milestone

| Milestone | Features | Defects | Rate | Signal |
|-----------|---------|---------|------|--------|
| Controlled Beta - u4 | 8 | 9 | 53% | 🔴 Bug fix milestone |
| Shoebox RC1 | 12 | 7 | 37% | 🟡 Early stabilization |
| Shoebox RC2 | 16 | 8 | 33% | 🟡 Continued stabilization |
| Demo Update - March | 17 | 8 | 32% | 🟡 Active development |
| Shoebox RC5 | 25 | 10 | 29% | 🟡 RC4 defects surfacing |
| Shoebox Public Beta 1 | 20 | 4 | 17% | 🟢 Moderate |
| **Shoebox RC4** | **55** | **4** | **7%** | 🟢 Best ratio — reconciliation rework was clean |
| Shoebox RC3 | 30 | 3 | 9% | 🟢 Healthy |

**Key insight:** RC4 (the largest milestone at 55 features) had the *lowest* defect rate (7%). The reconciliation rework was well-executed. RC5's higher rate (29%) is mostly RC4 edge cases surfacing in production.

## 6. Aggregate Risk Matrix (Full Cross-Reference)

| Aggregate | Lines | PRs (9mo) | Defects (via RM) | RestoreFromEvents | Source Ctor | Test Files | Risk |
|-----------|-------|----------|------------------|-------------------|-------------|------------|------|
| **ServerFinancialModel** | 3,557 | 9 | 7 | NO | NO | 40 | 🔴 God Aggregate |
| **ChartOfAccounts** | 540 | 14 | 3 (ChartOfAccountsRm) | NO | Yes | 40 | 🟡 Most active, growing |
| TasklistItem | 340 | 6 | 0 | NO | Yes | 7 | 🟢 Active, clean |
| DataTableMap | 370 | 4 | 1 (DataTableMapRm) | NO | Yes | 5 | 🟢 Stabilizing |
| ListDataTableMap | 369 | 4 | 1 (ListDataTableMapRm) | NO | Yes | 4 | 🟢 Stabilizing |
| CustomStatementTxType | 179 | 4 | 0 | NO | Yes | 12 | 🟢 Active, clean |
| ManualTableMap | 306 | 4 | 2 (ManualTableMapRm) | NO | Yes | 4 | 🟢 Stabilizing |
| DataSource | 271 | 3 | 4 (DataSourceRm) | NO | Yes | 8 | 🟡 RM defect hotspot |
| AccountBalance | 106 | 2 | 1 (AccountBalancesRm) | NO | Yes | 8 | 🟡 Next growth area |
| ReferenceDataSeries | 172 | 0 | 0 | NO | NO | 3 | ⚠️ Dormant — missing source ctor |
| DataSourceMapping | 61 | 0 | 0 | NO | NO | 3 | ⚠️ Dormant — missing source ctor |
| AccountingSystem | 29 | 0 | 0 | NO | NO | 44 | ⚠️ Dormant — missing source ctor |

## 7. Prioritized Action Items (Updated)

### P0 — Existential Risk (Validate Before Shipping)

| # | Action | Evidence | Effort |
|---|--------|----------|--------|
| 1 | **RestoreFromEvents test for ServerFinancialModel** | 0/27 tested, 87 Apply methods, 3,557 lines, 7 defects via RMs | 2-3 days |
| 2 | **RestoreFromEvents tests for all 27 aggregates** | 0/27 tested — if replay breaks, everything breaks | 1-2 weeks |
| 3 | **Fix 4 missing source constructors** (ServerFinancialModel, ReferenceDataSeries, DataSourceMapping, AccountingSystem) | 3 of 4 are dormant — will fail silently when needed | 1 day |

### P1 — Structural Debt (Address in Next Milestone)

| # | Action | Evidence | Effort |
|---|--------|----------|--------|
| 4 | **Extract table-mapping events from ServerFinancialModel** (20 events → dedicated aggregates exist) | god-aggregate.md: 65% of registered events are mapping ops; feature-provenance: 9 PRs with 26-94 file changes each | 1-2 weeks |
| 5 | **Explicit reconciliation saga** — extract coordination from SpreadsheetAdapter services into a dedicated process manager | saga-catalog.md: implicit, 5 contexts, 19 defects (29% of all); highest-risk saga | 2-3 weeks |
| 6 | **SpreadsheetAdapter RM audit** — review 40 read models, consolidate where possible, add integration tests | defect-analysis.md: 15% defect rate, 13/16 defect-hit RMs are in SpreadsheetAdapter | 1-2 weeks |

### P2 — Performance Scaling (Address Before Scale)

| # | Action | Evidence | Effort |
|---|--------|----------|--------|
| 7 | **Snapshot support for ServerFinancialModel** | data-usage-and-storage.md: largest stream, full replay per command, no snapshots | 2-3 weeks |
| 8 | **Audit ModelTemplateRm** (82 event subscriptions) | read-models.md: subscribes to 19% of all event types; defect-analysis: 2 defect PRs | 2-3 days |
| 9 | **Data import pipeline compensation** — add rollback capability for failed CSV/PDF imports | saga-catalog.md: ~1,001 events committed with no rollback; defect-analysis: 8 import corruption defects | 1-2 weeks |

### P3 — Strategic Communication (Address for Fundraising/Board)

| # | Action | Evidence | Effort |
|---|--------|----------|--------|
| 10 | **Translate architecture improvements into business metrics** | Q-analysis: 6 technical facets × all business audiences = Dim-0; feature-provenance: 0 features described in arch terms | 1 week |
| 11 | **Enrich drift-metric facet with defect trend data** | defect-analysis: monthly trend available; feature-provenance: velocity data | 2-3 days |

## 8. Feature Trial Candidates (Updated with GitHub Evidence)

### Trial A: God Aggregate Extraction (Analysis-Driven)

**Extract DataTable mapping from ServerFinancialModel**

| Support | Evidence |
|---------|----------|
| Structural | god-aggregate.md: 20 extractable events, dedicated aggregates already exist |
| Historical | feature-provenance: 9 PRs changed 26-94 files each — cascade effect is measured |
| Defect | defect-analysis: 7 defects via FinancialModelRm/ModelTemplateRm |
| Saga | saga-catalog: Financial Model Mapping workflow shows exactly where coupling occurs |
| Q-analysis | event-sourced-architecture × founding-team = Dim-3+ |

**Vault coverage: 5 of 9 architecture docs directly relevant.** This is the best-instrumented change in the vault.

### Trial B: Explicit Reconciliation Saga (Defect-Driven)

**Extract reconciliation coordination into a dedicated process manager**

| Support | Evidence |
|---------|----------|
| Defect | defect-analysis: 19 defects (29% of total), #1 defect pattern |
| Saga | saga-catalog: 5 contexts, 5 aggregates, implicit coordination, no compensation |
| Feature | feature-provenance: 4 epics, RC4's entire feature set |
| Scale | data-usage-and-storage: ~1,001 events per import × per-stream fan-out (max 9× on hottest stream) |
| Q-analysis | reconciliation-elimination × 4 audiences = Dim-3+ |

**This trial has the strongest defect justification.** It also validates the new defect-analysis and saga-catalog docs — if the explicit saga reduces defect rate, it proves the vault's analysis is actionable.

### Trial C: Opening Balance / Accounting Phase 2 (User Workflow)

**Implement the opening balance workflow for the March demo**

| Support | Evidence |
|---------|----------|
| Active | Issues #2106, #2107, #2108, #2109 — current milestone work |
| Saga | Business Setup saga (saga-catalog.md) — 50-80 streams created |
| Defect | 3 business setup defects including #2109 "stuck loading" |
| Growth | AccountBalance aggregate is next growth vector (2 PRs already) |
| Risk | Crosses 4 contexts, could expand God Aggregate if not careful |

**This trial tests the vault with live feature work** — can agents use the implementation docs to avoid expanding the God Aggregate while implementing the March demo features?

## 9. Updated Summary of All Implementation Vault Docs

| Doc | Source | Key Data Points |
|-----|--------|----------------|
| scan-data.json | Codebase scan | 27 aggregates, 249 commands, 443 events, 22 handlers, 80 RMs |
| aggregates.md | Codebase scan | Aggregate sizes, patterns, god-aggregate flag |
| message-map.md | Codebase scan | Command → handler routing |
| read-models.md | Codebase scan | RM subscriptions and output shapes |
| bounded-contexts.md | Codebase scan | Context ownership map |
| test-coverage.md | Codebase scan | 0/27 RestoreFromEvents, 24/27 idempotency |
| god-aggregate.md | Codebase scan | 3,557 lines, 87 Apply, expansion triggers |
| domain-model.md | Hand-authored | Entity hierarchy, relationships |
| application-topology.md | Hand-authored | Hosts, databases, isolation |
| project-architecture.md | Hand-authored | Dependency graph, layer rules |
| message-bus-topology.md | Hand-authored | Bus types, namespaces |
| isolation-acl.md | Hand-authored | Context hierarchy, ACL boundaries |
| data-usage-and-storage.md | Hand-authored | Streams, volumes, amplification, persistence |
| **feature-provenance.md** | **GitHub extraction** | **229 features, 10 milestones, aggregate touch history** |
| **saga-catalog.md** | **GitHub extraction** | **7 workflow chains, all implicit, risk matrix** |
| **defect-analysis.md** | **GitHub extraction** | **66 defects, context/RM hotspots, recurring patterns** |

The vault now has three data layers:
1. **What exists** (scan artifacts)
2. **What it looks like structurally** (architecture docs)
3. **Why it exists and where it breaks** (GitHub history)
