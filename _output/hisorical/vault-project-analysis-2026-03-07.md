# PowerModels Project Analysis — Vault-Driven

Generated: 2026-03-07
Sources: Implementation vault (scan-data.json, architecture docs) + Planning vault (253 intersections, 23 facets × 11 audiences)

---

## 1. Codebase Health Summary

| Metric | Value | Assessment |
|--------|-------|------------|
| Aggregates | 27 production | Appropriate for domain complexity |
| Commands | 249 | High — driven by God Aggregate |
| Events | 443 | 1.78× command count (healthy ratio) |
| Handler Services | 22 | Well-distributed across contexts |
| Read Models | 80 (46 ReadModelBase + 34 TransientSubscriber) | Per-category-stream fan-out: max 9× on `$ce-ServerFinancialModel`; TransientSubscribers are bus-only, zero startup cost |
| Test Files | 154 | Coverage breadth is good, depth is not |

## 2. Critical Risk: God Aggregate

**ServerFinancialModel is the single biggest risk in the codebase.**

| Metric | Value | Severity |
|--------|-------|----------|
| Lines of code | 3,557 | 🔴 6.6× the next largest aggregate (ChartOfAccounts: 540) |
| Apply methods | 87 | 🔴 2.8× its Register calls (31) — indicates overloaded event handling |
| Commands via FinancialModelService | 166 | 🔴 67% of all commands route through one handler |
| Registered events | 31 | 🟡 Many are table-mapping events that could be separate aggregates |

**Why this matters:**
- Every command to FinancialModelService replays the full ServerFinancialModel event stream
- This is the largest stream per business and the most frequent replay target
- With no snapshots, a mature financial model with thousands of edits means thousands of events replayed per command
- Any new feature that touches the financial model amplifies this problem

**Extraction candidates** (events that could be separate aggregates):
- **DataTable mapping** (DataTableMappedToModel, DataTableMapRowAdded, etc.) — 8 events, already have separate DataTableMap aggregate but events still register in ServerFinancialModel
- **ListDataTable mapping** (ListDataTableMappedToModel, etc.) — 7 events, same pattern
- **ManualTable mapping** (ManualTableMappedToModel, etc.) — 3 events
- **UserDefinedWorksheets** (UserDefinedWorksheetsAdded/Removed) — 2 events, already has own aggregate

