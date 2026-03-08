# Message Map

Generated: 2026-03-08 00:08:55

## By Handler

### AccountingSystemService

- **Path:** `src/ModelServer/ModelServer/Services/AccountingSystemService.cs`
- **Commands:** 34
- **Aggregates:** AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Customer, Employee, Product, Vendor

| Command |
|---------|
| CreateAccountingSystem |
| CreateChartOfAccounts |
| AddRootAccount |
| AddChildAccount |
| UpdateAccount |
| AddGroupSet |
| RemoveGroupSet |
| AddGroupToSet |
| RemoveGroupFromSet |
| AddAccountToGroup |
| RemoveAccountFromGroup |
| CreateCounterpartyAsOrganization |
| CreateCounterpartyAsIndividual |
| DisplayAsOrganization |
| DisplayAsIndividual |
| AddRoles |
| UpdateOrganizationName |
| UpdateOrganizationUrl |
| UpdateContactName |
| UpdateEmailAddress |
| UpdatePhoneNumber |
| UpdateNotes |
| CreateAccountBalance |
| UpdateAccountBalance |
| AssociateFiles |
| DeleteAccountBalance |
| CreateProduct |
| UpdateProduct |
| CreateCustomer |
| UpdateCustomer |
| CreateVendor |
| UpdateVendor |
| CreateEmployee |
| CreateContractor |

### AccountStatementConfigService

- **Path:** `src/ModelServer/ModelServer/Services/AccountStatementConfigService.cs`
- **Commands:** 4
- **Aggregates:** CustomStatementTxType

| Command |
|---------|
| CreateCustomType |
| RenameCustomType |
| HideCustomType |
| UnhideCustomType |

### ClientWorkspaceContextService

- **Path:** `src/ModelServer/SpreadsheetAdapter/Services/ClientWorkspaceContextService.cs`
- **Commands:** 5
- **Aggregates:** 

| Command |
|---------|
| CreateFileBasedClientWorkspace |
| CreateDbBasedClientWorkspace |
| ActivateClientWorkspace |
| AddBusiness |
| AddSandbox |

### ClientWorkspaceService

- **Path:** `src/ModelServer/ModelServer/Services/ClientWorkspaceService.cs`
- **Commands:** 9
- **Aggregates:** ClientWorkspace

| Command |
|---------|
| CreateClientWorkspace |
| UpdateWorkspaceMetadata |
| RenameClientWorkspace |
| AddBusiness |
| RenameBusiness |
| DeleteBusiness |
| AddSandbox |
| RenameSandbox |
| DeleteSandbox |

### DataMappingService

- **Path:** `src/ModelServer/SpreadsheetAdapter/Services/DataMappingService.cs`
- **Commands:** 0
- **Aggregates:** 


### DataSourceModelMapService

- **Path:** `src/ModelServer/ModelServer/Services/DataSourceModelMapService.cs`
- **Commands:** 3
- **Aggregates:** DataSourceMapping

| Command |
|---------|
| CreateDataSourceMapping |
| AssociateEntrySet |
| DisassociateEntrySet |

### DataSourceService

- **Path:** `src/ModelServer/ModelServer/Services/DataSourceService.cs`
- **Commands:** 12
- **Aggregates:** DataElement, DataSource

| Command |
|---------|
| CreateDataSource |
| UpdateDataSource |
| UpdateDataSourceWithInternalData |
| AddSampleData |
| AllowOriginDataPush |
| ProhibitOriginDataPush |
| UpdateRawData |
| RequestOriginDataPush |
| ArchiveDataSource |
| RestoreArchivedDataSource |
| DeleteDataSource |
| RestoreDeletedDataSource |

### DataTableDefinitionService

- **Path:** `src/ModelServer/ModelServer/Services/DataTableDefinitionService.cs`
- **Commands:** 7
- **Aggregates:** DataTableDefinition

| Command |
|---------|
| DeleteDataTable |
| RenameDataTable |
| ChangeDescription |
| AddFilter |
| RemoveFilter |
| HideColumn |
| ShowColumn |

### DataTableModelMappingService

