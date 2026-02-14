# PowerModels Vault — Project Review
**Date:** 2026-02-14  
**Scope:** Full pipeline assessment, geometry evaluation, constraint identification, and next actions

---

## 1. Pipeline Status (9-Step Methodology)

| Step | Status | Notes |
|------|--------|-------|
| 1. SCAN | ✅ Complete | 7 source documents ingested |
| 2. EXTRACT | ✅ Complete | 14 facets, 10 audiences, 3 evidence cards extracted |
| 3. CURATE | ⚠ Near-complete | 3 cards in inbox (reference docs, not blocking) |
| 4. COMPUTE | ✅ Complete | 140 intersections computed, 0 dim-0 gaps |
| 5. ENRICH | ⬜ Not started | No org context (`--org`) applied yet |
| 6. EVALUATE | ✅ This review | Geometry analyzed below |
| 7. DESIGN | 🔶 Partial | Routes identified below, not yet formalized |
| 8. GENERATE | ⬜ Not started | D_deliverables/ is empty |
| 9. REVIEW | 🔶 This document | First full review cycle |

**Verdict:** Steps 1–4 are solid. The vault is structurally complete and validated (0 errors, 0 warnings). Ready for deliverable generation.

---

## 2. Vault Health

```
Source Notes:      7
Facets:           14
Audiences:        10
Evidence:          3
Intersections:   140 (14 × 10, fully covered)
Deliverables:      0
Inbox:             3 (reference docs, low priority)
Validation:        0 errors, 0 warnings
```

---

## 3. Geometry Summary

### Dimension Distribution
| Dimension | Count | % | Interpretation |
|-----------|-------|---|----------------|
| 3+ (strong) | 15 | 11% | Well-connected, ready for deliverables |
| 2 (solid) | 107 | 76% | Adequate foundation, bridges available |
| 1 (weak) | 18 | 13% | Need enrichment before deliverables |
| 0 (disconnected) | 0 | 0% | None — all gaps resolved |

**Overall: 87% of intersections are at dim 2 or higher.** This is a healthy geometry.

### Strongest Connections (dim 3+, 4 hooks each)

All 15 dim-3+ connections:

| Facet | Audience |
|-------|----------|
| competitive-positioning | angel-investors, board-members, founding-team, seed-series-a-investors |
| event-sourced-architecture | cfos-controllers, founding-team |
| design-partner-program | founding-team, seed-series-a-investors |
| firm-first-gtm | founding-team, seed-series-a-investors |
| association-community-marketing | association-leadership |
| cloud-migration-strategy | founding-team |
| proven-completeness | seed-series-a-investors |
| quickbooks-xero-onboarding | cfos-controllers |
| tax-preparation-wedge | seed-series-a-investors |

**Pattern:** Internal audiences (founding-team, seed-series-a-investors) dominate dim-3+. This means the vault is strongest at telling the story to people already inside the tent.

---

## 4. TOC Constraint Identification

### Per-Audience Ranking (avg dimension, ascending = weakest first)

| Audience | Avg Dim | dim 3+ | Role |
|----------|---------|--------|------|
| **business-owners-operators** | **1.57** | **0** | Indirect beneficiary |
| **bookkeepers-junior-staff** | **1.71** | **0** | End user |
| **tax-preparers-cpas** | **1.79** | **0** | Primary user |
| **accounting-firm-partners** | **1.93** | **0** | **Decision maker** |
| association-leadership | 1.93 | 1 | Channel partner |
| angel-investors | 2.00 | 1 | Funder |
| board-members | 2.07 | 1 | Governance |
| cfos-controllers | 2.07 | 2 | User/buyer |
| founding-team | 2.36 | 5 | Internal |
| seed-series-a-investors | 2.36 | 5 | Funder |

### The Constraint: `accounting-firm-partners`

**This is the bottleneck audience.** Per the methodology:

> "An hour lost at the constraint is an hour lost for the system."

Accounting firm partners are the **buying decision makers** in the firm-first GTM model. Every other audience is downstream:
- Partners decide → CPAs/bookkeepers use → business owners benefit
- Partners decide → investors fund based on adoption proof

Yet firm partners have **avg dim 1.93 with zero dim-3+ connections**. No facet has a strong hook into their concerns. The vault can tell a compelling story to investors and the founding team, but **cannot yet deliver a high-confidence narrative to the people who sign the purchase order**.

### Per-Facet Ranking (ascending = weakest)

| Facet | Avg Dim | dim 3+ |
|-------|---------|--------|
| **unit-economics** | **1.40** | **0** |
| **reconciliation-elimination** | **1.50** | **0** |
| professional-judgment-preservation | 1.80 | 0 |
| firm-first-gtm | 1.90 | 2 |
| wpf-excel-interface-strategy | 1.90 | 0 |
| ai-empowerment-strategy | 2.00 | 0 |
| cloud-migration-strategy | 2.00 | 1 |
| association-community-marketing | 2.10 | 1 |
| proven-completeness | 2.10 | 1 |
| quickbooks-xero-onboarding | 2.10 | 1 |
| tax-preparation-wedge | 2.10 | 1 |
| design-partner-program | 2.20 | 2 |
| event-sourced-architecture | 2.20 | 2 |
| competitive-positioning | 2.40 | 4 |

**Weakest facets:** `unit-economics` (1.40) and `reconciliation-elimination` (1.50). These are core value propositions that don't yet have enough hooks into audience concerns.

