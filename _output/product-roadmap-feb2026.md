# PowerModels — Proposed Product Roadmap
**Date:** 2026-02-14  
**Method:** Vault geometry + codebase reality + design partner evidence + pricing structure  
**Principle:** Ship what's validated. Build what's blocking. Plan what's strategic.

---

## Product Vision

PowerModels prepares accounting firm clients' books using AI, preserves professional judgment, and grows from preparation into forecasting. The client dashboard is the bridge — it starts as monthly book visibility and becomes the forecasting surface.

```
PREPARATION (Phase 1)          →  FORECASTING (Phase 2+)
─────────────────────────────────────────────────────────
Shoebox ingestion                 Cash flow projections
AI categorization (85%)           Revenue forecasting
Professional review (15%)         Drift metrics
Trial balance (completeness)       Shadow transactions
Monthly book updates              Advisory triggers
Client dashboard (visibility)     Client dashboard (projections)
```

---

## Current State (Feb 2026)

### What's Built and Working

| Capability | Evidence | Demo Ready |
|---|---|---|
| AI classification (gpt-4o) | AIStep.cs — structured JSON, confidence scoring | ✅ |
| 85/15 boundary UI | Confidence < 0.9 threshold, 12+ WPF review views | ✅ |
| PDF bank statement ingestion | ExtractTransactionsFromPDFs, Tabula | ✅ |
| QB CSV Journal Report import | GetJournalReportFromCsv() — all builds | ✅ |
| Event-sourced data store | DataStore.cs, ReactiveDomain | ✅ |
| Multi-step reconciliation pipeline | PipelineBuilder — rules → AI → validation | ✅ |
| Excel add-in interface | ExcelDna, full integration | ✅ |

### What's In Progress

| Capability | Owner | Status |
|---|---|---|
| Cloud infrastructure + security | thefringeninja | Active development |
| Plaid bank account linking | thefringeninja | Authorized reseller, 5 products enabled |
| Standalone WPF app | Team | Epic #2017 open |
| Depreciation schedules | Team | 6 of 19 RC5 stories |
| Multi-client portfolio view | Team | Story #2023 in RC5 |

### What Doesn't Exist Yet

| Capability | Priority | Reason |
|---|---|---|
| ~~Completeness contract~~ | ✅ Resolved | Three-part mechanism: AI classification + reconciliation UI + trial balance report |
| Client dashboard website | 🟡 High | Monthly visibility + forecasting surface |
| Drift metric | ⬜ Phase 2+ | No code, delivers via dashboard |
| Future Ledger / shadow transactions | ⬜ Phase 2+ | No code, first step = depreciation |
| Xero connector | ⬜ Not planned | Zero code, Plaid reduces need |

---

## Phase 1: Micro/1099 — Current Focus (Now → Q3 2026)

### Objective
Ship a working product for micro/1099 clients at $20/mo, validated by design partners, ready for angel fundraise.

### Milestone 1.1: Demo-Ready (Now → March 2026)
**Goal:** Complete demo that works end-to-end for Stewart's 2 trial clients and Liberty Tax onboarding.