- **Path:** `src/ModelServer/SpreadsheetAdapter/Services/DataTableModelMappingService.cs`
- **Commands:** 0
- **Aggregates:** 


### EntrySetService

- **Path:** `src/ModelServer/ModelServer/Services/EntrySetService.cs`
- **Commands:** 7
- **Aggregates:** EntrySet

| Command |
|---------|
| AddEntrySet |
| RenameEntrySet |
| RemoveEntrySet |
| AddManualEntry |
| RenameManualEntry |
| CorrectManualEntry |
| RemoveManualEntry |

### FileStoreService

- **Path:** `src/ModelServer/ModelServer/Services/FileStoreService.cs`
- **Commands:** 3
- **Aggregates:** FileStore

| Command |
|---------|
| CreateFileStore |
| DeleteFileStore |
| AddFile |

### FinancialModelService

- **Path:** `src/ModelServer/ModelServer/Services/FinancialModelService.cs`
- **Commands:** 166
- **Aggregates:** DataTableDefinition, DataTableMap, ListDataTableMap, ManualTableMap, ServerFinancialModel

| Command |
|---------|
| CreateModel |
| CreateModelFromSolution |
| CreateModelFromTemplates |
| CreateModelFromModel |
| CreateDataMappingFromTemplate |
| DefineModelCellFormat |
| RenameModel |
| SetModelDescription |
| AddLibraryTemplate |
| AddLibraryTemplateWithCommands |
| AddTemplateWorksheets |
| AddModelTableWorksheet |
| DeleteModelTableWorksheet |
| ReorderModelTableWorksheets |
| RenameModelTableWorksheet |
| ArchiveModel |
| RestoreArchivedModel |
| DeleteModel |
| RestoreDeletedModel |
| AddTable |
| AddSingleRowTable |
| AddModelTableFromTemplate |
| ChangeTableDisplayOptions |
| UpdateTableSummaryStandardFormulaType |
| DeleteTable |
| RenameTable |
| SetTableDescription |
| DuplicateModelTable |
| MoveTableUp |
| MoveTableDown |
| AddUntypedRow |
| AddSpacerRow |
| AddCompositeRow |
| AssociateRowsToComposite |
| AddRowsToComposite |
| RemoveRowAssociationsFromComposite |
| AddCompositeRowAndAssociateChildren |
| AddCompositeRowWithChildren |
| UpdateCompositeSortOrder |
| UpdateCompositeRowCustomFormula |
| ClearCompositeRowCustomFormula |
| RenameRow |
| RenameRows |
| SetRowTypeToData |
| SetRowTypeToFormula |
| SetRowTypeToRowReference |
| SetRowTypeToTableReference |
| SetRowTypeToComposite |
| AddMultipleDataRows |
| UpdateFormulaRow |
| UpdateRowReference |
| UpdateTableReferenceRow |
| UpdateRowNotes |
| RestrictRowDateRanges |
| RemoveRowDateRangeRestrictions |
| RenameTableSummaryRow |
| UpdateTableSummaryCustomFormula |
| SetStandardRowFormat |
| SetCustomRowFormat |
| UpdateRowStyle |
| ClearRowStyle |
| MoveRow |
| DeleteRows |
| SetInitialValue |
| SetInitialValueFormula |
| ClearInitialValue |
| UpdateInitialValueDescription |
| AddListDataTable |
| ListDataTableUpdateEnvelope |
| RenameListDataTable |
| DeleteListDataTable |
| AddListDataTableSchemaFields |
| UpdateListDataTableFieldFormula |
| AddListDataTableAllowedValues |
| FormatListDataTableField |
| AddListDataTableRow |
| AddListDataTableRowWithPartialData |
| UpdateListDataTableRow |
| UpdateListDataTablePartialRow |
| UpdateListDataTableRowMetadata |
| DeleteListDataTableRow |
| AddSnapshotMarkerToListDataTable |
| AddJournalUpdateMarkerToListDataTable |
| AddDataTableDefinitionToModel |
| RemoveDataTableDefinitionFromModel |
| MapDataTableToModelPerRow |
| MapDataTableToModelToSingleRow |
| MapDataTableToModelGroupedByField |
| RemoveDataTableMapFromModel |
| AddRowsForDataTableMap |
| DeleteAllRowsForDataTableMap |
| DeleteRowsForDataTableMap |
| AddRowsForMapToCompositeChildren |
| MapListDataTableToModelPerRow |
| MapListDataTableToModelToSingleRow |
| MapListDataTableToModelGroupedByField |
| RemoveListDataTableMapFromModel |
| AddManualTableWorksheet |
| RenameManualTableWorksheet |
| AddManualTableToModel |
| RemoveManualTableFromModel |
| AddManualTableToWorksheet |
| MapManualTableToModelPerRow |
| MapManualTableToModelToSingleRow |
| MapManualTableToModelGroupedByField |
| RemoveManualTableMapFromModel |
| AddDataTablesToTemplate |
| UpdatePerRowMap |
| UpdateSingleRowMap |
| UpdateGroupedMap |
| AssociateDataElementToRow |
| AssociateDataElementsToRows |
| RemoveDataElementToRowAssociation |
| RemoveDataElementToRowAssociations |
| ClearDataElementToRowAssociations |
| UpdatePerRowMap |
| UpdateSingleRowMap |
| UpdateGroupedMap |
| AssociateDataElementToRow |
| AssociateDataElementsToRows |
| RemoveDataElementToRowAssociation |
| RemoveDataElementToRowAssociations |
| ClearDataElementToRowAssociations |
| UpdatePerRowMap |
| UpdateSingleRowMap |
| UpdateGroupedMap |
| AssociateManualTableRowToModelRow |
| AssociateManualTableRowsToModelRows |
| RemoveManualTableRowToModelRowAssociation |
| ClearManualTableRowToModelRowAssociations |
| AssociateMetadataGroupingToModelRows |
| ClearMetadataGroupingAssociations |
| UpdateDataSource |
| ApplyPencilEdit |
| RetainPencilEdit |
| AddGroup |
| RemoveGroup |
| ResizeGroup |
| MoveGroup |
| ShowGroupSummaryInHeader |
| RemoveGroupSummaryFromHeader |
| ShowGroupSubtotalRow |
| HideGroupSubtotalRow |
| UpdateGroupSummaryStandardFormulaType |
| UpdateGroupSummaryFormula |
| DuplicateGroup |
| CompleteChanges |
| ReviewModel |
| ApproveModel |
| RevokeModelApproval |
| RequestReview |
| RequestApproval |
| RecordUserReviewOfUpdates |
| CreateDefaultConstantsTable |
| CreateConstantsTableForSheet |
| AddConstant |
| DeleteConstant |
| RenameConstant |
| UpdateConstantValue |
| SetStandardFormatForConstant |
| SetCustomFormatForConstant |
| UpdateConstantDescription |
| UpdateWorksheetTabColor |
| ClearWorksheetTabColor |
| AddUserDefinedWorksheets |
| RemoveUserDefinedWorksheets |

