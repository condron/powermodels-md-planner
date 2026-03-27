# Data Usage and Storage

Last reviewed: 2026-03-27

Generated: 2026-03-27 (from `joshkempner/journal-aggregate` branch)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Stream Types and Auto-Creation

Every `AppendToStream()` call in DataStore triggers synchronous creation of projection streams:

| Stream Pattern | Example | Created By | Purpose |
|----------------|---------|------------|---------|
| `{prefix}.{Aggregate}-{guid}` | `pm.ChartOfAccounts-abc123` | Explicit append | Individual aggregate instance |
| `$ce-{category}` | `$ce-ChartOfAccounts` | Auto on first append | All events for an aggregate type (by-category projection) |
| `$et-{eventType}` | `$et-AccountAdded` | Auto on first append | All events of a given type (by-event-type projection) |
| `$All` | `$All` | Always exists | Global ordered stream of all events in this DataStore |
| `$streams` | `$streams` | Auto | Metadata stream tracking all created stream names |

**Stream naming:** `PrefixedCamelCaseStreamNameBuilder` produces `{prefix}.{AggregateTypeName}-{Guid}` where prefix defaults to `"pm"`. Category is extracted as the portion before the `-{guid}` suffix.

**Growth per aggregate creation:** Creating one aggregate instance adds 3+ new streams (instance, $ce if first of type, $et per unique event type). A new business with standard CoA creates ~50-80 streams on first setup.

## Write Volumes

| Operation | Events Generated | Estimated Size |
|-----------|-----------------|----------------|
| Create business + standard CoA | 25-45 events | 5-10 KB |
| Upload 1,000-row CSV | ~1,001 events (1 DataSource + 1,000 DataElements) | 200-300 KB |
| 12-month bank statement | ~722 events (12 months × ~60 transactions) | 150-200 KB |
| Manual journal entry (EntrySet) | 1-5 events | <1 KB |
| Financial model structure change | 1-10 events | 1-5 KB |

## Stream Growth Estimates

| Business Maturity | Estimated Events | Estimated Disk Size | Stream Count |
|-------------------|-----------------|---------------------|--------------|
| New (setup only) | 25-50 | 5-15 KB | 50-80 |
| First year, small business | 3,000-8,000 | 600 KB - 1.6 MB | 100-200 |
| Mature (2-3 years, regular imports) | 15,000-30,000 | 3-6 MB | 200-400 |
| Heavy use (large data imports) | 50,000+ | 10+ MB | 500+ |

**No compaction or snapshots.** Repository reads full aggregate stream from position 0 on every `GetById()`. No event pruning, archival, or snapshot mechanism exists. Streams grow unbounded.

## Read Patterns and Amplification

**Per-event processing steps:**

```
1. PersistentAllStream disk read        → DataStore in-memory hydration (1×)
2. $ce-{category} projection            → category stream append (1×)
3. $et-{eventType} projection           → event-type stream append (1×)
4. Repository.GetById()                  → full stream replay into aggregate (1×)
5. Category stream subscribers           → ReadModelBase RMs on matching $ce-* (varies by aggregate type)
6. Bus subscribers                       → active TransientSubscriber RMs with IHandle<T> (varies by UI state)
7. Wiretapper (if enabled)              → diagnostic logging (1×)
```

**Subscriber breakdown** (from vault scan with `Start<TAgg>()` extraction):

| Category | Count | Subscription | Startup Cost |
|----------|-------|-------------|-------------|
| ReadModelBase | ~53 | Category streams (`$ce-*`) via `Start<TAgg>()` — replays from position 0 | Per-stream replay |
| TransientSubscriber | ~37 | Bus (live only) — no stream replay, active only when UI context is open | None |
| **Total** | **~90** | | |

*Counts increased from 80 on main to ~90 on journal-aggregate branch due to journal domain RMs (JournalsRm, JournalEntriesRm) and 7 accounting report RMs (BalanceSheetRm, IncomeStatementRm, CashFlowStatementRm, GeneralLedgerRm, TrialBalanceRm, IncomeExpenseSummaryRm, JournalReportRm).*

**Category stream fan-out** (top streams by subscriber count):

