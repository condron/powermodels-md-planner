# Defect Analysis

Last reviewed: 2026-03-27

Source: GitHub Issues (`event-systems/powermodels`)
Period: 2025-06-01 — 2026-03-27 (10 months)
Branch: `joshkempner/journal-aggregate` (active development branch)
Total defects: 66+ (59+ closed, 7+ open)
Classification: Body pattern "Describe the bug" + title keyword matching

## Defects by Bounded Context

Mapped via PR file paths to scan-data.json artifact locations:

| Context | Feature PRs | Defect PRs | Total PRs | Defect Rate | Assessment |
|---------|------------|-----------|-----------|------------|------------|
| Excel | 167 | 25 | 192 | 13% | 🟡 Highest absolute defect count — primary host |
| SpreadsheetAdapter | 100 | 18 | 118 | 15% | 🟡 ACL bridge — defects in read model translation |
| ModelServer | 46 | 6 | 52 | 12% | 🟢 Domain core — relatively clean |
| ModelServer/AccountingSystem | 27 | 3 | 30 | 10% | 🟢 Accounting sub-domain — cleanest |
| Connectors | 24 | 3 | 27 | 11% | 🟢 Integration layer |
| UIBehavior | 20 | 3 | 23 | 13% | 🟢 UI components |
| WPF | 17 | 2 | 19 | 11% | 🟢 Desktop UI |
| App | 14+ | 0 | 14+ | 0% | 🟢 PMA host — no defects yet (primary target since RC5) |
| TeamModelMgmt | 5 | 0 | 5 | 0% | 🟢 Stable — minimal activity |

**SpreadsheetAdapter has the highest defect rate (15%)** despite being the ACL bridge. This is where domain events are translated into read model projections — the boundary where data format mismatches and state sync issues surface.

## Defects by Handler Service

| Handler | Defect PRs | Total Commands | Defect Density |
|---------|-----------|---------------|---------------|
| ListDataTableModelMappingService | 2 | 0 | — |
| AccountingSystemService | 2 | 34 | Low |
| DataTableModelMappingService | 2 | 0 | — |
| DataSourceService | 1 | 12 | Low |
| ManualTableModelMappingService | 1 | 0 | — |
| DataMappingService | 1 | 0 | — |
| FinancialModelService | 1 | 166 | Very low |
| QuickbooksDataSourceService | 1 | 0 | — |
| ClientWorkspaceContextService | 1 | 0 | — |

**Mapping services** (DataTableModelMappingService, ListDataTableModelMappingService, ManualTableModelMappingService) account for 5 defect PRs despite having 0 registered commands — these are SpreadsheetAdapter coordination services, not command handlers. Defects here are read model projection errors.

**FinancialModelService** (166 commands) has only 1 defect PR — surprisingly clean given its size. However, this may reflect that most FinancialModelService defects surface as *read model* bugs rather than handler bugs.

## Defects by Read Model

| Read Model | Defect PRs | Event Subscriptions | Context |
|------------|-----------|-------------------|---------|
| DataSourceRm | 4 | 11 | SpreadsheetAdapter |
| FinancialModelRm | 4 | 49 | SpreadsheetAdapter |
| DataSourceListRm | 3 | 10 | SpreadsheetAdapter |
| ChartOfAccountsRm | 3 | 4 | SpreadsheetAdapter |
| ManualTableMapRm | 2 | 14 | SpreadsheetAdapter |
| ModelTemplateRm | 2 | 82 | ModelServer |
| ListDataTableMapRm | 1 | 12 | SpreadsheetAdapter |
| DataTableMapRm | 1 | 12 | SpreadsheetAdapter |
| ProductListRm | 1 | 3 | SpreadsheetAdapter |
| AccountBalancesRm | 1 | 5 | SpreadsheetAdapter |
| CounterpartiesRm | 1 | 11 | SpreadsheetAdapter |
| FinancialModelListRm | 1 | 17 | SpreadsheetAdapter |
| FileStoreRm | 1 | 2 | SpreadsheetAdapter |
| JournalDataRm | 1 | 1 | SpreadsheetAdapter |
| DataSourceUsageRm | 1 | 5 | SpreadsheetAdapter |
| JournalCashAccountsRm | 1 | 2 | SpreadsheetAdapter |

**13 of 16 defect-affected read models are in SpreadsheetAdapter.** This confirms SpreadsheetAdapter is the defect hotspot — it's the ACL translation layer where domain events become UI-consumable projections.

**FinancialModelRm** (49 event subscriptions) and **DataSourceRm** (11 events) are tied for most defect PRs at 4 each. Both are complex projection read models that aggregate data from multiple event types.

## Defects by Milestone

| Milestone | Features | Defects | Defect Rate | Notes |
|-----------|---------|---------|-------------|-------|
| Controlled Beta Release - update 4 | 8 | 9 | 53% | 🔴 Bug fix milestone — expected |
| Shoebox Release Candidate 1 | 12 | 7 | 37% | 🟡 Early RC — initial defect wave |
| Shoebox Release Candidate 2 | 16 | 8 | 33% | 🟡 Continued stabilization |
| Shoebox Release Candidate 5 | 25 | 10 | 29% | 🟡 Recent — defects from RC4 features |
| Demo Update - March | 17 | 8 | 32% | 🟡 Active development + accumulated bugs |
| Shoebox Public Beta 1 | 20 | 4 | 17% | 🟢 Moderate |
| Team Bug Fix Cleanup | 12 | 2 | 14% | 🟢 Fix-focused milestone |
| Shoebox Release Candidate 4 | 55 | 4 | 7% | 🟢 Largest milestone, lowest defect rate |
| Shoebox Release Candidate 3 | 30 | 3 | 9% | 🟢 Healthy ratio |