### ImportFilesRm

- **Path:** `src/CustomUI/PowerModels.UIBehavior/Components/AccountingSystem/ReadModels/ImportFilesRm.cs`
- **Commands:** 0
- **Aggregates:** 


### ListDataTableModelMappingService

- **Path:** `src/ModelServer/SpreadsheetAdapter/Services/ListDataTableModelMappingService.cs`
- **Commands:** 0
- **Aggregates:** 


### ManagedFinancialModelSvc

- **Path:** `src/TeamModelManagement/TeamModelMgmt/Services/ManagedFinancialModelSvc.cs`
- **Commands:** 10
- **Aggregates:** ManagedFinancialModel

| Command |
|---------|
| CreateModel |
| UpdateModelData |
| PublishModel |
| AddDataSource |
| ConfigureNotifications |
| AddNotification |
| ClearNotifications |
| ConfigurePublishingSchedule |
| CreateReport |
| CreateDataSource |

### ManualTableModelMappingService

- **Path:** `src/ModelServer/SpreadsheetAdapter/Services/ManualTableModelMappingService.cs`
- **Commands:** 0
- **Aggregates:** 


### ManualTableService

- **Path:** `src/ModelServer/ModelServer/Services/ManualTableService.cs`
- **Commands:** 12
- **Aggregates:** ManualTable