These 20 events (65% of ServerFinancialModel's registered events) represent table-mapping operations that have dedicated aggregates but still raise events through the God Aggregate. Decoupling would reduce replay cost significantly.

## 3. Critical Risk: Zero RestoreFromEvents Tests

**0 of 27 aggregates have RestoreFromEvents tests.** This is the most important test in event-sourced systems — it verifies that an aggregate can be reconstructed from its event stream.

| Test Pattern | Coverage |
|--------------|----------|
| RestoreFromEvents | **0/27** 🔴 |
| Idempotency tests | 24/27 (89%) |
| State guard tests | 25/27 (93%) |

**Why this matters:**
- Without RestoreFromEvents tests, there's no automated verification that event replay produces correct state
- Any change to Apply methods could silently corrupt aggregate reconstruction
- This is the foundation of event sourcing — if replay breaks, everything breaks
- The God Aggregate (87 Apply methods) is the highest-risk target

**Priority order for adding RestoreFromEvents tests:**
1. ServerFinancialModel (87 Apply methods, highest replay frequency)
2. ChartOfAccounts (540 lines, 9 Apply methods, every business has one)
3. TasklistItem (340 lines, 16 Apply methods)
4. DataTableMap, ListDataTableMap, ManualTable, ManualTableMap (300-370 lines each)
5. All remaining aggregates

## 4. Structural Pattern Issues

### Missing Source Constructor (4 aggregates)

| Aggregate | Lines | Risk |
|-----------|-------|------|
| ServerFinancialModel | 3,557 | 🔴 God Aggregate — highest risk |
| ReferenceDataSeries | 172 | 🟡 Medium — 7 Apply methods |
| DataSourceMapping | 61 | 🟢 Low — small aggregate |
| AccountingSystem | 29 | 🟢 Low — trivial aggregate |

The `:base(source)` constructor pattern is required for proper correlation tracking in ReactiveDomain. Missing it means these aggregates can't properly attribute their events to a source command.

### Read Model Concentration

Top 5 read models by event subscription count:

| Read Model | Events | Context | Risk |
|------------|--------|---------|------|
| ModelTemplateRm | 82 | ModelServer | 🔴 Subscribes to 19% of all events |
| FinancialModelTablesRm | 54 | UIBehavior | 🟡 Heavy UI-layer subscriber |
| FinancialModelRm | 49 | SpreadsheetAdapter | 🟡 ACL-layer bridge |
| WorkspaceTasklistRm | 21 | SpreadsheetAdapter | 🟢 Reasonable |
| SingleClientTasklistRm | 21 | SpreadsheetAdapter | 🟢 Reasonable |

ModelTemplateRm subscribing to 82 events is concerning — it processes nearly 1 in 5 events in the system. Any performance issue here affects every event written.

## 5. Handler Service Distribution

| Service | Commands | % of Total | Assessment |
|---------|----------|------------|------------|
| FinancialModelService | 166 | 67% | 🔴 Extreme concentration |
| AccountingSystemService | 34 | 14% | 🟢 Multi-aggregate, appropriate |
| TasklistService | 20 | 8% | 🟢 Single-aggregate, well-scoped |
| All others (19 services) | 29 | 11% | 🟢 Well-distributed |

**FinancialModelService alone handles 2/3 of all commands.** This mirrors the God Aggregate — the handler is as concentrated as the aggregate it serves.

## 6. Bounded Context Health

| Context | Aggregates | Handlers | Read Models | Assessment |
|---------|-----------|----------|-------------|------------|
| ModelServer | 13 | 12 | 9 | 🟡 Large but cohesive — contains God Aggregate |
| ModelServer/AccountingSystem | 12 | — | — | 🟢 Clean sub-context |
| SpreadsheetAdapter | — | 5 | 40 | 🟡 40 read models is high — this is the ACL bridge |
| UIBehavior | — | 2 | 23 | 🟢 UI-layer, read-heavy is expected |
| TeamModelMgmt | 2 | 2 | 5 | 🟢 Clean, well-bounded |
| Connectors | — | 1 | — | 🟢 Minimal |
| App | — | — | 1 | 🟢 Minimal |
| Excel | — | — | 2 | 🟢 Minimal |

SpreadsheetAdapter has 40 read models — it's the biggest contributor to the ~60 subscriber fan-out. This is architecturally correct (it's the ACL bridge translating domain events for the UI) but it's the primary driver of read amplification.

## 7. Data Usage Risk Profile

From `data-usage-and-storage.md`:

| Risk | Current Impact | Trajectory |
|------|---------------|------------|
| Startup time | Linear with event count | 🔴 Worsens with every import/edit |
| Command latency | Linear with aggregate stream size | 🔴 ServerFinancialModel worst case |
| Memory | Entire DataStore in RAM | 🟡 Manageable at current scale (3-6MB) |
| Disk | Append-only, no compaction | 🟡 Manageable now, unbounded growth |
| Read amplification | Per-category-stream (max 9× on `$ce-ServerFinancialModel`) | 🔴 Grows when new RMs subscribe to hot streams |

**The compounding problem:** Every new feature that adds a ReadModelBase RM subscribing to `$ce-ServerFinancialModel` increases the fan-out on the hottest stream. Current trajectory: the 9 RMs on that stream each replay the full SFM event history on startup.

## 8. Planning Vault Q-Analysis

### Strongest Connections (Dimension 3+)

35 intersections have 3+ hooks — these are where the product story is strongest:

| Facet | Dim-3+ Audiences | Signal |
|-------|-------------------|--------|
| **unit-economics** | 9 of 11 audiences | Universal resonance — everyone cares about the business model |
| **competitive-positioning** | 4 audiences (investors, board, founding-team) | Strong investor/governance story |
| **reconciliation-elimination** | 4 audiences (investors, board, founding-team) | Core value prop connects broadly |
| **shoebox-offering** | 4 audiences (firm-partners, bookkeepers, founding-team, tax-preparers) | Strong practitioner resonance |
| **event-sourced-architecture** | 2 (CFOs, founding-team) | Technical moat understood by few |
| **drift-metric** | 2 (CFOs, founding-team) | Operational KPI resonates with financial users |
| **future-ledger** | 2 (CFOs, founding-team) | Deterministic forecasting = CFO language |

