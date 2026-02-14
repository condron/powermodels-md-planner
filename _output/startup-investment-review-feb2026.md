# PowerModels — Startup & Investment Review
**Date:** 2026-02-14  
**Method:** Vault geometry (170 intersections, 34 dim-3+) + codebase validation + design partner evidence  
**Purpose:** Comprehensive assessment of PowerModels as a startup and investment opportunity

---

## Executive Summary

PowerModels is a **financial preparation platform** that enters accounting firms through tax preparation, prepares books to 85% using AI (gpt-4o), and preserves the last 15% for professional judgment. It targets small accounting firms via an association-first distribution channel, with three-tier pricing anchored to QuickBooks cost.

**Current status (Feb 2026):**
- Working product: AI pipeline, 85/15 boundary, PDF ingestion, QB CSV import — all validated in code
- First beta user: George Bancroft (PSTAP chapter head, several hundred clients, $20/mo pricing validated)
- Design partner pipeline: 3 firms (1 active, 2 converting to trial)
- Plaid authorized reseller with 5 products enabled (cloud infra in progress)
- Association channel activated (beta user IS the chapter head)
- Monthly update cadence + client dashboard planned as retention driver and Phase 2 upsell surface

**Remaining blockers:** Competitive one-pager (not documented), Proof Room artifacts (not assembled)

---

## 1. What Makes This Investable

### 1.1 The 85/15 Boundary — Strategic Moat

Architecturally enforced in code:
- `AIStep.cs` classifies via gpt-4o with structured JSON, confidence scoring (0.0–1.0), threshold at 0.9
- Items below threshold routed to `UncategorizedTransactionsVm` for human review
- 12+ WPF views built for the review workflow (rebuilt in RC4)

**Why investors care:** Prevents "replace the accountant" backlash, avoids liability, creates clean scope boundary, preserves professional accountability. This is a moat, not just positioning.

### 1.2 Event-Sourced Architecture — Technical Moat

Genuine event sourcing via `DataStore.cs` (ReactiveDomain):
- Every financial event is immutable and time-ordered
- Full audit trail by design, not bolted on
- 174 stories closed across 5 milestones — mature, actively maintained codebase

**Why investors care:** Hard to build, harder to replicate. Competitors on CRUD databases cannot retrofit this.

### 1.3 Tax Wedge — Forcing Function

Tax preparation is mandatory, correctness-forcing, and deadline-driven:
- Adoption discipline: firms won't gamble during tax season
- Clear success criteria: correct, complete, defensible returns
- Sticky once validated: trust extends to year-round use

### 1.4 Firm-First Economics — Leveraged Growth

| Metric | Traditional SMB SaaS | PowerModels Firm-First |
|---|---|---|
| CAC unit | Per customer | Per firm (amortized) |
| Revenue expansion | Upsell features | Add clients (zero new CAC) |
| Decision maker | Business owner | Firm partner (controls portfolio) |
| Retention driver | Feature stickiness | Workflow integration + monthly cadence |

### 1.5 Founding Team

