---
type: evidence
id: technical-codebase-review
label: PowerModels Technical Codebase Review
source: Code analysis of ~/source/repos/powermodels
date: 2026-02-14
confidence: high
supports_claims:
  - WPF primary interface confirmed in code
  - Excel integration via ExcelDna confirmed
  - AI integration via Semantic Kernel v1.70.0
  - Event-sourced architecture via ReactiveDomain
  - Team collaboration features present (TeamModelManagement)
  - Local-first architecture currently implemented
challenges_claims:
  - April 2026 cloud launch requires architecture migration
  - Current codebase is local-first, not cloud-based
status: active
---

# PowerModels Technical Codebase Review

## Source
Code repository: `c:\Users\chris\source\repos\powermodels`
Review date: 2026-02-14

## Overview
Review of actual PowerModels source code to validate strategic decisions and identify technical realities.

---

## Key Findings

### ✅ Confirmed Strategic Decisions

#### 1. WPF Primary Interface
**Location**: `src/CustomUI/PowerModels.App/`
- **Project**: `PowerModels.App.csproj` (WinExe application)
- **Framework**: .NET 10.0-windows10.0.19041
- **UI**: WPF with ReactiveUI.WPF v22.3.1
- **Pattern**: MVVM with ReactiveUI.Fody
- **Logging**: Serilog with multiple sinks

**Evidence**:
```xml
<PropertyGroup>
    <OutputType>WinExe</OutputType>
    <TargetFramework>net10.0-windows10.0.19041</TargetFramework>
    <UseWPF>true</UseWPF>
    <AssemblyTitle>PowerModels Accounting</AssemblyTitle>
</PropertyGroup>
```

#### 2. Excel Integration
**Location**: `src/Excel/`
- **PowerModels.Excel**: Main Excel add-in (ExcelDna)
- **PowerModels.Excel.Core**: Core functionality
- **PowerModels.Excel.Toolkit**: Reusable components
- **Integration**: ExcelDna v1.8.0 and ExcelDna.Interop v15.0.1

#### 3. AI Integration via Semantic Kernel
**Evidence**:
Both `PowerModels.App` and `PowerModels.WPF` include:
```xml
<PackageReference Include="Microsoft.SemanticKernel.Core" Version="1.70.0" />
```

**Implications**:
- AI capabilities are integrated at the core platform level
- Semantic Kernel is Microsoft's AI orchestration framework
- Validates "AI to empower accountants" strategy
- Recent version (1.70.0) suggests active development

#### 4. Event-Sourced Architecture
**Location**: `src/ModelServer/`
- **Framework**: ReactiveDomain (event sourcing + CQRS)
- **Storage**: LocalDataStore with Event Store
- **Pattern**: Domain-Driven Design (DDD)

**Components**:
- `ModelServer`: Core domain logic and business rules
- `LocalDataStore`: Event persistence
- `SpreadsheetAdapter`: Bridge between Excel and domain model

#### 5. Team Collaboration Features
**Location**: `src/TeamModelManagement/`
- **Purpose**: Team-based model management and sharing
- **Implication**: Firms ARE teams (confirmed in code)

---

## Current Architecture Reality

### Local-First Implementation
- **Deployment**: MSI installer for Windows
- **Storage**: Local file system event store
- **Execution**: Runs within Excel process (add-in)
- **Collaboration**: Optional cloud sync via TeamModelManagement

### Components Found
```
src/
├── CustomUI/
│   ├── PowerModels.App         (WPF standalone app)
│   ├── PowerModels.WPF         (WPF UI components + Semantic Kernel)
│   └── PowerModels.UIBehavior  (UI logic and behaviors)
├── Excel/
│   ├── PowerModels.Excel       (ExcelDna add-in)
│   ├── PowerModels.Excel.Core  (Core Excel integration)
│   └── PowerModels.Excel.Toolkit (Reusable Excel components)
├── ModelServer/
│   ├── ModelServer             (Domain logic, ReactiveDomain)
│   ├── LocalDataStore          (Event storage, local file system)
│   └── SpreadsheetAdapter      (Excel ↔ Domain bridge)
├── Connectors/
│   ├── QBConnector             (QuickBooks Online integration)
│   └── QBConnector.Core        (QB connectivity core)
├── TeamModelManagement/        (Team collaboration, cloud sync)
├── Utilities/
│   ├── PModel                  (CLI for model operations)
│   ├── PLicense                (License management)
│   ├── QModel                  (QuickBooks utilities)
│   └── DataStoreRepl           (Data store REPL)
└── Installer/                  (MSI creation with Inno Setup)
```

---

## Technology Stack (Actual)

### Core Platform
- **.NET 10.0** (not .NET 8 - docs are outdated)
- **C#** (primary language)
- **WPF** (Windows Presentation Foundation)
- **Windows 10 API** (10.0.19041)

