---
type: facet
id: quickbooks-xero-onboarding
label: Data Onboarding — QuickBooks, Xero, and Plaid
synonyms:
  - platform onboarding
  - accounting platform integration
  - QB Xero connectivity
  - rapid client transition
  - Plaid integration
  - bank account linking
  - direct bank feed
  - transaction enrichment
  - Plaid Enrich
  - Plaid Transactions
links_to_facets:
  - event-sourced-architecture
  - firm-first-gtm
  - association-community-marketing
  - shoebox-offering
claims:
  - QuickBooks and Xero integration critical for rapid onboarding
  - Platform transition enables fast client migration to PowerModels
  - Firm clients already on QB/Xero = ready data source
  - QB CSV Journal Report import is the only production-ready QB onboarding path today [VALIDATED by code review Feb 2026]
  - QB live API connector exists but is sandbox-only and from a previous product direction [VALIDATED by code review Feb 2026]
  - Xero connector does not exist — zero code zero issues zero references in codebase [VALIDATED by code review Feb 2026]
  - PowerModels is an authorized Plaid reseller with production access [CONFIRMED Feb 2026]
  - Plaid Transactions product is enabled — provides up to 24 months of bank transaction history via direct bank link [CONFIRMED Feb 2026]
  - Plaid Enrich product is enabled — cleanses categorizes and enhances transaction data from any source including non-Plaid [CONFIRMED Feb 2026]
  - Plaid Recurring Transactions is enabled — detects recurring expenses and deposits automatically [CONFIRMED Feb 2026]
  - Plaid Transactions Refresh is enabled — on-demand extraction with proactive new-transaction notifications [CONFIRMED Feb 2026]
  - Plaid Liabilities is enabled — credit card and debt account information [CONFIRMED Feb 2026]
  - Plaid provides a third onboarding path that bypasses QB/Xero entirely — direct bank account linking [NEW capability]
  - Plaid Enrich can enhance transaction data from non-Plaid sources making it useful for QB CSV imports too [NEW capability]
  - No Plaid integration code exists in the codebase yet — GitHub issue 1757 is open with no milestone [GAP identified Feb 2026]
  - Cloud-to-cloud integration reduces setup friction [ASPIRATIONAL — no cloud migration code exists]
  - Event-sourced import creates audit trail from day one
evidence_refs:
  - codebase-review-feb2026
  - technical-codebase-review
owner: product-team
status: active
---

# Data Onboarding — QuickBooks, Xero, and Plaid

## Description

Data onboarding is the critical first step in client engagement. PowerModels supports three onboarding paths — **QuickBooks** (CSV import today, API future), **Xero** (not yet built), and **Plaid** (authorized reseller, products enabled, integration not yet built). Plaid is a strategic addition: it enables direct bank account linking that **bypasses QB/Xero entirely**, serving both platform-connected clients and shoebox clients whose only "system" is their bank account.

## Strategic Importance

### Three Onboarding Paths

1. **QB/Xero Import** — Firm clients already on accounting platforms. Import their data.
2. **Plaid Direct Link** — Connect directly to client's bank accounts. No accounting platform needed.
3. **PDF/CSV Upload** — Shoebox path for clients with no system at all.

### Why This Matters

- **Path 1 (QB/Xero)**: Covers clients who already have organized books
- **Path 2 (Plaid)**: Covers clients who have bank accounts but no accounting system — the **shoebox sweet spot**
- **Path 3 (Upload)**: Covers the absolute worst case — paper receipts and PDFs
- **Combined**: Every client type has an onboarding path

### Platform Coverage
- **QuickBooks Online**: Dominant SMB accounting platform in US
- **Xero**: Strong in international markets, growing US presence
- **Plaid**: 12,000+ financial institutions, direct bank account access, 24 months of history
- **Combined coverage**: Nearly all small business financial data sources

## Plaid — Authorized Reseller (Feb 2026)

