# Monday Stakeholder Demo — Feature Sprint Plan

**Date:** 2026-03-28
**Target:** Monday 2026-03-31 stakeholder demo
**Constraint:** Seeder-loaded data only (no ingestion workflow changes)

---

## Objective

Demonstrate visible execution progress with production-quality accounting features:
- Monthly P&L and Cash Flow breakdowns
- Fiscal year selection
- Excel and PDF export of all reports

## Current State (7 Reports Shipping)

| Report | RM | Tests | FlaUI |
|--------|-----|-------|-------|
| Trial Balance | `TrialBalanceRm` | ✅ Unit + Integration | ✅ |
| Income Statement | `IncomeStatementRm` | ✅ Integration | ✅ |
| Balance Sheet | `BalanceSheetRm` | ✅ Integration | ✅ |
| Cash Flow Statement | `CashFlowStatementRm` | ✅ | — |
| General Ledger | `GeneralLedgerRm` | ✅ | — |
| Income/Expense Summary | `IncomeExpenseSummaryRm` | ✅ | — |
| Journal Report | `JournalReportRm` | ✅ | — |

All reports use `AccountingReportBase` → `ReportBuilder` DSL → `IAccountingReport` → WPF renderer.

---

## Sprint Items (Priority Order)

### 1. P&L Monthly Breakdown

**Effort:** 0.5 day | **Risk:** Very low | **Agent-deliverable:** Yes

