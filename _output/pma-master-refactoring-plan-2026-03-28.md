# PMA Master Refactoring Plan

A phased plan to isolate the WPF accounting app from SpreadsheetAdapter, factor the data ingestion pipeline into a shared project, and deliver the full Lisa-aligned feature set — classification rules, review workflow, Excel export, new reports, enhanced rule types, and transaction splitting — with a de-risked path to full pipeline observability.

**Date:** 2026-03-28
**Branch:** `joshkempner/journal-aggregate`

---

## Executive Summary

PMA is currently coupled to SpreadsheetAdapter (SA), a large project that serves the Excel-based PME product. This coupling makes PMA slow to build, hard to reason about for code generation agents, and risky to modify (changes may break PME). Five high-leverage architectural changes have been identified (A–E). This plan sequences them across four phases to maximize value delivery while minimizing risk.

**The strategy:**
1. **Phase 0 — Isolation:** Factor the data ingestion pipeline out of SA into a shared `DataIngestion` project. Duplicate remaining SA types into the UI layer. Replace the God-class `SpreadsheetContextService` with a lightweight `AccountingContextService`. Remove SA from the WPF solution.
2. **Phase 1 — Sweet Spot 3+ (Changes B + C + D + new reports + enhanced rule types):** Deliver user-editable classification rules with advanced match types, a classification review workflow, Excel report export, and new accounting reports (Diagnostic, Exception) — the full pragmatic package.
3. **Phase 1.5 — Transaction Splitting:** Generalize journal entry splitting for tax/fee decomposition (e.g., GST splits).
4. **Phase 2 — Observable Pipeline (Change A) + Statement Reports:** Decompose the pipeline into observable, event-driven stages. Add statement-dependent reports (Run Summary, Reconciliation). De-risked by Phase 0's factoring.
5. **Phase 3 — Fiscal Year (Change E):** Small, independent domain change.

---

## Current Architectural Constraints

| # | Constraint | Addressed by |
|---|-----------|-------------|
| 1 | **Classification rules are auto-generated** — `ChartOfAccountsRm.GenerateClassificationRules()` builds rules on-the-fly from COA names. Users never author rules. | Phase 1: Change B |
| 2 | **Pipeline is fire-and-forget** — `JournalService` runs the pipeline inside `Task.Run`. No intermediate events, no pause points. | Phase 2: Change A |
| 3 | **JournalService is a God service** (1,334 lines) — orchestrates import, pipeline, journal generation, DataElement creation, PDF validation, opening balances. | Phase 0: Factoring (~500 lines move out) |
| 4 | **No statement-level identity** — files go in, DataElements come out. No aggregate represents "this import" as a trackable entity. | Phase 0 enables; Phase 2 implements |
| 5 | **Reports are view-only** — `ReportBuilder` DSL produces `IAccountingReport` for WPF views only. No export. | Phase 1: Change D |

---

## Change Inventory

| Change | Description | Effort | D1: Impl | D2: Lisa | D3: UX | Phase |
|--------|------------|--------|----------|----------|--------|-------|
| **A** | Decompose pipeline into observable stages | Medium* | ★★★★★ | ★★★★ | ★★★★★ | 2 |
| **B** | Decouple classification rules from COA auto-generation | Medium | ★★★★★ | ★★★★★ | ★★★★ | 1 |
| **C** | Add classification review step (human-in-the-loop) | Medium | ★★★ | ★★★★ | ★★★★★ | 1 |
| **D** | Excel export via ReportBuilder DSL + ClosedXML | Small | ★★ | ★★★★★ | ★★★★★ | 1 |
| **E** | Fiscal year configuration | Small | ★★ | ★★★ | ★★★★ | 3 |

*Change A drops from Large to Medium effort after Phase 0 factoring.

---

## Phase 0: WPF Isolation + Pipeline Factoring

**Goal:** Decouple `PowerModelsAccounting.slnx` from SpreadsheetAdapter. Build x64-only with ~12 projects instead of ~30.

**Duration:** 5–7.5 days

### Why factor the pipeline instead of just duplicating?

Sweet Spot 3 (Phase 1) modifies pipeline steps — `ClassificationRuleStep`, new `CreateImportReviewStep`, Phase 1/2 pipeline split. If we duplicate, those changes must be made twice and the copies diverge immediately.