**RC4** (the largest milestone with 55 features) had the lowest defect rate (7%). This suggests the reconciliation/categorization rework was well-executed. **RC5** has the highest recent defect rate (29%), likely because it inherited defects from RC4 features surfacing in production.

## Defect Timeline

| Month | Opened | Closed | Net Open | Trend |
|-------|--------|--------|----------|-------|
| 2025-06 | 19 | — | — | 🔴 Initial defect backlog |
| 2025-07 | 9 | — | — | 🟡 Decreasing |
| 2025-08 | 4 | — | — | 🟢 Low |
| 2025-09 | 4 | — | — | 🟢 Low |
| 2025-10 | 5 | — | — | 🟢 Low |
| 2025-11 | 2 | — | — | 🟢 Minimum |
| 2025-12 | 1 | — | — | 🟢 Minimum |
| 2026-01 | 6 | — | — | 🟡 Uptick — RC5 features surfacing |
| 2026-02 | 12 | — | — | 🟡 Spike — RC5 stabilization |
| 2026-03 | 4 | — | — | 🟡 Partial month |

**Pattern:** Defect rate dropped dramatically from June (19) to Nov-Dec (1-2), then spiked again in Jan-Feb as RC5 features introduced new defects. This is a healthy release cycle pattern — new features create defects that are then fixed.

## God Aggregate Defect Share

| Metric | Value |
|--------|-------|
| Defect PRs touching ServerFinancialModel (direct) | 0 |
| Defect PRs touching FinancialModelService | 1 |
| Defect PRs touching FinancialModelRm | 4 |
| Defect PRs touching ModelTemplateRm (82 events) | 2 |
| **Total God Aggregate ecosystem defects** | **7 (20% of 35 defect PRs)** |

ServerFinancialModel itself is not directly defect-prone — **defects manifest in its read model projections** (FinancialModelRm, ModelTemplateRm). This makes sense: the aggregate's 87 Apply methods are well-tested for idempotency and state guards, but the 49-82 event subscriptions in its read models create projection complexity where bugs surface.

## Defect-Test Correlation

| Aggregate | Has RestoreFromEvents | Idempotency Tests | State Guard Tests | Test Files | PR Defect Touches |
|-----------|---------------------|-------------------|-------------------|------------|------------------|
| ChartOfAccounts | NO | Yes | Yes | 40 | 0 |
| ServerFinancialModel | NO | Yes | Yes | 40 | 0 (7 via RMs) |
| AccountingSystem | NO | Yes | Yes | 44 | 0 |
| DataSource | NO | Yes | Yes | 8 | 0 (4 via RM) |
| CustomStatementTxType | NO | Yes | Yes | 12 | 0 |
| All others (22) | NO | Mostly yes | Mostly yes | 3-9 each | 0 |

**0/27 aggregates have RestoreFromEvents tests** — the correlation can't be computed because there's no baseline. However, the data shows that **defects don't hit aggregate files directly** — they surface in read model projections and handler services. This suggests:
1. The aggregate domain layer is robust (idempotency + state guard tests work)
2. The defect gap is in the **projection layer** (read models translating events to UI state)
3. RestoreFromEvents tests would protect against a different class of failure — **event replay corruption** — which may not have manifested yet but becomes critical as businesses accumulate more events

## Recurring Patterns

| Pattern | Count | Example Issues | Root Cause |
|---------|-------|---------------|-----------|
| Transaction data corruption on import | 8 | #2095, #2083, #2082, #2054, #2053 | PDF/CSV parsing edge cases — date formats, amount signs, numeric tokens in descriptions |
| Read model state sync | 6 | #1978, #1995, #2068, #2097 | Read model not updating when expected — race conditions, missing event handlers |
| UI component state | 5 | #2078, #2080, #2025, #2066 | WPF/UIBehavior component state out of sync with read model |
| Account mapping errors | 4 | #2096, #2053 | Incorrect account resolution during reconciliation |
| Business setup workflow | 3 | #2109, #2097 | Multi-step creation getting stuck or greyed out |

**Transaction data corruption on import is the #1 defect pattern** (8 issues). This aligns with the Data Import Pipeline saga — the highest-volume write operation (~1,001 events per CSV) is also the most fragile. The parsing layer (PDF/CSV → DataElements) is where most corruption originates.

## Performance-Related Defects

| # | Title | Symptom | Related Context |
|---|-------|---------|----------------|
| #2068 | Upload command unresponsive | Upload hangs | SpreadsheetAdapter — large file processing |
| #2089 | Performance in Recon and Categorize views | Slow UI | UIBehavior — read model query performance |
| #1978 | List of transactions doesn't always load | Timeout | SpreadsheetAdapter — read model hydration |

Only 3 explicitly performance-related defects in 10 months. This is low but may indicate that **performance issues aren't reported as defects** — they're accepted as expected behavior. The data-usage-and-storage analysis (per-category-stream amplification — max 9× on `$ce-ServerFinancialModel`, plus full category stream replay on startup for ~53 ReadModelBase RMs) suggests performance issues are latent and will surface as businesses mature.

**New risk from journal-aggregate branch:** The addition of ~10 new read models (journal domain RMs + 7 accounting report RMs) increases total RM count from ~80 to ~90. The new `$ce-Journal` and `$ce-JournalEntry` category streams add new replay targets on startup. The AccountingReportsContext RMs bypass the SpreadsheetAdapter ACL and subscribe directly to domain events — a new subscription pattern that may need monitoring.
