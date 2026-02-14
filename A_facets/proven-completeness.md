---
type: facet
id: proven-completeness
label: Proven Completeness and Completeness Contract
synonyms:
  - completeness contract
  - completeness spec
  - acceptance criteria
  - preparation quality standard
links_to_facets:
  - tax-preparation-wedge
  - professional-judgment-preservation
  - event-sourced-architecture
claims:
  - Proven completeness is measurable not abstract
  - Completeness Contract defines pass-fail criteria for preparation
  - Acceptance test validates reconciliation to source platform
  - Exception taxonomy surfaces what AI cannot confidently classify
  - Stop boundary at 85 percent is defined by measurable criteria
evidence_refs:
  - investor-market-evaluation-feb2026
  - accounting-podcast-ai-bookkeeping-sep2024
owner: product-team
status: active
---

# Proven Completeness and Completeness Contract

## Description

Investors and practitioners require that "proven completeness" be a **measurable specification**, not an abstract claim. The Completeness Contract is a one-page definition of what 85% preparation means in concrete, testable terms — including inputs required, reconciliation rules, exception taxonomy, output artifacts, and the stop boundary.

## Key Points

- **Input**: QB/Xero transaction history, bank feeds, prior year data
- **Process**: Event-sourced import + AI categorization + reconciliation
- **Output**: 85% prepared books with review-ready working papers
- **Flagged**: 15% items requiring professional judgment (explicit uncertainty)
- **Validation**: Reconciliation to source platform + CPA review and sign-off

## Completeness Contract Elements

### Pass Criteria
- All bank/CC statement line items reconciled to journal transactions
- Chart of accounts mapped and validated
- Categorization applied to all transactions (AI-assisted)
- Flagged items explicitly surfaced with reason codes
- Working papers generated for CPA review
- Trial balance balances to source platform

### Fail Criteria
- Missing bank feeds or statement gaps
- Unresolved duplicate transactions
- Unreconciled balances exceeding threshold
- Uncategorized transactions without flags

### Exception Taxonomy
- Ambiguous merchant descriptors
- Cross-account transfers
- Shareholder activity
- Multi-entity intercompany transactions
- Manual adjustments requiring judgment

## Why This Matters

### For Investors
- Converts abstract "completeness" into underwritable spec
- Creates measurable acceptance test for pilot validation
- Builds "trust moat" that competitors can't easily replicate

### For Practitioners
- Clear expectations of what the platform does and doesn't do
- Explicit boundary protects professional judgment
- Reduces adoption risk (know exactly what you're getting)

### For the Product
- Defines scope boundary that prevents services creep
- Creates testable quality standard for engineering
- Enables objective pilot success measurement