**Current:** `IncomeStatementRm.BuildReport()` produces a single aggregate view with 3 columns (Acct #, Account Name, Amount) and 2 sections (Revenue, Expenses).

**Change:** Loop over months in the `DateRange` period. Emit Revenue + Expenses sections **per month**, then a YTD Grand Total.

**Layout (section-per-month):**

```
Section: "January 2024"
  Revenue
    Consulting Revenue          $12,000.00
    Product Sales                $3,500.00
  Total Revenue                 $15,500.00
  Expenses
    Rent                         $2,000.00
    Payroll                      $8,000.00
  Total Expenses                $10,000.00
  Net Income                     $5,500.00

Section: "February 2024"
  Revenue
    ...
  Expenses
    ...
  Net Income                     $4,200.00

GRAND TOTAL: NET INCOME (YTD)   $9,700.00
```

**Implementation approach:**

```csharp
// In IncomeStatementRm.BuildReport(ReportPeriod period):
var builder = ReportBuilder
    .Vertical("Income Statement", "Income Statement")
    .InGroup(ReportGroup.FinancialStatements)
    .WithTemporalType(ReportTemporalType.Period)
    .Column<object?[]>("Acct #", l => l[0])
    .Column<object?[]>("Account Name", l => l[1])
    .Column<object?[]>("Amount", l => l[2], "$#,##0.00");

foreach (var month in EnumerateMonths(from, through)) {
    var monthStart = month;
    var monthEnd = new DateTime(month.Year, month.Month,
        DateTime.DaysInMonth(month.Year, month.Month));
    var lines = BuildIncomeStatementLines(monthStart, monthEnd);

    var revenueRows = lines.Where(l => l.Category == Income).Select(ToRow).ToArray();
    var expenseRows = lines.Where(l => l.Category == Expense).Select(ToRow).ToArray();
    var monthRevenue = lines.Where(l => l.Category == Income).Sum(l => l.Amount);
    var monthExpenses = lines.Where(l => l.Category == Expense).Sum(l => l.Amount);

    builder = builder
        .Section($"Revenue — {month:MMMM yyyy}", revenueRows,
            subtotal: _ => [null, "Total Revenue", monthRevenue])
        .Section($"Expenses — {month:MMMM yyyy}", expenseRows,
            subtotal: _ => [null, "Total Expenses", monthExpenses]);
    // Optional: Net Income row per month via a synthetic section
}

builder = builder.GrandTotal("NET INCOME (YTD)", _ => [null, "NET INCOME (YTD)", ytdNetIncome]);
```

**Why this works with zero framework changes:**
- `Section()` already takes a name and rows
- The WPF renderer already iterates sections and renders headers + rows + subtotals
- `BuildIncomeStatementLines(from, through)` already accepts date range — just call it per month
- Columns stay the same (Acct #, Name, Amount)

**Files to modify:**
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/IncomeStatementRm.cs`

**Tests to add:**
- Monthly section count matches month span
- Per-month revenue/expense totals match filtered journal data
- YTD grand total = sum of monthly net incomes
- Single-month period produces same output as current behavior

---

### 2. Excel Export (Generic — All Reports)

**Effort:** 1 day | **Risk:** Low | **Agent-deliverable:** After pattern established

**Approach:** Single generic exporter that takes any `IAccountingReport` and produces a ClosedXML worksheet. Multi-report mode produces a workbook with one tab per report.

**Data model mapping:**

| IAccountingReport | Excel |
|-------------------|-------|
| `SheetName` | Worksheet tab name (max 31 chars) |
| `ReportName` | Header row (merged, bold) |
| `Period.DisplayLabel` | Sub-header row |
| `Panel.Columns[].Header` | Column header row (bold, bordered) |
| `Panel.Columns[].NumberFormat` | Cell number format (e.g. `"$#,##0.00"`) |
| `Section.SectionName` | Section header row (bold) |
| `Section.Rows[]` | Data rows |
| `Section.SubtotalRow` | Subtotal row (bold) |
| `Panel.TotalRow` | Grand total row (bold, double-bordered) |

**Interface:**

```csharp
public static class ReportExporter {
    /// Export a single report to an Excel file
    public static void ToExcel(IAccountingReport report, string filePath);

    /// Export multiple reports to a multi-tab Excel workbook
    public static void ToExcelWorkbook(
        IEnumerable<IAccountingReport> reports, string filePath);

    /// Export a single report to PDF
    public static void ToPdf(IAccountingReport report, string filePath);
}
```

**NuGet dependency:** `ClosedXML` (MIT license, no Excel installation required)

**Files to create:**
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/Export/ReportExcelExporter.cs`

**Files to modify:**
- `src/CustomUI/PowerModels.UIBehavior/PowerModels.UIBehavior.csproj` — add ClosedXML package
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml` — add Export button
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml.cs` — wire button to exporter

**Tests to add:**
- Round-trip: BuildReport() → ToExcel() → read back with ClosedXML → verify cell values match
- Sheet name truncation at 31 chars
- Number format applied to money columns
- Multi-report workbook has correct tab count and names
- Empty report (HasData=false) produces empty sheet or is skipped

---

### 3. Fiscal Year Selector

**Effort:** 0.5 day | **Risk:** Low | **Agent-deliverable:** Partial (VM yes, XAML needs guidance)

**Current:** `ReportsContainerVm.SelectReport()` hardcodes `AsOf(DateTime.Today)`. No date picker or year selector in the UI.

**Change:**
1. Add `SelectedYear` property to `ReportsContainerVm`
2. Compute available years from journal data date range
3. Build `DateRange(fyStart, fyEnd)` from selected year
4. Rebuild report when year changes

**Default fiscal year:** Calendar year (January start). Configurable fiscal year start month is PMA Phase 3 scope — for Monday, calendar year is sufficient.

**VM changes:**

```csharp
// ReportsContainerVm additions:
[Reactive] public partial int? SelectedYear { get; set; }
public ObservableCollection<int> AvailableYears { get; } = [];

public void SelectReport(string reportName) {
    var rm = _context.GetReportRm(reportName);
    var period = BuildPeriod(rm);
    ReportVm.CurrentReport = rm.BuildReport(period);
}

private ReportPeriod BuildPeriod(AccountingReportBase rm) {
    var year = SelectedYear ?? DateTime.Today.Year;
    return rm.TemporalType switch {
        ReportTemporalType.Period => new ReportPeriod.DateRange(
            new DateTime(year, 1, 1),
            new DateTime(year, 12, 31)),
        _ => new ReportPeriod.AsOf(new DateTime(year, 12, 31))
    };
}
```

**XAML:** Year dropdown in the report header area, between report title and report content.

**Files to modify:**
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/ReportsContainerVm.cs`
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/AccountingReportsContext.cs` — expose date range from RMs
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml` — year dropdown
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml.cs` — binding

**Tests to add:**
- Year selection produces correct DateRange period
- AsOf reports use year-end date
- Period reports use Jan 1 – Dec 31

---

### 4. PDF Export (Generic — All Reports)

**Effort:** 1 day | **Risk:** Medium (library selection) | **Agent-deliverable:** After pattern established

**Recommended library:** QuestPDF (MIT license, fluent C# API, no external dependencies)

**Alternative:** HTML → PDF via PuppeteerSharp (heavier, requires Chromium download)

**Approach:** Same `IAccountingReport` traversal as Excel, but renders to PDF document with:
- Report title + period header
- Column headers with alignment
- Section headers, data rows, subtotals
- Grand total with ruling lines
- Page breaks between panels (side-by-side panels rendered as sequential pages)

**Interface:** Same `ReportExporter.ToPdf(IAccountingReport, filePath)` from Item 2.

**Files to create:**
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/Export/ReportPdfExporter.cs`

**Files to modify:**
- `src/CustomUI/PowerModels.UIBehavior/PowerModels.UIBehavior.csproj` — add QuestPDF package
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml` — add PDF export button
- `src/CustomUI/PowerModels.WPF/Components/Reports/ReportsContainer.xaml.cs` — wire button

**Tests to add:**
- PDF file produced is valid (non-zero size, starts with %PDF)
- All sections present in output
- Number formatting applied

---

### 5. Cash Flow Monthly Breakdown

**Effort:** 1 day | **Risk:** Low | **Agent-deliverable:** Yes

**Same pattern as P&L:** Section-per-month with Operating/Investing/Financing activities per month.

**Current:** `CashFlowStatementRm.BuildReport()` (283 lines) produces aggregate Operating/Investing/Financing sections with Net Change in Cash.

**Change:** Loop months, emit 3 activity sections per month (Operating, Investing, Financing), per-month subtotals, YTD grand total.

**Layout:**

```
Section: "Operating Activities — January 2024"
  Net Income                     $5,500.00
  Adjustments:
    Increase in Accounts Payable   $500.00
  Net Cash from Operating        $6,000.00

Section: "Investing Activities — January 2024"
  (none)

Section: "Financing Activities — January 2024"
  (none)

Section: "Operating Activities — February 2024"
  ...

GRAND TOTAL: NET CHANGE IN CASH  $8,200.00
```

**Files to modify:**
- `src/CustomUI/PowerModels.UIBehavior/Components/Reports/CashFlowStatementRm.cs`

**Tests to add:**
- Monthly section count = 3 × month count
- Per-month cash flow totals match filtered data
- YTD = sum of monthly net changes
- Beginning/ending cash balance correctness

---

## Execution Plan

### Friday Afternoon (Mar 28)

| Time | Task | Who |
|------|------|-----|
| PM | P&L monthly breakdown | Agent (via accounting-reports SKILL) |
| PM | P&L tests | Agent |

### Saturday (Mar 29)

| Time | Task | Who |
|------|------|-----|
| AM | Excel export — generic exporter | Human (establishes pattern) |
| AM | Excel export — tests | Human |
| PM | Fiscal year selector — VM + XAML | Human (XAML) + Agent (VM) |
| PM | Fiscal year selector — tests | Agent |

### Sunday (Mar 30)

| Time | Task | Who |
|------|------|-----|
| AM | PDF export — generic exporter | Human (library setup) + Agent (traversal) |
| PM | Cash Flow monthly breakdown | Agent |
| PM | Integration testing — all reports with year selector + export | Human |

### Monday AM (Mar 31)

| Time | Task | Who |
|------|------|-----|
| AM | Smoke test full demo flow: seed → open → select year → view reports → export Excel → export PDF | Human |
| AM | Screenshots / recording for stakeholder presentation | Human |

---

## Demo Script (Monday)

1. **Launch app** — open seeded Adventure Services workspace
2. **Navigate to Reports tab** — show 7+ reports in tree, grouped by category
3. **Select Trial Balance** — show balanced debits/credits, professional layout
4. **Select Income Statement** — show monthly breakdown sections (Jan, Feb, ...)
5. **Change fiscal year** — dropdown to different year, report refreshes
6. **Select Cash Flow Statement** — show monthly Operating/Investing/Financing
7. **Select Balance Sheet** — show Assets / Liabilities & Equity side-by-side
8. **Click "Export to Excel"** — save multi-tab workbook, open in Excel → show all reports as tabs
9. **Click "Export to PDF"** — save PDF, open → show print-ready report
10. **Talking points:**
    - "7 accounting reports, all event-sourced from the same data"
    - "Monthly breakdowns computed on-the-fly from journal events"
    - "One-click export to Excel or PDF — every report, every format"
    - "All built on our ES-DDD architecture — the data is auditable end-to-end"

---

## Dependencies

| Dependency | Status | Risk |
|------------|--------|------|
| ClosedXML NuGet package | Available (MIT) | None |
| QuestPDF NuGet package | Available (MIT, Community license for < $1M revenue) | Check license terms |
| Seeder data (adventure-services) | Working — 30 accounts, 615 journal entries | None |
| UITestRunner | Working — can verify reports render | None |
| WPF report renderer | Working — handles dynamic sections/columns | None |

## Success Criteria

- [ ] Income Statement shows monthly sections for selected fiscal year
- [ ] Cash Flow shows monthly Operating/Investing/Financing sections
- [ ] Year dropdown changes report period for all reports
- [ ] "Export to Excel" produces multi-tab workbook with all visible reports
- [ ] "Export to PDF" produces formatted PDF of current report
- [ ] All existing tests still pass
- [ ] UITestRunner trial balance test still passes
- [ ] Demo script runs end-to-end without errors
