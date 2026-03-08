---
type: facet
id: tax-preparation-wedge
label: Tax Preparation as Entry Wedge
synonyms:
  - tax-driven entry
  - preparation forcing function
  - tax season adoption
  - preparation to 85 percent
links_to_facets:
  - firm-first-gtm
  - event-sourced-architecture
  - professional-judgment-preservation
claims:
  - Tax preparation forces correctness and cannot tolerate incomplete records
  - PowerModels enters through tax prep then expands to reporting and forecasting
  - Platform prepares books to 85 percent completion for professional review
  - Final 15 percent judgment review and sign-off remains with CPA
  - Tax prep creates adoption discipline and establishes trust in financial record
evidence_refs:
  - investor-market-evaluation-feb2026
  - accounting-podcast-ai-bookkeeping-sep2024
  - bancroft-beta-user-feb2026
owner: product-team
status: active
---

# Tax Preparation as Entry Wedge

## Description

PowerModels enters accounting firms through tax preparation, not general bookkeeping or reporting. Tax prep is a forcing function: incomplete or inconsistent records cannot be deferred, errors surface immediately, and correctness is mandatory. This makes it the optimal entry point for establishing trust in a new financial system.

## Key Points

- **Tax forces correctness**: Can't file with unresolved gaps or assumptions
- **Natural adoption discipline**: Firms won't use unproven tools during tax season
- **Defined scope and timeline**: Tax season provides clear start/end for pilot validation
- **Preparation to 85%**: Platform automates ingestion, reconciliation, categorization, schedules
- **Professional judgment preserved**: Final 15% (review, interpretation, sign-off) stays with CPA
- **Trust before expansion**: Once tax prep is trusted, same record reused for reporting/forecasting

## The 85/15 Boundary

### Platform Handles (85%)
- Transaction ingestion from bank feeds, QB, etc.
- Completeness reconciliation
- Structured categorization
- Persistent schedule maintenance (depreciation, prepaids, deferred revenue)
- Review-ready working papers
- Explicit flagging of uncertain transactions

### Professional Handles (15%)
- Judgment on ambiguous categorizations
- Tax interpretation and strategy
- Client-specific adjustments
- Review and sign-off
- Client communication and explanations
- Final filing responsibility

## Why Tax (Not Bookkeeping or Reporting)

| Entry Point | Strengths | Weaknesses |
|-------------|-----------|------------|
| **Ongoing bookkeeping** | Continuous engagement | Easy to defer issues, low switching cost |
| **Management reporting** | High visibility | Low forcing function, optional for many SMBs |
| **Tax preparation** ✓ | Mandatory, forces correctness, deadline-driven | Seasonal, high stakes |

Tax preparation is **mandatory, correctness-forcing, and deadline-driven**, making it ideal for proving the platform works under real professional pressure.

## Adoption Sequence

1. **Pilot in off-season or with low-risk clients**: Prove preparation workflow
2. **Run parallel with existing process**: Validate completeness and accuracy
3. **Full adoption during tax season**: Platform becomes primary preparation tool
4. **Trust established**: Firm relies on financial record quality
5. **Expand to reporting**: Same record now used for interim financials, board packages
6. **Expand to forecasting**: Time-ordered record enables rolling forecasts without rebuilds

## Strategic Value

- **Adoption discipline**: Firms won't gamble with tax prep on unproven tools
- **Clear success criteria**: Did it produce correct, complete, defensible tax returns?
- **Sticky once validated**: If it works for high-stakes tax prep, trust extends to other uses
- **Not the destination**: Tax is the wedge, not the endpoint—platform expands to year-round use

## Risks

- **Seasonal pressure**: Cannot disrupt tax season; pilots must happen in off-season or with low-risk clients
- **High bar for trust**: One error during tax prep destroys confidence
- **Services creep danger**: Must maintain 85/15 boundary to preserve professional accountability