### Weakest Connections (Dimension 0)

78 intersections have zero hooks. The pattern is clear:

| Facet Group | Dim-0 Count | Why |
|-------------|-------------|-----|
| Technical architecture facets (6) | 63 | Technical facets don't connect to business audiences |
| Business facets × data-usage-reviewer | 15 | New technical audience doesn't connect to business facets |

**This is structurally correct** — technical architecture facets (domain-model, message-bus, isolation-acl, etc.) shouldn't have hooks into business audiences. These dimension-0 gaps are not problems to fix; they're confirmation that the facet/audience taxonomy is properly separated.

### Strategic Insight: The Q-Hole Pattern

The dimension-0 technical facets reveal a communication gap:
- **Founding team** is the only audience that connects to *both* technical and business facets
- **Investors and board** connect strongly to business facets but weakly to technical ones
- **Practitioners** (firm-partners, bookkeepers, tax-preparers) connect to workflow facets but not architecture

This means: **the technical architecture story (event sourcing, isolation, data usage) is being told to the wrong audiences.** The vault shows these facets only resonate with founding-team and CFOs. For investors and board, the architecture should be translated into business terms (competitive moat, scaling efficiency, infrastructure cost) rather than presented as technical detail.

## 9. Prioritized Action Items

### P0 — Existential Risk

| # | Action | Source | Effort |
|---|--------|--------|--------|
| 1 | **Add RestoreFromEvents test for ServerFinancialModel** | test-coverage.md: 0/27 | 2-3 days |
| 2 | **Add RestoreFromEvents tests for all aggregates** | test-coverage.md | 1-2 weeks |
| 3 | **Fix missing source constructors** (ServerFinancialModel, ReferenceDataSeries, DataSourceMapping, AccountingSystem) | aggregates.md | 1 day |

### P1 — God Aggregate Decomposition

| # | Action | Source | Effort |
|---|--------|--------|--------|
| 4 | **Extract table-mapping events** from ServerFinancialModel (20 events → dedicated aggregates already exist) | god-aggregate.md | 1-2 weeks |
| 5 | **Split FinancialModelService** into focused handlers aligned with extracted concerns | message-map.md: 166 commands | 1 week |

### P2 — Performance Scaling

| # | Action | Source | Effort |
|---|--------|--------|--------|
| 6 | **Add snapshot support** for ServerFinancialModel (largest stream, most-replayed) | data-usage-and-storage.md | 2-3 weeks |
| 7 | **Audit ModelTemplateRm** (82 event subscriptions — does it need all of them?) | read-models.md | 2-3 days |
| 8 | **Measure actual startup times** at different business maturity tiers | data-usage-and-storage.md | 1 day |

### P3 — Strategic Communication

| # | Action | Source | Effort |
|---|--------|--------|--------|
| 9 | **Translate architecture facets into business language** for investor/board audiences | Q-analysis dim-0 gaps | 1 week |
| 10 | **Strengthen shoebox-offering hooks** for investor audiences (currently dim-0 for investors) | intersection analysis | 2-3 days |

## 10. Feature Trial Candidates

Based on the vault analysis, these are the best candidates for end-to-end system trials:

### Trial A: Analysis-Driven (God Aggregate Extraction)

**Extract DataTable mapping from ServerFinancialModel**
- Vault data clearly identifies the problem (god-aggregate.md, 20 extractable events)
- Tests the architecture-reviewer agent's ability to enforce boundaries
- Tests the implementer agent's ability to use message-map.md for routing
- Validates the data-usage-reviewer's stream impact assessment
- High-value: reduces the #1 technical risk

### Trial B: User Workflow / ACL

**Add a new data source type (e.g., API-based real-time feed)**
- Crosses all layers: DataSource aggregate → DataSourceService → SpreadsheetAdapter → UIBehavior
- Tests the full ACL chain documented in isolation-acl.md
- Requires new read models (tests read amplification impact)
- Tests planner agent's ability to check domain-model.md for existing concepts
- Moderate complexity, high vault-coverage