PowerModels is an **authorized Plaid reseller** with production access. The following products are enabled:

### Enabled Products

| Product | Status | Capability |
|---|---|---|
| **Transactions** | Enabled (0/200 used) | Up to 24 months of bank transaction history via direct bank link |
| **Transactions Refresh** | Enabled (0/200) | On-demand extraction + proactive notifications when new transactions available |
| **Recurring Transactions** | Enabled (0/200) | Automatic detection of recurring expenses and deposits |
| **Enrich** | Enabled (0/50) | Cleanse, categorize, and enhance transaction data — works on non-Plaid sources too |
| **Liabilities** | Enabled (0/200) | Credit card, PayPal, student loan, mortgage account information |
| Investments | Not enabled | Investment account balances, holdings, transactions |

### Strategic Significance of Plaid

1. **Shoebox game-changer**: Plaid + Enrich could replace PDF ingestion for many shoebox clients. Link their bank account → pull 24 months of enriched, categorized transactions → feed into AI pipeline. No PDF extraction needed.
2. **Enrich for all paths**: Plaid Enrich works on non-Plaid sources. QB CSV imports could be enhanced through Enrich for better categorization before hitting the AI pipeline.
3. **Recurring detection**: Plaid identifies recurring transactions automatically — directly feeds the Future Ledger concept (known commitments).
4. **Real-time refresh**: Transactions Refresh enables continuous monitoring, not just point-in-time import. This is the foundation for drift detection.
5. **Liability data**: Credit card and debt information provides a more complete financial picture than bank transactions alone.

### Plaid vs QB for Shoebox Clients