| Deliverable | Priority | Status | Notes |
|---|---|---|---|
| ~~Completeness contract~~ | ✅ Resolved | Exists | Three-part mechanism already in product: AI classification (85%) + reconciliation/adjustment UI (15%) + trial balance report (proof, closed RC4 #1998). No separate artifact needed. |
| Multi-file upload UX | P1 | Needs wrapping | Current: single file. Need: drag-drop folder of PDFs. |
| Shoebox UX automation | P1 | In progress | Fully automate what was semi-automated for Bancroft |
| Monthly update workflow | P1 | Manual today | Automate re-run for existing clients |
| RC5 completion | P1 | 19 open stories | Depreciation, multi-client view, misc |

### Milestone 1.2: Plaid Integration (March → May 2026)
**Goal:** Link bank account → 24mo enriched transactions → AI pipeline → trial balance.

| Deliverable | Priority | Status | Notes |
|---|---|---|---|
| Cloud infrastructure | P0 | In progress (thefringeninja) | Secure credentials, PII handling |
| Plaid Link integration | P0 | Authorized, 0 usage | Connect Plaid Link UI to transaction pipeline |
| Transaction ingestion adapter | P1 | Not started | Map Plaid transaction format to event store |
| Enrich integration | P2 | Not started | Use Plaid Enrich to pre-categorize before AI pipeline |
| Recurring transaction detection | P2 | Not started | Plaid Recurring Transactions product |

**Outcome:** Shoebox clients (no QB, just a bank account) can onboard in minutes. Biggest UX win possible.

### Milestone 1.3: Cloud GTM Launch (April → June 2026)
**Goal:** Cloud-hosted product accessible to design partners without local install.

| Deliverable | Priority | Status | Notes |
|---|---|---|---|
| Cloud event store | P0 | In progress | Migrate from local file-backed to cloud |
| Multi-tenant firm isolation | P0 | TeamModelManagement exists | Needs cloud adaptation |
| Cloud deployment pipeline | P1 | Not started | CI/CD for cloud app |
| Standalone WPF → web transition plan | P2 | Epic #2017 is desktop | Web client is future |

### Milestone 1.4: Design Partner Proof (Ongoing → Q3 2026)
**Goal:** 2-4 firms processing 10-30 entities with measured outcomes.

| Firm | Current Status | Next Step |
|---|---|---|
| **Bancroft** | ✅ Active beta | Measure: time-to-first-cycle, entities processed, monthly cadence |
| **Stewart** | 🟡 2 trial clients | Onboard clients, measure 100-hour → X improvement |
| **Liberty Tax** | 🟡 Conversation next week | Define pilot scope, seasonal workflow test |
| **Firm 4** | Not identified | Source from PSTAP Buxmont Chapter via Bancroft |

**Proof artifacts to capture:**
- Time-to-first-cycle per entity
- Hours saved per client onboarding
- Monthly update time per client
- CPA satisfaction / NPS
- Pricing acceptance (per tier)

---

## Phase 2: Client Dashboard + Solo/Partnership Tier (Q3 2026 → Q1 2027)

### Objective
Launch client dashboard for monthly book visibility. Expand product to handle solo/partnership tier ($50-100/mo). Begin building forecasting foundation.

### Milestone 2.1: Client Dashboard v1
**Goal:** End-clients see critical financial info from books their accountant manages.

| Deliverable | Priority | Notes |
|---|---|---|
| Client-facing web dashboard | P0 | Read-only view of monthly book status |
| Firm-controlled access | P0 | Firm grants dashboard access per client |
| Monthly summary view | P1 | Key metrics: income, expenses, P&L snapshot |
| Notification system | P2 | "Your books are updated for [month]" |

**Strategic value:** This is the surface for Phase 3 forecasting upsell. Every client who sees monthly books will eventually want projections.

### Milestone 2.2: Solo/Partnership Tier
**Goal:** Handle more complex books — partnerships, multi-entity, accrual basis.

| Deliverable | Priority | Notes |
|---|---|---|
| Partnership entity support | P0 | Multi-partner allocation, K-1 preparation foundation |
| Accrual basis support | P1 | Beyond cash basis for solo/partnership |
| Enhanced CoA templates | P1 | Industry-specific chart of accounts |
| Tier pricing implementation | P1 | $50-100/mo billing, firm-managed |

### Milestone 2.3: Association Expansion
**Goal:** Expand from Bancroft's chapter to 2-3 additional PSTAP chapters.

| Deliverable | Priority | Notes |
|---|---|---|
| CPE presentation materials | P0 | "AI-Powered Tax Preparation" CPE-eligible session |
| Proof Room deck | P0 | Demo recording + case study + metrics from Phase 1 |
| Chapter head recruitment playbook | P1 | Replicate the Bancroft model |
| PSTAP state-level partnership | P2 | Elevate from chapter to state |

---

## Phase 3: Forecasting Product via Dashboard (Q1 2027 → Q3 2027)

### Objective
Deliver full business cash and revenue forecasting with drift metrics through the client dashboard. This is the primary Phase 2+ revenue stream.

### Milestone 3.1: Drift Metric v1
**Goal:** Calculate and display drift between projected and actual financial position.

| Deliverable | Priority | Notes |
|---|---|---|
| Drift calculation engine | P0 | Compare committed events (depreciation, recurring) to actuals |
| Dashboard drift display | P0 | Client sees "tracking 92% to plan" |
| Firm-level drift portfolio | P1 | Sort clients by drift — highest gets attention first |
| Advisory alert triggers | P2 | "Client X drift increased 40% this month" |

### Milestone 3.2: Future Ledger v1
**Goal:** Shadow transactions for committed future events.

| Deliverable | Priority | Notes |
|---|---|---|
| Depreciation schedule shadows | P0 | Already in RC5 backlog — extend to shadow transactions |
| Recurring revenue/expense shadows | P1 | From Plaid Recurring Transactions data |
| Lease/contract shadow transactions | P2 | Manual entry of known commitments |
| Deterministic projection engine | P2 | "What WILL happen" based on committed events |

### Milestone 3.3: Forecasting Product Launch
**Goal:** Paid forecasting add-on sold through client dashboard.

| Deliverable | Priority | Notes |
|---|---|---|
| Cash flow projection view | P0 | 3/6/12 month forward view based on shadow transactions |
| Revenue forecasting | P1 | Based on committed contracts + recurring patterns |
| Scenario modeling | P2 | "What if" — adjust assumptions, see impact |
| Forecasting tier pricing | P1 | Add-on price per client (illustrative: $10/mo) |

---

## Phase 4: SMB Tier + Platform Expansion (Q3 2027+)

### Objective
Full SMB books at $200/mo. Platform becomes financial infrastructure, not just preparation.

### Milestone 4.1: SMB Tier
- Full accrual accounting, multi-department, inventory, payroll integration
- Advanced reporting package
- $200/mo pricing tier

### Milestone 4.2: Channel Expansion
- Additional state associations beyond PSTAP
- Fractional CFO networks
- CPA aggregators
- Direct SMB channel (only after firm-first model proven)

### Milestone 4.3: Platform APIs
- Drift as industry metric (lending, insurance, credit)
- Future Ledger API for embedded finance
- M&A simulation via shadow transactions

---

## Roadmap Summary

```
2026 Q1-Q2 (NOW)     Phase 1: Micro/1099 @ $20/mo
                      ├── Shoebox UX automation + multi-file upload
                      ├── Plaid integration
                      ├── Cloud GTM launch
                      └── Design partner proof (3-4 firms)

2026 Q3 - 2027 Q1    Phase 2: Dashboard + Solo/Partnership @ $50-100/mo
                      ├── Client dashboard v1 (monthly visibility)
                      ├── Solo/partnership tier
                      └── Association expansion (2-3 chapters)

2027 Q1-Q3            Phase 3: Forecasting via Dashboard
                      ├── Drift metric v1
                      ├── Future Ledger v1 (shadow transactions)
                      └── Forecasting product launch (add-on pricing)

2027 Q3+              Phase 4: SMB + Platform
                      ├── SMB tier @ $200/mo
                      ├── Channel expansion
                      └── Platform APIs (drift, Future Ledger)
```

---

## Revenue Model by Phase

| Phase | Firms | Avg Clients | Price | MRR | ARR |
|---|---|---|---|---|---|
| **1 (angel)** | 3-4 | 150 micro | $20/mo | $9-12K | $108-144K |
| **1 (seed)** | 10-15 | 150 micro | $20/mo | $30-45K | $360-540K |
| **2 (expansion)** | 15-25 | 150 blended | $38 avg | $86-143K | $1.0-1.7M |
| **3 (forecasting)** | 25-40 | 175 blended | $48 avg (+forecast) | $210-336K | $2.5-4.0M |

**NRR > 100% across all phases** — three expansion vectors compound: more clients, tier upgrades, forecasting upsell.

---

## Critical Path

```
NOW:     Complete Stewart + Liberty Tax onboarding ──→ 3 active partners
         Assemble Proof Room ──→ Angel-ready
         Produce competitive one-pager ──→ Investor-ready

Q2 2026: Plaid integration ──→ Shoebox game-changer
         Cloud launch ──→ Scalable GTM

Q3 2026: Client dashboard ──→ Foundation for everything Phase 2+
         Solo/partnership tier ──→ Revenue expansion

Q1 2027: Forecasting product ──→ NRR >100% confirmed
         Association expansion ──→ Repeatable channel
```

**No P0 blockers remain.** Completeness is solved by the three-part mechanism (AI classification + reconciliation UI + trial balance report). The most important near-term deliverables are: Stewart/Liberty Tax onboarding, Proof Room assembly, and the competitive one-pager.
