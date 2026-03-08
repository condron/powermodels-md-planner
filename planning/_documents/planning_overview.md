# PowerModels — Current Context (Detailed)
**As of:** 2026-02-14 (America/New_York)  
**Purpose:** Consolidate the *current* “truth” of the PowerModels project across prior chats, reflecting how newer ideas replaced older ones. This is the working context for decks, product decisions, and execution.

---

## 1) Executive Summary

PowerModels is a financial modeling and forecasting SaaS built for the “AI remakes finance” transition, where the platform that wins will not be the one that *sounds* the smartest—but the one that is **trustworthy, auditable, reproducible, and extensible**.

The core product direction has converged on:
- A **ledger-grade data spine** (event-sourced, immutable lineage) that supports auditability and repeatability.
- **Agent-assisted workflows** constrained by verifiable data and explicit model state (minimizing hallucinations by design).
- **Excel as a first-class surface area** (pragmatic adoption wedge, distribution-aligned).
- A **partner-led GTM** (CPA firms/aggregators and fractional CFO networks) as the primary scaling engine.
- **Four major product offerings**: *three via channel partners, one direct to SMB*.
- **Enterprise-grade isolation + locality constraints** from the start, because channel partners require it.

---

## 2) The Core Thesis (“Why this exists”)

### The market reality we’re betting on
Finance teams are increasingly overwhelmed by:
- Fragmented systems: accounting, banking, payroll, AP/AR, and FP&A living in silos.
- Manual “glue work”: reconciliation, categorization, variance explanation, and report packaging.
- Low trust in automation: finance cannot tolerate black-box workflows that can’t be audited.

### The contrarian wedge
Most “AI for finance” products optimize for:
- Demo magic
- Fast automation without provenance
- Shallow integrations and “one-off” insights

PowerModels optimizes for:
- **Auditability** (lineage, traceability, repeatable outputs)
- **Reproducibility** (same inputs → same outputs)
- **Extensibility** (partners can build solutions on top of stable primitives)
- **Distribution leverage** (partners can scale delivery without bespoke rework)

---

## 3) Product Definition (What the product is now)

### Product category
**Financial modeling + forecasting SaaS** with strong accounting alignment and channel distribution.

### Product shape
A platform that:
1. Ingests and normalizes financial/operational signals
2. Maintains a ledger-like, immutable record of changes and derived states
3. Supports model creation and forecasting with strong “truth controls”
4. Enables partners to deliver repeatable solutions to SMB and mid-market customers

### Offerings (current structure)
PowerModels has **4 major offerings**:
- **3 offerings delivered through channel partners**
- **1 offering sold direct to SMB customers**

> The “four offerings” structure is part of the operating plan and should show up consistently in packaging, pricing, and deck narrative.

---

## 4) Target Customers and Users

### Primary distribution customers (who sells/implements)
- **Accounting firms** (small to mid-size)
- **State associations / accounting networks** (top-of-funnel + trust)
- **CPA aggregators** (scaled distribution)
- **Fractional CFO firms / consultant networks**

### End customers (who benefits)
- **SMBs** (core)
- **Mid-market** (where partner delivery and higher ACV becomes meaningful)

### Key user roles
- Firm owner / partner (economic buyer)
- Fractional CFO / controller (power user)
- Staff accountant / bookkeeper (daily operator)
- SMB owner / operator (consumer of outcomes)

---

## 5) Distribution and GTM (Current posture)

### GTM center of gravity: Partner-led
The platform is built to scale via partners because:
- Trust and workflow adoption is mediated by CPAs and finance operators.
- Partners already own the relationship and have distribution scale.
- Partner delivery forces standardization, packaging, and repeatability (good product discipline).

### Channel breakdown (how it plays out)
- **CPA firms & associations (breadth):** many clients, standardized needs, strong trust moat
- **CPA aggregators (scale):** structured distribution and predictable partner economics
- **Fractional CFO networks (depth):** higher ACV per client, higher willingness to pay for modeling outcomes
- **Direct SMB motion (selective):** a single direct offering; used as a learning lab and growth lever later

### Implicit replacement of older ideas
Earlier “just hire direct sales and sell to SMBs” thinking is now replaced by:
- **Distribution-first** (partner leverage), with direct as a secondary engine once the platform and packaging are battle-tested.

---

## 6) Architecture and “Trust Moat”

### The non-negotiable design constraints
- **Data isolation:** required for all customers (multi-tenant, but hard isolation boundaries)
- **Data locality:** at least one partner requires locality (region-specific storage/processing)
- **Auditability:** lineage for data + transformations + model outputs
- **Reproducibility:** deterministic reruns (or explicitly tracked non-determinism)

### The “ledger spine” concept
PowerModels is architected around an **event-sourced ledger-like system** that:
- Records atomic events and transformations
- Produces derived states (models, forecasts, reconciliations) in a traceable way
- Enables “why is this number here?” with provenance

### Agentic workflows—constrained, not magical
AI is positioned as:
- A **copilot** operating over verifiable data, not a replacement for accounting truth
- A **workflow engine** that produces *explainable outputs* with references to lineage
- A system that can be audited, replayed, and reviewed