| Dimension | QB CSV Import | Plaid Direct Link |
|---|---|---|
| **Requires QB account** | Yes | No |
| **Transaction history** | Whatever user exports | Up to 24 months automatic |
| **Data quality** | Raw CSV, needs parsing | Enriched, categorized by Plaid |
| **Recurring detection** | Manual | Automatic (Recurring Transactions) |
| **Refresh capability** | Manual re-export | On-demand or automatic |
| **Shoebox clients** | Need QB first | Works with just a bank account |
| **Implementation status** | Working (CSV import) | Not yet built (issue #1757) |

### Implementation Status
- **Plaid authorization**: ✅ Authorized reseller with production access
- **Products enabled**: ✅ Transactions, Enrich, Liabilities, Recurring, Refresh
- **Cloud infrastructure + security**: 🔶 In progress (thefringeninja) — Plaid requires cloud deployment, secure credential management, and PII handling infrastructure before API integration can begin
- **Code integration**: 🔶 In progress — not yet visible in main codebase; stories may be tracked outside GitHub repo
- **Dependencies**: Cloud hosting, secure token storage, PII filtering (logging #1604-1612 is related prerequisite work)
- **Priority**: HIGH — Plaid could be the fastest path to "zero to client in 5 minutes" for shoebox clients

## How It Actually Works Today (Feb 2026 Code Review)

### Path A: QB CSV Journal Report Import (PRODUCTION-READY)
1. **User exports** Journal Report from QuickBooks Online as CSV
2. **File picker** opens in PowerModels
3. **CsvHelper parses** the QB export (handles multi-row headers, field mapping)
4. **Matches accounts** against existing Chart of Accounts
5. **Detects cash vs accrual** layer based on CashPositionType
6. **Produces JournalReportRow** objects feeding the standard pipeline

### Path B: QB Live API (SANDBOX-ONLY — previous product direction)
1. **OAuth authentication** via Intuit sandbox (hardcoded sandbox URLs)
2. **Synchronize handler** pulls CoA, Products, Vendors, Employees, Customers
3. **Journal Report API** retrieves 17-column transaction data
4. **Also available**: P&L, Balance Sheet, Bills, Invoices, Purchase Orders
5. **BLOCKED**: No production OAuth app, no Intuit approval, sandbox credentials only
6. **Direct API provider**: compiled out of release builds (`#if DEBUG` only)

### Path C: Xero (DOES NOT EXIST)
- Zero code, zero issues, zero references in entire codebase

### Phase 3: Event Sourcing Transformation
1. **QB/Xero transactions** converted to PowerModels events
2. **Event stream created** with complete lineage
3. **Audit trail established** from import timestamp
4. **Reproducible state** - can rebuild from events
5. **Platform independence** - no longer tied to QB/Xero

### Phase 4: Tax Prep Readiness
1. **Books prepared to 85%** using imported data
2. **AI categorization** applied to transactions
3. **Flagged items** surface for professional review
4. **Tax schedules** generated from event-sourced record
5. **Professional review** and sign-off

## Technical Architecture

### QuickBooks Connector (Implemented)
**Location**: `src/Connectors/QBConnector/`
- **QBConnector**: Main integration component
- **QBConnector.Core**: Core QB connectivity
- **QBConnector.Tests**: Integration test suite
- **QModel**: CLI utility for QB operations

**Capabilities**:
- OAuth authentication to QB Online
- Data retrieval via QB API
- Field mapping (QB schema → PowerModels domain)
- Sandbox support for development/testing
- Transaction synchronization

### Xero Connector (Needs Development)
**Status**: Not present in current codebase (as of Feb 2026)
**Priority**: Critical for April 2026 cloud launch
**Requirements**:
- Parallel architecture to QBConnector
- OAuth to Xero API
- Field mapping (Xero schema → PowerModels domain)
- Support for Xero's multi-currency and international features
- Transaction import and sync

## Onboarding Flow (Firm Perspective)

### Traditional Manual Onboarding (Without QB/Xero)
1. Firm creates new client in PowerModels
2. **Manual data entry**: Chart of accounts, transactions, balances
3. **Reconciliation from scratch**: Match bank statements manually
4. **Historical recreation**: Build prior year data if needed
5. **Timeline**: Days to weeks per client
6. **Error-prone**: Manual entry increases mistakes

### Rapid Onboarding (With QB/Xero Integration)
1. Firm connects to QB/Xero
2. **Selects client** from existing QB/Xero list
3. **Authorizes data access**
4. **Import runs** (minutes to hours depending on transaction volume)
5. **AI prep applied** automatically
6. **Review flagged items** only
7. **Timeline**: Hours to onboard, ready for tax prep same day
8. **Accuracy**: Automated import reduces errors

## Value Proposition

### For Accounting Firms
- **Portfolio onboarding**: Transition entire client base in days, not months
- **Tax season readiness**: Clients ready for tax prep immediately after import
- **Resource efficiency**: No manual data entry staff hours
- **Competitive advantage**: Faster onboarding = more clients served
- **Risk reduction**: Fewer manual entry errors

### For Firm Clients
- **Zero disruption**: Data migrated automatically
- **Historical preservation**: All prior transactions accessible
- **Continuity**: Same data, better platform
- **No learning curve**: Accountant handles transition
- **Trust**: Existing QB/Xero data validates PowerModels accuracy

## Integration with Other Strategic Elements

### + Event-Sourced Architecture
- QB/Xero transactions become PowerModels events
- Complete audit trail from import forward
- Reproducible state enables rewind/refresh/replay
- Platform independence after import

### + Tax Preparation Wedge
- Historical data enables immediate tax prep
- Prior year comparison built-in
- Tax schedules generated from imported transactions
- Firms can onboard clients mid-tax season

### + Firm-First GTM
- Portfolio migration = leveraged CAC
- One firm connection → many client imports
- Firm controls which clients transition when
- Firm manages client authorizations

### + Association Marketing
- "Works with QuickBooks and Xero" = association credibility
- CPE presentations can demo live import
- Removes "switching cost" objection
- Peer validation: "We migrated 50 clients in a week"

## Revenue Model Alignment

### Pricing Per Client (Firm-Licensed)
- $50-150/entity/month
- **Onboarding via QB/Xero**: Firm can add clients instantly
- **Expansion revenue**: Easy to expand within firm's client portfolio
- **Low friction**: No setup fee, no manual onboarding cost

### Unit Economics Improvement
- **Reduce onboarding cost**: No manual setup labor
- **Faster time-to-revenue**: Client billable immediately after import
- **Higher conversion**: Firms commit when onboarding is easy
- **Retention**: Imported data creates lock-in (sunk cost)

## Competitive Positioning

### vs Manual Entry Solutions
- **PowerModels**: Automated import from QB/Xero
- **Competitors**: Manual chart of accounts and transaction entry
- **Advantage**: 10-100x faster client onboarding

### vs Platform-Locked Solutions
- **PowerModels**: Platform-independent after import (event-sourced)
- **QB/Xero native tools**: Locked to platform forever
- **Advantage**: Flexibility and control, no platform dependency

### vs "Rip and Replace" Migrations
- **PowerModels**: Import and enhance, incremental transition
- **Competitors**: Full replacement, high risk
- **Advantage**: Low-risk adoption, preserve existing workflow

## Implementation Priorities (April 2026 Launch)

### QuickBooks — Actual Status (Feb 2026 code review)
- [x] CSV Journal Report import (production-ready, all builds)
- [x] OAuth authentication (sandbox only)
- [x] API transaction import (sandbox only, DEBUG builds only)
- [x] Field mapping (QB schema → JournalReportRow)
- [x] Synchronize handler: CoA + Products + Vendors + Employees + Customers (sandbox only)
- [x] Report parsing: Journal, P&L, Balance Sheet
- [ ] Production OAuth app approval from Intuit
- [ ] Swap sandbox URLs to production
- [ ] Cloud-to-cloud integration (migrate from local)
- [ ] Multi-tenant architecture (firm → clients)

### Xero — Actual Status (Feb 2026 code review)
- [ ] Everything — zero code exists
- [ ] OAuth authentication
- [ ] API integration
- [ ] Transaction import
- [ ] Field mapping
- [ ] Multi-currency support
- [ ] Cloud-to-cloud integration
- [ ] Multi-tenant architecture

### Event Sourcing Integration
- [ ] QB/Xero transaction → PowerModels event transformation
- [ ] Import event stream creation
- [ ] Lineage preservation (source platform metadata)
- [ ] Reproducibility validation

## Risks and Mitigations

### Risk: Platform API changes
- **Mitigation**: Versioned API support, regression testing
- **Mitigation**: Active monitoring of QB/Xero API announcements

### Risk: Client authorization friction
- **Mitigation**: Firm-level blanket authorization where possible
- **Mitigation**: Clear client communication about benefits

### Risk: Data mapping errors
- **Mitigation**: Extensive field mapping tests
- **Mitigation**: Manual review workflow for ambiguous mappings
- **Mitigation**: AI-assisted categorization post-import

### Risk: Import performance (large clients)
- **Mitigation**: Incremental import (date range batching)
- **Mitigation**: Background processing with progress tracking
- **Mitigation**: Optimization for common transaction patterns

## Success Metrics

- **Import completion rate**: % of initiated imports that complete successfully
- **Import time**: Median time to import client (by transaction volume)
- **Error rate**: % of transactions requiring manual correction post-import
- **Onboarding velocity**: Clients onboarded per firm per week
- **Firm satisfaction**: NPS for onboarding experience
- **Client retention**: % of imported clients still active after 6 months

## Next Steps

1. **Complete Xero connector** (parallel to QBConnector)
2. **Cloud migration** (enable cloud-to-cloud integration)
3. **Multi-tenant architecture** (firm → clients mapping)
4. **Import optimization** (performance and accuracy)
5. **Association demo** (live import for CPE presentations)
6. **Documentation** (firm onboarding playbook)
