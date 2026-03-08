# Domain Model

Last reviewed: 2026-03-08

Generated: 2026-03-07 (seeded from pre-scanned context)
Source: PowerModels codebase (via `implementation-vault/PowerModels-src` junction)

## Entity Hierarchy

```
Firm (implicit — TeamSettings singleton + the app installation)
 └── Customer (= ClientWorkspace, one per client of the firm)
      ├── metadata: contactName, email, phone, description
      └── Business[] (one or more per customer, generally 1 for now)
           ├── Sandbox[] (development/testing copies of a business)
           └── AccountingSystem (1:1 with business, keyed by AccountingSystemId)
                ├── ChartOfAccounts (1:1, ID = AccountingSystemId)
                │    ├── Account[] (hierarchical: root + child accounts)
                │    │    ├── AccountingCategory: Income|Expense|Asset|Liability|Equity|Dividend
                │    │    ├── Subtype: 40+ (Revenue, Cogs, CurrentAssets, AccountsPayable, etc.)
                │    │    └── CashPositionType: None|Checking|Savings|LOC|CreditCard|Cash
                │    └── GroupSet[] (named groupings of accounts)
                │         └── Group[] → Account assignments
                ├── Counterparty[] (contact entities with roles)
                │    ├── Organization mode (orgName required) or Individual mode (contactName required)
                │    └── Roles [Flags]: Customer=1, Vendor=2, Employee=4, Contractor=8
                ├── Vendor[] | Customer[] | Employee[] | Contractor[] (per-business role-specific entities)
                ├── Product[] (income/expense account mappings)
                ├── DataSource[] (external data feeds: File|Worksheet|ApiCall)
                │    ├── DataElement[] (individual records from the source)
                │    └── DataSourceMapping[] (column-to-account mappings for a source)
                ├── DataTableDefinition[] (schema for imported data tables)
                │    └── DataTableMap[] (maps data table columns → model table rows)
                │    └── ListDataTableMap[] (maps list-style data tables → model)
                ├── ManualTable[] (user-defined manual data entry tables)
                │    └── ManualTableMap[] (maps manual table → model table rows)
                ├── ReferenceDataSeries[] (named reference data for formulas)
                ├── FileStore[] (binary file storage per business)
                ├── EntrySet[] (manual journal entry containers)
                │    └── ManualEntry[] (amount, appliedOn date, name)
                ├── AccountBalance[] (point-in-time balance snapshots per account)
                ├── CustomStatementTxType[] (user-defined transaction type classifications)
                ├── TasklistItem[] (trackable work items per business)
                ├── UserDefinedWorksheets (custom worksheet layouts for a model)
                └── ServerFinancialModel (financial model: tables, rows, formulas, worksheets)

 ManagedFinancialModel (TeamModelMgmt — team-shared model metadata)
      └── Tracks model lifecycle: creation, sharing, permissions
```

## Key Relationships

| Relationship | Cardinality | Key |
|-------------|-------------|-----|
| Firm : Customer | 1 : N | Implicit (workspace per client) |
| Customer : Business | 1 : N | ClientWorkspace._businessesAndSandboxes |
| Business : AccountingSystem | 1 : 1 | AccountingSystemId (shared GUID) |
| AccountingSystem : ChartOfAccounts | 1 : 1 | ChartOfAccountsId = AccountingSystemId |
| ChartOfAccounts : Account | 1 : N | Parent-child hierarchy |
| Business : ServerFinancialModel | 1 : N | Multiple models per business |
| Business : DataSource | 1 : N | Multiple data feeds per business |
| DataSource : DataSourceMapping | 1 : N | Column-to-account mappings per source |
| AccountingSystem : DataTableDefinition | 1 : N | Schema definitions for data tables |
| DataTableDefinition : DataTableMap | 1 : N | Maps data table → model rows |
| AccountingSystem : ManualTable | 1 : N | Manual data entry tables |
| ManualTable : ManualTableMap | 1 : N | Maps manual table → model rows |
| AccountingSystem : TasklistItem | 1 : N | Work items per business |
| Firm : ManagedFinancialModel | 1 : N | Team-shared model records (TeamModelMgmt) |

## Counterparty Model

### Shared (Global Counterparty)

The **Counterparty aggregate** is global, not scoped to AccountingSystem:
- Universal contact record: name, email, phone, URL, notes
- Org/Individual display mode (toggleable)
- Role flags: which roles this entity plays
- Can be referenced across multiple businesses

### Business-Specific (Vendor/Customer/Employee/Contractor)

**Vendor, Customer, Employee, Contractor aggregates** are per-AccountingSystem:
- Business-specific entities scoped to one AccountingSystemId
- Carry business-specific data: defaultAccountId, number, externalId
- Each business creates its own role-specific records

### Practical Example

- "Apple Inc." exists as a global Counterparty (shared contact info)
- Business A creates a Vendor record for "Apple Inc." with its own account mapping
- Business B creates a separate Vendor record for "Apple Inc." with different account mapping
- Both reference the same universal contact data

### Private Counterparties

Owner accounts, employees — business-specific by nature. They exist only as Counterparty + Employee/Contractor records within one AccountingSystem.

## Temporal Model

- **No explicit fiscal year or period aggregates** — this is a known gap
- `AccountBalance.asOfDate` (DateOnly) — point-in-time balance snapshots
- `EntrySet.ManualEntry.appliedOn` (DateTime) — journal entry dates
- `DataSource` updates tracked by startVersion/endVersion
- Period/year logic lives in read models and application layer, not domain aggregates

## Business Creation Chain

```
1. CreateClientWorkspace(workspaceId, name, contact info)
   → ClientWorkspaceCreated
2. AddBusiness(businessId, name, location)
   → BusinessAdded
3. CreateAccountingSystem(accountingSystemId)    ← accountingSystemId = new Guid
   → AccountingSystemCreated
4. CreateChartOfAccounts(chartOfAccountsId)      ← chartOfAccountsId = accountingSystemId
   → ChartOfAccountsCreated
5. [Optional] AddRootAccount / AddChildAccount   ← populates account hierarchy
6. [Optional] InitializeJournalDataSource        ← sets up journal feed
7. [Optional] CreateModel                        ← creates financial model
```

The `NewBusinessWorkflowVm` guides users through this as: Settings → CoA → Journal → Tasks → Create.

## Data Sources and the Ledger

- **DataSource** represents raw external data (CSV uploads, PDF extracts, API feeds)
- **DataElement** represents individual records within a source
- `DataSource.IsJournal` flag marks a source as journal data
- **JournalService** (SpreadsheetAdapter) processes journal data into transactions
- **EntrySet** handles manual journal entries (corrections, adjustments)
- Journal entries don't decompose into per-account debits/credits at the domain level — that's handled by read models
