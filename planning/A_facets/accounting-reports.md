---
type: facet
id: accounting-reports
label: Accounting Reports — Parallel Read-Side Interface
synonyms:
  - financial statements
  - balance sheet
  - income statement
  - cash flow statement
  - general ledger
  - trial balance
  - report builder DSL
  - read-side projection layer
  - AccountingReportsContext
links_to_facets:
  - event-sourced-architecture
  - domain-model-structure
  - wpf-excel-interface-strategy
  - proven-completeness
claims:
  - First dedicated read-side projection layer built directly in PMA, operating parallel to the existing write-side CQRS path
  - Seven report read models covering standard accounting statements — Balance Sheet, Income Statement, Cash Flow, General Ledger, Trial Balance, Income & Expense Summary, Journal Report
  - ReportBuilder DSL enables declarative report construction with period grouping, column definitions, and account filtering
  - AccountingReportsContext wires report RMs independently of the main ModelServerReadContext
  - Full test coverage for all 7 report types including integration tests
  - Dedicated report-builder agent and accounting-reports skill enable agentic implementation of new report types
  - Pattern establishes the template for all future PMA read-side features
evidence_refs:
  - technical-codebase-review
owner: product-team
status: active
---

# Accounting Reports — Parallel Read-Side Interface

## Description

The accounting reports subsystem is the first dedicated read-side projection layer built directly in PMA (PowerModels Accounting), operating parallel to the existing write-side CQRS path through SpreadsheetAdapter. This is architecturally significant because it establishes the pattern for all future PMA read-side features — report read models subscribe directly to domain event streams and project into structured report outputs without routing through the ACL.

## Key Points

- **7 report read models**: BalanceSheetRm (135 lines), IncomeStatementRm (99), CashFlowStatementRm (282), GeneralLedgerRm (110), IncomeExpenseSummaryRm (163), TrialBalanceRm (89), JournalReportRm (57)
- **Framework classes**: AccountingReportBase (178 lines), ReportBuilder (187 lines), AccountingReportsContext (44 lines), ReportPeriod, ReportColumn, ReportGroup, ReportTemporalType
- **WPF views**: AccountingReport.xaml, ReportsContainer.xaml
- **Temporal grouping**: Reports use ReportPeriod for monthly, quarterly, and annual views
- **Dedicated agent**: `report-builder` agent + `accounting-reports` skill for agentic implementation of new reports
- **Full test coverage**: Integration tests for all 7 report types plus unit tests for ReportBuilder and ReportPeriod

## Architectural Significance

This subsystem breaks the pattern where all read-side data flows through SpreadsheetAdapter. AccountingReportsContext wires report RMs directly in the PMA presentation layer, subscribing to domain event streams independently. This is the migration path for moving PMA away from the SpreadsheetAdapter dependency.

## Implementation Reference

Report read models in `src/CustomUI/PowerModels.UIBehavior/Components/AccountingReports/`. Framework in same directory. Tests in `PowerModels.UIBehavior.Tests/`. Detailed data in `implementation/scan-data.json`.
