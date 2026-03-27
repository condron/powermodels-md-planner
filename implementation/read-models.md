# Read Models

Generated: 2026-03-27 19:42:20

## Subscription Summary

| Category | Count | Subscription | Startup Cost |
|----------|-------|-------------|-------------|
| ReadModelBase | 49 | Category streams (`$ce-*`) — replays from position 0 | Per-stream replay |
| TransientSubscriber | 35 | Bus (live only) — no stream replay | None |
| **Total** | **84** | | |

### Streams Subscribed

| Stream | Subscribing RMs |
|--------|----------------|
| `$ce-AccountBalance` | AccountBalancesRm (1) |
| `$ce-ClientWorkspace` | ClientWorkspacesRm (1) |
| `$ce-Contractor` | ContractorListRm (1) |
| `$ce-Counterparty` | CounterpartiesRm, CounterpartiesRm (2) |
| `$ce-Customer` | CustomerListRm (1) |
| `$ce-CustomStatementTxType` | CustomTxTypesRm, StatementTxTypesRm (2) |
| `$ce-DataElement` | DataSourceSampleDataRm (1) |
| `$ce-DataSource` | DataSourceListRm, DataSourceRm, DataSourceUsageRm (3) |
| `$ce-DataSourceMapping` | DataMappingsRm (1) |
| `$ce-DataTableDefinition` | DataTableDefinitionRm, DataTableDefinitionRm (2) |
| `$ce-DataTableMap` | DataTableMapRm (1) |
| `$ce-Employee` | EmployeeListRm (1) |
| `$ce-EntrySet` | ManualEntriesRm (1) |
| `$ce-FileStore` | FileStoreRm (1) |
| `$ce-ManagedFinancialModel` | DataSourcesRm, ModelsRm, TaskMetricsRm (3) |
| `$ce-ManualTable` | ModelWorksheetTablesRm, ManualDataTableRm (2) |
| `$ce-ManualTableMap` | ManualTableMapRm (1) |
| `$ce-Product` | ProductListRm (1) |
| `$ce-ReferenceDataSeries` | ReferenceDataRm (1) |
| `$ce-ServerFinancialModel` | ModelListRm, ModelWorksheetTablesRm, DataTableFromTemplateRm, FinancialModelListRm, ModelsRm, SingleModelTaskMetricsRm (6) |
| `$ce-TasklistItem` | SingleClientTasklistRm, WorkspaceTasklistRm (2) |
| `$ce-TeamSettings` | TeamSettingsRm (1) |
| `$ce-Vendor` | VendorListRm (1) |
| `$et-DataSourceCreated` | DataSourcesRm (1) |
| `$et-DataSourceUpdated` | DataSourceUsageRm (1) |
| `$et-DataTableDefinitionCreated` | DataTablesDefinitionRm (1) |
| `$et-DataTableDeleted` | DataTablesDefinitionRm (1) |
| `$et-DataTableRenamed` | DataTablesDefinitionRm (1) |

## App

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| LicenseRm | `src/CustomUI/PowerModels.App/Context/ReadModels/LicenseRm.cs` | TransientSubscriber | 2 | Bus (live only) | ReadModelProperty |

## Excel

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| InputTablesRm | `src/Excel/PowerModels.Excel.Core/UIComponents/PredictiveForecasting/ReadModels/InputTablesRm.cs` | TransientSubscriber | 6 | Bus (live only) | Dictionary-based |
| ManualTableListRm | `src/Excel/PowerModels.Excel.Core/UIComponents/ManualTables/ReadModels/ManualTableListRm.cs` | TransientSubscriber | 4 | Bus (live only) | Dictionary-based |

