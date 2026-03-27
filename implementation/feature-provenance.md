# Feature Provenance

Last reviewed: 2026-03-27

Source: GitHub Issues & PRs (`event-systems/powermodels`)
Period: 2025-06-01 — 2026-03-27 (10 months)
Issues: 317+ (229+ features, 66+ defects, 16+ epics, 6+ tech-debt)
PRs: 240+ merged (189+ with issue links, ~79% linkage rate)
Authors: joshkempner (202+), dependabot (44+), farouk-it (23), thefringeninja (11), condron (8), tombissaillon (3)
Branch: `joshkempner/journal-aggregate` (active development branch)

## By Milestone

| Milestone | Features | Defects | Epics | PRs | Period | Status |
|-----------|---------|---------|-------|-----|--------|--------|
| Controlled Beta Release - update 4 | 8 | 9 | 0 | — | 2025-06 | Closed |
| Controlled Beta Release - update 5 | 3 | 1 | 0 | — | 2025-06 to 2025-07 | Closed |
| Shoebox Public Beta 1 | 20 | 4 | 0 | 3 | 2025-06 to 2025-07 | Closed |
| Shoebox Release Candidate 1 | 12 | 7 | 0 | 2 | 2025-06 to 2025-08 | Closed |
| Shoebox Release Candidate 2 | 16 | 8 | 0 | 2 | 2025-06 to 2025-09 | Closed |
| Shoebox Release Candidate 3 | 30 | 3 | 0 | 12 | 2025-06 to 2025-11 | Closed |
| Shoebox Release Candidate 4 | 55 | 4 | 5 | 12 | 2025-06 to 2026-02 | Closed |
| Shoebox Release Candidate 5 | 25 | 10 | 2 | — | 2026-02 to 2026-03 | Closed |
| Team Bug Fix Cleanup | 12 | 2 | 2 | — | 2025-06 to 2025-08 | Open |
| Demo Update - March | 17+ | 8+ | 4+ | — | 2025-07 to 2026-03 | Open |
| (no milestone) | 27+ | 7+ | — | — | various | — |

**Shoebox RC4 was the largest milestone** — 55 features, 5 epics, spanning 8 months. This was the reconciliation and categorization workflow rework.

**Active branch (`journal-aggregate`) adds:** Journal aggregate promotion, opening balances, PMA/PME separation, accounting reports read-side, data ingestion pipeline, business seeding API, agentic development workflow.

## Epics (Multi-Issue Features)

| # | Title | Milestone | State | Saga Potential |
|---|-------|-----------|-------|---------------|
| 2099 | PowerModels Accounting phase 2 | Demo Update - March | Open | High — journal aggregates, accounting reports, PMA workflows |
| 2098 | Improve our use of `ReactiveUI.SourceGenerators` | Demo Update - March | Open | Low — tooling |
| 2055 | Replace `Fody` with `ReactiveUI.CodeGenerators` | Shoebox RC5 | Closed | Low — tooling |
| 2017 | Create a standalone desktop app | Shoebox RC5 | Closed | Medium — new host, cross-context |
| 1946 | Rework reconciliation and categorization workflow | Shoebox RC4 | Closed | High — multi-aggregate workflow |
| 1905 | Enhancements to reconciliation workflow | Shoebox RC4 | Closed | High — cross-context coordination |
| 1869 | User can adjust reconciled transactions | Shoebox RC4 | Closed | High — domain state mutation |
| 1851 | User can reconcile statement transactions | Shoebox RC4 | Closed | High — multi-step domain workflow |
| 1846 | User can import and view external statements | Shoebox RC4 | Closed | High — data import pipeline |
| 1797 | Depreciation schedules | Demo Update - March | Open | Medium — new aggregate candidate |
| 1793 | Schedules for tasks | Demo Update - March | Open | Medium — cross-aggregate task gen |
| 1612 | Logging Phase 3 | (none) | Open | Low — infrastructure |
| 1611 | Logging Phase 2 | (none) | Open | Low — infrastructure |
| 1610 | Logging Phase 1 | (none) | Closed | Low — infrastructure |
| 1593 | Logging | Team Bug Fix Cleanup | Open | Low — infrastructure |
| 1526 | Chart of accounts | Team Bug Fix Cleanup | Open | Medium — core domain aggregate |

## Aggregate Provenance (Reverse Index)

Which features touched each aggregate (via PR file path mapping):