This replaces “AI generates models” with:
- **AI assists model creation and operations**, while the system enforces correctness, lineage, and reviewability.

### Excel as a first-class surface
Excel is treated as:
- A distribution-friendly UI surface
- A familiar workspace for finance pros
- A template and solution packaging channel (via Excel-DNA)

---

## 7) Data Model and Platform Layers (Conceptual)

> Names below reflect the *direction* that’s emerged in prior work; exact implementation can evolve, but the layered intent is stable.

### Core layers
1. **Ingestion Layer**
   - Bank feeds, credit card statements, accounting exports, operational inputs
   - Normalization + identity resolution (vendors, accounts, entities)

2. **Ledger / Event Store Layer**
   - Append-only record of events
   - State derived via projections/materialized views
   - Full lineage and replay capability

3. **Model Layer**
   - Forecast models, templates, scenario versions
   - Model-as-artifact: versioned, reviewable, reproducible

4. **Workflow / Agent Layer**
   - Reconciliation agents, classification agents, narrative/variance agents
   - Guardrails: source-backed reasoning, explicit confidence, human review loops

5. **Presentation Layer**
   - Web app
   - Excel add-in (Excel-DNA)
   - Packaged templates and partner-delivered solutions

---

## 8) Product Packaging Principles

PowerModels should be packaged so it is:
- **Repeatable for partners** (solutions, templates, playbooks)
- **Composable** (building blocks that combine into offerings)
- **Permissioned** (roles + access boundaries)
- **Auditable** (every key number has lineage)

The packaging should naturally align with:
- Partner delivery workflows
- Standardized onboarding paths
- Multi-tier pricing and expansion

---

## 9) Differentiation (Current direction)

### Primary differentiators (what matters)
1. **Auditability and lineage by design**
   - Not “trust me,” but “here’s the chain of provenance.”

2. **Reproducibility and model versioning**
   - Models are artifacts you can replay, compare, and govern.

3. **Partner-scale delivery**
   - The platform is designed to make a CPA firm *more scalable* per employee.

4. **AI that is constrained by verifiable data**
   - AI is valuable when it operates inside strong truth controls.

5. **Excel-first adoption wedge**
   - Meeting finance where they are, while building toward a modern platform.

### Known work item
- **Market differentiators need revision** for the next review: the messaging should lean harder into the “trust moat + partner-scale + constrained AI” thesis and avoid vague “AI does finance” claims.

---

## 10) International Expansion (Planning Assumption)

International expansion remains part of the plan, but the key current implication is technical:
- **Data locality** (and jurisdictional compliance readiness) needs to be supported early because partner requirements already demand it.

---

## 11) Operating Model Implications

### Why this context matters for execution
If partner-led and auditability are the thesis, then execution priorities shift:
- “Fast demo” is not the north star; “trusted outcomes at scale” is.
- The platform must support:
  - Partner onboarding
  - Repeatable solution delivery
  - Strong controls and permissions
  - Clear artifacts and review workflows

### What success looks like operationally
- Partners can deliver outcomes with fewer bespoke projects.
- End customers get consistent, explainable numbers and forecasts.
- The platform produces a durable data asset (lineage-rich) that compounds in value over time.

---

## 12) Metrics That Matter (North-star candidates)

### Partner-led growth metrics
- # of active partner firms
- Partner activation rate (onboarded → delivering outcomes)
- Clients per partner (and growth over time)
- Revenue per partner / gross margin per partner

### Product value metrics (trust + outcomes)
- Time-to-first-forecast / time-to-first-usable model
- Reconciliation coverage and exception rate
- Variance explanation coverage (what % of changes have attributable drivers)
- Reproducibility rate (successful reruns with identical outputs when inputs unchanged)

### Expansion metrics
- Net revenue retention by partner cohort
- Feature adoption by role (CFO vs accountant vs owner)
- Template/solution reuse rate

---

## 13) Known Open Decisions / Pending Items

### Differentiator narrative revision
- Must be updated for the next review; messaging should be consistent across deck, site, and investor updates.

### Corporate restructure communication
- There was an explicit pending decision:
  - include restructure location in investor update **vs**
  - communicate it as a separate note

---

## 14) Practical “If you’re new here” Summary

PowerModels is not “AI bookkeeping.”
It is a **trust-first finance platform** that makes partners and finance pros more scalable by combining:
- ledger-grade lineage,
- reproducible models,
- constrained agentic workflows,
- and Excel-native adoption.

Distribution is the strategy:
**partners first**, direct second.

---

## 15) Working One-Liners (for internal alignment)

- **Product:** “Auditable, reproducible modeling + forecasting with a ledger-grade spine and constrained AI copilots.”
- **GTM:** “Partner-led distribution via CPAs and fractional CFO networks; one direct SMB offering as a controlled channel.”
- **Moat:** “Trust moat (lineage + replay) + distribution leverage (partner-scale delivery) + ecosystem compounding (templates/solutions).”

---
