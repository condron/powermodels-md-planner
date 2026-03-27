# God Aggregate: ServerFinancialModel

Generated: 2026-03-27 19:42:20
Path: `src/ModelServer/ModelServer/Domain/ServerFinancialModel.cs`
Lines: 3557

## What It Owns

- **Apply methods:** 87
- **Register calls:** 31

## Handlers

### FinancialModelService
- **Commands handled:** 166
- **Path:** `src/ModelServer/ModelServer/Services/FinancialModelService.cs`

## Registered Events (31)

- ArchivedModelRestored
- BeginListDataTableContentUpdate
- ChangesCompleted
- DataSourceUpdated
- DataTableDefinitionAdded
- DataTableDefinitionRemoved
- DataTableMappedToModel
- DataTableMapRemovedFromModel
- DataTableMapRowAdded
- DataTableMapRowsAdded
- DataTableMapRowsDeleted
- DeletedModelRestored
- EndListDataTableContentUpdate
- ListDataTableJournalUpdateMarkerAdded
- ListDataTableMappedToModel
- ListDataTableMapRemovedFromModel
- ListDataTableRowAdded
- ListDataTableRowDeleted
- ListDataTableRowUpdated
- ListDataTableSnapshotMarkerAdded
- ManualTableMappedToModel
- ManualTableMapRemovedFromModel
- ModelApprovalRevoked
- ModelApproved
- ModelArchived
- ModelCreated
- ModelDeleted
- ModelReviewed
- PencilEditApplied
- UserDefinedWorksheetsAdded
- UserDefinedWorksheetsRemoved

## Expansion Triggers

Any change that does the following is expanding the God Aggregate:
- Adds a new `Apply()` method to ServerFinancialModel
- Adds new state fields to the class
- Adds new event types handled by this aggregate
- Increases line count beyond 3557

## Recommendation

New domain concepts should be modeled as separate aggregates. If a feature seems like it belongs in ServerFinancialModel, consider whether it can be:
1. A new aggregate with its own event stream
2. A read model that projects from ServerFinancialModel events
3. An automation/policy that reacts to ServerFinancialModel events
