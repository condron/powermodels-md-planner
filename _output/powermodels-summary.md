# PowerModels — Company Summary
**Prepared:** February 2026  
**Stage:** Pre-revenue, angel-ready  
**Sector:** AI-powered accounting / fintech infrastructure

---

## One Paragraph

PowerModels is a financial preparation platform that uses AI (gpt-4o) to prepare accounting firm clients' books to 85% — with the last 15% preserved for professional judgment. The architecture is event-sourced (technical moat), pricing is three-tier ($20/$50-100/$200/mo) anchored to QuickBooks, and distribution is firm-first via accounting associations. The first beta user is a PSTAP chapter head who validated $20/mo pricing on his worst shoebox client. Two more firms are converting to trial. A development team with 5+ years together and deep ES/DDD/CQRS expertise built the entire codebase on sweat equity. The client dashboard creates a natural upsell path to forecasting with NRR > 100%.

---

## Founders

| Name | Role | Background |
|---|---|---|
| **Chris Condron** | Primary Founder, CTO/CPO | Former CTO & Head of Product at Event Store (Greg Young's EventStoreDB). ~25yr systems engineering. Wellington Management ($1T+ AUM), PerkinElmer (Solaris, 100M+ dps), Linedata, Hanover Insurance. ES/DDD/CQRS since 2012. |
| **Lisa Payne** | CEO | Industry veteran CFO and CEO. $100M+ raised across career. Founder of "Women Who Rock" (Vancouver women-focused founders group). |
| **James Geall** | VP Engineering | Cloud infrastructure, security, Plaid integration. Building cloud/security infra for GTM launch. |

**Development team** (separate from founders): 5+ years working together, ES/DDD/CQRS experts, ~2 years sweat equity, collectively own 12%.

---

## Product

| Element | Detail |
|---|---|
| **What it does** | Ingests messy financial documents (PDFs, bank statements, QB exports), classifies transactions via AI, surfaces uncertain items for professional review, produces a trial balance |
| **Core mechanism** | 85% AI classification (gpt-4o, confidence scoring) + 15% professional review (12+ WPF views) + trial balance report (completeness proof) |
| **Architecture** | Event-sourced (immutable, time-ordered financial record). Rewind/refresh/replay without rebuilds. Technical moat. |
| **Current interfaces** | WPF desktop app + Excel add-in (ExcelDna). Cloud migration in progress. |
| **Key differentiator** | 85/15 boundary — AI empowers accountants, doesn't replace them. Architecturally enforced, not marketing. |

---

## Pricing

| Tier | Client Type | Price/mo | QB Equivalent |
|---|---|---|---|
| **Micro/1099** | 1099 contractors, minimal books | $20 | QB Simple Start |
| **Solo/Partnership** | Solo proprietors, partnerships | $50-100 | QB Essentials/Plus |
| **SMB** | Small businesses, full books | $200 | QB Advanced |

- **Pass-through model**: Firm adds to client's monthly bill, same as QB
- **Current focus**: Micro/1099 tier ($20/mo validated by beta user)

---

## Go-To-Market

| Element | Detail |
|---|---|
| **Model** | Firm-first B2B2B: Association → Firms → Clients |
| **First channel** | PSTAP (PA Society of Tax & Accounting Professionals) |
| **Champion** | George Bancroft — Buxmont Chapter head AND active beta user |
| **CAC structure** | Single firm acquisition → revenue expands per client (zero new CAC) |
| **Entry wedge** | Tax preparation (mandatory, deadline-driven, correctness-forcing) |

---

## Traction

| Metric | Status |
|---|---|
| **Beta user** | George Bancroft (PSTAP chapter head, several hundred clients, worst client onboarded) |
| **Pricing validation** | $20/mo pass-through confirmed viable |
| **Pipeline** | 3 firms: Bancroft (active), Stewart (2 trial clients), Liberty Tax (onboarding conversation) |
| **Codebase** | 174 stories closed across 5 milestones. AI pipeline, 85/15 boundary, event-sourced data store — all production-quality. |
| **Plaid** | Authorized reseller, 5 products enabled (Transactions, Enrich, Recurring, Refresh, Liabilities) |

---

## Unit Economics

**Per-firm (micro-only, current):**
```
200 clients × $20/mo = $4,000 MRR / $48K ARR per firm
```

**Per-firm (blended portfolio, future):**
```
150 micro × $20 + 30 solo × $75 + 10 SMB × $200 = $7,250 MRR / $87K ARR per firm
```

**NRR > 100%** via three expansion vectors:
1. More clients per firm (zero new CAC)
2. Client tier upgrades (micro → solo → SMB)
3. Forecasting upsell via client dashboard

---

## Product Roadmap

```
Phase 1 (NOW):     Micro/1099 @ $20/mo
                   Shoebox UX, Plaid integration, cloud launch, design partners

Phase 2 (Q3 2026): Client dashboard + Solo/Partnership @ $50-100/mo
                   Monthly book visibility, association expansion

Phase 3 (Q1 2027): Forecasting via dashboard
                   Drift metrics, Future Ledger, add-on pricing

Phase 4 (Q3 2027): SMB @ $200/mo + platform expansion
                   Channel expansion, platform APIs
```

---

## What Works Today (Code-Validated)

- ✅ AI classification (gpt-4o structured output, confidence scoring)
- ✅ 85/15 boundary UI (confidence threshold, 12+ WPF review views)
- ✅ PDF bank statement ingestion (Tabula + PdfDocument)
- ✅ QB CSV Journal Report import (production-ready)
- ✅ Event-sourced data store (DataStore.cs, ReactiveDomain)
- ✅ Multi-step reconciliation pipeline
- ✅ Trial balance report (completeness proof)

## What's In Progress

- 🟡 Plaid bank account linking (authorized reseller, cloud infra building)
- 🟡 Cloud infrastructure (James Geall)
- 🟡 Standalone desktop app (Epic #2017)

## What's Phase 2+

- ⬜ Client dashboard website
- ⬜ Drift metric / Future Ledger
- ⬜ Forecasting product
- ⬜ Solo/partnership and SMB tiers

---

## Funding

| Round | Amount | Requirements |
|---|---|---|
| **Angel** | $500K | 2-4 design partner firms, 10-30 entities processed, measured time-to-first-cycle |
| **Seed** | $3M | Repeatable onboarding, firm expansion proven, CAC payback <12mo |

**Remaining before raising:**
- Complete Stewart + Liberty Tax onboarding
- Produce competitive one-pager
- Assemble Proof Room (demo recording, runbook, case study)

---

## Competitive Position

| Category | Examples | PowerModels Advantage |
|---|---|---|
| Bookkeeping automation | Botkeeper, Bench | 85/15 preserves judgment; event-sourced moat |
| Practice management | Canopy, Karbon | Preparation platform, not workflow tool |
| Platform-native | QB Advanced, Xero HQ | Architecture enables rewind/refresh/replay; not locked to one platform |
| AI startups | Various | Trust boundary (85/15) prevents backlash; firm-first distribution |

---

## Key Risks

| Risk | Severity | Mitigation |
|---|---|---|
| Cloud migration timeline | Medium | Event-sourced arch designed for cloud; VP Eng on it |
| Design partner pipeline stalls | Medium | 3 firms active/converting |
| Practitioner adoption resistance | Medium | 85/15 preserves judgment; monthly cadence creates habit |
| Single-association dependency | Low | Bancroft proves model; replicable playbook |

---

## Links

- [Chris Condron](https://www.linkedin.com/in/eventsourced/) · [Lisa Payne](https://www.linkedin.com/in/lisa-j-payne/) · [James Geall](https://www.linkedin.com/in/jamesgeall/)
- GitHub: `event-systems/PowerModels`
- Beta user: [mycpageorge.com](https://www.mycpageorge.com/)
- Association: [PSTAP](https://www.pstap.org/) · [Buxmont Chapter](https://www.pstap.org/buxmont-chapter)