| Aggregate | PR Count | Key Features | Primary Milestone |
|-----------|---------|-------------|-------------------|
| Journal | new | Journal aggregate promotion from application-layer concept | journal-aggregate branch |
| JournalEntry | new | Journal entry with lines, categorization, recategorization | journal-aggregate branch |
| ChartOfAccounts | 14+ | CoA setup, account subtypes, filtering, reconciliation prep, opening balances | RC3-RC4, journal-aggregate |
| ServerFinancialModel | 9 | Semantic formulas, single-line tables, mapping operations | RC3-RC4 |
| TasklistItem | 6 | Task lists, recurrence schedules, cross-workspace tasks | RC4-RC5 |
| DataTableMap | 4 | Data mapping updates, cumulative mappings, delete mappings | RC3 |
| ListDataTableMap | 4 | List data table operations, mapping updates | RC3 |
| CustomStatementTxType | 4 | Custom transaction types, opening balance workflow | RC4-March |
| ManualTableMap | 4 | Manual table mapping, cumulative mappings | RC3 |
| DataSource | 3 | Data source creation, RM threading simplification | RC3-RC4 |
| Product | 3 | Product import/add/edit, boolean fields | RC3-RC4 |
| Customer | 3 | Customer/vendor UI, item numbers | RC3-RC4 |
| Vendor | 3 | Customer/vendor UI, item numbers | RC3-RC4 |
| Counterparty | 2 | Counterparties domain + UI implementation | RC3 |
| Contractor | 2 | Contractor entity, item numbers | RC3-RC4 |
| Employee | 2 | Employee entity, item numbers | RC3-RC4 |
| AccountBalance | 2+ | Balance snapshots, opening balances, opening balance during setup | RC4-March, journal-aggregate |
| DataTableDefinition | 1 | Boolean field type for LDTs | RC3 |
| FileStore | 1 | File store service/aggregate | RC3 |
| DataElement | 1 | Boolean field type for data sources | RC3 |

**Not touched in 9 months** (stable aggregates): AccountingSystem, ClientWorkspace, DataSourceMapping, EntrySet, ManualTable, ManagedFinancialModel, ReferenceDataSeries, TeamSettings, UserDefinedWorksheets

**New aggregates on journal-aggregate branch:** Journal, JournalEntry (promoted from application-layer DataSource+DataElement pattern to proper domain aggregates with JournalAggregatesService handler)

## Features by Context

| Context | Feature PRs | Defect PRs | Total | Assessment |
|---------|------------|-----------|-------|------------|
| Excel | 167 | 25 | 192 | PME host — most historical development activity (now on hold) |
| SpreadsheetAdapter | 100 | 18 | 118 | ACL bridge — heavy read model work |
| ModelServer | 46 | 6 | 52 | Domain core — aggregate and handler changes |
| ModelServer/AccountingSystem | 27 | 3 | 30 | Accounting sub-domain |
| Connectors | 24 | 3 | 27 | Integration layer |
| UIBehavior | 20 | 3 | 23 | UI components |
| WPF | 17 | 2 | 19 | Desktop UI |
| App | 14+ | 0 | 14+ | PMA standalone app (primary target since RC5) |
| TeamModelMgmt | 5 | 0 | 5 | Team collaboration — minimal activity |

## Monthly Feature Velocity

| Month | Issues Created | Category Breakdown |
|-------|---------------|-------------------|
| 2025-06 | 65 | Heavy initial planning — RC1-RC4 backlog seeded |
| 2025-07 | 41 | Active development |
| 2025-08 | 20 | Stabilization |
| 2025-09 | 12 | Low — focused execution |
| 2025-10 | 27 | RC4 feature push |
| 2025-11 | 28 | RC4 completion |
| 2025-12 | 28 | RC4→RC5 transition |
| 2026-01 | 37 | RC5 + standalone app |
| 2026-02 | 48 | RC5 close + March demo prep |
| 2026-03 | 11+ | March demo + journal-aggregate branch (partial month) |

## Author Distribution

| Author | PRs | % | Primary Focus |
|--------|-----|---|---------------|
| joshkempner | 202 | 69% | Feature implementation across all contexts |
| dependabot | 44 | 15% | Dependency updates |
| farouk-it | 23 | 8% | Feature implementation |
| thefringeninja | 11 | 4% | Infrastructure and tooling |
| condron | 8 | 3% | Architecture and design |
| tombissaillon | 3 | 1% | Feature implementation |
