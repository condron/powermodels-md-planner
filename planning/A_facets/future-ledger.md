---
type: facet
id: future-ledger
label: Future Ledger and Shadow Transactions
synonyms:
  - shadow transactions
  - committed future events
  - forward-looking ledger
  - deterministic forecasting
  - future ledger
  - predictive preparation
links_to_facets:
  - event-sourced-architecture
  - reconciliation-elimination
  - proven-completeness
  - unit-economics
  - drift-metric
claims:
  - Platform treats committed future events as first-class ledger entries alongside historical actuals [ASPIRATIONAL — zero code exists, Feb 2026 code review]
  - Signed contracts payroll and lease schedules are ingested as shadow transactions with known timing and amounts [ASPIRATIONAL — no shadow transaction code]
  - Future Ledger enables deterministic forecasting not probabilistic guessing [ASPIRATIONAL — no implementation]
  - Predictions are mathematically provable calculations based on committed events not statistical inference [ASPIRATIONAL — no implementation]
  - System of Truth combines event sourcing with formal logical models [PARTIAL — event sourcing exists via DataStore.cs but no formal logical models]
  - Depreciation schedules are in active development in RC5 which could be the first shadow transaction type [IN PROGRESS — 6 stories in RC5 backlog]
evidence_refs:
  - strategy-overview-feb2026
  - codebase-review-feb2026
owner: product-team
status: active
---

# Future Ledger and Shadow Transactions

## Description

The Future Ledger extends PowerModels' event-sourced architecture forward in time. While the core platform maintains a complete, immutable history of all financial events, the Future Ledger treats **committed future events** — signed contracts, payroll schedules, lease agreements, known tax obligations — as first-class ledger entries.

This means the system doesn't just tell you what happened. It tells you **what will happen**, with mathematical certainty for committed events and measurable uncertainty for everything else.

## How It Works

### Shadow Transactions
Committed future events are stored as "shadow transactions" in the event stream:
- **Payroll**: Known amounts, known dates, deterministic
- **Lease payments**: Contractual obligations, fully predictable
- **Recurring revenue**: Signed contracts with defined terms
- **Tax obligations**: Calculated from known positions
- **Depreciation**: Scheduled, formulaic, certain

These shadow transactions participate in all projections, reports, and reconciliations exactly like historical transactions — because they are equally real.

### Deterministic vs. Probabilistic

| Approach | Method | Confidence |
|----------|--------|-----------|
| Legacy forecasting | Statistical extrapolation from past | Low — "might happen" |
| AI wrappers | Pattern matching / LLM inference | Unknown — "guessed" |
| **Future Ledger** | **Committed event calculation** | **High — "will happen"** |

The key insight: most of a business's near-term financial future is already committed. By capturing those commitments as events, the Future Ledger turns forecasting from guesswork into arithmetic.

## Strategic Importance

### For Accounting Firms
- Firms can show clients their **committed financial position** — not a guess
- Advisory conversations shift from "what might happen" to "here's what's already locked in, here's what's variable"
- Proactive client management: see which clients are heading toward trouble before it manifests

### For Investors
- Demonstrates that PowerModels is not just a preparation tool — it's a **financial logic infrastructure**
- "System of Truth" positioning differentiates from both legacy CRUD systems and AI wrappers
- Creates a moat: committed-event forecasting requires event-sourced architecture that incumbents can't retrofit

### For Product Vision
- Phase 1 (current): Preparation and reporting with historical events — micro/1099 clients first
- Phase 2: Client dashboard exposes monthly books to end-clients — creates the surface for forecasting
- Phase 3: Future Ledger activates via client dashboard — full business cash and revenue forecasting with drift metrics, sold as upsell through existing dashboard interface
- Phase 4: Full Financial OS with real-time lending, embedded finance, M&A simulations via Future Ledger API

**Delivery route:** The client dashboard website (Phase 1 = monthly book visibility) becomes the forecasting surface (Phase 3). Clients who see their books monthly will naturally want projections. The dashboard is the trojan horse.