| Command |
|---------|
| CreateDataTable |
| CreateUnstructuredTable |
| CreateEmptyTableOnModelWorksheet |
| DeleteTable |
| ChangeWorksheet |
| RenameTable |
| ChangeDescription |
| UpdateAsDataTable |
| UpdateAsUnstructuredTable |
| UpdateTableContents |
| ResizeTable |
| UpdateTableCells |

### QuickbooksDataSourceService

- **Path:** `src/Connectors/QBConnector/AddIn/QuickbooksDataSourceService.cs`
- **Commands:** 0
- **Aggregates:** 


### TasklistService

- **Path:** `src/ModelServer/ModelServer/Services/TasklistService.cs`
- **Commands:** 20
- **Aggregates:** TasklistItem

| Command |
|---------|
| CreateManualTasklistItem |
| AssociateManualTasklistItemWithBusiness |
| AssociateManualTasklistItemWithJournalEntry |
| AssociateManualTasklistItemWithModel |
| RemoveManualTasklistItemAssociation |
| UpdateTasklistItemSummary |
| ConvertManualTasklistItemToInternal |
| ConvertManualTasklistItemToExternal |
| UpdateTasklistItemNotes |
| ConfigureWeeklyRecurrence |
| ConfigureBiweeklyRecurrence |
| ConfigureMonthlyRecurrence |
| ConfigureQuarterlyRecurrence |
| ConfigureSemiAnnualRecurrence |
| ConfigureYearlyRecurrence |
| RemoveRecurrence |
| AssignTasklistItem |
| UpdateTasklistItemDueDate |
| CompleteTasklistItem |
| ReopenTasklistItem |

### TeamSettingsSvc

- **Path:** `src/TeamModelManagement/TeamModelMgmt/Services/TeamSettingsSvc.cs`
- **Commands:** 7
- **Aggregates:** TeamSettings

| Command |
|---------|
| ConfigureModelWorkflow |
| RequireApprovalByOthers |
| AllowSelfApproval |
| RequireChangeCompletion |
| MakeChangeCompletionOptional |
| RequireReviewForDataUpdates |
| MakeDataUpdatesReviewOptional |

### TransactionsRm

- **Path:** `src/CustomUI/PowerModels.UIBehavior/Components/TransactionWorkflow/ReadModels/TransactionsRm.cs`
- **Commands:** 0
- **Aggregates:** 


### UserDefinedWorksheetsService

- **Path:** `src/ModelServer/ModelServer/Services/UserDefinedWorksheetsService.cs`
- **Commands:** 4
- **Aggregates:** UserDefinedWorksheets

| Command |
|---------|
| AddUserDefinedWorksheets |
| UpdateUserDefinedWorksheets |
| DeleteUserDefinedWorksheets |
| RecoverUserDefinedWorksheets |

## By Aggregate (reverse lookup)

### AccountBalance

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Customer, Employee, Product, Vendor |

<details><summary>Commands for AccountBalance</summary>

**AccountingSystemService:** CreateAccountBalance, UpdateAccountBalance, AssociateFiles, DeleteAccountBalance

</details>

### AccountingSystem

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, ChartOfAccounts, Contractor, Counterparty, Customer, Employee, Product, Vendor |

<details><summary>Commands for AccountingSystem</summary>

**AccountingSystemService:** CreateAccountingSystem

</details>

### ChartOfAccounts

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, Contractor, Counterparty, Customer, Employee, Product, Vendor |

<details><summary>Commands for ChartOfAccounts</summary>

**AccountingSystemService:** CreateChartOfAccounts, AddRootAccount, AddChildAccount, UpdateAccount, AddGroupSet, RemoveGroupSet, AddGroupToSet, RemoveGroupFromSet, AddAccountToGroup, RemoveAccountFromGroup

</details>

### ClientWorkspace

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| ClientWorkspaceService | 9 | — |

<details><summary>Commands for ClientWorkspace</summary>