| Dimension | Duplicate | Factor out |
|-----------|-----------|-----------|
| SS3 pipeline changes | Done **twice** (SA + WPF) | Done **once** in `DataIngestion` |
| SA modification risk | Zero | **Minimal** (add project ref) |
| Long-term maintenance | Two diverging copies | One shared pipeline |
| Pipeline test coverage | Tests duplicated or single-copy | **One test suite** |
| Agent code generation | Two copies to understand | One canonical pipeline |

**The pipeline is domain logic, not adapter logic.** Classification, journal posting, and AI classification are accounting operations.

### Target Architecture

```
PowerModelsAccounting.slnx (x64 only)
├── /App/
│   ├── PowerModels.App          → Toolkit, WPF  (NO SpreadsheetAdapter)
│   ├── PowerModels.WPF          → UIBehavior    (NO SpreadsheetAdapter)
│   └── PowerModels.UIBehavior   → Toolkit, ModelServer, DataIngestion, LocalDataStore
├── /Shared/
│   └── PowerModels.Toolkit      → ModelServer    (NO SpreadsheetAdapter)
├── /Domain/
│   ├── ModelServer
│   ├── DataIngestion             → ModelServer  (NEW — factored from SA)
│   └── LocalDataStore
├── /Utilities/
│   ├── SeedTestBusiness          → SpreadsheetAdapter (unchanged, builds via PowerModels.sln)
│   └── UITestRunner              → (standalone, FlaUI only)
├── /Tests/
│   ├── PowerModels.App.Tests
│   ├── PowerModels.Toolkit.Tests
│   ├── PowerModels.UIBehavior.Tests
│   ├── ModelServer.Tests
│   ├── DataIngestion.Tests       (NEW)
│   └── LocalDataStore.Tests
```