## ModelServer

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| ClientWorkspacesRm | `src/ModelServer/ModelServer/Services/ClientWorkspacesRm.cs` | ReadModelBase | 1 | $ce-ClientWorkspace | Dictionary-based |
| CounterpartiesRm | `src/ModelServer/ModelServer/Services/CounterpartiesRm.cs` | ReadModelBase | 5 | $ce-Counterparty | Dictionary-based |
| CustomTxTypesRm | `src/ModelServer/ModelServer/Services/CustomTxTypesRm.cs` | ReadModelBase | 2 | $ce-CustomStatementTxType | Dictionary-based |
| DataSourcesRm | `src/ModelServer/ModelServer/TemplateReadModels/DataSourcesRm.cs` | ReadModelBase | 1 | $et-DataSourceCreated | Dictionary-based |
| DataTableDefinitionRm | `src/ModelServer/ModelServer/TemplateReadModels/DataTableDefinitionRm.cs` | ReadModelBase | 9 | $ce-DataTableDefinition | Dictionary-based |
| DataTableMapRm | `src/ModelServer/ModelServer/TemplateReadModels/DataTableMapRm.cs` | ReadModelBase | 5 | $ce-DataTableMap | ReadModelProperty |
| JournalEntriesRm | `src/ModelServer/ModelServer/Services/JournalEntriesRm.cs` | ReadModelBase | 1 | Category (unknown) | HashSet-based |
| JournalsRm | `src/ModelServer/ModelServer/Services/JournalsRm.cs` | ReadModelBase | 1 | Category (unknown) | HashSet-based |
| ModelListRm | `src/ModelServer/ModelServer/Services/ModelListRm.cs` | ReadModelBase | 4 | $ce-ServerFinancialModel | Dictionary-based |
| ModelTemplateRm | `src/ModelServer/ModelServer/Domain/ModelTemplateRm.cs` | ReadModelBase | 82 | Category (unknown) | Dictionary-based |
| ModelWorksheetTablesRm | `src/ModelServer/ModelServer/Services/ModelWorksheetTablesRm.cs` | ReadModelBase | 14 | $ce-ManualTable, $ce-ServerFinancialModel | Dictionary-based |

## SpreadsheetAdapter

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| AccountBalancesRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/AccountBalancesRm.cs` | ReadModelBase | 5 | $ce-AccountBalance | Dictionary-based |
| ChartOfAccountsRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ChartOfAccountsRm.cs` | TransientSubscriber | 1 | Bus (live only) | SourceCache<Account, Guid> |
| ChartOfAccountsRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/ChartOfAccountsRm.cs` | ReadModelBase | 4 | Category (unknown) | Dictionary-based |
| ClientWorkspaceContextRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ClientWorkspaceContextRm.cs` | ReadModelBase | 10 | Category (unknown) | Dictionary-based |
| ContractorListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/ContractorListRm.cs` | ReadModelBase | 2 | $ce-Contractor | List-based |
| CounterpartiesRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/CounterpartiesRm.cs` | ReadModelBase | 11 | $ce-Counterparty | Dictionary-based |
| CustomerListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/CustomerListRm.cs` | ReadModelBase | 3 | $ce-Customer | Dictionary-based |
| CustomersRm | `src/ModelServer/SpreadsheetAdapter/PredictiveForecasting/CustomersRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<Customer, Guid> |
| DataMappingsRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataMappingsRm.cs` | ReadModelBase | 3 | $ce-DataSourceMapping | Dictionary-based |
| DataSourceListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataSourceListRm.cs` | ReadModelBase | 10 | $ce-DataSource | Dictionary-based |
| DataSourceRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataSourceRm.cs` | ReadModelBase | 11 | $ce-DataSource | Dictionary-based |
| DataSourceSampleDataRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataSourceSampleDataRm.cs` | ReadModelBase | 1 | $ce-DataElement | Dictionary-based |
| DataSourceUsageRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataSourceUsageRm.cs` | ReadModelBase | 5 | $ce-DataSource, $et-DataSourceUpdated | Dictionary-based |
| DataTableDefinitionRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataTableDefinitionRm.cs` | ReadModelBase | 9 | $ce-DataTableDefinition | List-based |
| DataTableFromTemplateRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataTableFromTemplateRm.cs` | ReadModelBase | 5 | $ce-ServerFinancialModel | Dictionary-based |
| DataTableMapRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataTableMapRm.cs` | ReadModelBase | 12 | Category (unknown) | Dictionary-based |
| DataTablesDefinitionRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/DataTablesDefinitionRm.cs` | ReadModelBase | 3 | $et-DataTableDefinitionCreated, $et-DataTableDeleted, $et-DataTableRenamed | Dictionary-based |
| EmployeeListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/EmployeeListRm.cs` | ReadModelBase | 2 | $ce-Employee | List-based |
| FileStoreRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/FileStoreRm.cs` | ReadModelBase | 2 | $ce-FileStore | Dictionary-based |
| FinancialModelListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/FinancialModelListRm.cs` | ReadModelBase | 17 | $ce-ServerFinancialModel | Dictionary-based |
| FinancialModelRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/FinancialModelRm.cs` | ReadModelBase | 49 | Category (unknown) | Dictionary-based |
| ForecasterInputTablesRm | `src/ModelServer/SpreadsheetAdapter/PredictiveForecasting/ForecasterInputTablesRm.cs` | TransientSubscriber | 2 | Bus (live only) | Dictionary-based |
| JournalCashAccountsRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/JournalCashAccountsRm.cs` | TransientSubscriber | 2 | Bus (live only) | List-based |
| JournalDataRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/JournalDataRm.cs` | TransientSubscriber | 1 | Bus (live only) | Unknown |
| ListDataTableMapRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ListDataTableMapRm.cs` | ReadModelBase | 12 | Category (unknown) | Dictionary-based |
| ManualDataTableRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ManualDataTableRm.cs` | ReadModelBase | 3 | $ce-ManualTable | Dictionary-based |
| ManualEntriesRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ManualEntriesRm.cs` | ReadModelBase | 9 | $ce-EntrySet | Dictionary-based |
| ManualTableMapRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ManualTableMapRm.cs` | ReadModelBase | 14 | $ce-ManualTableMap | Dictionary-based |
| ManualTableRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ManualTableRm.cs` | ReadModelBase | 14 | Category (unknown) | ReadModelProperty |
| ModelVerificationRM | `src/ModelServer/SpreadsheetAdapter/ReadModels/ModelVerificationRM.cs` | ReadModelBase | 7 | Category (unknown) | Dictionary-based |
| PersonnelRm | `src/ModelServer/SpreadsheetAdapter/PredictiveForecasting/PersonnelRm.cs` | TransientSubscriber | 4 | Bus (live only) | SourceCache<Employee, Guid> |
| ProductListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/ProductListRm.cs` | ReadModelBase | 3 | $ce-Product | Dictionary-based |
| ProductsRm | `src/ModelServer/SpreadsheetAdapter/PredictiveForecasting/ProductsRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<Product, Guid> |
| ReferenceDataRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/ReferenceDataRm.cs` | ReadModelBase | 8 | $ce-ReferenceDataSeries | Dictionary-based |
| SingleClientTasklistRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/Workspaces/SingleClientTasklistRm.cs` | ReadModelBase | 21 | $ce-TasklistItem | Dictionary-based |
| StatementTxTypesRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/StatementTxTypesRm.cs` | ReadModelBase | 5 | $ce-CustomStatementTxType | Dictionary-based |
| UserDefinedWorksheetsRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/UserDefinedWorksheetsRm.cs` | ReadModelBase | 5 | Category (unknown) | ReadModelProperty |
| VendorListRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/AccountingSystem/VendorListRm.cs` | ReadModelBase | 3 | $ce-Vendor | Dictionary-based |
| VendorsRm | `src/ModelServer/SpreadsheetAdapter/PredictiveForecasting/VendorsRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<Vendor, Guid> |
| WorkspaceTasklistRm | `src/ModelServer/SpreadsheetAdapter/ReadModels/Workspaces/WorkspaceTasklistRm.cs` | ReadModelBase | 21 | $ce-TasklistItem | Dictionary-based |

