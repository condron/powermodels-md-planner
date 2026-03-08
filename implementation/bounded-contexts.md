# Bounded Contexts

Generated: 2026-03-08 00:08:55

## App

### Read Models
- **LicenseRm** — 2 events → ReadModelProperty

## Connectors

### Handlers
- **QuickbooksDataSourceService** — 0 commands

## Excel

### Read Models
- **InputTablesRm** — 6 events → Dictionary-based
- **ManualTableListRm** — 4 events → Dictionary-based

## ModelServer

### Aggregates
- **ClientWorkspace** — `src/ModelServer/ModelServer/Domain/ClientWorkspace.cs` (247 lines)
- **DataSourceMapping** — `src/ModelServer/ModelServer/Domain/DataSourceMapping.cs` (61 lines)
- **DataTableDefinition** — `src/ModelServer/ModelServer/Domain/DataTableDefinition.cs` (176 lines)
- **DataTableMap** — `src/ModelServer/ModelServer/Domain/DataTableMap.cs` (370 lines)
- **EntrySet** — `src/ModelServer/ModelServer/Domain/EntrySet.cs` (169 lines)
- **FileStore** — `src/ModelServer/ModelServer/Domain/FileStore.cs` (98 lines)
- **ListDataTableMap** — `src/ModelServer/ModelServer/Domain/ListDataTableMap.cs` (369 lines)
- **ManualTable** — `src/ModelServer/ModelServer/Domain/ManualTable.cs` (365 lines)
- **ManualTableMap** — `src/ModelServer/ModelServer/Domain/ManualTableMap.cs` (306 lines)
- **ReferenceDataSeries** — `src/ModelServer/ModelServer/Domain/ReferenceDataSeries.cs` (172 lines)
- **ServerFinancialModel** — `src/ModelServer/ModelServer/Domain/ServerFinancialModel.cs` (3557 lines)
- **TasklistItem** — `src/ModelServer/ModelServer/Domain/TasklistItem.cs` (340 lines)
- **UserDefinedWorksheets** — `src/ModelServer/ModelServer/Domain/UserDefinedWorksheets.cs` (69 lines)

### Handlers
- **AccountingSystemService** — 34 commands
- **AccountStatementConfigService** — 4 commands
- **ClientWorkspaceService** — 9 commands
- **DataSourceModelMapService** — 3 commands
- **DataSourceService** — 12 commands
- **DataTableDefinitionService** — 7 commands
- **EntrySetService** — 7 commands
- **FileStoreService** — 3 commands
- **FinancialModelService** — 166 commands
- **ManualTableService** — 12 commands
- **TasklistService** — 20 commands
- **UserDefinedWorksheetsService** — 4 commands

### Read Models
- **ClientWorkspacesRm** — 1 events → Dictionary-based
- **CounterpartiesRm** — 5 events → Dictionary-based
- **CustomTxTypesRm** — 2 events → Dictionary-based
- **DataSourcesRm** — 1 events → Dictionary-based
- **DataTableDefinitionRm** — 9 events → Dictionary-based
- **DataTableMapRm** — 5 events → ReadModelProperty
- **ModelListRm** — 4 events → Dictionary-based
- **ModelTemplateRm** — 82 events → Dictionary-based
- **ModelWorksheetTablesRm** — 14 events → Dictionary-based

## ModelServer/AccountingSystem