| Stream | Subscribing RMs | Per-Event Fan-Out |
|--------|----------------|-------------------|
| `$ce-ServerFinancialModel` | 9 | Hottest — every SFM event replayed 9× on startup |
| `$ce-DataSource` | 3 | |
| `$ce-ManagedFinancialModel` | 3 | |
| `$ce-ManualTable` | 3 | |
| `$ce-Journal` | 1–2 | New — journal aggregate promotion |
| `$ce-JournalEntry` | 1–2 | New — journal entry aggregate promotion |
| Most other streams | 1–2 | |

**Amplification is per-category-stream, not global.** A single `AccountAdded` event fans out to ~1–2 RMs (on `$ce-ChartOfAccounts`), while a `DataTableMapped` event fans out to ~9 RMs (on `$ce-ServerFinancialModel`). The previous ~65× estimate incorrectly assumed all subscribers receive every event.

**Repository reads are full replays:** `ICorrelatedRepository.GetById(Guid id, ICorrelatedMessage source)` calls `IStreamStoreConnection.ReadStreamForward(streamName, 0, ...)` every time. No caching between handler invocations. If an aggregate has 500 events, each command replays all 500.

**Read model fan-out example:** A ServerFinancialModel event (e.g., `DataTableMapped`) is handled by:
- 9 ReadModelBase RMs on `$ce-ServerFinancialModel` (ModelTemplateRm, FinancialModelRm, FinancialModelListRm, ModelListRm, ModelWorksheetTablesRm, DataTableFromTemplateRm, ModelVerificationRM, ModelsRm, SingleModelTaskMetricsRm)
- Active TransientSubscriber RMs in UIBehavior (FinancialModelTablesRm, DataTableMapsRm, etc.) — only when UI is open

**Startup cost:** Opening a business reads the full PersistentAllStream file into the in-memory DataStore (fixed cost). Then ~53 ReadModelBase RMs each replay their category streams from position 0. For a 15K-event business where ServerFinancialModel has ~5K events, the hottest stream alone produces 5K × 9 = ~45K handler invocations. Total startup invocations depend on the per-stream event distribution, not a flat multiplier across all events.

## Disk Persistence Format

- **Serialization:** Protobuf3 (Google.Protobuf) with length-delimited encoding
- **Storage:** `PersistentAllStream` writes all events sequentially to a single file per DataStore
- **File location:** `%LocalAppData%\PowerModels\Data\{WorkspaceName}_dbs\{BusinessName}`
- **Load:** Entire file read into memory on DataStore initialization; all streams rebuilt from $All
- **No incremental loading:** Cannot load a single stream from disk — must load entire DataStore

## Three-Database Summary

| Database | Scope | Technology | Read Pattern | Write Pattern | Growth Driver |
|----------|-------|------------|-------------|---------------|---------------|
| **DataStore** | Per business | Custom in-memory + Protobuf3 disk file | Full $All replay on open; full stream replay per GetById; per-category-stream subscriber fan-out (max 9× on `$ce-ServerFinancialModel`) | Append-only, sequential | Data imports, journal entries, model edits |
| **PowerModelsContext** | Firm-wide | SQL Server + EF Core | Standard EF queries | Standard EF writes | User/role management only |
| **File System** | Per business | Files on disk | On-demand file access | File save on export/import | Model files (.xlsx), imported data (CSV/PDF) |

**PowerModelsContext contains no domain data.** Its schema is limited to ASP.NET Identity tables (AspNetUsers, AspNetRoles, AspNetUserRoles, etc.) plus ManagedFinancialModel and TeamSettings entities for team collaboration. All financial domain state lives exclusively in per-business DataStore files.

**File system storage:**
- `%MyDocuments%\PowerModels\{BusinessName}\Models\` — Excel financial model workbooks
- `%MyDocuments%\PowerModels\{BusinessName}\Imported Data\` — uploaded CSV/PDF source files
- These are working copies, not the source of truth — the DataStore event stream is authoritative

## Performance Implications

| Concern | Current State | Impact |
|---------|--------------|--------|
| Startup time | Full $All disk read + 46 ReadModelBase category stream replays from position 0 | Scales linearly with event count per category stream × subscribers |
| Command latency | Full stream replay per GetById | Scales linearly with aggregate event count |
| Memory | Entire DataStore in memory | Scales linearly with total event count |
| Disk | Single append-only file, no compaction | Grows monotonically |
| ServerFinancialModel | God aggregate (3,556 lines, 100+ commands) | Largest stream, most frequent replay target |