**ClientWorkspaceService:** CreateClientWorkspace, UpdateWorkspaceMetadata, RenameClientWorkspace, AddBusiness, RenameBusiness, DeleteBusiness, AddSandbox, RenameSandbox, DeleteSandbox

</details>

### Contractor

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Counterparty, Customer, Employee, Product, Vendor |

<details><summary>Commands for Contractor</summary>

**AccountingSystemService:** CreateContractor

</details>

### Counterparty

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Customer, Employee, Product, Vendor |

<details><summary>Commands for Counterparty</summary>

**AccountingSystemService:** DisplayAsOrganization, DisplayAsIndividual, AddRoles, UpdateOrganizationName, UpdateOrganizationUrl, UpdateContactName, UpdateEmailAddress, UpdatePhoneNumber, UpdateNotes

</details>

### Customer

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Employee, Product, Vendor |

<details><summary>Commands for Customer</summary>

**AccountingSystemService:** CreateCustomer, UpdateCustomer

</details>

### CustomStatementTxType

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountStatementConfigService | 4 | — |

<details><summary>Commands for CustomStatementTxType</summary>

**AccountStatementConfigService:** CreateCustomType, RenameCustomType, HideCustomType, UnhideCustomType

</details>

### DataElement

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| DataSourceService | 12 | DataSource |

<details><summary>Commands for DataElement</summary>

**DataSourceService:** UpdateDataSource, UpdateDataSourceWithInternalData, RequestOriginDataPush

</details>

### DataSource

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| DataSourceService | 12 | DataElement |

<details><summary>Commands for DataSource</summary>

**DataSourceService:** CreateDataSource, UpdateDataSource, UpdateDataSourceWithInternalData, AddSampleData, AllowOriginDataPush, ProhibitOriginDataPush, UpdateRawData, RequestOriginDataPush, ArchiveDataSource, RestoreArchivedDataSource, DeleteDataSource, RestoreDeletedDataSource

</details>

### DataSourceMapping

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| DataSourceModelMapService | 3 | — |

<details><summary>Commands for DataSourceMapping</summary>

**DataSourceModelMapService:** AssociateEntrySet, DisassociateEntrySet

</details>

### DataTableDefinition

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| DataTableDefinitionService | 7 | — |
| FinancialModelService | 166 | DataTableMap, ListDataTableMap, ManualTableMap, ServerFinancialModel |

<details><summary>Commands for DataTableDefinition</summary>

**DataTableDefinitionService:** DeleteDataTable, RenameDataTable, ChangeDescription, AddFilter, RemoveFilter, HideColumn, ShowColumn

**FinancialModelService:** AddDataTableDefinitionToModel

</details>

### DataTableMap

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| FinancialModelService | 166 | DataTableDefinition, ListDataTableMap, ManualTableMap, ServerFinancialModel |

<details><summary>Commands for DataTableMap</summary>

**FinancialModelService:** MapDataTableToModelPerRow, MapDataTableToModelToSingleRow, MapDataTableToModelGroupedByField, RemoveDataTableMapFromModel, MapListDataTableToModelPerRow, MapListDataTableToModelToSingleRow, MapListDataTableToModelGroupedByField, MapManualTableToModelPerRow, MapManualTableToModelToSingleRow, MapManualTableToModelGroupedByField

</details>

### Employee

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Customer, Product, Vendor |

<details><summary>Commands for Employee</summary>

**AccountingSystemService:** CreateEmployee

</details>

### EntrySet

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| EntrySetService | 7 | — |

<details><summary>Commands for EntrySet</summary>

**EntrySetService:** RenameEntrySet, RemoveEntrySet, AddManualEntry, RenameManualEntry, CorrectManualEntry, RemoveManualEntry

</details>

### FileStore

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| FileStoreService | 3 | — |

<details><summary>Commands for FileStore</summary>

**FileStoreService:** CreateFileStore, DeleteFileStore, AddFile

</details>

### ListDataTableMap

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| FinancialModelService | 166 | DataTableDefinition, DataTableMap, ManualTableMap, ServerFinancialModel |

<details><summary>Commands for ListDataTableMap</summary>