### Aggregates
- **AccountBalance** — `src/ModelServer/ModelServer/Domain/AccountingSystem/AccountBalance.cs` (106 lines)
- **AccountingSystem** — `src/ModelServer/ModelServer/Domain/AccountingSystem/AccountingSystem.cs` (29 lines)
- **ChartOfAccounts** — `src/ModelServer/ModelServer/Domain/AccountingSystem/ChartOfAccounts.cs` (540 lines)
- **Contractor** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Contractor.cs` (36 lines)
- **Counterparty** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Counterparty.cs` (224 lines)
- **Customer** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Customer.cs` (46 lines)
- **CustomStatementTxType** — `src/ModelServer/ModelServer/Domain/AccountingSystem/CustomStatementTxType.cs` (179 lines)
- **DataElement** — `src/ModelServer/ModelServer/Domain/AccountingSystem/DataElement.cs` (143 lines)
- **DataSource** — `src/ModelServer/ModelServer/Domain/AccountingSystem/DataSource.cs` (271 lines)
- **Employee** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Employee.cs` (36 lines)
- **Product** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Product.cs` (90 lines)
- **Vendor** — `src/ModelServer/ModelServer/Domain/AccountingSystem/Vendor.cs` (49 lines)

## SpreadsheetAdapter

### Handlers
- **ClientWorkspaceContextService** — 5 commands
- **DataMappingService** — 0 commands
- **DataTableModelMappingService** — 0 commands
- **ListDataTableModelMappingService** — 0 commands
- **ManualTableModelMappingService** — 0 commands

### Read Models
- **AccountBalancesRm** — 5 events → Dictionary-based
- **ChartOfAccountsRm** — 1 events → SourceCache<Account, Guid>
- **ChartOfAccountsRm** — 4 events → Dictionary-based
- **ClientWorkspaceContextRm** — 10 events → Dictionary-based
- **ContractorListRm** — 2 events → List-based
- **CounterpartiesRm** — 11 events → Dictionary-based
- **CustomerListRm** — 3 events → Dictionary-based
- **CustomersRm** — 2 events → SourceCache<Customer, Guid>
- **DataMappingsRm** — 3 events → Dictionary-based
- **DataSourceListRm** — 10 events → Dictionary-based
- **DataSourceRm** — 11 events → Dictionary-based
- **DataSourceSampleDataRm** — 1 events → Dictionary-based
- **DataSourceUsageRm** — 5 events → Dictionary-based
- **DataTableDefinitionRm** — 9 events → List-based
- **DataTableFromTemplateRm** — 5 events → Dictionary-based
- **DataTableMapRm** — 12 events → Dictionary-based
- **DataTablesDefinitionRm** — 3 events → Dictionary-based
- **EmployeeListRm** — 2 events → List-based
- **FileStoreRm** — 2 events → Dictionary-based
- **FinancialModelListRm** — 17 events → Dictionary-based
- **FinancialModelRm** — 49 events → Dictionary-based
- **ForecasterInputTablesRm** — 2 events → Dictionary-based
- **JournalCashAccountsRm** — 2 events → List-based
- **JournalDataRm** — 1 events → Unknown
- **ListDataTableMapRm** — 12 events → Dictionary-based
- **ManualDataTableRm** — 3 events → Dictionary-based
- **ManualEntriesRm** — 9 events → Dictionary-based
- **ManualTableMapRm** — 14 events → Dictionary-based
- **ManualTableRm** — 14 events → ReadModelProperty
- **ModelVerificationRM** — 7 events → Dictionary-based
- **PersonnelRm** — 4 events → SourceCache<Employee, Guid>
- **ProductListRm** — 3 events → Dictionary-based
- **ProductsRm** — 2 events → SourceCache<Product, Guid>
- **ReferenceDataRm** — 8 events → Dictionary-based
- **SingleClientTasklistRm** — 21 events → Dictionary-based
- **StatementTxTypesRm** — 5 events → Dictionary-based
- **UserDefinedWorksheetsRm** — 5 events → ReadModelProperty
- **VendorListRm** — 3 events → Dictionary-based
- **VendorsRm** — 2 events → SourceCache<Vendor, Guid>
- **WorkspaceTasklistRm** — 21 events → Dictionary-based

## TeamModelMgmt

### Aggregates
- **ManagedFinancialModel** — `src/TeamModelManagement/TeamModelMgmt/Domain/ManagedFinancialModel.cs` (125 lines)
- **TeamSettings** — `src/TeamModelManagement/TeamModelMgmt/Domain/TeamSettings.cs` (105 lines)

### Handlers
- **ManagedFinancialModelSvc** — 10 commands
- **TeamSettingsSvc** — 7 commands

### Read Models
- **DataSourcesRm** — 3 events → Dictionary-based
- **ModelsRm** — 15 events → SourceCache<ModelInfo, Guid>
- **SingleModelTaskMetricsRm** — 8 events → Unknown
- **TaskMetricsRm** — 1 events → List-based
- **TeamSettingsRm** — 7 events → ReadModelProperty

## UIBehavior

### Handlers
- **ImportFilesRm** — 0 commands
- **TransactionsRm** — 0 commands

### Read Models
- **CompositeRowsRm** — 5 events → Dictionary-based
- **ConnectionsRm** — 3 events → ReadModelProperty
- **CounterpartiesRm** — 2 events → SourceCache<CounterpartyModel, Guid>
- **CreateBusinessWorkflowRm** — 4 events → Dictionary-based
- **DataMappingsRm** — 4 events → Dictionary-based
- **DataSourceListRm** — 11 events → SourceCache<DataSourceModel, Guid>
- **DataTableMapsRm** — 16 events → SourceCache<DataSourceVersionInUse, Guid>
- **DataTableModelRm** — 17 events → SourceCache<DataTableModel, Guid>
- **FileStoreRm** — 2 events → SourceCache<string, string>
- **FinancialModelListRm** — 15 events → SourceCache<FinancialModelListItemModel, Guid>
- **FinancialModelTablesRm** — 54 events → SourceCache<TableSummary, Guid>
- **JournalCashTransactionsRm** — 2 events → SourceCache<CashTransaction, Guid>
- **ManualEntriesRm** — 4 events → Dictionary-based
- **PreprocessorsRm** — 2 events → HashSet-based
- **ReadableFormulasRm** — 5 events → SourceCache<ReadableRowFormula, Guid>
- **ReferenceDataRm** — 6 events → Dictionary-based
- **ReferencesRm** — 7 events → Dictionary-based
- **SingleUserTasklistRm** — 2 events → SourceCache<TasklistItem, Guid>
- **SingleWorkspaceTasklistRm** — 2 events → SourceCache<TasklistItem, Guid>
- **SolutionLibraryRm** — 4 events → SourceCache<Solution, Guid>
- **StatementTxTypesRm** — 2 events → SourceCache<CustomStatementTxType.StatementTxType, Guid>
- **TemplatesInModelRm** — 2 events → SourceCache<SpreadSheetMsgs.TemplateDetails, Guid>
- **WorkspacesRm** — 6 events → SourceCache<ClientWorkspace, Guid>

