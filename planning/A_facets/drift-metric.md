---
type: facet
id: drift-metric
label: Drift Metric and Measurable Deviation
synonyms:
  - measurable deviation
  - drift
  - future vs actual delta
  - client health KPI
  - proactive client management
  - early warning system
links_to_facets:
  - future-ledger
  - event-sourced-architecture
  - proven-completeness
  - unit-economics
claims:
  - Drift measures the mathematical delta between the Future Ledger and Actual Event Stream [ASPIRATIONAL — zero code zero issues zero references in codebase, Feb 2026 code review]
  - Drift tells firms exactly which clients need attention before a crisis occurs [ASPIRATIONAL — no implementation]
  - Drift is a new industry metric for business health and creditworthiness [ASPIRATIONAL — no implementation]
  - High drift signals divergence between committed plans and actual activity requiring professional review [ASPIRATIONAL — no implementation]
  - Low drift confirms business is tracking to committed financial position [ASPIRATIONAL — no implementation]
evidence_refs:
  - strategy-overview-feb2026
  - codebase-review-feb2026
owner: product-team
status: active
---

# Drift Metric and Measurable Deviation

## Description

Drift is a new financial metric introduced by PowerModels that measures the **mathematical delta between the Future Ledger (committed events) and the Actual Event Stream (what really happened)**. It provides a single, quantifiable measure of how much a business's reality is diverging from its commitments.

## How Drift Works

```
Drift = Σ |Future Ledger position - Actual Event Stream position|
        across all accounts, time periods, and entity dimensions
```

### Low Drift
- Business is tracking to plan
- Committed events are materializing as expected
- Minimal professional attention needed
- Firm can manage this client efficiently

### High Drift
- Reality diverging from commitments
- Something changed: missed payments, unexpected transactions, contract modifications
- **Flags for professional review** (aligns with 85/15 boundary)
- Firm knows exactly where to focus attention

## Strategic Value

### For Accounting Firms
- **Triage tool**: Sort client portfolio by drift score — highest drift gets attention first
- **Proactive advisory**: "Your drift increased 40% this month — here's why, here's what to do"
- **Capacity optimization**: Low-drift clients need minimal touch, freeing staff for high-drift cases
- **Revenue model**: Advisory services triggered by drift alerts = recurring high-margin revenue

### For Business Owners
- **Health check**: "Your business is tracking 92% to plan" vs. opaque financial statements
- **Early warning**: Know before your accountant tells you — drift alerts in real time
- **Trust builder**: Transparency about what's on track and what's not

### For Investors
- **Novel IP**: First platform to define and measure financial drift
- **Industry standard potential**: Drift could become the credit score for business health
- **Phase 2 differentiator**: Establishes PowerModels as infrastructure, not just a tool

## Phased Rollout

- **Phase 1**: Drift calculated internally for preparation quality (does refreshed output match expectations?)
- **Phase 2**: Drift exposed as client-facing KPI via the **client dashboard website** — the same dashboard that shows monthly books becomes the forecasting surface
- **Phase 3**: Full business cash and revenue forecasting with drift metrics, sold through the client dashboard as an upsell product
- **Phase 4**: Drift as industry standard for creditworthiness, embedded in lending and insurance APIs

### Delivery via Client Dashboard
The client dashboard (monthly book visibility for end-clients) is the **trojan horse** for the forecasting product. Clients already see their books monthly → they naturally want to see where they're headed → drift + forecasting becomes a paid upgrade delivered through an interface they already use. This is the primary sell-through route for Phase 2+ revenue.