| Name | Role | Background |
|---|---|---|
| **Chris Condron** | Primary Founder, CTO/CPO | Former CTO & Head of Product at Event Store (Greg Young's EventStoreDB). ~25 years systems engineering. Chief Architect at Linedata, Principal Architect at PerkinElmer (award-winning Solaris, 100M+ data points/sec), Lead Architect at Wellington Management ($1T+ AUM). ES/DDD/CQRS since 2012. |
| **Lisa Payne** | CEO | Industry veteran CFO and CEO. $100M+ raised. Founder of "Women Who Rock" (Vancouver women-focused founders group). |
| **James Geall** | VP Engineering | Cloud infrastructure, security, Plaid integration. Currently building cloud/security infra for GTM launch. |

- [Chris Condron LinkedIn](https://www.linkedin.com/in/eventsourced/) · [Lisa Payne LinkedIn](https://www.linkedin.com/in/lisa-j-payne/) · [James Geall LinkedIn](https://www.linkedin.com/in/jamesgeall/)
- CTO built Event Store's SaaS platform — directly relevant to PowerModels' event-sourced architecture
- CEO has raised $100M+ — knows how to fundraise and run a company
- VP Eng building the cloud/Plaid infra that enables GTM

**Development Team (separate from founders):**
- **5+ years working together** on average — cohesive, not assembled for this venture
- **ES/DDD/CQRS experts** — the architecture is their domain expertise
- **~2 years sweat equity** (minimal/deferred cash), collectively own 12%
- Built the entire codebase: 174 stories closed, AI pipeline, event-sourced data store, 12+ WPF views

---

## 2. Product & Pricing

### Three-Tier Pricing (Aligned to QB)

| Tier | Client Type | Price/mo | QB Equivalent | Status |
|---|---|---|---|---|
| **Micro/1099** | 1099 contractors, minimal books | $20/mo | QB Simple Start | ✅ Current focus, Bancroft validated |
| **Solo/Partnership** | Solo proprietors, partnerships | $50-100/mo | QB Essentials/Plus | 🟡 Next phase |
| **SMB** | Small businesses, full books | $200/mo | QB Advanced | 🟡 Future |

**Pass-through model:** Firm adds to client's monthly bill, same as QB. Zero friction.

### Product Expansion Sequence

```
Phase 1 (NOW):    Micro/1099 → monthly books + client dashboard
Phase 2:          Solo/Partnership → richer books + enhanced dashboard
Phase 3:          SMB → full books + forecasting via dashboard
Phase 4:          Forecasting product sold through client dashboard
                  (cash forecasting, revenue forecasting, drift metrics)
```

### Client Dashboard & Monthly Cadence

- **Client dashboard website:** End-clients see critical info from books each month
- **Monthly update cadence:** Prevents work explosions at critical times — the antidote to Stewart's 100-hour problem
- **Strategic value:** Dashboard becomes **sell-through route for forecasting product** — clients who see monthly books naturally want projections

### Net Revenue Retention (NRR) — Three Expansion Vectors

| Vector | Mechanism |
|---|---|
| **More clients per firm** | Firm adds clients, zero new CAC |
| **Client tier upgrades** | Micro ($20) → Solo ($50-100) → SMB ($200) |
| **Forecasting upsell via dashboard** | Monthly books → client wants projections → paid upgrade |

**Illustrative NRR (single firm, Year 1 → Year 2):**
```
Year 1: 150 micro × $20                = $3,000/mo
Year 2: 170 micro × $20                = $3,400/mo  (more clients)
      +  15 upgraded to solo × $75      = $1,125/mo  (tier upgrades)
      +  50 forecasting add-ons × $10   = $500/mo    (dashboard upsell)
                                         ─────────
Year 2 MRR:                              $5,025/mo
NRR: 168%
```

---

## 3. Design Partner Pipeline

| Firm | Status | Evidence |
|---|---|---|
| **George Bancroft** (Bancroft Financial CPAs) | ✅ Active beta | PSTAP chapter head, worst client onboarded, $20/mo validated, several hundred clients |
| **Stewart** | 🟡 2 trial clients identified | 100-hour unbillable setup = anchor data point |
| **Liberty Tax** | 🟡 Onboarding conversation next week | Seasonal tax prep, $2K-$2.5K bookkeeping packages |

**3 of 2-4 target firms for angel round.**

### Why Bancroft Matters

1. **Design partner evidence exists** — real client data processed, not hypothetical
2. **Association channel has a champion** — chapter head who can present to peers
3. **Pricing validated** — $20/mo pass-through confirmed viable
4. **Shoebox pipeline works** — on real messy data
5. **B2B2B embodied:** PSTAP → Bancroft (chapter head) → member firms

---

## 4. Product Reality — Codebase Validation

### What Works (Demo-Ready)

| Capability | Evidence |
|---|---|
| AI classification (gpt-4o) | AIStep.cs — structured JSON, confidence scoring |
| 85/15 boundary UI | confidence < 0.9 threshold, 12+ WPF review views |
| PDF bank statement ingestion | ExtractTransactionsFromPDFs, PdfDocument + Tabula |
| QB CSV Journal Report import | GetJournalReportFromCsv() — production-ready, all builds |
| Event-sourced data store | DataStore.cs, ReactiveDomain, 174 stories closed |
| Multi-step reconciliation | PipelineBuilder, rules → AI → validation |

### What's In Progress

| Capability | Owner |
|---|---|
| Plaid bank account linking (authorized reseller, 5 products) | thefringeninja |
| Cloud infrastructure + security | thefringeninja |
| Standalone desktop app | Epic #2017 |

### What Doesn't Exist

| Capability | Impact |
|---|---|
| ~~Completeness contract~~ | ✅ Resolved — three-part mechanism: AI classification (85%) + reconciliation/adjustment UI (15%) + trial balance report (proof) |
| Drift metric / Future Ledger | Phase 2+ — will deliver via client dashboard |
| Xero connector | Zero code, not planned |
| QB live API (production) | Sandbox-only, previous direction |
| Client dashboard website | Phase 2 — monthly book visibility + forecasting surface |

### Plaid — Authorized Reseller

5 products at 0 usage: Transactions (24mo), Enrich, Recurring Transactions, Transactions Refresh, Liabilities. When cloud infra ready: link bank → 24mo enriched transactions → AI pipeline. Bypasses QB entirely.

---

## 5. Go-To-Market — Association Channel

| Element | Detail |
|---|---|
| **First target** | PSTAP (Pennsylvania Society of Tax & Accounting Professionals) |
| **Champion** | George Bancroft — Buxmont Chapter head AND active beta user |
| **Model** | B2B2B: PSTAP → chapter heads → member firms |
| **CAC structure** | Single association partnership → hundreds of member firms aware |

**The channel is activated, not theoretical.** The chapter head who leads it is already using the product.

**Expansion path:**
```
Bancroft uses PowerModels
  → Presents at Buxmont Chapter CPE event
    → 5-10 member firms try it
      → Successful firms tell other chapters
        → PSTAP state-wide → other state associations
```

---

## 6. Investor Q-Hole Resolution

### Angel Round

| Q-Hole | Previous | Current | Evidence |
|---|---|---|---|
| Product-market fit | ⚠️ None | 🟡 Partial | 3 firms in pipeline, 1 active beta |
| Pricing unvalidated | ⚠️ None | ✅ First validation | $20/mo confirmed by Bancroft |
| Team completeness | ✅ Strong | ✅ Strong | 2 years sweat equity, 12% ownership |
| Onboarding scaling | ⚠️ No mechanism | 🟡 Exists | QB CSV works, Plaid in progress |
| Competitive positioning | ⚠️ Not explicit | ⚠️ Still needs work | Conceptual but no deliverable |
| ~~Completeness contract~~ | ✅ Resolved | Three-part mechanism exists: AI classification + reconciliation UI + trial balance report |

### What's Left to Close

| Must-Have | Status | Action |
|---|---|---|
| ✅ ~~Completeness contract~~ — resolved (three-part mechanism in product) |
| ⚠️ Competitive one-pager — must produce |
| ⚠️ Proof Room artifacts (demo recording, runbook, test dataset) must be assembled |
| ⚠️ Cohort chart (even simulated) must be created |

---

## 7. Unit Economics

### Per-Firm (Micro-Only, Current Focus)
```
200 micro/1099 clients × $20/mo = $4,000/mo MRR
                                  $48,000/yr ARR per firm
```

### Per-Firm (Blended Portfolio, Future)
```
150 micro/1099   × $20/mo  = $3,000/mo
 30 solo/partner × $75/mo  = $2,250/mo
 10 SMB          × $200/mo = $2,000/mo
─────────────────────────────────────
190 clients total            $7,250/mo MRR
                             $87,000/yr ARR per firm
```

### Portfolio Projections
```
Angel (3-4 firms):   3 × 150 micro × $20 = $9,000 MRR → $108K ARR
Seed (10-15 firms):  10 × 150 micro × $20 = $30,000 MRR → $360K ARR
With tier upgrades + forecasting: multiply by NRR (168% illustrative)
```

---

## 8. Risk Assessment

| Risk | Severity | Mitigation |
|---|---|---|
| Cloud migration timeline | Medium | Event-sourced arch designed for cloud; thefringeninja on it |
| Design partner pipeline stalls | Medium | 3 firms active/converting |
| Practitioner adoption resistance | Medium | 85/15 preserves judgment; monthly cadence creates habit |
| Single-association dependency | Low | Bancroft proves model; replicable playbook |
| Pricing compression | Low | $20/mo is floor (micro); solo/SMB tiers are 2.5-10x |

---

## 9. Fundability Assessment

### Jan 2026: "High potential, contingent on proof"
### Feb 2026: "Early proof exists, must complete"

**What improved:**
- ✅ Codebase validated — AI pipeline, 85/15, event sourcing confirmed working
- ✅ First beta user is PSTAP chapter head (association channel proof)
- ✅ Pricing validated at $20/mo pass-through
- ✅ Plaid authorized reseller (new onboarding path)
- ✅ 174 closed stories confirm active engineering team
- ✅ Three-tier pricing with NRR > 100% model

**What remains:**
- ✅ ~~Completeness contract~~ — resolved (three-part mechanism in product)
- ⚠️ Competitive one-pager — must produce
- ⚠️ Proof Room — must assemble
- ⚠️ Cohort chart — must create

---

## 10. Recommended Actions (Priority Order)

### Before Raising
1. **Complete Stewart + Liberty Tax onboarding** — convert pipeline to active trials
3. **Produce competitive one-pager** — position vs 4 competitor categories
4. **Assemble Proof Room** — demo recording, runbook, test dataset, case study

### While Raising
5. **Lead with Bancroft story** — chapter head, worst client, $20/mo validated
6. **Position Plaid as upside** — authorized reseller, direct bank linking when ready
7. **Show NRR model** — three expansion vectors, dashboard as forecasting surface

### After Raising
8. **Complete Plaid integration** — highest-value next build
9. **Build client dashboard** — monthly visibility → forecasting trojan horse
10. **Expand to solo/partnership tier** — grow beyond micro focus
11. **Expand to 2nd PSTAP chapter** — prove association playbook repeats

---

## The Investment in One Paragraph

PowerModels is a financial preparation platform that uses AI to prepare accounting firm clients' books to 85% — with the last 15% preserved for professional judgment. The architecture is event-sourced (technical moat), the AI pipeline runs gpt-4o with confidence scoring, and the first beta user is a PSTAP chapter head who validated $20/mo/client pricing on his worst shoebox client. Two more firms are converting to trial next week. Three-tier pricing ($20/$50-100/$200) anchored to QuickBooks creates a pass-through model with NRR > 100% through three expansion vectors: more clients, tier upgrades, and a forecasting product sold through the client dashboard. The product works today; cloud migration is in progress for GTM. $500K angel round buys 2-4 design partners, measured proof artifacts, and the foundation for a $3M seed on repeatable onboarding and firm expansion.