**FinancialModelService:** RemoveListDataTableMapFromModel, AssociateDataElementToRow, AssociateDataElementsToRows, RemoveDataElementToRowAssociation, RemoveDataElementToRowAssociations, ClearDataElementToRowAssociations

</details>

### ManagedFinancialModel

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| ManagedFinancialModelSvc | 10 | — |

<details><summary>Commands for ManagedFinancialModel</summary>

**ManagedFinancialModelSvc:** CreateModel, UpdateModelData, PublishModel, AddDataSource, ConfigureNotifications, AddNotification, ClearNotifications, ConfigurePublishingSchedule, CreateReport, CreateDataSource

</details>

### ManualTable

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| ManualTableService | 12 | — |

<details><summary>Commands for ManualTable</summary>

**ManualTableService:** DeleteTable, ChangeWorksheet, RenameTable, ChangeDescription, UpdateAsDataTable, UpdateAsUnstructuredTable, UpdateTableContents, ResizeTable, UpdateTableCells

</details>

### ManualTableMap

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| FinancialModelService | 166 | DataTableDefinition, DataTableMap, ListDataTableMap, ServerFinancialModel |

<details><summary>Commands for ManualTableMap</summary>

**FinancialModelService:** RemoveManualTableMapFromModel, UpdatePerRowMap, UpdateSingleRowMap, UpdateGroupedMap, AssociateManualTableRowToModelRow, AssociateManualTableRowsToModelRows, RemoveManualTableRowToModelRowAssociation, ClearManualTableRowToModelRowAssociations, AssociateMetadataGroupingToModelRows, ClearMetadataGroupingAssociations

</details>

### Product

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Customer, Employee, Vendor |

<details><summary>Commands for Product</summary>

**AccountingSystemService:** CreateProduct, UpdateProduct

</details>

### ServerFinancialModel

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| FinancialModelService | 166 | DataTableDefinition, DataTableMap, ListDataTableMap, ManualTableMap |

<details><summary>Commands for ServerFinancialModel</summary>