---

## 5. Constraint × Bottleneck Analysis

The critical intersection is **unit-economics × accounting-firm-partners** (dim 1, only 1 hook: "capital efficiency"). This pair has **10 q-holes** — the firm partners care about profitability, retention, staff utilization, ROI, etc., but the unit-economics facet has no synonyms or claims that connect to those concerns.

**This is the single highest-leverage fix in the vault:** enriching the `unit-economics` facet with synonyms that map to firm partner language (margin improvement, staff leverage, client expansion economics) would raise dimension across all 10 audiences simultaneously.

---

## 6. Inbox Triage

| Card | Content | Recommendation |
|------|---------|----------------|
| `business-plan.md` | Full Jan 2026 business plan | Keep as reference — already extracted into facets/evidence |
| `planning-overview.md` | Phase 2+ vision document | Keep as reference — future-phase material |
| `strategic-summary-current.md` | Reconciled strategy summary | Keep as reference — strategic context |

These are **reference documents**, not cards needing curation. They were correctly normalized and parked. No action needed.

---

## 7. Communication Routes (Design Phase)

### Route 1: Investor Narrative (READY)
**Path:** competitive-positioning → event-sourced-architecture → tax-preparation-wedge → firm-first-gtm → design-partner-program  
**Audiences:** seed-series-a-investors (avg 2.36), angel-investors (2.00)  
**Status:** All intersections at dim 2+. This route is **ready for deliverable generation**.

### Route 2: Firm Partner Adoption (CONSTRAINT — needs work)
**Path:** tax-preparation-wedge → professional-judgment-preservation → quickbooks-xero-onboarding → proven-completeness → design-partner-program  
**Audience:** accounting-firm-partners (avg 1.93)  
**Status:** Most intersections at dim 2 but no dim 3+. Needs enrichment on unit-economics and reconciliation-elimination to strengthen the business case for partners.

### Route 3: Association Channel (READY)
**Path:** association-community-marketing → firm-first-gtm → design-partner-program  
**Audience:** association-leadership (avg 1.93, has 1 dim 3+)  
**Status:** Strongest community-marketing hook is already dim 3+. **Ready for CPE presentation deliverable**.

### Route 4: Practitioner Enablement (NEEDS ENRICHMENT)
**Path:** professional-judgment-preservation → ai-empowerment-strategy → wpf-excel-interface-strategy → quickbooks-xero-onboarding  
**Audiences:** tax-preparers-cpas (1.79), bookkeepers-junior-staff (1.71)  
**Status:** All dim 2 but no strong hooks. Needs practitioner-specific evidence (e.g., Liberty Tax interview insights → evidence card).

---

## 8. Evidence Gap

Only **3 evidence cards** support 14 facets:

| Evidence | Linked Facets |
|----------|---------------|
| technical-codebase-review | event-sourced, reconciliation, ai-empowerment, wpf-excel, quickbooks-xero |
| investor-market-evaluation-feb2026 | tax-prep, judgment, ai-empowerment, gtm, association, unit-economics |
| accounting-podcast-ai-bookkeeping-sep2024 | event-sourced, tax-prep, judgment, ai-empowerment |

**Missing evidence for:**
- `competitive-positioning` — needs competitor analysis evidence
- `cloud-migration-strategy` — needs technical migration plan evidence
- `design-partner-program` — needs pilot results or LOI evidence
- `proven-completeness` — needs completeness contract specification evidence
- `unit-economics` — needs pricing validation / financial model evidence

**The Liberty Tax interview** (`liberty-tax-interview` in S_sources) has not been promoted to an evidence card. It contains validation data for onboarding, edge cases, and practitioner workflow.

---

## 9. Recommended Next Actions (Priority Order)

### Immediate (exploit the constraint)
1. **Enrich `unit-economics` facet** — add synonyms: `margin improvement`, `staff leverage`, `client expansion economics`, `technology ROI`, `scalable service delivery`. This single change raises dimension across all 10 audiences.
2. **Enrich `reconciliation-elimination` facet** — add synonyms: `no rework`, `fewer corrections`, `clean books`, `reporting confidence`. Same leverage effect.
3. **Promote Liberty Tax interview to evidence card** — creates a practitioner-evidence link that strengthens Routes 2 and 4.

### Near-term (generate first deliverables)
4. **Generate investor deck deliverable** via Route 1 — geometry is ready, all dim 2+.
5. **Generate association CPE presentation deliverable** via Route 3 — strongest channel hook.
6. **Create `completeness-contract` evidence card** — investors flagged this as must-have.

### Medium-term (elevate the constraint)
7. **Add design partner evidence** — once pilots begin, capture results as evidence cards.
8. **Add pricing validation evidence** — LOIs or paid pilot commitments.
9. **Enrich org context** — run `compute-intersection --all --org` once organizational data is structured.

---

## 10. Tooling Note

This review was produced using the newly improved VaultTool (9 commands, 80 tests). Key commands used:

- `validate` — confirmed 0 errors, 0 warnings
- `geometry-summary` — produced per-audience and per-facet rankings
- `coverage-matrix` — visual grid of all 140 intersections
- `compute-intersection --all --stale-only` — recomputed only changed pairs after fixing dim-0 root cause

The dim-0 gaps (professional-judgment-preservation) were caused by a synonym mismatch (`85 15 boundary` vs `85/15 boundary`). Fixed and recomputed in one pass.