### Key Dependencies
- **ExcelDna 1.8.0** - Excel add-in framework
- **ExcelDna.Interop 15.0.1** - Excel object model access
- **ReactiveDomain** - Event sourcing and CQRS framework
- **Microsoft.SemanticKernel.Core 1.70.0** - AI orchestration
- **ReactiveUI.WPF 22.3.1** - Reactive MVVM framework
- **ReactiveUI.Fody 19.5.41** - Property change weaving
- **Serilog** - Logging (Console, File, Async sinks)

### External Integrations
- **QuickBooks SDK/API** - QB Online integration
- **Event Store** - Event persistence (local implementation)

---

## Strategic Implications

### ✅ Alignment with Strategy
1. **WPF primary interface** - CONFIRMED in code
2. **Excel for advanced modeling** - CONFIRMED (dual interface exists)
3. **AI integration** - CONFIRMED (Semantic Kernel in both App and WPF)
4. **Event sourcing** - CONFIRMED (ReactiveDomain throughout)
5. **Firms as teams** - CONFIRMED (TeamModelManagement exists)
6. **QuickBooks integration** - CONFIRMED (QBConnector components)

### ⚠️ Architecture Migration Required
**Current**: Local-first (MSI, local event store, desktop app)
**April 2026**: Cloud-based GTM launch

**Migration needs**:
- Event store → cloud-hosted persistence
- Multi-tenant architecture (firm-level tenancy)
- Cloud-to-cloud integrations (QB/Xero)
- Team collaboration as first-class (not optional sync)
- Authentication and authorization for cloud access

---

## Product Scope Reality

### What the Code Shows
The codebase implements a **comprehensive financial modeling platform**:
- Model creation and management
- Excel integration with custom functions
- QuickBooks data import and sync
- Team collaboration and model sharing
- Event-sourced audit trail
- Version control and backup/restore
- Financial analysis and reporting
- License management and distribution

### Alignment with "Tax Prep Wedge → Platform" Strategy
- **Current code** = broad platform capabilities (matches long-term vision)
- **Phase 1 strategy** = market only tax prep features (subset of capabilities)
- **Implication**: Product can deliver tax prep wedge TODAY, expand features over time

---

## QuickBooks + Xero Integration

### QuickBooks: CONFIRMED
**Location**: `src/Connectors/QBConnector/`
- Extensive QB Online integration
- Authentication, data retrieval, mapping
- Utilities for QB operations (`QModel` CLI)

### Xero: NOT FOUND in current codebase
**Implication**: Xero integration needs to be built for April 2026 launch
**Priority**: Critical for "rapid onboarding" strategy

---

## Dual Interface Strategy Validated

### Standalone WPF App
- `PowerModels.App` - Windows executable
- Full application experience
- Team collaboration features
- Model management UI

### Excel Add-In
- `PowerModels.Excel` - ExcelDna add-in
- Runs within Excel process
- Custom ribbon and functions
- Spreadsheet integration

**Both share**:
- `PowerModels.Excel.Core` - Core functionality
- Same domain logic (ModelServer)
- Same event store
- Same AI capabilities (Semantic Kernel)

---

## Missing or Needs Attention

### For April 2026 Cloud Launch
1. **Xero integration** - Not present, needs to be built
2. **Cloud event store** - Currently local file system
3. **Multi-tenant architecture** - Single-user focused currently
4. **Cloud authentication** - OAuth, firm-level access control
5. **Association CPE materials** - Not code, but GTM requirement

### Documentation Gaps
- Architecture docs reference .NET 8.0 (code uses .NET 10.0)
- No cloud deployment docs
- No Xero integration docs

---

## Recommendations

### Technical Priorities for April 2026
1. **Cloud migration**: Event store → cloud-hosted (Azure, AWS, etc.)
2. **Xero connector**: Build parallel to QBConnector
3. **Multi-tenancy**: Firm-level data isolation
4. **Team features**: Make collaboration first-class, not optional
5. **Integration testing**: QB + Xero onboarding flows

### Product Strategy Validation
- ✅ Current codebase CAN deliver tax prep wedge
- ✅ Platform capabilities exist for future expansion
- ✅ Dual interface (WPF + Excel) is implemented
- ✅ AI integration is present and active
- ⚠️ Architecture migration to cloud is critical path item

---

## Confidence Assessment

**High confidence**:
- WPF, Excel, AI, Event sourcing, Teams, QB integration all confirmed in code
- Technology stack matches strategic decisions
- Product scope aligns with "wedge → platform" vision

**Medium confidence**:
- Cloud migration timeline (April 2026 is aggressive)
- Xero integration completeness (not started)

**Needs clarification**:
- Multi-tenant architecture design
- Cloud provider selection
- Migration path from local-first to cloud-first