**FinancialModelService:** CreateDataMappingFromTemplate, DefineModelCellFormat, RenameModel, SetModelDescription, AddLibraryTemplate, AddLibraryTemplateWithCommands, AddTemplateWorksheets, AddModelTableWorksheet, DeleteModelTableWorksheet, ReorderModelTableWorksheets, RenameModelTableWorksheet, ArchiveModel, RestoreArchivedModel, DeleteModel, RestoreDeletedModel, AddTable, AddSingleRowTable, AddModelTableFromTemplate, ChangeTableDisplayOptions, UpdateTableSummaryStandardFormulaType, DeleteTable, RenameTable, SetTableDescription, DuplicateModelTable, MoveTableUp, MoveTableDown, AddUntypedRow, AddSpacerRow, AddCompositeRow, AssociateRowsToComposite, AddRowsToComposite, RemoveRowAssociationsFromComposite, AddCompositeRowAndAssociateChildren, AddCompositeRowWithChildren, UpdateCompositeSortOrder, UpdateCompositeRowCustomFormula, ClearCompositeRowCustomFormula, RenameRow, RenameRows, SetRowTypeToData, SetRowTypeToFormula, SetRowTypeToRowReference, SetRowTypeToTableReference, SetRowTypeToComposite, AddMultipleDataRows, UpdateFormulaRow, UpdateRowReference, UpdateTableReferenceRow, UpdateRowNotes, RestrictRowDateRanges, RemoveRowDateRangeRestrictions, RenameTableSummaryRow, UpdateTableSummaryCustomFormula, SetStandardRowFormat, SetCustomRowFormat, UpdateRowStyle, ClearRowStyle, MoveRow, DeleteRows, SetInitialValue, SetInitialValueFormula, ClearInitialValue, UpdateInitialValueDescription, AddListDataTable, ListDataTableUpdateEnvelope, RenameListDataTable, DeleteListDataTable, AddListDataTableSchemaFields, UpdateListDataTableFieldFormula, AddListDataTableAllowedValues, FormatListDataTableField, AddListDataTableRow, AddListDataTableRowWithPartialData, UpdateListDataTableRow, UpdateListDataTablePartialRow, UpdateListDataTableRowMetadata, DeleteListDataTableRow, AddSnapshotMarkerToListDataTable, AddJournalUpdateMarkerToListDataTable, AddDataTableDefinitionToModel, RemoveDataTableDefinitionFromModel, MapDataTableToModelPerRow, MapDataTableToModelToSingleRow, MapDataTableToModelGroupedByField, RemoveDataTableMapFromModel, AddRowsForDataTableMap, DeleteAllRowsForDataTableMap, DeleteRowsForDataTableMap, AddRowsForMapToCompositeChildren, MapListDataTableToModelPerRow, MapListDataTableToModelToSingleRow, MapListDataTableToModelGroupedByField, RemoveListDataTableMapFromModel, AddManualTableWorksheet, RenameManualTableWorksheet, AddManualTableToModel, RemoveManualTableFromModel, AddManualTableToWorksheet, MapManualTableToModelPerRow, MapManualTableToModelToSingleRow, MapManualTableToModelGroupedByField, RemoveManualTableMapFromModel, AddDataTablesToTemplate, UpdateDataSource, ApplyPencilEdit, RetainPencilEdit, AddGroup, RemoveGroup, ResizeGroup, MoveGroup, ShowGroupSummaryInHeader, RemoveGroupSummaryFromHeader, ShowGroupSubtotalRow, HideGroupSubtotalRow, UpdateGroupSummaryStandardFormulaType, UpdateGroupSummaryFormula, DuplicateGroup, CompleteChanges, ReviewModel, ApproveModel, RevokeModelApproval, RequestReview, RequestApproval, RecordUserReviewOfUpdates, CreateDefaultConstantsTable, CreateConstantsTableForSheet, AddConstant, DeleteConstant, RenameConstant, UpdateConstantValue, SetStandardFormatForConstant, SetCustomFormatForConstant, UpdateConstantDescription, AddUserDefinedWorksheets, RemoveUserDefinedWorksheets, UpdateWorksheetTabColor, ClearWorksheetTabColor

</details>

### TasklistItem

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| TasklistService | 20 | — |

<details><summary>Commands for TasklistItem</summary>

**TasklistService:** CreateManualTasklistItem, AssociateManualTasklistItemWithBusiness, AssociateManualTasklistItemWithJournalEntry, AssociateManualTasklistItemWithModel, RemoveManualTasklistItemAssociation, UpdateTasklistItemSummary, ConvertManualTasklistItemToInternal, ConvertManualTasklistItemToExternal, UpdateTasklistItemNotes, ConfigureWeeklyRecurrence, ConfigureBiweeklyRecurrence, ConfigureMonthlyRecurrence, ConfigureQuarterlyRecurrence, ConfigureSemiAnnualRecurrence, ConfigureYearlyRecurrence, RemoveRecurrence, AssignTasklistItem, UpdateTasklistItemDueDate, CompleteTasklistItem, ReopenTasklistItem

</details>

### TeamSettings

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| TeamSettingsSvc | 7 | — |

<details><summary>Commands for TeamSettings</summary>

**TeamSettingsSvc:** ConfigureModelWorkflow, RequireApprovalByOthers, AllowSelfApproval, RequireChangeCompletion, MakeChangeCompletionOptional, RequireReviewForDataUpdates, MakeDataUpdatesReviewOptional

</details>

### UserDefinedWorksheets

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| UserDefinedWorksheetsService | 4 | — |

<details><summary>Commands for UserDefinedWorksheets</summary>

**UserDefinedWorksheetsService:** UpdateUserDefinedWorksheets, DeleteUserDefinedWorksheets, RecoverUserDefinedWorksheets

</details>

### Vendor

| Handler | Handler Total | Also Touches |
|---------|---------------|--------------|
| AccountingSystemService | 34 | AccountBalance, AccountingSystem, ChartOfAccounts, Contractor, Counterparty, Customer, Employee, Product |

<details><summary>Commands for Vendor</summary>

**AccountingSystemService:** CreateVendor, UpdateVendor

</details>