SpreadsheetAdapter + SA.Tests **removed from slnx** (still in repo, built by `PowerModels.sln` for PME).
SpreadsheetAdapter **gains a reference** to `DataIngestion` (SA's `JournalService` uses the shared pipeline).

### Pipeline Dependency Analysis

The pipeline (`SpreadsheetAdapter/Reconciliation/`, 20 files) is nearly SA-independent:

| Component | SA dependency | Move strategy |
|-----------|--------------|---------------|
| `DataIngestionPipeline<T>`, `IDataIngestionStep<T>` | **None** | Move directly |
| Models (6): `AccountTransaction`, `ClassificationRule`, `ClassificationResult`, `JournalEntry`, `JournalSchema`, `StandardFields` | **Only ModelServer** | Move directly |
| `IChartOfAccountsQuery`, `IJournalDataSourceQuery` | Already extracted interfaces | Move to `DataIngestion/Queries/` |
| `CsvReaderStep`, `ColumnMappingStep`, `AddOrUpdateDataSourceStep` | Pipeline models only | Move directly |
| `ClassificationRuleStep`, `JournalPostingStep` | Pipeline models + interfaces | Move directly |
| `TransactionNormalizerStep` | `Func<Guid, DataSourceRm?>` | Refactor to `IDataSourceQuery` interface |
| `AIClassificationStep` | Concrete `ChartOfAccountsRm` + `JournalService.GetResourceText` | Refactor to `IChartOfAccountsQuery` + injected resource provider |
| `PDFReaderStep` | `SpreadsheetAdapter.Messages` | Inject via `IPublisher` + delegate |

Only 2 of 8 steps require interface refactoring. The rest move as-is.

### Proposals (sequential)

**P0.1 — Factor pipeline into `DataIngestion` project** (2–3 days)
- Create `ModelServer/DataIngestion/DataIngestion.csproj` → references `ModelServer` only
- Move pipeline framework, models, query interfaces, and steps (~20 files)
- Refactor `AIClassificationStep` and `TransactionNormalizerStep` to use interfaces
- Update SA: add `DataIngestion` project reference, update `using` statements (~10 files)
- SA behavior unchanged — same pipeline, same orchestration, different namespace

**P0.2 — Duplicate remaining SA types into UI layer** (1.5–2 days)
- Messages → `UIBehavior/Messages/` (keep `namespace SpreadsheetAdapter.Messages` for event deserialization)
- Read Models → `UIBehavior/ReadModels/` (ChartOfAccountsRm, DataSourceRm, workspace RMs, etc.)
- Infrastructure → `Toolkit/` (LocalConnection, RemoteConnection, Wiretapper)
- Forecasting types → `UIBehavior/PredictiveForecasting/`
- ~25–30 files copied, ~107 files repointed

**P0.3 — Replace SpreadsheetContextService** (1–2 days)
- New `AccountingContextService` in `PowerModels.App/Context/`
- Manages data store connections, boots ModelServer domain services
- Orchestrates `DataIngestion` pipeline for WPF's transaction import workflow
- Handles ~15–20 commands (vs 70+ in `SpreadsheetContextService`)

**P0.4 — Solution cleanup & verification** (0.5 day)
- Remove SA + SA.Tests from `PowerModelsAccounting.slnx`
- Add `DataIngestion` + `DataIngestion.Tests` to `/Domain/`
- Add `UITestRunner` to `/Utilities/`
- Set x64-only platform
- Verify: `dotnet build` both solutions, `dotnet test` all, seeder works, UITestRunner launches

---

## Phase 1: Sweet Spot 3 — Changes B + C + D

**Goal:** Deliver the pragmatic package: authored classification rules → user reviews results → journal posts → reports → Excel export.

**Duration:** 10–15 days

This is Lisa's full workflow adapted for PMA's interactive model.

### Change D: Excel Report Export (3–4 days)

**Independent — can start immediately after Phase 0.**

| Item | Detail |
|------|--------|
| **What** | `ExportToExcel` method taking `IAccountingReport` → Excel workbook via ClosedXML |
| **Where** | `PowerModels.UIBehavior` + `PowerModels.WPF` |
| **SA impact** | None — already SA-independent |
| **Key insight** | `ReportBuilder` DSL already produces structured `IAccountingReport` with panels, sections, columns, totals. Mapping to Excel is mechanical. |
| **Risk** | Minimal. ClosedXML is MIT-licensed, mature. |

### Change B: Classification Rules as Domain Concept (3–5 days)

**Uses `DataIngestion` pipeline and `ModelServer` aggregate.**

| Item | Detail |
|------|--------|
| **What** | Promote classification rules to a persistent, event-sourced `ClassificationRuleSet` aggregate. Users create/edit rules. COA-derived rules become a one-time seed. |
| **New aggregate** | `ClassificationRuleSet` in `ModelServer/Domain/` |
| **New messages** | `ClassificationRuleSetMsgs` in `ModelServer/Messages/` |
| **Pipeline change** | `ClassificationRuleStep` in `DataIngestion/Steps/` reads from `IClassificationRulesQuery` instead of receiving rules via context — **modified once, shared by SA and WPF** |
| **Read model** | `ClassificationRulesRm` in `DataIngestion/` or `UIBehavior/` |
| **UI** | Rule management grid in `UIBehavior/Components/Classification/` |
| **SA impact** | **~3 files** — `using` statement updates only |
| **What it replaces** | `ChartOfAccountsRm.GenerateClassificationRules()` (60+ lines) becomes a seed, not the sole source |
| **Risk** | Existing imports may classify differently. Seed rules from COA on migration to preserve behavior. |

### Change C: Classification Review Workflow (4–6 days)

**Depends on Change B. Uses the simpler approach: pipeline runs to completion, presents results in review grid, user confirms, then journal posting fires.**

| Item | Detail |
|------|--------|
| **What** | Two-phase import: Phase 1 (classify) pauses for user review, Phase 2 (post) fires on approval |
| **New aggregate** | `ImportReview` in `ModelServer/Domain/` |
| **New pipeline step** | `CreateImportReviewStep` in `DataIngestion/Steps/` — replaces `JournalPostingStep` as terminal step in Phase 1 |
| **New command** | `PostApprovedImport` — triggers Phase 2 pipeline (posting only) |
| **UI** | Classification review grid with accept/reject/reclassify per transaction, confidence highlighting |
| **Existing hooks** | `JournalEntry.Status = Draft` (exists, unused), `NeedsReview` flag (exists) |
| **SA impact** | **None** — WPF orchestrator builds its own Phase 1/2 pipelines from shared `DataIngestion` steps |
| **Risk** | Slows import workflow. Mitigate with "trust mode" / auto-post toggle for experienced users. |

### Change B Extensions: Enhanced Rule Types (+1–2 days, folded into Change B)

The action plan's Lisa feature inventory identifies rule capabilities beyond keyword matching that should be part of Change B's `ClassificationRuleSet` aggregate:

| Feature | What it adds | Lisa equivalent |
|---------|-------------|----------------|
| **F-CLS-05** `AmountRangeMatch` | Match by amount range + day-of-month (e.g., rent: $3,000–$3,500 on day 1–2) | LBTY-011 Rent Heuristic |
| **F-CLS-06** `LiabilityCreditGuardrail` | Pre-fallback rule: liability credits are NOT income | LIAB-001 |
| `CompositeMatch` | Combine keyword + amount + date conditions in a single rule | Lisa's multi-condition rules |

These are new match types on the `ClassificationRuleSet` aggregate:
```
Rule match types:
  - KeywordMatch (existing PM behavior)
  - AmountRangeMatch (new — min, max, optional dayOfMonth range)
  - CompositeMatch (new — AND/OR combination of conditions)
  - LiabilityCreditGuardrail (new — system rule, pre-fallback)
```

PM already handles liability polarity correctly via `NormalBalanceSide` + `CashDirection`, but lacks the explicit guardrail rule that Lisa runs before fallback classification.

### New Reports: Diagnostic + Exception (+2–3 days)

Two Lisa reports that need only existing journal data (no Phase 2 dependency):

| Report | Lisa tab | What it shows | Base class |
|--------|----------|--------------|------------|
| **F-RPT-04** `DiagnosticRm` | DIAGNOSTICS | Per-account: type, debit, credit, net, included-in-NI | `AccountingReportBase` |
| **F-RPT-05** `ExceptionReportRm` | EXCEPTIONS | Income/Expense accounts with contra-directional activity (both debit AND credit) | `AccountingReportBase` |

Both follow the established `ReportBuilder` DSL pattern. `ExceptionReportRm` flags potential misclassifications or refunds — a key accountant review tool.

### Phase 1 Effort Summary

| Change | Effort | SA files modified |
|--------|--------|-------------------|
| D: Excel Export | 3–4 days | 0 |
| B: Classification Rules + Enhanced Types | 4–7 days | ~3 (using statements) |
| C: Review Workflow | 4–6 days | 0 |
| New Reports (Diagnostic + Exception) | 2–3 days | 0 |
| **Total** | **13–20 days** | **~3** |

---

## Phase 1.5: Transaction Splitting

**Goal:** Generalize `JournalPostingStep.GenerateEntryWithFee()` to support N-way journal splits for tax/fee decomposition.

**Duration:** 1–2 days. Depends on Phase 1 Change B.

**Lisa equivalent:** F-CLS-03 (Moneris Revenue/GST Split) — a single transaction produces multiple journal lines (e.g., net revenue + GST payable).

| Item | Detail |
|------|--------|
| **What** | `SplitTemplate` value object on `ClassificationRuleSet` — percentage-based splits with rounding control |
| **Where** | `ModelServer/Domain/` (value object) + `DataIngestion/Steps/JournalPostingStep` (apply splits) |
| **Pattern** | Extend existing `GenerateEntryWithFee()` 3-line pattern to N-line splits via `SplitTemplate` |
| **Example** | Moneris settlement → 95.238% Income (net) + 4.762% GST Payable, last line absorbs rounding |
| **Risk** | Low — additive change to existing posting step. Split rules are opt-in per classification rule. |

---

## Phase 2: Change A — Observable Pipeline + Statement Reports

**Goal:** Replace fire-and-forget `Task.Run` with a `StatementImport` aggregate whose lifecycle emits domain events at each stage. De-risked by Phase 0's factoring.

**Duration:** 5–8 days (down from 10–15 days without factoring)

### Why Phase 0 de-risks this

| Factor | Without Phase 0 | With Phase 0 |
|--------|-----------------|--------------|
| Pipeline location | Embedded in SA's JournalService | Standalone `DataIngestion` project |
| Interface injection | Mixed concrete/interface deps | All steps use interfaces |
| Scope of change | "Touches the core import path" — risky | Scoped to `DataIngestion` + orchestrators |
| StatementImport aggregate | No natural home | `ModelServer/Domain/` — clean |
| Risk rating | **Large** | **Medium** |

### What this enables
- Import status tracking and progress visibility
- Report RMs subscribe to pipeline events instead of reconstructing from DataElement events
- Audit trail falls out for free
- Reconciliation becomes an event handler, not a separate report
- Aligns with Lisa's `CanonicalStatement` model

### Key components
- `StatementImport` aggregate in `ModelServer/Domain/` — lifecycle: Extracted → Classified → Reviewed → Posted → Reconciled
- Pipeline steps raise domain events via injected `IPublisher`
- `DataIngestion` pipeline becomes event-driven, not purely in-memory context mutation
- Both SA and WPF orchestrators adopt the new event-driven pipeline

### Statement-Dependent Reports (+2–3 days)

Two Lisa reports that require `StatementImport` aggregate data and therefore ship with Phase 2:

| Report | Lisa tab | What it shows | Depends on |
|--------|----------|--------------|------------|
| **F-RPT-03** `RunSummaryRm` | RUN_SUMMARY | Per-statement: file, account, balances, txn count, diff | `StatementImport` events |
| **F-ACC-06** `ReconciliationReportRm` | RECONCILIATION | Per-statement: opening + credits − debits = closing, journal control totals | `StatementImport` + `DataElement` events |

`RunSummaryRm` subscribes to `StatementImport` lifecycle events. `ReconciliationReportRm` cross-references statement metadata with journal totals per source account.

---

## Phase 3: Change E — Fiscal Year Configuration

**Goal:** Add `FiscalYearStartMonth` to the accounting system. Enhance P&L to use fiscal year grouping.

**Duration:** 2–3 days. Can slot in anywhere after Phase 0.

- One new command/event on `ChartOfAccounts` or `ClientWorkspace` aggregate
- One new RM property or RM for fiscal year context
- `IncomeStatementRm` enhanced: monthly matrix pivoted by fiscal year (Lisa's F-RPT-01 P&L-by-FY)
- Risk: near zero

---

## Future: Deterministic PDF Parsers (Optional)

Lisa features F-ING-01/02 and F-PAR-01 represent institution-specific deterministic parsers that use regex pattern matching instead of AI vision. PM's current AI approach (GPT-4o) already works but is more expensive per page.

| Item | Detail |
|------|--------|
| **What** | `IStatementParser` interface + `ParserRegistry` for confidence-scored plugin detection. Try deterministic parser first, fall back to AI. |
| **Where** | `DataIngestion/Parsers/` (interface + registry) + per-institution parser files |
| **Effort** | Large (per institution) — each parser needs regex patterns, layout knowledge, test PDFs |
| **When** | After Phase 2. Only justified for high-volume known formats where AI cost matters. |
| **Risk** | Maintenance burden — bank statement formats change. Each parser needs ongoing updates. |

---

## Full Roadmap

```
Phase 0: Isolation + Factoring (5–7.5 days)
  P0.1: Factor pipeline ──→ P0.2: Duplicate remaining ──→ P0.3: Context service ──→ P0.4: Cleanup
    │
    ├──→ Phase 1: Sweet Spot 3+ (13–20 days)
    │      Change D: Excel Export (3–4 days)                    ← independent, starts immediately
    │      Change B: Classification Rules + Enhanced Types (4–7 days)
    │        │
    │        ├──→ Change C: Review Workflow (4–6 days)           ← depends on Change B
    │        └──→ Phase 1.5: Transaction Splitting (1–2 days)   ← depends on Change B
    │      New Reports: Diagnostic + Exception (2–3 days)       ← parallel with B or C
    │
    ├──→ Phase 3: Change E — Fiscal Year + P&L-by-FY (2–3 days) ← independent
    │
    └──→ Phase 2: Change A — Observable Pipeline + Statement Reports (7–11 days)
           De-risked by Phase 0, benefits from Phase 1's new aggregates
           Includes: RunSummaryRm, ReconciliationReportRm
```

### Timeline Summary

| Phase | What | Days | Cumulative |
|-------|------|------|------------|
| **0** | Isolation + Pipeline Factoring | 5–7.5 | 5–7.5 |
| **1** | Sweet Spot 3+ (B + C + D + reports + rule types) | 13–20 | 18–27.5 |
| **1.5** | Transaction Splitting | 1–2 | 19–29.5 |
| **2** | Observable Pipeline + Statement Reports (A) | 7–11 | 26–40.5 |
| **3** | Fiscal Year + P&L-by-FY (E) | 2–3 | 28–43.5 |

---

## Cumulative Impact on Constraints

| Constraint | After Phase 0 | After Phase 1 | After Phase 2 |
|-----------|--------------|---------------|---------------|
| **1. Auto-generated rules** | Unchanged | **Resolved** — rules are a domain concept with keyword, amount-range, and composite match types | Resolved |
| **2. Fire-and-forget pipeline** | Unchanged (factored but still fire-and-forget) | Partially addressed (Phase 1/2 split) | **Resolved** — event-driven |
| **3. God service (1,334 lines)** | **~800 lines** (pipeline factored out) | ~800 lines | **~500 lines** (events replace orchestration) |
| **4. No statement identity** | **Enabled** | Partially addressed (`ImportReview` aggregate) | **Resolved** (`StatementImport` lifecycle + RunSummary + Reconciliation reports) |
| **5. Reports view-only** | Unchanged | **Resolved** — Excel export + Diagnostic + Exception reports | Resolved + RunSummary + Reconciliation |

---

## Risk Assessment

| Risk | Phase | Severity | Mitigation |
|------|-------|----------|-----------|
| SA breaks when pipeline moves to DataIngestion | 0 | Medium | SA adds project ref; update `using` statements. Run SA test suite. |
| Event deserialization breaks with duplicated message types | 0 | High | Keep `namespace SpreadsheetAdapter.Messages` in duplicated files. R-D resolves by full type name, not assembly-qualified name. Integration test. |
| AIClassificationStep refactor breaks AI classification | 0 | Medium | `IChartOfAccountsQuery` already covers methods used. Inject resource text provider. Run existing AI test suite. |
| Existing import classification quality degrades with authored rules | 1 | Medium | Seed rules from COA on migration. Run classification tests against existing test data. |
| Review step slows import workflow | 1 | Low | Add "trust mode" / auto-post toggle for experienced users. |
| Pipeline event-driven refactor introduces bugs | 2 | Medium | Pipeline is already factored and interface-injected. Incremental: add events alongside existing context mutation, then remove context mutation. |
| Missing types discovered during build | 0 | Low | Iterative — copy/move additional types as compiler errors surface. |

---

## Quantified Benefits

| Metric | Current | After Phase 0 | After Phase 1 | After Phase 2 |
|--------|---------|---------------|---------------|---------------|
| Projects in WPF solution | ~30 (PowerModels.sln) | **~12** (slnx, x64) | ~12 | ~12 |
| SA files modified by feature work | 8–12 per feature | **0–3** (using) | 0–3 | 0–3 |
| Pipeline copies to maintain | 1 (in SA) | **1** (in DataIngestion) | 1 | 1 |
| PME regression risk per feature | Medium | **Minimal** | Minimal | Minimal |
| Classification rules | Auto-generated, no user control | Auto-generated | **User-editable with keyword, amount-range, composite match types** | Same |
| Import visibility | Black box | Black box | **Review step with confidence tiers** | **Full lifecycle tracking** |
| Report delivery | View-only in app | View-only | **Excel export + Diagnostic + Exception** | + RunSummary + Reconciliation |
| Pipeline observability | None | None | None | **Event-driven, auditable** |
| Transaction splitting | 1:1 or 1:1+fee only | Same | Same (Phase 1.5: **N-way splits**) | Same |
| Accounting reports total | 4 (TB, BS, IS, IES) | 4 | **6** (+Diagnostic, Exception) | **8** (+RunSummary, Reconciliation) |

---

## Decision: Why This Ordering

Sweet Spot 3 (B + C + D) was selected as Phase 1 because it **maximizes across all three evaluation dimensions** while keeping risk manageable:

- **D1 (Implementation Impact): ★★★★** — Change B alone scores ★★★★★; the combination unblocks the entire classification feature set
- **D2 (Lisa Alignment): ★★★★★** — Authored rules + review step + Excel export covers Lisa's core workflow
- **D3 (User Journey): ★★★★★** — Users get: editable rules → AI + rule classification → review results → journal posts → reports → Excel export

Phase 0 (isolation) precedes Sweet Spot 3 because:
- SS3 pipeline changes (ClassificationRuleStep, CreateImportReviewStep, Phase 1/2 split) happen **once** in shared `DataIngestion` instead of twice
- ~5–7 days invested saves ~7–11 days across SS3 execution
- Eliminates PME regression risk permanently
- Makes the codebase faster for code generation agents

Change A (observable pipeline) is deferred to Phase 2 because:
- It was rated "Largest refactor" in the impact analysis
- Phase 0's factoring drops it from Large to Medium risk
- Sweet Spot 3 delivers the most urgent user-facing value first
- Phase 1's `ImportReview` aggregate provides partial statement identity that Phase 2 completes

---

## Lisa Feature Coverage Cross-Reference

### Features addressed by this plan

| Feature | Description | Phase | How |
|---------|------------|-------|-----|
| F-CLS-01 | Sequenced Rule Engine | 1 | Change B: `ClassificationRuleSet` aggregate |
| F-CLS-02 | Confidence Tier System | 1 | Change B: discrete tiers on rules |
| F-CLS-03 | Moneris Revenue/GST Split | 1.5 | `SplitTemplate` value object |
| F-CLS-05 | Rent Heuristic Detection | 1 | `AmountRangeMatch` rule type |
| F-CLS-06 | Liability Credit Handling | 1 | `LiabilityCreditGuardrail` system rule |
| F-RPT-01 | P&L by Fiscal Year | 3 | `IncomeStatementRm` + `FiscalYearConfig` |
| F-RPT-02 | Excel Workbook Output | 1 | Change D: ClosedXML export |
| F-RPT-03 | Run Summary | 2 | `RunSummaryRm` (needs `StatementImport`) |
| F-RPT-04 | Diagnostic Sheet | 1 | `DiagnosticRm` |
| F-RPT-05 | Exception Report | 1 | `ExceptionReportRm` |
| F-ACC-06 | Reconciliation Control | 2 | `ReconciliationReportRm` (needs `StatementImport`) |
| F-AUD-02 | Rule Register | 1 | `RuleRegisterRm` (from Change B) |
| F-AUD-03 | Rule Hit Tracking | 1 | `RuleHitsRm` (from Change B) |
| F-AUD-04 | Statement ID Hashing | 2 | `StatementImport` aggregate |
| F-CFG-02 | Fiscal Year Configuration | 3 | Change E |
| F-PAR-02 | Canonical Statement Model | 2 | `StatementImport` aggregate |

### Features already implemented in PM (Full coverage)

| Feature | PM equivalent |
|---------|---------------|
| F-PAR-03 | Multi-instrument polarity (`NormalBalanceSide` + `CashDirection`) |
| F-ACC-01 | Ledger-to-journal (`JournalPostingStep`) |
| F-ACC-02 | Opening balances (`ChartOfAccounts` aggregate) |
| F-ACC-04 | Trial Balance (`TrialBalanceRm`) |
| F-ACC-05 | Balance Sheet (`BalanceSheetRm`) |
| F-CFG-01 | Opening Balances Config (domain model + UI) |

### Features excluded (N/A or deferred)

| Feature | Reason |
|---------|--------|
| F-ING-03 | Auto-discovery — batch paradigm, doesn't fit PMA interactive UI |
| F-ING-04 | Run list CSV — Lisa batch mode |
| F-PAR-04 | Parser scaffolding — dev tooling |
| F-CLS-04 | Payroll subset-sum — client-specific, too narrow |
| F-CLS-07 | Overrides system — deferred, adds config complexity |
| F-ACC-03 | Auto-computed opening balances — not used by Lisa either |
| F-AUD-05 | Layout archive — dev tooling, PM uses AI |
| F-CFG-03 | Payroll overrides — paired with F-CLS-04 |
| F-CFG-04 | Plugin registration — deferred to optional deterministic parsers |
| F-ING-01/02, F-PAR-01 | Deterministic PDF parsers — optional future, PM's AI works |