## TeamModelMgmt

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| DataSourcesRm | `src/TeamModelManagement/TeamModelMgmt/Domain/DataSourcesRm.cs` | ReadModelBase | 3 | $ce-ManagedFinancialModel | Dictionary-based |
| ModelsRm | `src/TeamModelManagement/TeamModelMgmt/ReadModels/ModelsRm.cs` | ReadModelBase | 15 | $ce-ManagedFinancialModel, $ce-ServerFinancialModel | SourceCache<ModelInfo, Guid> |
| SingleModelTaskMetricsRm | `src/TeamModelManagement/TeamModelMgmt/ReadModels/SingleModelTaskMetricsRm.cs` | ReadModelBase | 8 | $ce-ServerFinancialModel | Unknown |
| TaskMetricsRm | `src/TeamModelManagement/TeamModelMgmt/ReadModels/TaskMetricsRm.cs` | ReadModelBase | 1 | $ce-ManagedFinancialModel | List-based |
| TeamSettingsRm | `src/TeamModelManagement/TeamModelMgmt/ReadModels/TeamSettingsRm.cs` | ReadModelBase | 7 | $ce-TeamSettings | ReadModelProperty |

## UIBehavior

| Read Model | Path | Base Class | Events | Subscription | Output |
|------------|------|------------|--------|-------------|--------|
| AccountingReportBase | `src/CustomUI/PowerModels.UIBehavior/Components/Reports/AccountingReportBase.cs` | ReadModelBase | 6 | Category (unknown) | Dictionary-based |
| CompositeRowsRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/CompositeRowsRm.cs` | TransientSubscriber | 5 | Bus (live only) | Dictionary-based |
| ConnectionsRm | `src/CustomUI/PowerModels.UIBehavior/Connections/ConnectionsRm.cs` | TransientSubscriber | 3 | Bus (live only) | ReadModelProperty |
| CounterpartiesRm | `src/CustomUI/PowerModels.UIBehavior/Components/AccountingSystem/ReadModels/CounterpartiesRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<CounterpartyModel, Guid> |
| CreateBusinessWorkflowRm | `src/CustomUI/PowerModels.UIBehavior/Components/AccountingSystem/ReadModels/CreateBusinessWorkflowRm.cs` | TransientSubscriber | 4 | Bus (live only) | Dictionary-based |
| DataMappingsRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/DataMappingsRm.cs` | TransientSubscriber | 4 | Bus (live only) | Dictionary-based |
| DataSourceListRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/DataSourceListRm.cs` | TransientSubscriber | 11 | Bus (live only) | SourceCache<DataSourceModel, Guid> |
| DataTableMapsRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/DataTableMapsRm.cs` | TransientSubscriber | 16 | Bus (live only) | SourceCache<DataSourceVersionInUse, Guid> |
| DataTableModelRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/DataTableModelRm.cs` | TransientSubscriber | 17 | Bus (live only) | SourceCache<DataTableModel, Guid> |
| FileStoreRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/FileStoreRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<string, string> |
| FinancialModelListRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/FinancialModelListRm.cs` | TransientSubscriber | 15 | Bus (live only) | SourceCache<FinancialModelListItemModel, Guid> |
| FinancialModelTablesRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/FinancialModelTablesRm.cs` | TransientSubscriber | 54 | Bus (live only) | SourceCache<TableSummary, Guid> |
| JournalCashTransactionsRm | `src/CustomUI/PowerModels.UIBehavior/Components/AccountingSystem/ReadModels/JournalCashTransactionsRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<CashTransaction, Guid> |
| JournalDataRm | `src/CustomUI/PowerModels.UIBehavior/Components/AccountingSystem/ReadModels/JournalDataRm.cs` | TransientSubscriber | 1 | Bus (live only) | SourceCache<JournalLineModel, Guid> |
| ManualEntriesRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/ManualEntriesRm.cs` | TransientSubscriber | 4 | Bus (live only) | Dictionary-based |
| PreprocessorsRm | `src/CustomUI/PowerModels.UIBehavior/Components/TransactionWorkflow/ReadModels/PreprocessorsRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<string, string> |
| ReadableFormulasRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/ReadableFormulasRm.cs` | TransientSubscriber | 5 | Bus (live only) | SourceCache<ReadableRowFormula, Guid> |
| ReferenceDataRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/ReferenceDataRm.cs` | TransientSubscriber | 6 | Bus (live only) | Dictionary-based |
| ReferencesRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/ReferencesRm.cs` | TransientSubscriber | 7 | Bus (live only) | Dictionary-based |
| SingleUserTasklistRm | `src/CustomUI/PowerModels.UIBehavior/Components/Tasklists/ReadModels/SingleUserTasklistRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<TasklistItem, Guid> |
| SingleWorkspaceTasklistRm | `src/CustomUI/PowerModels.UIBehavior/Components/Tasklists/ReadModels/SingleWorkspaceTasklistRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<TasklistItem, Guid> |
| SolutionLibraryRm | `src/CustomUI/PowerModels.UIBehavior/Components/ModelTemplating/ReadModels/SolutionLibraryRm.cs` | TransientSubscriber | 4 | Bus (live only) | SourceCache<Solution, Guid> |
| StatementTxTypesRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/StatementTxTypesRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<CustomStatementTxType.StatementTxType, Guid> |
| TemplatesInModelRm | `src/CustomUI/PowerModels.UIBehavior/ContextReadModels/TemplatesInModelRm.cs` | TransientSubscriber | 2 | Bus (live only) | SourceCache<SpreadSheetMsgs.TemplateDetails, Guid> |
| WorkspacesRm | `src/CustomUI/PowerModels.UIBehavior/Connections/WorkspacesRm.cs` | TransientSubscriber | 6 | Bus (live only) | SourceCache<ClientWorkspace, Guid> |

