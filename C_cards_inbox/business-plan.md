---
type: source
id: business-plan
status: needs-structuring
origin_path: S_sources\business-plan.md
---

Business Plan\
Purpose: Single source of truth for strategy, product, GTM, and economics

## Section 1: Executive Summary

**Section 1: Executive Summary**

Do you ever wonder why accountants are always reconciling --- and why everyone is always waiting for the numbers?

Financial reports are delivered late, revised at the last minute, and often change after they are first circulated. This is not because the data is missing or because people are not working hard. It is because most accounting systems store the same financial reality in multiple places --- transactions in one location, balances in another, reports in another --- and those representations inevitably drift.

Reconciliation exists to stitch these fragments back together. Rebuilds happen because corrections overwrite history instead of preserving it.

PowerModels is a financial preparation and reporting platform designed to ensure completeness and preserve a continuous financial history, so accounting, reporting, and forecasting can be rewound, refreshed, and replayed without rebuilding. Rebuilds are replaced with refresh.

Most accounting systems are built as collections of modules --- accounts payable, accounts receivable, payroll, and the general ledger --- rather than as a single financial record. As modern stacks expand through direct bank feeds, payment tools, and third-party add-ons, fragmentation increases. Each integration introduces another source of "truth," forcing accountants and finance teams to reconcile what was sent, what was received, and what ultimately posted to the ledger.

Over time, these representations drift. The result is structural cost, delayed insight, and low confidence in financial outputs --- not just inefficiency.

PowerModels addresses this problem at its root. Most systems give you financial snapshots; PowerModels gives you a financial movie --- one record you can rewind, refresh, and replay without rebuilding anything. Corrections propagate deterministically across accounting, reporting, and forecasting, eliminating the Excel rebuilds that dominate finance work today.

PowerModels is sold through accounting firms, not one business at a time. Once the firm-led rollout motion is proven, growth accelerates mechanically through staffing rather than market education, allowing PowerModels to scale in parallel across firms without changing pricing, product scope, or workflows. This creates a highly leveraged go-to-market model: a single firm adoption expands across many client entities under the firm's control, rather than requiring repeated SMB sales. Growth is driven by new firm adoption and, more importantly, by internal expansion as firms standardize preparation across clients.

The platform enters through tax preparation because it is the point where incomplete or inconsistent records cannot be avoided and correctness is forced. Once preparation is standardized and trust in the financial record is established, the same record is reused for reporting and forecasting without additional cleanup or rework.

PowerModels replaces fragmented accounting, reporting, and modeling workflows for firms, producing decision-ready financial information without rework.

------------------------------------------------------------------------

**Why Now**

The problem PowerModels addresses is not new. Finance teams have always reconciled, rebuilt reports, and relied on spreadsheets to bridge gaps between accounting, reporting, and forecasting. What has changed is not the pain, but the feasibility of fixing it structurally.

Three shifts make PowerModels possible now.

First, accounting systems have become increasingly fragmented. Modern platforms emphasize speed of ingestion and surface-level reporting but store financial information across multiple representations that must be reconciled manually. As businesses operate across more tools, entities, and reporting requirements, the cost and fragility of this structure has become untenable.

Second, finance work has become continuous rather than periodic. Boards, lenders, and operators now expect near-real-time visibility, rolling forecasts, and frequent updates. Snapshot-based systems designed for monthly or quarterly close cannot support this cadence without constant rebuilds and rework.

Third, advances in system architecture make it practical to maintain a single, time-ordered financial record at scale. Event-based data models, deterministic processing, and modern infrastructure now make it practical to preserve and replay financial history at scale --- enabling systems that accountants can trust immediately without changing how they work. Because PowerModels fits directly into existing preparation and review workflows, firms can adopt it as soon as it demonstrates correctness, without retraining staff or reworking client processes. What was previously theoretical is now operationally feasible.

Accounting firms are also under pressure --- not because their role is diminishing, but because demand for reliable, timely financial information has increased faster than preparation workflows have evolved. Talent constraints, margin pressure, and rising client expectations are forcing firms to eliminate rework and standardize preparation so professional time is spent on judgment rather than cleanup.

PowerModels exists at the intersection of these shifts. It does not change who does the work. It changes how financial reality is recorded so that work does not have to be repeated.

------------------------------------------------------------------------

**Philosophy: Professional Preparation, Business Focus**

PowerModels is built on a simple operating philosophy: financial preparation is a professional discipline, and businesses are best served when that work is done correctly, economically, and at scale by professionals.

In practice, many small and growing businesses are pushed toward fragile, do-it-yourself financial processes because professional preparation has historically been expensive and labor-intensive. This leads to a false economy: business owners spend time managing bookkeeping, reconciling errors, and reworking reports instead of running their business, while professionals are pulled into cleanup rather than higher-value work.

PowerModels is designed to change the economics of doing things properly. By standardizing preparation and eliminating repetitive rework, the platform makes it cost-effective for accounting professionals to own the financial record end-to-end, while allowing business owners to focus on operating decisions rather than financial mechanics.

Importantly, PowerModels does not attempt to replace professional judgment or shift accountability away from firms. It preserves professional responsibility while removing the structural inefficiencies that make correctness expensive. This alignment benefits firms, businesses, and downstream stakeholders by improving reliability, reducing rework, and increasing confidence in financial outputs.

This philosophy directly informs PowerModels' go-to-market strategy. By selling through accounting firms and expanding across their client base, the platform enables professional-led preparation at scale rather than one-off, self-serve adoption. The result is higher quality financial data, lower total cost of preparation, and a more sustainable operating model for both firms and their clients.

------------------------------------------------------------------------

**Problem Context / Market Reality**

Finance teams and accountants spend a disproportionate amount of time reconciling and reworking financial reports. Reports are frequently delivered late and revised at the last minute, not due to lack of effort or expertise, but because underlying systems require constant stitching together of inconsistent data.

This behavior is structural. Most accounting systems store the same financial information in multiple representations --- transactional data, balances, and reports --- that must be reconciled back together as changes occur. This separation is embedded in how systems are built, with financial activity flowing through distinct modules and tools rather than a single financial record.

As modern finance stacks expand through direct bank feeds, payments platforms, and third-party add-ons, fragmentation increases rather than decreases. Each integration introduces another source of "truth," requiring teams to reconcile what was initiated, what was transmitted, and what ultimately posted to the ledger. Even small corrections propagate downstream, breaking reports, forecasts, and models and forcing manual intervention.

The cost of this structure is not limited to accounting labor. It manifests as delayed decision-making, repeated Excel rebuilds, and reduced confidence in financial outputs at the board and executive level. Finance teams compensate by maintaining parallel spreadsheets and manual models to bridge gaps between accounting systems, reporting tools, and planning workflows.

As reporting cadence increases and expectations shift toward continuous visibility, the limitations of snapshot-based systems become more pronounced. Tools designed for periodic close cannot support rolling forecasts, frequent updates, or multi-entity oversight without escalating reconciliation effort and rework.

PowerModels is built to address this structural problem directly by changing how financial reality is recorded, preserved, and reused.

------------------------------------------------------------------------

**Product Architecture & Differentiation**

Traditional accounting platforms are built around static, period-based snapshots of financial position. When underlying data changes, those snapshots must be regenerated and downstream reports, forecasts, and models are often rebuilt manually.

PowerModels is built on a different architectural assumption. It maintains a single, time-ordered, double-entry financial record that preserves the full sequence of events rather than overwriting balances. Financial outputs behave as views over this record, not as independent artifacts that must be reconciled back together.

This design allows finance teams and accounting professionals to rewind to a prior point in time, correct an entry, and refresh the record so that accounting, reporting, and forecasting update together. Downstream outputs remain consistent because they are derived from the same underlying history. Rebuilds are replaced with refresh.

By preserving event history and enforcing balance at the record level, PowerModels avoids the drift that occurs when transactions, balances, and reports are maintained separately. Reconciliation becomes a completeness check rather than a repair process, and changes propagate deterministically rather than through manual intervention.

This architectural choice underpins all downstream capabilities. Standardized preparation, persistent schedules, review workflows, and future reporting and planning features all rely on the same financial record. Differentiation does not come from surface-level features, but from the structure of the record itself and its ability to be reused without rework.

------------------------------------------------------------------------

**The Wedge: Tax Preparation as a Forcing Function**

PowerModels enters through tax preparation because it is the point where incomplete, inconsistent, or fragile financial records cannot be avoided. Tax preparation forces correctness. Errors surface immediately, assumptions must be explicit, and unresolved gaps cannot be deferred.

This makes tax-driven preparation the most effective entry point for standardizing financial records under professional control. Unlike ongoing bookkeeping, tax preparation requires a complete and defensible record for a defined period, creating a natural forcing function for adoption and trust.

Within this context, PowerModels is designed to take client books to approximately 85% completion. The platform ingests all transactions, reconciles for completeness, applies structured categorization, maintains persistent schedules (including depreciation, prepaids, and deferred revenue), and produces review-ready working papers. Transactions that cannot be confidently classified are surfaced explicitly for professional review.

The remaining 15% --- judgment, review, tax interpretation, and sign-off --- intentionally remains with the professional. This boundary is explicit and deliberate. It preserves professional accountability, avoids services creep, and aligns with how accounting firms operate in practice.

PowerModels is designed for firms to begin with a small number of high-pain clients and expand usage internally as confidence in the financial record grow. Expansion is firm-controlled rather than driven by per-client selling. As preparation becomes standardized, PowerModels becomes the default system for additional clients without renegotiation or incremental sales effort.

Importantly, tax preparation is not the product's destination. It is the entry point. Once a trusted financial record is established through preparation, the same record is reused for reporting and forecasting without additional cleanup or rework. This sequencing ensures adoption discipline while creating a foundation for durable expansion.

### 

**Go-To-Market Model & Expansion Mechanics**

PowerModels is sold through accounting firms rather than directly to individual businesses. This is a deliberate, leveraged go-to-market strategy designed to scale through internal rollout rather than repeated customer acquisition.

A single firm adoption expands across many client entities under the firm's control. Firms standardize preparation workflows once and then apply them repeatedly across their client base. Growth occurs through internal expansion rather than per-client selling, allowing PowerModels to compound usage without linear increases in sales effort.

This model reflects how accounting firms operate. Firms control tools, standards, and workflows across their portfolio of clients and capture the economic upside of improved efficiency and capacity. PowerModels aligns with this structure by enabling firms to onboard clients incrementally, at their own pace, without renegotiation or incremental procurement.

Tax preparation serves as the initial entry point because it enforces adoption discipline and establishes trust in the financial record. Once preparation is standardized, PowerModels becomes the default system for additional clients within the firm. Expansion is governed by firm confidence and internal rollout rather than external selling.

As client count per firm increases, unit economics improve structurally. Customer acquisition cost is incurred once per firm, while revenue expands as additional clients are onboarded. Retention is driven by workflow integration and record reuse rather than contractual lock-in.

This go-to-market model prioritizes durability over speed. PowerModels is designed to grow through standardization and reuse, not through high-velocity SMB sales. As firms deepen adoption and rely on a single financial record for preparation, reporting, and forecasting, expansion becomes mechanical rather than promotional.

PowerModels is sold as a **firm-licensed, per-client subscription**, billed monthly. Pricing is intentionally conservative and aligned to workload, not seats or files.

The go-to-market motion is deliberate and trust-based:

- sell once to a firm,

- demonstrate value in a single cycle,

- expand through standardization,

- avoid SMB self-serve funnels entirely.

Growth is constrained by onboarding capacity, not marketing spend, enabling disciplined scaling.

The base operating plan reaches cash-flow breakeven through firm adoption and client rollout alone, without relying on new product lines or pricing expansion.

------------------------------------------------------------------------

### **Why PowerModels Is Different**

Existing tools address fragments of the accounting workflow:\
• SMB accounting platforms optimize for business owners,\
• engagement systems manage documents, not books,\
• tax trial balance tools solve narrow, seasonal problems.

None own the accountant-controlled accounting record where messy data, adjustments, review, and explainability converge.

PowerModels occupies this missing layer by owning the accounting record itself. It preserves financial history as it evolves, rather than reconstructing it period by period, allowing professional preparation, review, reporting, and forecasting to operate on the same record without rework. This creates a durable foundation for professional work.

------------------------------------------------------------------------

### **Platform Advantage & Long-Term Direction**

While PowerModels does not position itself as "a platform," its architecture enables natural expansion once a trusted accounting record is established. These capabilities emerge through reuse of the same financial record, not through the addition of loosely connected modules.

By treating pro forma activity as first-class, PowerModels can connect historical results, current operations, and planned future activity within a single system. This enables a range of financial and operational insights, including:

• renewal tracking through projected invoices,\
• ARR, billings, and deferred revenue visibility,\
• cash forecasting and liquidity planning,\
• cap table and option vesting schedules,\
• and selective operational models where financial outcomes are directly affected.

These examples are illustrative, not exhaustive. PowerModels is not attempting to replicate full ERP or operational execution systems. Instead, it provides decision-ready views that allow professionals, operators, and stakeholders to understand financial position, risk, and trajectory without rework.

Finance and operations converge in PowerModels because they share the same underlying financial record --- not because separate systems are stitched together. This creates optionality over time while preserving focus on correctness, reuse, and professional accountability.

As more decisions, expectations, and historical context are anchored to the same financial record, the cost of reverting to fragmented tools increases, reinforcing long-term retention without requiring contractual lock-in.

------------------------------------------------------------------------

### **The Outcome**

PowerModels allows accounting firms to grow without hiring linearly by eliminating reconciliation and rework at the preparation layer. Review quality improves because professionals operate on a single, time-based financial record rather than stitched-together snapshots.

The result is a durable system of record that supports preparation, reporting, and planning without rebuilds as data changes. PowerModels starts where trust is earned --- preparation --- and expands with discipline through reuse of the same financial record.

------------------------------------------------------------------------

## Section 2: Company Overview

PowerModels is an **accountant-hosted financial preparation and reporting platform** built for organizations that manage multiple entities. The platform is designed to standardize accounting preparation, reduce reconciliation overhead, and enable portfolio-level reporting without replacing professional judgment.

PowerModels is **not** built for SMBs as direct users. SMBs do not have the authority, incentives, or workflow discipline required to enforce standardized preparation. Instead, PowerModels is sold to **intermediary organizations**---primarily accounting and bookkeeping firms, and secondarily lenders, private equity firms, and portfolio operators---that already serve as financial stewards for multiple businesses.

The company's core belief is that accounting preparation should be:

- **Model-driven**, not ad hoc

- **Event-based**, not snapshot-based

- **Standardized**, not customized per client

Most accounting work today is slowed by inconsistency: incomplete records, different categorization schemes, fragmented tools, and manual reconciliation. These problems compound when scaled across dozens or hundreds of entities.

PowerModels addresses this by taking each entity to a clearly defined **85% completion** state, producing consistent, auditable working papers while explicitly reserving the final 15%---judgment, interpretation, and sign-off---for accountants. This boundary is intentional and non-negotiable.

For organizations with existing accounting systems, PowerModels can ingest historical and ongoing records and reconstruct the general ledger within its own financial record. This allows firms to standardize reporting and preparation across entities without forcing operational systems to change, while ensuring consistency, auditability, and reuse going forward.

The platform is architected as long-term infrastructure rather than a point solution. While the initial application focuses on bookkeeping and preparation for tax, the underlying system supports expansion into portfolio reporting, cash and tax forecasting, and financial modeling without requiring a new data foundation.

PowerModels is designed to become the **default preparation and reporting layer** for multi-entity organizations.

------------------------------------------------------------------------

**A. Product Description**

**Product Overview**

PowerModels is an accountant-hosted preparation platform that takes client books to **85% completion**, producing review-ready, auditable working papers while deliberately preserving professional judgment.

The platform is designed for accounting and bookkeeping firms --- not SMBs --- and is used as internal infrastructure to standardize preparation across clients.

PowerModels does **not** attempt to replace accountants, automate judgment, or perform filing. Instead, it removes the bulk of mechanical preparation work so firms can scale capacity without scaling headcount linearly.

**What PowerModels Delivers**

For each client file, PowerModels:

- Ingests 100% of bank and card transactions

- Applies initial categorization and normalization

- Establishes and maintains formal accounting schedules (e.g., prepaids, deferred revenue, depreciation)

- Tracks expected vs actual events

- Produces review-ready working papers, including:

  - General Journal

  - Trial Balance

  - Balance Sheet

  - P&L

At this point, the file is considered 85% complete.

**Explicit Boundary (By Design)**

The remaining 15% is intentionally left to the firm and includes:

- Professional judgment

- Client explanations and clarifications

- Tax elections and interpretation

- Final review and sign-off

- Filing or submission

This boundary is fixed. It preserves trust, aligns with how firms actually work, and prevents services creep.

**Why This Works**

Most existing tools focus on ingestion and categorization but leave preparation fragmented, inconsistent, and reviewer-heavy.

PowerModels approaches preparation as a **structured accounting problem**, not a UI or workflow problem. By standardizing the underlying accounting model and expected events, it produces predictable, auditable outputs that reviewers can rely on.

This allows firms to:

- Reduce preparation friction

- Surface exceptions earlier

- Increase reviewer leverage

- Expand capacity without proportional hiring

**Architectural Advantage (Brief, Non-Technical)**

PowerModels is built on an **event-sourced accounting ledger** that records financial activity as a continuous stream rather than static period snapshots.

This architecture:

- Preserves full history

- Enables deterministic replay

- Creates a native audit trail

- Supports reliable automation today and forecasting tomorrow

While PowerModels initially enters through bookkeeping for tax preparation, this same foundation enables future solutions such as cash forecasting, tax payment forecasting, and financial modeling --- without rebuilding the data layer.

(Technical detail is provided in the Appendix.)

------------------------------------------------------------------------

## Section 3: The Problem --- Structural Constraints in Accounting Preparation

PowerModels exists now because **five structural forces have converged**, and all five are required. Any one of them alone would be insufficient.

------------------------------------------------------------------------

**1. Firms Are Capacity-Constrained, Not Demand-Constrained**

Accounting and bookkeeping firms are not short of demand. They are short of **preparation capacity**.

Across firms:

- Cleanup and preparation consume the majority of staff time

- Hiring junior staff scales linearly and is increasingly fragile

- Turnover, training, and quality control absorb partner attention

- Review and advisory work is crowded out by mechanical prep

This pressure compounds for firms serving many small clients, where:

- records are incomplete or messy,

- work is seasonal,

- and margins are sensitive to prep hours.

This has been true for years --- but firms are now **at the limit** of how much labor can be added without degrading quality or margin.

------------------------------------------------------------------------

**2. Data Access Has Improved, But Accounting Structure Has Not**

Over the last decade:

- Bank and card feeds have become broadly available

- Ingestion reliability has improved materially

- Source data is easier to access than ever

However, most accounting systems still:

- overwrite balances instead of preserving history

- fragment logic across modules (GL, AR, AP, payroll)

- hide uncertainty rather than surfacing it

- require reconciliation downstream

As a result, better data access has **not** translated into less prep work. It has often increased noise instead of reducing effort.

PowerModels becomes viable now because:

- ingestion is no longer the bottleneck, but structure still is

- preparation can finally be standardized end-to-end

------------------------------------------------------------------------

**3. AI Makes Acceleration Possible --- But Only With Controls**

Recent advances in machine learning and agentic AI make it feasible to:

- parse messy inputs,

- classify transactions,

- automate multi-step workflows.

But uncontrolled AI is **not acceptable** in accounting:

- it is probabilistic,

- it can hallucinate,

- and it breaks auditability.

PowerModels exists now because:

- AI can accelerate preparation,

- **but only when anchored to deterministic, auditable systems**

- and only when judgment remains human-owned.

Earlier attempts failed because they tried to automate decisions instead of structure.

------------------------------------------------------------------------

**4. Multi-Entity Oversight Has Become a First-Order Requirement**

Private equity firms, lenders, and portfolio operators now require:

- more frequent reporting,

- standardized metrics,

- faster visibility into risk,

- and fewer manual handoffs.

Yet most portfolio reporting today relies on:

- emailed PDFs,

- exported trial balances,

- spreadsheets,

- ad hoc KPI definitions.

This creates delays, errors, and blind spots --- especially during volatility.

PowerModels is viable now because:

- standardized preparation can be enforced upstream,

- making aggregation downstream straightforward.

------------------------------------------------------------------------

**5. Incumbents Are Structurally Blocked**

Incumbent accounting platforms are constrained by:

- snapshot-based data models,

- mutable balances,

- tightly coupled modules,

- and massive installed bases.

They can add AI features and UI improvements, but:

- they cannot easily adopt event-sourced, append-only models,

- they cannot enforce standardized preparation without breaking existing workflows,

- and they cannot cleanly aggregate across entities without ETL layers.

This creates an opening for a platform built correctly **from first principles**.

------------------------------------------------------------------------

**Why PowerModels, Why Now (Summary)**

PowerModels is possible now because:

- firms are hitting hard capacity limits,

- data access is finally sufficient,

- AI can assist (if constrained),

- portfolio oversight has become mandatory,

- and incumbents cannot easily re-architect.

This is not a timing window driven by hype.\
It is a window driven by structural necessity.

------------------------------------------------------------------------

## Section 4: The Solution --- Standardized Preparation to 85%

PowerModels serves **organizations that manage many entities and can mandate financial standards**. The market is defined by **control and aggregation needs**, not by the number of individual SMBs.

**4.1 Target Customer (ICP)**

**Primary ICP: Accounting & Bookkeeping Firms**

**Firm profile**

- 5--30 staff (sweet spot: 8--20)

- 50--300 active clients

- High concentration of sole proprietors and micro-businesses

- Monthly bookkeeping combined with annual tax work

- Fixed-fee or package pricing models

**Operational reality**

- Cleanup and preparation dominate workloads

- Prep work, not demand, is the bottleneck

- Review capacity is scarce

- Inconsistency across clients creates rework

**Economic profile**

- Margin pressure from prep labor

- Growth constrained by hiring

- Incentive to standardize, not customize

------------------------------------------------------------------------

**Buyer Persona**

**Primary buyer**

- Firm owner, managing partner, or operations lead

**Responsibilities**

- Margin and delivery accountability

- Staffing and capacity planning

- Workflow standardization

- Tool selection and enforcement

**Decision authority**

- Can mandate defaults

- Can require platform adoption

- Can roll out changes across clients

------------------------------------------------------------------------

**4.2 The "Shoebox" Reality (Why Pain Is Acute)**

A significant portion of small business clients:

- keep incomplete or no books during the year,

- provide records late,

- mix personal and business transactions,

- rely on accountants to "figure it out" at year-end.

This "shoebox" dynamic creates:

- unpredictable prep effort,

- high cleanup costs,

- review bottlenecks,

- compressed tax-season timelines.

The issue is not client behavior alone --- it is the lack of standardized preparation infrastructure that can absorb messiness without collapsing.

PowerModels is designed specifically for this reality.

------------------------------------------------------------------------

**4.3 Why Firms --- Not SMBs --- Are the Economic Buyer**

SMBs:

- lack incentive to standardize

- resist structured workflows

- churn frequently

- optimize for lowest visible cost

Firms, by contrast:

- already perform the work

- bear the cost of inefficiency

- value consistency and auditability

- control multiple entities

- can mandate standards

Selling directly to SMBs would:

- raise CAC

- increase churn

- reduce leverage

- undermine standardization

Selling to firms aligns:

- incentives,

- economics,

- and adoption dynamics.

------------------------------------------------------------------------

**4.4 Secondary and Expansion Buyers (Built In)**

Once standardized preparation exists, additional buyers emerge naturally:

- Private equity firms seeking portfolio-level visibility

- Lenders requiring standardized borrower reporting

- VCs and family offices monitoring financial health across companies

These buyers:

- already request financials

- struggle with inconsistent formats

- rely on manual aggregation today

PowerModels supports these buyers without changing the core product, because the preparation layer is already standardized.

------------------------------------------------------------------------

**4.5 Explicit Non-ICP**

PowerModels does **not** target:

- lifestyle accounting firms

- firms proud of manual processes

- pure hourly billing shops

- SMBs as direct users

- very large enterprises (initially)

Excluding these segments is intentional and protective.

------------------------------------------------------------------------

## Section 5: Why the Current Model No Longer Scales

The problems PowerModels addresses are **structural**, not behavioral or cosmetic. They persist across firm size, geography, and tooling because they are rooted in how accounting preparation is organized today. What has changed is not the nature of accounting work, but the tolerance for how much unstructured preparation firms can absorb before growth, quality, and margins break."

------------------------------------------------------------------------

**5.1 Where Time Actually Goes**

In accounting and bookkeeping firms, the majority of effort is spent **before** any value-added review or advisory work can begin.

Time is consumed by:

- reconstructing incomplete records

- reconciling bank and card activity

- identifying missing periods or accounts

- normalizing inconsistent categories

- resolving unclear transactions with clients

- rebuilding Trial Balances into a reviewable state

This work is largely mechanical, repetitive, and unavoidable given current systems. It scales directly with client count and complexity.

Crucially, this effort is front-loaded and non-deferrable. Review cannot start until preparation is complete.

------------------------------------------------------------------------

**5.2 Why Reconciliation Dominates Workflows**

Reconciliation exists because modern accounting workflows are fragmented.

Typical realities include:

- bank data living outside the general ledger

- AR, AP, payroll, and expenses handled in separate modules or tools

- balances updated independently across systems

- corrections overwriting prior values

As a result, firms must constantly ask:

- "Did we get everything?"

- "Which balance is correct?"

- "What changed since last month?"

Reconciliation is not a failure of people; it is a failure of system design.

------------------------------------------------------------------------

**5.3 Mutable Balances (Defined at First Use)**

Most accounting systems rely on **mutable balances** *(balances that are overwritten when changes occur, rather than preserving the full history --- see Appendix C)*.

When balances are mutable:

- prior states are lost or difficult to reconstruct

- changes are hard to trace

- explanations require inference rather than evidence

- audit trails are fragmented

This design forces accountants to:

- recreate history manually,

- rely on spreadsheets,

- and perform reconciliation to regain confidence.

------------------------------------------------------------------------

**5.4 Tool Sprawl and Structural Failure**

Firms typically rely on:

- an SMB accounting system (e.g., QBO/Xero),

- spreadsheets for cleanup and normalization,

- separate tax software,

- email and PDFs for client communication.

Each tool solves a narrow task, but **none standardize preparation end-to-end**.

Incumbents optimize:

- user interfaces,

- ingestion convenience,

- surface-level automation.

They do not address:

- how preparation should be structured,

- how uncertainty should be handled,

- how outputs should be standardized across clients.

------------------------------------------------------------------------

**5.5 Why Labor Scaling Breaks**

Hiring junior staff appears to be the default solution, but it breaks down quickly:

- prep effort scales linearly with headcount

- training and supervision consume senior time

- turnover introduces quality risk

- review becomes the bottleneck

The more a firm grows, the more fragile its delivery model becomes.

This is not a staffing problem.\
It is a systems problem.

------------------------------------------------------------------------

**5.6 Why This Is Worse in Multi-Entity Contexts**

For firms, lenders, or investors managing many entities:

- inconsistencies multiply,

- normalization becomes manual,

- reporting cycles lengthen,

- and risk visibility degrades.

Portfolio-level insight requires:

- consistent preparation upstream,

- not heroic aggregation downstream.

Without standardization at the preparation layer, aggregation will always be brittle.

------------------------------------------------------------------------

## Section 6: Product Overview

PowerModels addresses the preparation and reconciliation problem by standardizing accounting preparation itself, rather than attempting to automate judgment or layer features onto fragmented systems.

The product is designed to take each entity to a clearly defined state of readiness---85% completion---and then stop.

------------------------------------------------------------------------

**6.1 What PowerModels Does**

For each entity, PowerModels:

- Ingests 100% of bank and card transactions

- Reconciles accounts for completeness *(confirming that all expected activity is present, not inferring accuracy)*

- Applies initial categorization and normalization

- Routes uncertain or low-confidence items to a review account *(an explicit holding account for human review)*

- Establishes and maintains formal accounting schedules, including:

  - prepaids

  - deferred revenue

  - depreciation and amortization

- Applies transparent, rules-based calculations *(via a rules engine with inspectable logic, see Appendix C)*

- Produces review-ready working papers, including:

  - General Journal

  - Trial Balance

  - Profit & Loss

  - Balance Sheet

At this point, the file is considered 85% complete.

------------------------------------------------------------------------

**6.2 The 85% Completion Boundary (Reaffirmed)**

The remaining 15% of work is explicitly out of scope for PowerModels and remains with the accountant:

- professional judgment

- client explanations and clarifications

- adjusting journal entries driven by interpretation

- tax elections and treatment decisions

- final review and sign-off

- filing or submission

This boundary is intentional and fixed.

It:

- preserves accountant trust,

- avoids services creep,

- aligns with professional responsibility,

- and ensures PowerModels remains infrastructure, not a substitute for expertise.

------------------------------------------------------------------------

**6.3 What Is Automated vs. What Is Not**

**Automated / System-Driven**

- Data ingestion

- Reconciliation for completeness

- Initial categorization

- Schedule generation

- Rules-based calculations (including jurisdiction-specific accounting rules, where applicable)

- Working paper production

- Exception surfacing

**Explicitly Human-Owned**

- Resolution of review items

- Adjusting journal entries based on judgment

- Client conversations

- Interpretation and elections

- Final approval

PowerModels does not attempt to eliminate judgment.\
It creates the conditions for judgment to be applied efficiently.

------------------------------------------------------------------------

**6.4 Outputs Accountants Trust**

PowerModels is built around the outputs accountants already rely on.

The system produces:

- unadjusted Trial Balances,

- clear exception and review queues,

- adjusting journal entry support,

- adjusted Trial Balances as the authoritative state.

Reports are:

- directly derived from underlying events,

- traceable to source transactions,

- reproducible over time.

This allows reviewers to trust the outputs without needing to trust the system blindly.

------------------------------------------------------------------------

**6.5 Why This Is Not Bookkeeping Automation**

PowerModels does **not**:

- auto-resolve ambiguous transactions,

- hide uncertainty,

- overwrite history,

- replace professional decision-making.

Instead, it:

- standardizes preparation,

- makes uncertainty explicit,

- preserves full history,

- and supports review-first workflows.

Bookkeeping for tax preparation is the entry point, not the identity.

------------------------------------------------------------------------

## Section 7: Architecture & Why It Matters

**7.1 Architectural Principles**

The platform is designed around five non-negotiable principles:

1.  **Preserve full financial history**

2.  **Make uncertainty explicit**

3.  **Keep accounting logic deterministic and auditable**

4.  **Separate preparation from judgment**

5.  **Enable aggregation by design, not by export**

Every architectural choice flows from these principles.

------------------------------------------------------------------------

**7.2 Event-Based Financial Record (Defined at First Use)**

PowerModels maintains an **event-sourced, append-only financial record**\
*(a system that records every financial action as a sequence of events rather than overwriting balances; see Appendix C)*.

Instead of storing only current balances, the platform records:

- transactions,

- journal entries,

- adjustments,

- schedule events,

- corrections.

Balances and reports are **derived from this history**, rather than stored and overwritten.

This approach:

- preserves historical context,

- enables reconstruction of prior states,

- supports auditability,

- reduces reconciliation overhead.

**7.3 Double-Entry Accounting Core**

All financial changes in PowerModels are expressed as **double-entry journal entries**\
*(each event records equal debits and credits across accounts; see Appendix E)*.

This ensures:

- internal consistency,

- Trial Balance integrity,

- traceable changes over time.

The **Adjusted Trial Balance** is the authoritative reporting state.

Double-entry accounting is not replaced or abstracted away.\
It is **made explicit and enforced by the system**.

------------------------------------------------------------------------

**7.4 Rules Engine (Transparent Calculations)**

PowerModels includes a **rules engine**\
*(a transparent system that applies predefined accounting and calculation rules consistently; see Appendix C and Appendix E)*.

The rules engine is used for:

- double-entry posting logic,

- depreciation and amortization,

- accruals and deferrals,

- allocations,

- jurisdiction-specific tax calculations.

Rules are:

- explicit,

- inspectable,

- versioned,

- traceable to resulting journal entries.

This ensures calculations are explainable and auditable, not inferred.

------------------------------------------------------------------------

**7.5 Controlled Use of AI**

AI is used **only where it improves efficiency without compromising control**.

**Machine learning assists with:**

- ingesting messy inputs,

- extracting structured data,

- suggesting categorizations,

- flagging anomalies.

**Agentic workflows:**

- execute predefined functions,

- follow explicit rules,

- log every action,

- cannot modify historical records.

AI does **not**:

- perform accounting calculations,

- apply judgment,

- overwrite history,

- generate untraceable outcomes.

The accounting core remains rules-based and auditable.

------------------------------------------------------------------------

**7.6 Multi-Entity Aggregation by Design**

Because every entity is prepared using:

- the same accounting model,

- the same event structure,

- the same rules,

PowerModels can aggregate across entities **without ETL layers**\
*(extract--transform--load processes that copy and normalize data after the fact; see Appendix C)*.

Portfolio-level views can be generated by:

- entity,

- group,

- portfolio,

- relevance (not just time).

This enables:

- firm-wide dashboards,

- lender and investor reporting,

- portfolio oversight.

------------------------------------------------------------------------

**7.7 Governance and Access Control**

PowerModels is designed for institutional use.

The platform supports:

- role-based access,

- explicit permissions,

- multi-party approvals (e.g., client + accountant + portfolio manager),

- controlled data sharing.

This ensures:

- trust,

- compliance readiness,

- and clear accountability.

------------------------------------------------------------------------

**7.8 Why This Architecture Matters**

This architecture allows PowerModels to:

### standardize preparation without **replacing** judgment,

- reduce reconciliation effort structurally,

- scale across entities cleanly,

- support future forecasting and modeling without rework.

Most competitors optimize workflows or UI.\
PowerModels optimizes **what the system records, remembers, and can prove**.

------------------------------------------------------------------------

## Section 8: Product Roadmap

The PowerModels roadmap is structured around **provable preparation states**, not feature velocity. Each milestone demonstrates that the platform can reliably move a business from **raw financial data to review-ready, tax-preparation outputs**, while explicitly preserving professional control and stopping short of filing or execution.

------------------------------------------------------------------------

**8.1 Demo Milestone --- January 15, 2026**

*(Proof of Concept: End-to-End Preparation Cycle)*

**Objective**

Demonstrate a complete, controlled preparation cycle that takes a business from raw financial data to **annual, review-ready reporting for tax preparation**, stopping explicitly short of filing or submission.

This demo is designed to prove:

- the **85% completion boundary**,

- reconciliation for completeness,

- explicit handling of uncertainty,

- and accountant-controlled finalization.

------------------------------------------------------------------------

**Demo Structure (Scripted, Repeatable)**

**A. Environment Setup**

- PowerModels Chart of Accounts (CoA) developed and curated internally

- Base CoA plus industry-specific CoA variants

- Controlled **PowerModels Working User Data**

- Multiple controlled databases / businesses to simulate real processing states

------------------------------------------------------------------------

**B. Business Setup**

- Create a business

- Select an industry

  - Industry-specific data mappings

  - Industry-specific Chart of Accounts

  - Industry-specific Trial Balance structure

  - Industry-specific Three-Statement Model

- Define external data accounts:

  - 1 bank account

  - 3 credit card accounts

------------------------------------------------------------------------

**C. Input Data Validation**

- One year of financial data

- 4 accounts total

- CSV files and PDF statements

- 96 total files

- Structured directory:

  - 1 root directory

  - 4 subdirectories (one per account)

  - 24 files per account

This phase demonstrates **ingestion discipline and completeness expectations**, not speed.

------------------------------------------------------------------------

**D. Reconciliation Phase (Explicit and Visible)**

- Load data by account

- Reconcile each journal transaction to a statement line item

- Surface reconciliation tasks automatically

- Perform manual reconciliation where required

- Mark reconciliation complete via the task system

This phase explicitly demonstrates **proven completeness**, not inferred accuracy.

------------------------------------------------------------------------

**E. Adjustment Phase (Judgment Preserved)**

- Review data prior to adjustments

- Apply adjusting entries:

  - account reassignment

  - account splits

  - debit/credit corrections

- Tasks updated automatically and manually

- Adjustments reviewed through the Trial Balance

This demonstrates:

- review accounts,

- human judgment,

- transparent double-entry corrections.

------------------------------------------------------------------------

**F. Trial Balance Review**

- Use the Trial Balance as the primary refinement and control surface

- Validate that:

  - accounts balance,

  - mappings are correct,

  - exceptions are resolved or explicitly flagged

- Mark Trial Balance as complete via tasks

------------------------------------------------------------------------

**G. Three-Statement Finalization**

- Generate Balance Sheet, P&L, and Cash Flow Statement

- Validate statement mappings

- Confirm all data is accounted for

- Identify any remaining clean-up items (if required)

This demonstrates **review-ready outputs**, not perfection theater.

------------------------------------------------------------------------

**H. Completion (Simulated Final Steps)**

- Annual reporting package generated

- Reports delivered to accountant and/or client

- Data prepared for **tax preparation and review**

**Explicit boundary reaffirmed:**\
PowerModels does **not** submit data to tax authorities, initiate filings, or perform attestation. Filing remains the responsibility of the tax accountant, exactly as it does today.

------------------------------------------------------------------------

**8.2 Beta Release --- April 15, 2026**

*(Production Readiness)*

**Capabilities**

- Cloud-hosted platform

- PDF ingestion

- Plaid ingestion

- Adjusting journal entries

- Task list and workflow tracking

- Multi-entity environment support

**What This Proves**

- Platform reliability

- Repeatability beyond a controlled demo

- Early firm adoption readiness

- Preparation workflows that function outside a scripted environment

This phase validates that preparation correctness holds under real firm usage, not just controlled conditions.

------------------------------------------------------------------------

**8.3 Phase 2 --- Accounting Completeness (June 30, 2025)**

**Capabilities**

- Basic small business accounting

- Accounts Receivable (AR) --- recording and reconciliation only

- Accounts Payable (AP) --- recording and reconciliation only

- Accrual accounting support

**Guardrail**

AR/AP support accounting completeness and schedules.\
PowerModels does **not** perform invoicing, collections, payments, or treasury execution.

**Why This Comes Now**

- Only after preparation trust is established

- Only after firms operate PowerModels as default preparation infrastructure

------------------------------------------------------------------------

**8.4 Phase 3 --- Tax & Review Enablement (September 30, 2025)**

**Capabilities**

- Full tax report mapping

- Expanded task automation

- Recurring entries

- Workflow prompts

- Early CFO-style insights focused on explanation and visibility

**Guardrail**

Insights surface patterns and variances; they do **not** recommend tax positions or decisions.

------------------------------------------------------------------------

**8.5 Phase 4 --- Insight & Explanation (End of Year)**

**Capabilities**

- Automated AI workflows (**controlled, rule-bound, explainable**)

- Enhanced planning tools

- Expanded modeling support

- Portfolio-level insight surfaces

**Guardrail**

AI explains, surfaces, and summarizes.\
AI does **not** decide, file, submit, or advise.

------------------------------------------------------------------------

**8.6 Phase 5 --- Forward-Looking Models (Within 24 Months)**

**Capabilities**

- Cash forecasting

- Scenario analysis

- Forward-looking models built on the standardized financial record

- Pro forma invoices and renewal tracking as **expectation models**

Pro forma items are:

- removable,

- editable,

- and never auto-converted without approval.

------------------------------------------------------------------------

**8.7 Why This Roadmap Is Disciplined (Important)**

This roadmap:

- starts with preparation correctness,

- proves value before adding breadth,

- avoids premature automation claims,

- reinforces firm-led GTM,

- aligns exactly with the **85% completion boundary**.

No step contradicts:

- the platform architecture,

- the GTM strategy,

- or the professional trust model.

------------------------------------------------------------------------

## Section 9: Go-To-Market Strategy

PowerModels' go-to-market strategy is built on a single premise:

Sell once to a firm. Expand through the firm. Scale without per-client CAC.

This is not a volume SMB motion. It is a capacity-driven, trust-based motion where the firm---not PowerModels---controls rollout.

As a result:

• sales cycles are deliberate,\
• onboarding is structured,\
• growth is governed by operational readiness, not marketing spend.

If meaningful expansion requires repeated selling, the strategy has failed.

Once this motion is proven, scaling is achieved by adding sales and onboarding capacity---not by changing the customer, pricing, or delivery model.

------------------------------------------------------------------------

**9.2 Firm Access via Professional Associations**

Once onboarding and first-cycle repeatability are proven, PowerModels scales firm acquisition through professional accounting and tax associations.

These organizations aggregate hundreds to thousands of practitioners under shared professional standards, continuing education requirements, and trusted peer networks. They provide structured access to firms at scale without altering the underlying firm-led GTM motion.

Engagement with associations is structured around:

• education and workflow-oriented sessions (not product marketing),\
• demonstrations anchored in real preparation use cases,\
• peer-led validation rather than vendor-driven promotion,\
• opt-in participation by firms that recognize preparation pain.

For example, the Pennsylvania Society of Tax & Accounting Professionals (PSTAP) has approximately 1,200 members across accounting and tax practices.

Associations are not treated as a channel to "sell" individual firms. They are used to:

• compress discovery time across many firms simultaneously,\
• seed multiple qualified firm conversations in parallel,\
• accelerate trust through professional context,\
• scale firm acquisition without increasing per-firm CAC.

Importantly, association access does not relax onboarding discipline. Firms still enter through small land motions and complete real preparation cycles before expansion. This channel increases parallelism, not looseness.

------------------------------------------------------------------------

**9.3 Target Buyer & Entry Point**

**Economic Buyer**

• firm owner\
• managing partner\
• practice or operations lead

These buyers have authority to mandate defaults and standardize workflows.

**Initial Entry Use Case**

• cleanup and preparation for tax-relevant clients\
• clients with messy or incomplete books\
• high preparation effort relative to revenue

This entry point is chosen because:

• pain is immediate and visible,\
• success can be demonstrated in a single cycle,\
• value is recognized by both preparers and reviewers.

------------------------------------------------------------------------

**9.4 Land Motion (How We Enter a Firm)**

PowerModels does not attempt firm-wide rollout initially.

**Land Criteria**

• one firm\
• typically five clients\
• clients selected by the firm, not by PowerModels\
• representing high preparation pain

**Initial Engagement Characteristics**

• founder-led and relationship-driven early\
• high-touch onboarding\
• clear definition of "done": 85% completion\
• first-cycle completion is the success milestone

The objective of the land phase is not efficiency metrics or ROI slides.\
It is trust.

The initial land is intentionally narrow to validate repeatability; it is not representative of the eventual firm footprint.

------------------------------------------------------------------------

**9.5 Onboarding & Time-to-Value**

**Onboarding Characteristics**

• high-confidence early onboarding\
• structured, repeatable process\
• led by PowerModels in partnership with the firm\
• designed to establish quality and speed of rollout

**Time-to-Value**

• first visible value within the first monthly or quarterly cycle\
• expansion discussions follow demonstrated results, not forward promises

Onboarding capacity is treated as a quality gate rather than a bottleneck. Once first-cycle success is consistently achieved, onboarding is parallelized through additional staffing rather than slowed by process redesign.

------------------------------------------------------------------------

**9.6 Expansion Motion (Where Growth Comes From)**

Once trust is established:

• PowerModels becomes the default preparation path\
• new clients are onboarded directly into PowerModels\
• similar clients are migrated in controlled batches\
• exceptions remain visible and manageable

From Appendix H:

• firms add approximately 2--5 clients per month

Expansion is driven by:

• internal standardization,\
• reviewer confidence,\
• staff adoption.

PowerModels does not upsell expansion. It enables it.

Expansion velocity increases as firms internalize PowerModels as default preparation infrastructure, reducing incremental onboarding effort per client over time.

------------------------------------------------------------------------

**9.7 Standardization (Where Stickiness Is Created)**

Over time:

• firm-level templates are established\
• default charts, mappings, and workflows are enforced\
• task and deadline rollups operate across all clients\
• portfolio-level views become standard

At this stage:

• PowerModels is infrastructure, not a tool\
• switching costs are organizational\
• partial adoption becomes impractical\
• churn risk collapses

------------------------------------------------------------------------

**9.8 Why CAC Is Structurally Low**

Customer acquisition cost is low by design:

• one buyer controls dozens of entities\
• trust already exists between firm and client\
• expansion occurs without incremental selling\
• no SMB marketing funnel\
• no per-client onboarding

CAC is measured per firm, not per entity.

Expected outcomes (model-dependent):

• payback under six months\
• net revenue retention above 120 percent\
• sales efficiency improves with scale

As onboarding becomes repeatable, sales efficiency improves without requiring broader marketing spend or customer education cycles.

------------------------------------------------------------------------

**9.9 What We Explicitly Do Not Do**

To protect leverage and focus, PowerModels will not:

• sell directly to SMBs\
• run inbound content funnels\
• offer freemium tiers\
• hire SDR teams early\
• customize workflows per client\
• deliver services-heavy implementations\
• use transaction-based or per-file pricing

These motions destroy economics and dilute the platform.

------------------------------------------------------------------------

**9.10 Early GTM Execution Plan**

**Phase 1 --- Design Partners**

• 5--10 accounting firms\
• close collaboration\
• feedback loops into product\
• operational validation, not marketing case studies

**Phase 2 --- Repeatable Sale**

• defined onboarding playbook\
• time-to-first-cycle tracked\
• expansion measured at 30 / 60 / 90 days

**Phase 3 --- Parallel Firm Acquisition**

• association-led education and discovery\
• multiple firm conversations in parallel\
• continued discipline on land-and-expand mechanics

**9.11 Summary**

PowerModels' go-to-market strategy is designed for durable momentum rather than short-term acceleration.

By selling to firms and expanding through them, PowerModels grows:

• without per-client customer acquisition cost,\
• without SMB churn,\
• without becoming services-heavy.

This is a co-designed product and GTM system.

Speed is measured by time to first completed cycle and depth of rollout within early firms---not by logo count. Once these are proven, growth accelerates structurally rather than incrementally.

##  Section 10: Business Model & Pricing

PowerModels' business model is designed to align firm incentives, adoption dynamics, and long-term scale. Pricing is structured around firms and portfolios --- not individual SMBs --- and is intentionally conservative relative to the value created.

The objective is not short-term revenue maximization, but durable expansion within each firm through standardization and internal rollout.

This section explains **how PowerModels makes money**. Growth capacity and funding implications are addressed separately.

------------------------------------------------------------------------

**10.1 Business Model Overview**

PowerModels operates on a firm-licensed, per-entity subscription model.

- The accounting firm (or portfolio owner) is the customer

- Client entities under management are the usage unit

- Contracts are sold as **minimum one-year licenses**, billed monthly

- Revenue expands organically as additional entities are standardized

PowerModels sells once per firm and grows through internal expansion, not per-client selling.

------------------------------------------------------------------------

**10.2 Pricing Structure**

Pricing is designed to be simple, predictable, and easy for firms to justify internally.

Indicative pricing reflects differences in:

- transaction volume,

- schedule complexity,

- reporting and workflow requirements.

Illustrative ranges exist to explain firm economics, not to introduce pricing friction.

The pricing assumptions used for financial modeling are defined in Section 10.8.

------------------------------------------------------------------------

**10.3 Pricing Rationale**

PowerModels is priced to underprice **labor**, not software.

It replaces:

- repetitive preparation work,

- manual reconciliation,

- spreadsheet-driven cleanup.

It does **not** replace:

- professional judgment,

- review responsibility,

- client relationships,

- filing or submission.

We intentionally avoid aggressive ROI claims. Preparation effort varies widely by firm and client. The value shows up in **capacity, margin protection, and service quality**, not headline "hours saved."

------------------------------------------------------------------------

**10.4 Why Firms Can Afford This**

For firms:

- preparation is the bottleneck,

- growth is constrained by staffing,

- senior review time is scarce.

By standardizing preparation:

- firms can take on more clients without linear hiring,

- margins on fixed-fee work improve,

- senior staff are redeployed to higher-value activities.

PowerModels increases firm revenue potential rather than cannibalizing it.

------------------------------------------------------------------------

**10.5 Expansion Economics (Within Firms)**

Revenue growth is driven by:

- onboarding additional clients under existing firms,

- PowerModels becoming the default preparation path,

- portfolio-level standardization over time.

Expansion improves unit economics because it:

- requires no incremental CAC,

- requires no additional sales effort,

- is controlled by the firm, not the vendor.

Quantitative expansion assumptions are defined in Appendix H.

------------------------------------------------------------------------

**10.6 Expansion Beyond Client Rollout (Over Time)**

Once preparation is standardized and trusted, optional expansion opportunities emerge:

- reporting enhancements,

- planning and forecasting,

- portfolio-level insight.

These are introduced **only after** preparation is stable. The core business model works without them.

------------------------------------------------------------------------

**10.7 What Is Not Monetized Early**

PowerModels does not initially charge for:

- per-use actions,

- reconciliation events,

- adjustments,

- early onboarding support.

This encourages standardization and avoids adoption friction.

------------------------------------------------------------------------

**10.8 Pricing Model Used for Financial Planning**

Modeling assumptions:

- Monthly subscription

- Per entity under management

- Billed to the firm

- Minimum one-year contract, paid monthly

Reference price:

- **\$100 per client per month (blended average)**

This price is intentionally conservative.

------------------------------------------------------------------------

**10.9 Who Pays and Why**

Economic buyer:

- Firm owner or managing partner

Firms pay because:

- they bear preparation and staffing costs,

- they control workflows and standards,

- they capture the margin upside.

SMBs do not purchase PowerModels directly.

------------------------------------------------------------------------

**10.10 Why Per-Client Pricing**

Per-client pricing aligns with:

- workload,

- expansion mechanics,

- value creation.

Alternative models were rejected because they distort adoption or usage behavior.

------------------------------------------------------------------------

**10.11 Steady-State Economics (Per Firm)**

From Appendix H:

- \~75 clients per firm

- \$100 per client per month

- **\$90,000 ARR per firm**

This occurs:

- without incremental CAC,

- without increased sales effort,

- under firm control.

------------------------------------------------------------------------

**10.12 Billing Cadence**

Initial approach:

- Monthly billing under annual contract

Future option (not assumed):

- Annual prepay with incentive, after onboarding is repeatable.

------------------------------------------------------------------------

**10.13 Why Pricing Is Conservative**

Pricing does not attempt to:

- fully capture labor savings,

- price against urgency,

- monetize advisory upside.

This is intentional.

------------------------------------------------------------------------

**10.14 What Is Explicitly Not Priced (Yet)**

PowerModels does not initially price:

- advisory features,

- CFO insights

- Forecasting or forward looking modules,

- portfolio or lender reporting,

- AI-driven insights.

------------------------------------------------------------------------

**10.15 Summary**

PowerModels' business model:

- sells once per firm,

- expands through internal rollout,

- compounds revenue without linear sales cost,

- supports high retention and capital-efficient growth.

## Section 11: Capital Plan, Sales Capacity, and ARR Scaling

Early execution prioritizes correctness and trust so that sales and onboarding can scale in parallel once the motion is proven, rather than compounding fragility.

PowerModels' growth is governed by execution readiness rather than market discovery.\
Near- and mid-term ARR is driven by how many accounting firms we can onboard and support at high quality at any given time. Early growth is deliberately sequenced to establish a repeatable firm-level sales and onboarding motion; once validated, growth accelerates mechanically through added sales capacity rather than speculative demand creation.

This section explains:

- why near-term capital is required,

- how incremental capital translates directly into incremental ARR,

- and how speed increases intentionally once repeatability is proven.

------------------------------------------------------------------------

**11.1 Growth Model Overview (Firm-Led Expansion)**

PowerModels grows by onboarding accounting firms and expanding usage inside those firms over time.\
It does not grow by selling to individual SMBs.

As a result:

- revenue scales with the number of firms onboarded,

- expansion occurs internally within firms,

- customer acquisition cost is incurred once per firm, not per client.

This creates a compounding revenue model driven by standardization and internal rollout rather than repeated per-client selling.

------------------------------------------------------------------------

**11.2 Sales and Onboarding Capacity as the Growth Lever**

Early growth velocity is driven by onboarding quality and sales coverage, not by market education or customer discovery.

Specifically, ARR scales based on:\
• the number of firms onboarded in parallel,\
• the speed at which firms complete a first successful preparation cycle,\
• and how quickly internal rollout begins once trust is established.

PowerModels sequences growth deliberately: correctness and repeatability are established first, then sales capacity is added to scale the same motion in parallel. Once the firm-level motion is proven, additional sales capacity increases ARR mechanically without changing pricing, customer profile, or delivery model.

Growth is sequenced for acceleration --- early rigor enables faster, cleaner scale rather than limiting it.

------------------------------------------------------------------------

**11.3 ARR Scaling Through Sales Capacity**

The financial model demonstrates that ARR growth is additive and predictable once the sales motion is validated:

- A base case with founder-led sales plus one sales hire supports a credible path to \~\$3--4M ARR.

- Adding a second sales hire materially accelerates firm acquisition once onboarding is repeatable.

- Adding additional sales hires pushes ARR toward \~\$10M within the modeled period.

ARR scales predictably because:

- each firm has a known expansion ceiling,

- client rollout is controlled by the firm,

- pricing remains constant,

- and no per-client selling is introduced.

This is not a hit-driven model.\
It is arithmetic applied at increasing velocity.

------------------------------------------------------------------------

**11.4 Speed and Acceleration**

PowerModels is designed to accelerate, not to remain deliberately slow.

Early execution prioritizes proving a repeatable firm-level motion --- onboarding, first-cycle success, and initial internal expansion. Once this motion is demonstrated, speed increases by adding sales capacity rather than changing strategy.

Sales acceleration is planned, not deferred:

- The first sales hire defines and validates the motion.

- A second sales hire is added immediately once repeatability is established.

- From that point forward, each additional sales hire increases firm acquisition capacity and drives ARR growth without altering unit economics.

This sequencing ensures that growth accelerates structurally after proof, rather than relying on premature scaling or speculative demand assumptions.

**11.5 Why ARR Accelerates Faster Than It Initially Appears**

At first glance, PowerModels' growth may appear linear because firm onboarding is paced deliberately and expansion occurs inside firms over time. In practice, ARR acceleration emerges from the interaction of three reinforcing dynamics.

First, revenue expands within firms after the initial sale. PowerModels is not sold client-by-client. Once a firm adopts the platform and completes its first successful preparation cycle, additional clients are added under the firm's control. This creates a widening revenue base per firm without incremental sales effort.

Second, expansion lags firm acquisition by design. Early ARR understates future run-rate because firms typically begin with a small subset of high-pain clients. As standardization takes hold, rollout accelerates internally. This means that reported ARR reflects prior sales activity, while embedded expansion continues to compound beneath the surface.

Third, sales capacity compounds rather than resets. Each additional sales hire increases the number of firms onboarded per year, but previously onboarded firms continue expanding independently. As a result, incremental sales capacity stacks on top of an expanding installed base rather than replacing it.

The combination of these effects produces accelerating ARR even under conservative assumptions:

- pricing remains constant,

- per-firm expansion ceilings are fixed,

- onboarding discipline is maintained,

- no speculative demand assumptions are introduced.

In other words, ARR acceleration is not driven by changing behavior or taking on new risk. It is driven by the natural compounding of firm-level expansion layered on top of repeatable firm acquisition.

This is why PowerModels' growth profile improves over time and why additional sales capacity has a disproportionate impact once the motion is proven.

**11.6 Base Case vs. Accelerated Execution**

**Base Case**

- Founder-led sales plus one sales hire

- Conservative firm onboarding

- Expansion driven by internal firm rollout

- ARR grows steadily and deliberately

**Accelerated Case**

- Additional sales hires added once onboarding is repeatable

- More firms onboarded per year

- Same expansion dynamics applied to a larger base

- ARR scales faster without introducing new categories of risk

Importantly:

- no price increases are assumed,

- no services-heavy delivery model is introduced,

- no speculative demand assumptions are required.

The upside comes from doing more of the same thing well.

![](./media/image1.png){width="6.437661854768154in" height="3.8368055555555554in"}

*This chart holds all assumptions constant --- pricing, per-firm expansion, onboarding discipline --- and changes only sales capacity.*

![](./media/image2.emf){width="4.447916666666667in" height="2.0in"}

Model scope note:

These scenarios model post-onboarding expansion, assuming firms have completed their initial preparation cycle and are operating independently. Acquisition and onboarding are treated as discrete, front-loaded stages; once validated, expansion scales mechanically with added sales and onboarding capacity.

------------------------------------------------------------------------

**11.7 Capital Requirements and Staging**

PowerModels requires capital in two deliberate stages.

**Stage 1 --- Angel Round (Current Quarter)**

**Objective:**

**Capital required:** \~\$500,000

This funds:

- completion of the demo milestone,

- delivery of the first production release,

- retention of the core engineering and product team,

- early design-partner onboarding.

This round exists to:

- remove execution risk at the most fragile phase,

- avoid team disruption,

- reach tangible validation before institutional fundraising.

This is an angel round, not a full seed.

------------------------------------------------------------------------

**Stage 2 --- Seed Round (Mid-Year)**

**Objective:** Transition from proof to repeatable execution.

**Target raise:** \~\$3M (with optional extension toward \~\$5M)

This funds:

- expansion of sales capacity,

- onboarding process maturity,

- deeper product development aligned with the roadmap,

- faster firm acquisition without compromising quality.

The difference between \$3M and \$5M is speed, not viability.

------------------------------------------------------------------------

**11.8 Relationship Between Capital and Growth**

We are not creating a new category of demand; we are validating demand for a structured solution to a well-known problem. We are raising capital to validate a disciplined, firm-level go-to-market model and then scale it cautiously as it proves repeatable.

Incremental capital primarily unlocks:

- additional sales coverage,

- higher concurrent onboarding throughput,

- faster internal firm expansion.

The underlying unit economics already work under conservative assumptions.\
Capital controls timing, not direction.

------------------------------------------------------------------------

**11.9 Instrument Structure (Angel Round)**

The \$500,000 angel round is structured as a SAFE with a discount to the next priced round.

Key characteristics:

- discount-only SAFE,

- no valuation cap,

- conversion into the subsequent seed round.

This structure reflects that:

- the purpose of the round is to reach defined proof points,

- valuation should be set based on evidence, not conjecture,

- early investors are rewarded for risk without anchoring pricing prematurely.

------------------------------------------------------------------------

**11.10 Use of Funds (High Level)**

**Angel Round (\$500k):**

- product and engineering continuity,

- demo and production readiness,

- early firm validation.

**Seed Round (\$3M--\$5M):**

- sales and onboarding capacity,

- partner and firm development,

- continued product expansion aligned with the preparation-first roadmap.

Detailed operating budgets and runway projections are provided separately.

------------------------------------------------------------------------

**11.11 Summary**

PowerModels is an execution-gated growth business, where ARR scales predictably as sales and onboarding capacity are added.

- The revenue engine is defined and being validated.

- Expansion math is conservative and repeatable.

- The primary lever is sales capacity.

Capital accelerates growth mechanically rather than speculatively.\
This financing plan reflects confidence in execution, not reliance on momentum.

## Section 12: Market Opportunity & Context

*(Firm-Level, Capacity-Constrained)*

**12.1 Purpose and Scope**

This section establishes the scale, stability, and structure of the professional accounting market in which PowerModels operates.

It does not define growth targets, penetration assumptions, or revenue outcomes. Those are addressed explicitly and quantitatively in Appendix J (SAM / SOM), where execution limits, onboarding throughput, and firm-level rollout dynamics are modeled directly.

The purpose here is to demonstrate that:

• the preparation work PowerModels addresses exists at meaningful scale,\
• the demand for that work is recurring and non-discretionary, and\
• the operating model is intentionally scoped to a small, realistic subset of this market.

Importantly, this scoping reflects execution discipline rather than market limitation; expansion speed is a function of staffing and onboarding capacity once repeatability is proven.

------------------------------------------------------------------------

**12.2 Market Definition (Firm-Level, Not SMB-Level)**

PowerModels serves professional intermediaries who are accountable for financial preparation and reporting across multiple entities.

Primary buyers include:

• accounting firms\
• bookkeeping firms\
• and, over time, other multi-entity operators such as lenders and investment platforms

The unit of value is managed entities per firm, not individual end users.

PowerModels does not compete in:

• SMB self-serve accounting software\
• DIY tax filing\
• consumer financial tools

Those markets optimize for distribution and ease of use. PowerModels operates where correctness, explainability, and accountability are mandatory.

**12.3 Market Size --- Canada (Context Only)**

Canada illustrates the scale and durability of the professional accounting market.

Based on Statistics Canada, Innovation, Science and Economic Development Canada, and IBISWorld data:

• approximately 27,000 accounting, tax preparation, bookkeeping, and payroll firms operate in Canada (NAICS 5412, including employer and non-employer firms)\
• the sector generates approximately CAD \$18--28B in annual revenue, depending on scope definition

These figures are not used to estimate PowerModels revenue or penetration. They establish that the preparation workload PowerModels addresses exists at national scale and is structurally persistent.

------------------------------------------------------------------------

**12.4 Market Size --- United States (Context Only)**

The United States exhibits the same structural characteristics as the Canadian accounting market, at larger scale.

Based on IBISWorld and U.S. Census Bureau data:

• approximately 85,000--86,000 accounting services businesses operate in the United States (NAICS 54121)\
• the market is dominated by small and mid-sized firms, with a long tail of single-partner and small multi-staff practices

These figures are cited for structural context only. PowerModels does not model or plan against the full U.S. market. Execution assumptions, firm adoption rates, and revenue projections remain bounded by onboarding and rollout capacity and are defined separately.

------------------------------------------------------------------------

**12.5 Where Operational Pain Concentrates**

Pain is not evenly distributed across firms.

PowerModels targets firms in a specific operational band:

• 5--30 staff\
• 50--300 active clients\
• recurring bookkeeping combined with annual tax work\
• fixed-fee or package pricing\
• visible preparation and review bottlenecks

These firms are large enough to experience meaningful rework costs and margin pressure, but small enough that hiring does not scale cleanly and review capacity is scarce.

Larger firms face similar structural issues but differ materially in procurement dynamics and adoption cycles and are not part of the initial execution scope.

------------------------------------------------------------------------

**12.6 Partial Adoption, Not Firm Saturation**

The financial model assumes 75 managed entities per firm at steady state.

This represents **early internal rollout**, not firm-wide penetration.

  ---------------------------------------------------------------------------------
  **Firm Size (Staff)**   **Typical Clients Managed**   **75 Clients Represents**
  ----------------------- ----------------------------- ---------------------------
  5--10 staff             \~250--500                    \~15--30%

  10--20 staff            \~500--1,000                  \~7--15%

  20--50 staff            \~1,000--2,500                \~3--7%
  ---------------------------------------------------------------------------------

Under this model, PowerModels reaches strong unit economics well before firm saturation. The assumed rollout reflects realistic adoption behavior during the first phase of standardization, with substantial expansion headroom remaining inside each firm.

This framing emphasizes disciplined adoption while preserving clear upside through deeper rollout over time.

------------------------------------------------------------------------

**12.7 Preparation Workload as the Structural Constant**

Professional accounting demand is driven by entity-level preparation, not discretionary software adoption.

As context:

• Canada sees approximately 2.7--2.9 million corporate (T2) tax returns filed annually, in addition to millions of sole proprietorship business schedules.\
• The United States sees approximately 5--6 million corporate returns, 4--5 million partnership returns, and tens of millions of Schedule C filings each year.

These figures are cited only to illustrate the volume and recurrence of preparation work and why preparation capacity governs firm economics.

This workload persists regardless of tooling choices.

------------------------------------------------------------------------

**12.8 Intentional Focus on the Operating Model**

PowerModels is designed to win decisively in a specific operating lane before expanding beyond it.\
It does not attempt to serve every firm, every client within a firm, or every accounting workflow simultaneously.

Initial execution is focused on:

- firms with authority to mandate standards,

- an initial subset of clients inside those firms,

- preparation workflows where correctness is unavoidable and value is immediately visible.

This focus enables rapid validation, repeatable onboarding, and reliable outcomes without services-heavy delivery or fragile customization. It is what allows PowerModels to move from proof to scale efficiently, rather than diluting execution across too many use cases too early.

The quantitative implications of this focused rollout---firm counts, client expansion assumptions, and resulting ARR---are defined explicitly in Appendix J and modeled conservatively.

------------------------------------------------------------------------

**12.9 Summary**

The professional accounting market is large, stable, and compliance-driven.\
PowerModels operates within this market but deliberately models only a small, controlled slice of it.

The opportunity does not depend on discovering demand. It depends on disciplined execution inside a market that already exists.

Compliance-driven preparation work already exists at scale, and firms are actively seeking ways to reduce time spent on repetitive, low-judgment tasks that constrain capacity and margin. When a solution demonstrably reduces preparation friction without changing core workflows, obscuring judgment, or introducing risk, firms have strong incentives to adopt it quickly.

As a result, PowerModels' growth velocity is governed less by market education and more by how quickly the company elects to deploy additional sales and onboarding capacity once the firm-level motion is validated.

## Section 13: Competitive Landscape

PowerModels operates in a fragmented accounting technology landscape where no single vendor owns the system of record for professional accounting preparation.

Existing solutions address discrete parts of the accounting workflow but consistently leave firms to bridge critical gaps using manual processes, spreadsheets, and ad hoc judgment.

PowerModels is built to serve the layer that incumbents avoid: firm-owned accounting truth, where cleanup, adjustments, schedules, review, and explainability converge.

------------------------------------------------------------------------

**13.1 SMB Accounting Platforms**

(QuickBooks Online, Xero)

QuickBooks and Xero are the dominant systems of record for small businesses. They are optimized for:

• transaction capture\
• basic reporting\
• ease of use by business owners

They are not designed for professional cleanup, structured adjustments, review, or explainability. As a result, accounting firms routinely export data into Excel to:

• normalize accounts\
• perform cleanup\
• prepare tax trial balances\
• support review and sign-off

PowerModels positioning\
PowerModels does not compete with these platforms for day-to-day business operations. It addresses the professional accounting work that begins once data quality degrades and SMB-first tools no longer suffice.

Encroachment risk: Medium (6/10)\
While these vendors have the resources to improve workflows, their SMB-first incentives and backward-compatibility constraints make it difficult to surface incompleteness, preserve evolving financial history, or shift ownership of accounting truth to firms.

------------------------------------------------------------------------

**13.2 Engagement & Workpaper Systems**

(Caseware, CCH Engagement, Workpapers CS)

Engagement and workpaper systems are the closest adjacent category. These tools support:

• trial balance management\
• workpapers\
• review workflows\
• compliance documentation

They are document-centric by design and sit on top of existing accounting systems. They do not own the general ledger or the underlying accounting data model. Adjustments are treated as period-end snapshots, and financial history is reconstructed rather than preserved.

PowerModels positioning\
PowerModels eliminates the need to reconstruct accounting truth by owning it directly---integrating cleanup, adjustments, schedules, review, and disclosure into a single system of record rather than a document binder.

Encroachment risk: High (7/10)\
These vendors understand professional workflows and already sell to firms. However, moving into PowerModels' position would require re-architecting core data models, accepting ongoing accounting liability, and disrupting engagement-centric business models.

------------------------------------------------------------------------

**13.3 Tax Trial Balance & Workpaper Tools**

(Tallyfor, TreeBeam, regroupTAX)

Tax-focused trial balance tools exist because QuickBooks-plus-Excel workflows break down during tax season. These products address a narrow but real pain point: producing clean trial balances for tax filing.

They are point solutions:

• tax-centric\
• period-bound\
• not intended to operate year-round

They do not manage evolving financial history, expectations versus actuals, or completeness standards over time.

PowerModels positioning\
PowerModels subsumes this functionality while extending it across monthly workflows, cleanup and adjustments, and advisory use cases.

Encroachment risk: Low--Medium (4/10)\
These tools are more likely to remain tax-specific or become acquisition targets than to expand into a full accounting system of record.

------------------------------------------------------------------------

**13.4 Embedded & Platform-Owned General Ledgers**

(Teal, Layer, Vertical SaaS GLs)

Embedded general ledger infrastructure targets developers and vertical SaaS platforms. These systems are modern in architecture and optimized for:

• clean transaction flows\
• controlled environments\
• platform-owned data

They explicitly avoid messy data, professional judgment, and accounting liability. Accounting truth remains platform-owned, not firm-owned.

PowerModels positioning\
PowerModels is orthogonal to this category. It serves accounting firms directly and is designed to support judgment, cleanup, and review---capabilities embedded general ledgers intentionally avoid.

Encroachment risk: Low (2/10)\
Platform incentives and accountability avoidance fundamentally conflict with PowerModels' thesis.

------------------------------------------------------------------------

**13.5 Competitive Positioning Summary**

PowerModels occupies a distinct position between transaction systems and tax filing: the firm-owned system of record for professional accounting preparation.

No incumbent or emerging solution currently combines:

• firm ownership of the general ledger\
• structured cleanup and adjustment workflows\
• persistent schedules and expectations\
• in-system review and disclosure\
• a complete, explainable financial history

This gap has persisted because it is judgment-heavy, liability-bearing, and difficult to scale.

PowerModels is purpose-built to operate in this space.

**13.6 Competitive Comparison (Condensed)**

The table below summarizes where PowerModels differs structurally from adjacent categories.

![](./media/image3.emf){width="5.78125in" height="6.781851487314086in"}

**13.7 Bottom Line**

PowerModels is not another accounting tool. It is the missing system of record for professional accounting preparation.

##  Section 14: Metrics That Matter

**14.1 Metric Philosophy**

PowerModels is managed as infrastructure, not as a usage-driven application.

Accordingly, the company does not optimize for:

• top-of-funnel volume\
• raw user counts\
• short-term activity spikes

Instead, performance is governed by:

• firm-level adoption\
• expansion within firms\
• durable workflow standardization

The metrics below are designed to measure whether PowerModels is becoming embedded infrastructure or remaining a discretionary tool.

------------------------------------------------------------------------

**14.2 Metric Hierarchy (How the Business Is Controlled)**

PowerModels uses a tiered metric framework.

**Governing Metric** (Primary Control Variable)

• Clients per firm (distribution over time)

These metric captures adoption depth, economic expansion, and workflow entrenchment. If this metric grows, revenue, retention, and efficiency follow.

**Diagnostic Metrics** (Explain Why It's Moving)

• Time to first value\
• Time to standardization\
• Percentage of firms expanding\
• Client expansion rate per firm

These metrics diagnose onboarding quality and product fit.

**Guardrail Metrics** (Protect Quality and Scalability)

• Implementation hours per firm\
• Support tickets per client\
• Rework rate\
• Engineering load versus adoption

These prevent growth from degrading quality or margin.

------------------------------------------------------------------------

**14.3 GTM & Adoption Metrics**

Firm-Level Metrics

**• Firms signed**\
Number of accounting firms under contract.

**• Active firms**\
Firms with at least one client completing a full review-ready cycle.

**• Time to first value**\
Time from contract to first completed cycle.

**• Clients per firm (distribution)**\
Measured at land, 90 days, and 180 days.

------------------------------------------------------------------------

**14.4 Expansion & Standardization Metrics**

**• Client expansion rate per firm**\
Net new clients added per firm per month.

**• Percentage of firms expanding**\
Firms increasing client count after initial adoption.

**• Time to standardization**\
Time until PowerModels becomes the default workflow for new clients.

These metrics indicate whether PowerModels is transitioning from initial adoption to firm-wide infrastructure.

------------------------------------------------------------------------

**14.5 Value & Workflow Quality Metrics**

• Percentage of clients reaching 85 percent completion\
Measured per cycle.

• Exceptions surfaced per client\
Indicates clarity and explainability, not failure.

• Adjustments per client (trend)\
Expected to stabilize or decline over time.

• Rework rate\
Frequency of post-review revisions.

These are leading indicators of trust, correctness, and long-term retention.

------------------------------------------------------------------------

**14.6 Economic & Efficiency Metrics**

• ARR per firm\
Driven primarily by expansion within firms.

• Average clients per firm\
Core revenue driver.

• Net revenue retention (NRR)\
Target: greater than 120 percent once expansion stabilizes.

• Payback period per firm\
Time to recover CAC.

• Gross margin\
Ensures onboarding remains product-led rather than services-heavy.

------------------------------------------------------------------------

**14.7 Metrics Explicitly De-Emphasized**

PowerModels intentionally de-emphasizes:

• MAUs / DAUs\
• feature usage counts\
• SMB end-user metrics\
• marketing funnel conversion rates

These metrics are poor proxies for success in a firm-led infrastructure model.

------------------------------------------------------------------------

**14.8 Summary (Operating Truth)**

If clients per firm is increasing, and guardrails remain stable, PowerModels is becoming infrastructure.

If it is not, growth will stall regardless of market demand.

This metric framework ensures issues are visible early and corrected before scale.

## Section 15: Budget and Use of Funds

PowerModels' budget and use of funds are designed to reduce core product risk, preserve runway, and avoid premature scaling.

This is not a growth-at-all-costs plan. It is a correctness-first execution plan.

The operating budget is constructed bottom-up, tying headcount, onboarding capacity, and support costs directly to firm adoption and client rollout assumptions. The base case reflects only Stage 1--2 capabilities from the Product Expansion Ladder and excludes revenue from future planning or reporting modules.

The plan reaches cash-flow breakeven through controlled firm acquisition, steady client expansion within firms, and disciplined headcount growth aligned to onboarding and support capacity. Breakeven is achieved without relying on step-changes in sales efficiency, pricing, or scope.

------------------------------------------------------------------------

**15.1 Budget Philosophy**

Three principles govern spending:

1.  Fund architecture and correctness first\
    The primary risk in PowerModels is not demand generation; it is building a system that professionals trust.

2.  Delay GTM scaling until expansion is proven\
    Sales and marketing investment increases only after land-and-expand dynamics are validated through completed preparation cycles and repeatable onboarding throughput.

3.  Preserve flexibility\
    Accrual-based compensation, flat early headcount, and staged hiring preserve the ability to adjust without structural damage.

------------------------------------------------------------------------

**\**

**15.2 Cost Structure (High-Level)**

The cost base is intentionally concentrated in a small number of controllable categories.

**People (Primary Cost)**\
A senior, execution-focused team with deep prior experience in accounting systems, financial infrastructure, and enterprise software delivery, spanning engineering, modeling, onboarding, and operational support.

Core technical leadership includes significant experience designing and operating large-scale, event-sourced and data-intensive systems, which reduces architectural risk in building an append-only financial record.

This experience allows the company to operate with a smaller team, shorter feedback loops, and less managerial overhead than a junior-heavy organization."

**Infrastructure and Platform**\
Cloud hosting, financial data integrations, and core software tooling required to operate a production-grade accounting platform. Infrastructure costs scale with adoption but remain modest relative to personnel.

**Other Operating Expenses**\
Travel, firm meetings, legal, accounting, and compliance costs required to support enterprise-grade customers, managed conservatively.

Detailed cost assumptions, burn rates, and cash timing are maintained in the financial model.

------------------------------------------------------------------------

**\**

**15.3 Hiring Plan and Capacity Sequencing**

Hiring is paced explicitly to operational milestones rather than revenue targets.

The plan prioritizes:

• core engineering and modeling capacity to complete and harden the preparation platform,\
• onboarding and customer support roles aligned to firm acquisition and first-cycle completion,\
• limited go-to-market capacity focused on firm acquisition, not SMB end-user selling.

Headcount remains flat until onboarding throughput and within-firm expansion behavior are demonstrated and repeatable.

------------------------------------------------------------------------

**15.4 Use of Funds (Priority Order)**

Capital raised is allocated in the following order:

1.  Complete and Harden the Core Platform\
    Investment focuses on the event-sourced financial record, deterministic accounting engine, rules and schedules, reconciliation, review workflows, and reporting outputs. This is the defensive moat and must be correct before anything scales.

2.  Execute Demo to Beta to Production Roadmap\
    Funding supports demo readiness, beta release, cloud hardening, and controlled onboarding of early firms to validate real-world usage, repeatability, and trust.

3.  Prove Firm-Led Expansion\
    Funds are used to onboard an initial cohort of firms, support first-cycle completion, validate client expansion within firms, and refine onboarding and support playbooks.

4.  Prepare for Portfolio-Level Expansion (Not Monetized in Base Case)\
    Selective investment in multi-entity views, portfolio reporting, and governance workflows that deepen stickiness but are not required to reach breakeven.

------------------------------------------------------------------------

**\**

**15.5 What Funds Are Explicitly Not Used For**

To protect focus and economics, capital is not allocated to:

• SMB mass marketing\
• services-heavy delivery\
• large outbound sales teams\
• per-client customization\
• feature sprawl unrelated to preparation and reporting

These activities increase burn without improving defensibility.

------------------------------------------------------------------------

**15.6 Runway Discipline and Breakeven Logic**

The current structure supports controlled burn, flexible hiring, and staged investment tied to operational milestones.

Accrual-based compensation and delayed GTM scaling are intentional tools to extend runway, preserve ownership, and avoid premature dilution. Detailed runway and cash-flow assumptions are maintained in the financial model and reviewed continuously as execution progresses.

## Section 16: Operating Model

PowerModels' operating model is designed to support high-trust adoption, low ongoing overhead, and scalable delivery without drifting into services-heavy execution.

The operating model mirrors the product philosophy:

• standardize what can be standardized,\
• keep judgment with professionals,\
• and avoid bespoke work that does not compound.

------------------------------------------------------------------------

**16.1 Operating Philosophy**

PowerModels is not operated as a professional services firm or a managed bookkeeping provider.

It operates as a software platform with:

• disciplined onboarding,\
• explicit scope boundaries,\
• and a deliberate transition from high-touch adoption to standardized operation.

The objective is to support early adopters deeply, then reduce marginal support as firms internalize standard workflows and PowerModels becomes default infrastructure.

------------------------------------------------------------------------

**16.2 Onboarding Model**

Onboarding is intentionally staged.

Early Stage (Design Partners)

Early firms receive high-touch onboarding with direct involvement from product and domain experts. The focus is on:

• correct initial setup,\
• clean preparation cycles,\
• and confidence in outputs.

Onboarding success is measured by:

• completion of the first full review-ready cycle,\
• reviewer confidence,\
• and willingness to expand usage internally.

------------------------------------------------------------------------

Steady State (Post-Standardization)

Once a firm:

• adopts PowerModels as the default preparation workflow, and\
• uses standard templates and rules,

onboarding becomes:

• repeatable,\
• faster,\
• and largely self-directed within the firm.

PowerModels does not staff ongoing cleanup or execution services. Preparation work remains with the firm.

------------------------------------------------------------------------

**16.3 Support Model**

Support is tiered by lifecycle stage.

Early Support

• design partners,\
• first-cycle preparation,\
• initial firm rollouts.

Ongoing Support

• platform questions,\
• edge-case handling,\
• rule and model clarification.

Support explicitly excludes:

• manual bookkeeping,\
• client-by-client cleanup,\
• ongoing reconciliations.

Those responsibilities remain with the firm, preserving role clarity and scalability.

**16.4 Guardrails Against Services Creep**

PowerModels avoids services creep through explicit guardrails:

• enforcing the 85 percent completion boundary,\
• standardizing outputs and workflows,\
• refusing one-off custom logic,\
• keeping professional judgment with accountants,\
• pricing as software rather than labor.

Requests that require repeated manual intervention, bespoke per-client logic, or ongoing human effort are either declined, deferred, or redesigned into rule-based capabilities.

This preserves scalability and protects margins.

------------------------------------------------------------------------

**16.5 Internal Operating Discipline**

Internally, the company operates with:

• a senior-heavy team,\
• centralized early decision-making,\
• and strict architectural discipline.

There is:

• no large support organization,\
• no outsourced services arm,\
• no shadow operations team.

This structure keeps burn predictable, coordination overhead low, and product quality high as the system scales.

## Section 17: Team

PowerModels is led by a two-axis leadership structure that combines:

• deep financial, governance, and commercialization expertise, and\
• advanced event-sourced systems and platform architecture expertise.

This structure is intentional. PowerModels sits at the intersection of accounting discipline and high-integrity data systems. Both domains are required, and neither substitutes for the other.

------------------------------------------------------------------------

**Lisa Payne --- Co-Founder, Chief Executive Officer, Director**

Role: Market definition, financial reporting discipline, go-to-market execution, capital strategy, and governance.

Lisa leads PowerModels' commercial and financial direction, ensuring the platform solves real problems for professional users and is built into a durable, scalable business.

Her responsibilities include:

• defining product use cases and professional workflows\
• setting financial reporting standards and structure\
• ensuring outputs meet accountant, lender, and investor expectations\
• owning pricing, customer selection, and go-to-market strategy\
• leading fundraising, partnerships, and long-term growth planning\
• maintaining governance discipline and investor readiness

Lisa brings more than three decades of experience across fintech, enterprise software, infrastructure, and regulated industries, including turnarounds, scale-ups, and exits. She has raised over \$100M across equity, debt, and government funding and has rebuilt financial and operating systems in close collaboration with boards, governments, and institutional investors in Canada and internationally.

Her role is to ensure PowerModels is commercially grounded, professionally credible, and designed for adoption by real institutions rather than theoretical users.

------------------------------------------------------------------------

**\
Chris --- Co-Founder, Chairman of the Board, Chief Technology Officer**

Role: Product vision, platform architecture, event-sourced systems, and technical execution.

Chris owns the product vision and technical direction for PowerModels. He brings deep experience designing and operating large-scale, event-sourced and data-intensive systems in regulated environments.

Relevant experience includes:

• CTO and Head of Product at Event Store, contributing to the scaling of a SaaS platform beyond \$2M ARR and participating in multiple funding rounds\
• Chief Architect roles across asset management and enterprise platforms, including the design of unified event-sourced frameworks and enterprise-scale digital transformations\
• Senior leadership roles focused on large-scale data strategy and platform integrity

He is responsible for:

• the event-sourced, append-only financial record\
• the deterministic accounting engine\
• the separation of rules, machine learning, and agentic workflows\
• auditability, replayability, and system integrity\
• translating accounting and product requirements into durable platform capabilities

This expertise directly addresses the hardest technical risk in PowerModels and underpins its long-term defensibility.

------------------------------------------------------------------------

**\**

**Engineering & Platform Team**

The engineering team executes against the platform vision with a focus on correctness, reliability, and controlled automation.

Key roles include:

• VP Engineering --- delivery discipline and scaling readiness\
• Principal Engineer --- core accounting engine and event model\
• Senior Engineers --- workflows, schedules, and integrations\
• Integrations Engineer --- data ingestion, boundaries, and external systems

The team is intentionally senior-heavy to reduce architectural risk and avoid trial-and-error execution.

------------------------------------------------------------------------

**Financial Modeling, Operations, and Quality**

• Modeling and Customer Support --- bridges accounting logic with firm workflows\
• Operations and Administration --- internal execution and feedback loops\
• QA and IT --- reliability, regression prevention, and release confidence

Quality and correctness are treated as first-class concerns, not post-launch fixes.

## Section 18: Risks and Mitigation

PowerModels is built around deliberate architectural, product, and go-to-market choices. The primary risks facing the company are the direct consequences of those choices.

These risks are intentional and managed through sequencing, scope discipline, and execution controls rather than avoidance.

------------------------------------------------------------------------

**18.1 Risk: Slower Initial Adoption Due to Correctness-First Sequencing**

**Description**\
PowerModels prioritizes correctness, professional trust, and architectural integrity over rapid deployment. Early adoption requires firms to complete real preparation cycles rather than superficial trials.

**Impact**\
• Slower early top-of-funnel conversion\
• Longer initial sales and onboarding cycles

**Mitigation**\
• Enter through tax preparation, where correctness is mandatory\
• Limit early firm count to ensure repeatable preparation outcomes\
• Defer growth optimization until the financial record is proven

This sequencing reduces long-term churn, rework, and credibility risk.

------------------------------------------------------------------------

**18.2 Risk: Architectural Complexity and Execution Difficulty**

**Description**\
Maintaining a single, rewindable, time-based financial record is more complex than snapshot-based systems. Errors in event handling or propagation could undermine trust.

**Impact**\
• Higher upfront engineering effort\
• Slower surface-area feature expansion

**Mitigation**\
• Constrain scope to preparation and reporting\
• Explicitly exclude operational execution and judgment automation\
• Validate the architecture under professional scrutiny before scaling

Complexity is accepted early to avoid compounding fragility later.

------------------------------------------------------------------------

**18.3 Risk: Firm Adoption Pace and Internal Rollout Velocity**

**Description**\
Revenue expansion depends on firms adopting PowerModels as a standard and rolling it out across clients. Internal change management may progress unevenly across firms.

**Impact**\
• Growth tied to firm confidence rather than pure sales effort\
• Expansion timing may vary by firm

**Mitigation**\
• Start with high-pain clients to demonstrate immediate value\
• Preserve professional control over rollout timing\
• Align pricing and onboarding to incremental adoption

Once trust is established, expansion becomes mechanical.

------------------------------------------------------------------------

**18.4 Risk: Market Misinterpretation as a Tax or Cleanup Tool**

**Description**\
Because PowerModels enters through tax preparation, there is risk it is perceived as a narrow tax or cleanup solution rather than a foundational financial system.

**Impact**\
• Narrow initial positioning\
• Underappreciation of long-term platform value

**Mitigation**\
• Frame tax preparation as a forcing function, not the endpoint\
• Emphasize reuse of the financial record for reporting and forecasting\
• Maintain disciplined messaging around architecture and scope

------------------------------------------------------------------------

**18.5 Risk: Conservative AI Positioning Relative to Market Hype**

**Description**\
PowerModels does not lead with AI-driven automation claims in its core narrative, which may appear conservative relative to market hype cycles.

**Impact**\
• Reduced appeal to momentum-driven investors\
• Misalignment with AI-first expectations

**Mitigation**\
• Position AI as an outcome of correctness, not a substitute for it\
• Emphasize reliability, explainability, and reuse\
• Introduce advanced automation only after the financial record is trusted

This prioritizes durability over narrative velocity.

------------------------------------------------------------------------

**18.6 Risk: Incumbent Encroachment**

**Description**\
Incumbent accounting, engagement, or SMB platforms may attempt to move toward preparation and reporting workflows adjacent to PowerModels.

**Impact**\
• Increased competitive noise\
• Feature-level market confusion

**Mitigation**\
• Maintain architectural differentiation (single financial record vs snapshots)\
• Avoid feature-by-feature competition\
• Emphasize the difficulty of retrofitting legacy systems\
• Reinforce category language around preparation and record reuse

------------------------------------------------------------------------

**Summary**

PowerModels accepts early discipline in exchange for long-term leverage.

The company does not attempt to eliminate risk by narrowing ambition. It manages risk through sequencing, scope boundaries, and architectural integrity.

Each risk reflects a conscious tradeoff aligned with the objective of building a durable, professional-grade financial platform.

## Section 19: Long-Term Vision

**19.1 Vision Principle**

PowerModels' long-term vision is to connect finance and operations through a single, accountant-controlled system of financial and operational record, owned and governed by professionals.

The system begins with accounting preparation because that is where:

• data quality is established,\
• professional judgment is applied,\
• and accountability is enforced.

From this foundation, operational intelligence emerges through modeling, tracking, and explanation --- not as a separate execution system, but as an extension of the same underlying record.

------------------------------------------------------------------------

**19.2 From Financial Record to Operating Model**

Today, finance and operations are fragmented:

• accounting systems record the past,\
• operational systems manage day-to-day activity,\
• forecasts and plans live in spreadsheets,\
• and assumptions are rarely reconciled to outcomes.

PowerModels connects these layers by maintaining a continuous financial record that includes:

• historical actuals,\
• current accounting state,\
• and explicitly modeled future expectations.

This allows companies and their advisors to see what happened, understand why it happened, and model what is expected to happen next --- without executing or controlling operations.

------------------------------------------------------------------------

**19.3 Pro Forma as the Bridge Between Finance and Operations**

A core capability of PowerModels is treating pro forma activity as first-class, explicit planning objects rather than disposable spreadsheets.

Examples include:

• projected invoices,\
• expected renewals,\
• anticipated billings,\
• planned capital events.

These pro forma entries:

• are explicitly modeled, editable, and reviewable,\
• roll into forecasts and projections,\
• and convert into actual accounting events only through approval and professional review.

This creates a disciplined loop between planning, realized outcomes, and financial reporting --- anchoring operational intent to financial truth.

------------------------------------------------------------------------

**19.4 Operational Domains Addressed Through Modeling (Not Execution)**

Because PowerModels already maintains reconciled actuals, persistent schedules, and explicit expectations, it can support financially grounded operational modeling without re-ingestion or duplication.

Natural extensions include:

Revenue Operations (Modeling and Tracking)\
• ARR tracking\
• deferred revenue schedules\
• billings versus revenue reconciliation\
• renewal tracking via pro forma invoices

Cash and Liquidity Planning\
• cash forecasting\
• timing analysis of inflows and outflows\
• scenario-based liquidity modeling

Inventory and Cost Modeling (Where Applicable)\
• financial representation of inventory movements\
• cost recognition aligned with accounting events\
• working capital analysis

Capital Structure and Equity Modeling\
• cap table representation\
• option and vesting schedules\
• dilution modeling tied to financial projections

These are not bolt-on modules. They are expressions of the same underlying financial model.

------------------------------------------------------------------------

**19.5 Why This Is Structurally Different from ERP**

Traditional ERP systems:

• separate finance from operations,\
• rely on reconciliation between modules,\
• and treat forecasts as external artifacts.

PowerModels records accounting events once, preserves financial history as it evolves, and derives financial and operational insight from the same record.

It does not execute operational workflows or replace ERP systems. It explains and models their financial impact, avoiding the reconciliation burden inherent in modular architectures.

------------------------------------------------------------------------

**19.6 Multi-Entity and Portfolio Context**

The convergence of finance and operations is most powerful in multi-entity environments:

• accounting firms managing client portfolios,\
• lenders managing borrower portfolios,\
• investors managing company portfolios.

PowerModels supports:

• entity-level reporting,\
• portfolio-level aggregation,\
• role-based access,\
• standardized definitions across entities.

This enables oversight and comparison without repeated data requests or bespoke reporting.

------------------------------------------------------------------------

**19.7 What PowerModels Is Explicitly Not**

PowerModels does not claim to:

• replace operational execution systems,\
• initiate payments, invoicing, or fulfillment,\
• eliminate domain-specific tools,\
• or become a monolithic ERP.

The strategy is progressive unification through modeling and explanation, driven by data integrity, professional oversight, and incremental value.

------------------------------------------------------------------------

**19.8 Summary**

PowerModels starts with accounting preparation because that is where trust is earned.

From there, it becomes the system that connects financial history, modeled operational reality, and future plans.

By treating past, present, and projected activity as part of the same record --- without executing operations --- PowerModels enables better decisions without forcing organizations to stitch together disconnected systems.

------------------------------------------------------------------------

## Appendix A --- Product Definition 

**A.1 Product Definition**

PowerModels is an **accountant-hosted preparation and reporting system** that takes client books to **85% completion** using formal accounting models, producing **review-ready working papers** while explicitly preserving professional judgment, review, and sign-off.

------------------------------------------------------------------------

**A.2 Definition of 85% Completion (Locked)**

**In Scope**

1.  **100% Transaction Ingestion**

    - Bank and credit card transactions

    - CSV, PDF, and direct connections (e.g., Plaid)

    - No filtering or exclusion at ingestion

2.  **Reconciliation for Completeness**

    - Each journal entry must correspond to a source transaction

    - Missing, duplicated, or unmatched items are surfaced

    - Completeness is proven, not assumed

3.  **Initial Categorization & Normalization**

    - Deterministic, rule-based where possible

    - Industry and firm-level defaults supported

4.  **Review Account for Unresolved Items**

    - Transactions that cannot be confidently categorized are routed to a Review Account

    - Explicitly flagged for accountant review and client discussion

    - Nothing is silently forced

5.  **Formal Accounting Schedules**

    - Depreciation

    - Prepaids

    - Deferred revenue

    - Schedules persist across periods

6.  **Expected vs Actual Visibility**

    - Expected accounting events are tracked

    - Variances are surfaced

    - Missing or delayed events are flagged

7.  **Adjusting Entries**

    - Manual adjustments

    - Recurring entries

    - Automated depreciation and amortization

    - All entries are explicit, auditable, and reversible

8.  **Task & Workflow Tracking**

    - Reconciliation steps

    - Review checkpoints

    - Filing reminders (tracking only)

    - Aggregate firm-level visibility

9.  **Review-Ready Working Papers**

    - General Journal

    - Trial Balance

    - Balance Sheet

    - Profit & Loss

Outputs are **review-ready**, not filing-ready.

------------------------------------------------------------------------

**Explicitly Out of Scope**

PowerModels **does not** perform or imply:

- Tax filing or submission

- Payment initiation or execution

- Invoicing, collections, or treasury actions

- Tax elections or legal determinations

- Advisory recommendations

- Client communication on behalf of firms

- Attestation or sign-off

All of the above **remain the responsibility of the accountant**.

------------------------------------------------------------------------

**A.3 Why the 85% Boundary Exists**

The 85% boundary exists to:

1.  Preserve professional judgment and accountability

2.  Avoid services creep

3.  Align with real-world preparation → review workflows

4.  Enable scale without expanding liability

5.  Prevent roadmap and messaging drift

This boundary is **structural**, not temporary.

------------------------------------------------------------------------

**A.4 Enforcement Rule**

Any feature that:

- implies execution (filing, payment, invoicing),

- auto-converts plans into actions,

- hides or replaces judgment,

- or obscures accountability

**violates Appendix A** and requires explicit leadership approval.

------------------------------------------------------------------------

**A.5 Anchor Statement**

"PowerModels takes books to 85% completion using formal accounting models, producing review-ready working papers while preserving professional judgment, review, and sign-off."

------------------------------------------------------------------------

## Appendix B --- How the Wedge Product Works (and Why It Is Hard to Copy)

**1. The Wedge Is Preparation, Not "Bookkeeping Automation"**

PowerModels' wedge product is **accounting preparation to 85% completion**, delivered as a repeatable, accountant-controlled workflow.

This wedge is intentionally chosen because:

- it sits at the highest pain point in firm workflows,

- it is required for every downstream activity (tax, reporting, planning),

- and it is constrained by correctness, not UI or ingestion speed.

Unlike bookkeeping automation tools, PowerModels does not attempt to:

- classify everything automatically,

- hide uncertainty,

- or push responsibility back to the business.

Instead, it formalizes preparation as a **provable state**.

------------------------------------------------------------------------

**2. How the Wedge Product Actually Works (Step-by-Step)**

At a mechanical level, the wedge product works as follows:

1.  **Ingest Everything**

    - All bank and credit card transactions

    - No filtering, no pre-judgment

2.  **Prove Completeness**

    - Every journal entry reconciled to a source line item

    - Missing, duplicated, or unmatched items surfaced explicitly

3.  **Apply Deterministic Structure**

    - Rule-based categorization where possible

    - Industry and firm defaults applied consistently

4.  **Surface Uncertainty**

    - Anything not confidently categorized is routed to a Review Account

    - Uncertainty is explicit, not hidden

5.  **Maintain Persistent Schedules**

    - Depreciation, prepaids, deferred revenue

    - Updated incrementally, not rebuilt each period

6.  **Enable Explicit Adjustments**

    - Manual and recurring journal entries

    - Fully auditable, reversible, preserved as history

7.  **Produce Review-Ready Working Papers**

    - General Journal

    - Trial Balance

    - Financial statements

This sequence results in a **review-ready but not finalized** accounting state --- the 85% boundary.

------------------------------------------------------------------------

**3. Why This Wedge Is Hard to Copy (The Real Reasons)**

This wedge is not hard to copy because it is "advanced."\
It is hard to copy because it **forces uncomfortable structural decisions**.

**3.1 It Requires Owning the Accounting Record**

Most tools:

- sit on top of QuickBooks or Xero,

- operate on exports,

- or manage documents, not books.

PowerModels:

- owns the accounting record itself,

- preserves history as it evolves,

- and carries ongoing accounting accountability.

This is a responsibility most vendors avoid --- and the source of defensibility when paired with a clear professional boundary

------------------------------------------------------------------------

**3.2 It Requires Making Incompleteness Visible**

Most systems are designed to:

- look "finished,"

- minimize exceptions,

- and avoid surfacing uncertainty.

PowerModels:

- explicitly shows what is missing,

- routes unresolved items to review,

- and proves completeness.

Surfacing incompleteness **slows sales** but builds trust.\
Most competitors optimize for the opposite.

------------------------------------------------------------------------

**3.3 It Requires Separating Determinism from Judgment**

Most automation tools blur the line between:

- what can be automated,

- and what requires professional judgment.

PowerModels enforces a hard separation:

- deterministic work is automated,

- judgment is preserved, visible, and owned by professionals.

This makes the product harder to demo casually --- and harder to fake.

------------------------------------------------------------------------

**3.4 It Requires an Event-Sourced Foundation**

Snapshot-based systems cannot:

- preserve evolving financial history,

- explain how numbers changed,

- or reconcile expectations vs outcomes cleanly.

Because the wedge depends on:

- explainability,

- auditability,

- and persistent schedules,

it **requires** an event-sourced, append-only core (Appendix B).

This is not a refactor-friendly change for incumbents.

------------------------------------------------------------------------

**3.5 It Requires Saying "No" to Scope Expansion**

The 85% boundary:

- prevents services creep,

- blocks premature automation,

- and keeps liability contained.

Most competitors expand scope to grow revenue faster.

PowerModels' wedge **constrains scope by design**, which is culturally and economically hard to replicate.

------------------------------------------------------------------------

**4. Why Incumbents Can't "Just Add This"**

For incumbents, copying the wedge would require:

- re-architecting their data model,

- surfacing incompleteness they currently hide,

- shifting ownership from SMBs to firms,

- accepting new accounting liability,

- and slowing down feature-driven growth.

This is not a feature gap.\
It is a **business model and architecture conflict**.

------------------------------------------------------------------------

**5. Why the Wedge Expands Naturally (But Safely)**

Because the wedge establishes:

- a complete, reconciled financial record,

- explicit expectations vs actuals,

- and preserved history,

it becomes the foundation for:

- pro forma planning,

- renewals and ARR modeling,

- cash forecasting,

- portfolio-level reporting.

Expansion does not require:

- re-ingestion,

- re-selling,

- or redefining responsibility.

It reuses the same record.

------------------------------------------------------------------------

**6. Summary**

PowerModels' wedge works because it formalizes preparation as a provable state and enforces a hard boundary between automation and judgment.

It is hard to copy because it requires:

- owning the accounting record,

- surfacing uncertainty,

- accepting liability,

- and resisting scope creep.

Most vendors choose not to make these tradeoffs.

##  Appendix C --- Platform Architecture

**Purpose**\
Explain how PowerModels is built, why this architecture is required to support the wedge product, and why it creates durable defensibility --- without assuming technical expertise.

This appendix exists to:

- support Appendix A (Product Definition),

- enable Appendix B (Wedge Product),

- and prevent architectural drift as the product expands.

------------------------------------------------------------------------

**C.1 Architectural Intent**

PowerModels is built on an event-sourced, append-only accounting architecture with derived relational read models.

This architecture was chosen deliberately --- not for novelty --- because it is the only practical way to support:

- provable completeness,

- explicit handling of uncertainty,

- persistent accounting schedules,

- explainable financial history,

- and future modeling without reconstruction.

The architecture exists to **enforce** the 85% completion boundary, not to bypass it.

![](./media/image4.png){width="5.534983595800525in" height="4.9833245844269465in"}

------------------------------------------------------------------------

**C.2 Event-Sourced Accounting Core**

In PowerModels, every meaningful accounting action is recorded as an event.

Examples include:

- transaction ingestion,

- reconciliation confirmation,

- categorization decisions,

- adjusting entries,

- schedule updates,

- workflow and review steps.

Each event is:

- time-stamped,

- preserved permanently,

- and never overwritten.

Events record **what happened**, not just the latest state.

------------------------------------------------------------------------

**C.3 Preserved History and Refreshed Balances**

PowerModels preserves a complete history of what happened in the books and updates totals based on that history.

When something changes or is corrected, PowerModels records the correction explicitly rather than rewriting or erasing prior activity. Account balances and reports are then refreshed based on the full history of events.

This mirrors how accountants already work in practice:\
you do not delete journal entries --- you post adjustments.

As a result:

- the path from one number to another is visible,

- changes can be explained,

- and prior states can be revisited without reconstruction.

History is preserved. Totals are refreshed.

------------------------------------------------------------------------

**C.4 Why This Matters for Review, Reconciliation, and Trust**

Most accounting systems overwrite prior states or hide how numbers changed, which is why reconciliation becomes a repair process and reports drift over time.

PowerModels avoids this by keeping every step explicit. Review decisions, adjustments, and corrections remain visible rather than being absorbed silently into balances.

This makes reconciliation a completeness check rather than a cleanup exercise and allows reviewers to understand not just the final number, but how it was produced.

Because reports and schedules are always derived from the same preserved history, accounting, reporting, and analysis remain consistent as data changes.

**C.5 Relational Read Models**

While the event store is the system of record, PowerModels maintains relational read models for usability and performance.

Read models:

- are derived consistently from the same underlying history (deterministically) from events,

- are optimized for reporting and queries,

- can be rebuilt at any time,

- and never become the source of truth.

Examples include:

- Trial Balance,

- Balance Sheet and P&L,

- task and review dashboards,

- portfolio-level aggregations.

------------------------------------------------------------------------

**C.6 Determinism vs Professional Judgment**

The architecture explicitly separates:

- **Deterministic work** (automated, repeatable, auditable)\
  such as ingestion, reconciliation checks, schedule math, and recurring entries

- **Judgment-based work** (contextual, interpretive, professionally owned)\
  such as review resolution, adjusting entries, classification decisions, and sign-off

PowerModels automates the former and surfaces the latter explicitly.\
Judgment is preserved --- not hidden or replaced.

------------------------------------------------------------------------

**C.7 Model Master & Excel-Based Professional Interface**

PowerModels separates **financial data** from **financial models** by design.

The event-sourced ledger is the system of record. On top of it sits the **Model Master** --- a controlled layer where accounting logic, schedules, and financial models are defined, versioned, and enforced.

The Model Master:

- contains hardened models and schedules,

- defines how events become balances and reports,

- enforces consistency across periods and entities,

- and prevents ad hoc model breakage.

Professionals interact with PowerModels primarily through an **Excel-based interface**.

This is intentional. Excel is already the dominant tool for accounting review and analysis. PowerModels preserves this familiarity while removing Excel's fragility.

Through Excel, professionals can:

- review transactions and schedules,

- propose adjustments,

- trace calculations,

- and work within locked, reusable models.

All actions are logged, auditable, reversible, and tied to event history.\
Models can be locked at the firm level while data continues to refresh beneath them.

------------------------------------------------------------------------

**C.8 Persistent Schedules**

Accounting schedules (depreciation, prepaids, deferred revenue) are treated as first-class objects.

They:

- persist across periods,

- update incrementally,

- and remain tied to explicit events.

Schedules are not rebuilt each period.\
They evolve as reality evolves.

**C.9 Multi-Entity & Portfolio Support**

Because all entities share the same event model, PowerModels can:

- support multiple clients per firm,

- aggregate across portfolios,

- enforce standardized definitions,

- and apply role-based access.

Portfolio views are derived from shared events --- not duplicated data.

------------------------------------------------------------------------

**C.10 Data Master & Access Control**

PowerModels enforces privacy and governance through a **Data Master architecture**.

Each firm operates within a **Firm Data Master**, defining:

- firm-level models and standards,

- client relationships,

- access permissions,

- and visibility boundaries.

Each client operates within a **Client Data Master**, containing:

- that client's financial events,

- derived balances and reports,

- client-specific metadata.

Client data is logically isolated.

------------------------------------------------------------------------

**C.10.1 Three-Key Access Model**

Access to client data requires approval from:

1.  the client,

2.  the firm,

3.  and PowerModels (platform enforcement).

All three must be present.\
No single party can bypass controls.

This ensures consent, accountability, and auditability.

------------------------------------------------------------------------

**C.11 Explicit Exclusions**

This architecture does not:

- execute payments,

- initiate collections,

- submit filings,

- replace ERP systems,

- or remove professional accountability.

It supports preparation, modeling, and explanation only.

------------------------------------------------------------------------

**C.12 Why This Is Hard to Copy**

Replicating this architecture would require a competitor to:

- re-architect their core data model,

- stop overwriting accounting history,

- surface incompleteness,

- accept accounting responsibility,

- and redesign workflows around review and judgment.

This is not a feature gap.\
It is a structural and business-model conflict.

**C.13 Summary**

PowerModels' architecture exists to make accounting preparation:

- provable,

- explainable,

- reviewable,

- and reusable.

It is a deliberate foundation for the wedge product --- not a generic platform.

## Appendix D --- AI Strategy & Controlled Automation

**Purpose**\
Explain how PowerModels uses AI to accelerate preparation, review, and analysis without compromising accounting integrity, auditability, or professional accountability.

This appendix exists to:

- clarify PowerModels' position on AI,

- distinguish controlled automation from AI-driven accounting,

- and prevent architectural or narrative drift as AI capabilities expand.

Appendix D is explicitly subordinate to Appendix C (Platform Architecture) and Appendix A (Product Definition).

------------------------------------------------------------------------

**D.1 Guiding Principle**

PowerModels is **not an AI accounting system**.

It is a deterministic accounting system with **AI-assisted workflows**.

Deterministic accounting means numbers change only because an explicit accounting event occurred --- not because the system inferred or guessed differently.

AI is used to assist, orchestrate, and explain --- not to decide, execute, or replace professional judgment. Accounting correctness, review responsibility, and accountability always remain with professionals and within the boundaries defined in Appendix A and Appendix C.

------------------------------------------------------------------------

**D.2 AI Is Downstream of the Accounting Architecture**

AI in PowerModels is **downstream of the accounting architecture, not parallel to it**.

Appendix C establishes the foundations:

- a preserved financial history through event sourcing,

- controlled interpretation via the Model Master,

- professional interfaces through Excel,

- and explicit governance through the Data Master.

AI operates strictly within these constraints.

It:

- never invents data,

- never overwrites history,

- never bypasses professional judgment,

- and never becomes a system of record.

All AI-assisted actions are bounded, explainable, auditable, and reversible.

This design eliminates hallucinations, prevents over-dependence on probabilistic outputs, and ensures complete transparency. AI accelerates professional work, but accounting truth remains deterministic and accountable.

This design is intentional. It keeps AI from conflicting with the accounting system, preserves trust with professionals, and avoids the risks created by AI-first shortcuts.

------------------------------------------------------------------------

**D.3 Three-Level AI Architecture**

PowerModels' AI capabilities are organized into three distinct layers, each with a clearly defined role and boundary. These layers operate **on top of** the event-sourced accounting core and never bypass it.

![](./media/image5.png){width="5.822076771653543in" height="3.898484251968504in"}

Agentic AI operates only through explicitly defined system functions and model constraints, ensuring that all actions are deterministic, auditable, and reversible, and that no accounting logic is generated or modified by AI.

------------------------------------------------------------------------

**Level 1 --- Machine Learning (Data Interpretation)**

This layer focuses on interpreting messy, real-world inputs.

Examples include:

- transaction classification,

- vendor and counterparty recognition,

- pattern detection,

- anomaly identification,

- matching related transactions.

Characteristics:

- probabilistic,

- assistive,

- non-authoritative.

Outputs from this layer are suggestions or signals. They do not create accounting truth and do not bypass review.

------------------------------------------------------------------------

**Level 2 --- Agentic AI (Controlled Execution)**

This layer coordinates deterministic accounting workflows using predefined system functions and rules.

Agentic AI:

- invokes only approved functions,

- operates within the Model Master,

- records all actions as explicit events,

- produces auditable and reversible outputs.

Examples include:

- proposing adjusting entries,

- advancing workflow states,

- generating draft schedules based on explicit rules.

Agentic AI **cannot**:

- invent accounting logic,

- finalize judgment-based decisions,

- bypass professional review,

- or operate outside enforced boundaries.

------------------------------------------------------------------------

**Level 3 --- LLM Interface (Interaction & Explanation)**

This layer provides a natural-language interface for users.

It:

- routes user intent to permitted system actions,

- explains calculations and outcomes,

- summarizes changes and variances,

- assists navigation and analysis.

LLMs do **not**:

- compute balances,

- create accounting entries directly,

- store financial data,

- or serve as a source of truth.

They explain and orchestrate --- they do not calculate.

------------------------------------------------------------------------

**\**

**D.4 Why AI Works in PowerModels (and Often Fails Elsewhere)**

AI succeeds in PowerModels because it operates on:

- a complete financial record,

- preserved event history,

- deterministic accounting models,

- and explicit separation between automation and judgment.

Most accounting systems apply AI to fragmented snapshots, overwritten balances, and incomplete context, forcing AI to infer missing information.

PowerModels gives AI full context --- without giving it control.

------------------------------------------------------------------------

**D.5 Explicit AI Exclusions**

PowerModels does **not**:

- allow AI to make accounting judgments,

- auto-finalize preparation beyond the 85% boundary,

- perform filing or execution,

- override professional review,

- generate numbers outside deterministic models.

Any AI capability that violates these constraints requires explicit leadership approval and revision of Appendices A--D.

------------------------------------------------------------------------

**D.6 Long-Term Role of AI**

As the financial record deepens, AI becomes more useful --- not more powerful.

Its value increases through:

- better explanations,

- faster analysis,

- improved exception handling,

- richer insights derived from consistent history.

AI scales **with trust**, not ahead of it.

------------------------------------------------------------------------

**D.7 Summary**

PowerModels uses AI to:

- reduce mechanical effort,

- improve clarity,

- and accelerate professional work.

It does not use AI to:

- replace judgment,

- obscure accountability,

- or compromise correctness.

AI is a multiplier --- not the foundation.

The foundation remains the accounting record.

## Appendix E --- Accounting & Data Definitions

**Purpose:** Establish a shared, precise vocabulary for accounting, data, and workflow concepts used throughout PowerModels.

This appendix assumes no prior accounting or systems expertise. It exists to prevent:

- misunderstanding,

- scope creep,

- and misinterpretation of PowerModels' role and responsibilities.

------------------------------------------------------------------------

**E.1 Why These Definitions Matter**

Accounting systems often fail not because calculations are wrong, but because:

- terminology is overloaded,

- assumptions are implicit,

- and responsibility boundaries are unclear.

PowerModels is built on explicit, enforced definitions to ensure:

- professional accountability,

- auditability,

- and disciplined product scope.

These definitions are authoritative and are not intended to drift.

------------------------------------------------------------------------

**E.2 Double-Entry Accounting (Plain Definition)**

Double-entry accounting means that every financial event affects at least two accounts:

- one debit,

- one credit.

The purpose is internal consistency, not complexity.

If debits do not equal credits:

- something is missing,

- duplicated,

- or misclassified.

PowerModels preserves double-entry accounting because it:

- exposes errors,

- enables reconciliation,

- and supports audit defensibility.

------------------------------------------------------------------------

**E.3 Journal Entries**

A journal entry is the atomic unit of accounting change.

Each journal entry:

- records a specific event,

- includes debits and credits,

- is time-stamped,

- and is preserved permanently.

In PowerModels:

- journal entries originate from ingestion, reconciliation, adjustments, or schedules,

- and are never silently overwritten.

------------------------------------------------------------------------

**E.4 General Ledger (GL)**

The General Ledger is the complete collection of all journal entries for an entity.

It represents:

- the full accounting history,

- not just current balances.

In PowerModels:

- the GL is derived from preserved events,

- not from manually editable balances.

------------------------------------------------------------------------

**E.5 Trial Balance**

A Trial Balance is a summary view of the General Ledger at a point in time.

It:

- lists all accounts,

- shows debit or credit balances,

- must mathematically balance.

The Trial Balance is the primary review surface for accountants because it:

- surfaces misclassifications,

- exposes unusual balances,

- validates completeness.

In PowerModels, Trial Balance completion is an explicit, trackable step.

------------------------------------------------------------------------

**E.6 Reconciliation (Critical Concept)**

Reconciliation means proving that:

- every accounting entry corresponds to a real-world source.

Examples include:

- bank statement lines,

- credit card charges,

- known accruals or adjustments.

Reconciliation is proof of completeness, not estimation.

In PowerModels:

- unreconciled items are explicitly flagged,

- reconciliation is task-driven,

- completion is visible and auditable.

------------------------------------------------------------------------

**E.7 Review Accounts (Explicit Uncertainty)**

A Review Account is used when a transaction:

- cannot be confidently categorized,

- requires client clarification,

- or needs professional judgment.

Review Accounts:

- preserve uncertainty instead of hiding it,

- prevent forced automation,

- signal where judgment is required.

This is a deliberate design choice.

------------------------------------------------------------------------

**E.8 Adjusting Entries**

Adjusting entries are journal entries made by accountants to:

- correct classifications,

- apply accruals,

- recognize depreciation or deferrals,

- reflect professional judgment.

Adjustments are:

- explicit,

- auditable,

- preserved as part of history.

PowerModels never auto-applies judgmental adjustments.

------------------------------------------------------------------------

**E.9 Accounting Schedules**

An accounting schedule represents how certain balances change over time.

Examples:

- depreciation schedules,

- prepaid expense schedules,

- deferred revenue schedules.

In PowerModels:

- schedules are persistent,

- updated incrementally,

- directly tied to events.

They are not rebuilt each period.

------------------------------------------------------------------------

**E.10 Deterministic vs Judgment-Based Work**

PowerModels enforces a strict separation between:

**Deterministic Work**

- rule-based,

- repeatable,

- verifiable.

Examples:

- ingestion,

- reconciliation checks,

- schedule calculations,

- recurring entries.

**Judgment-Based Work**

- interpretive,

- contextual,

- professionally owned.

Examples:

- resolving review accounts,

- classification decisions,

- adjusting entries,

- final review.

PowerModels automates deterministic work and surfaces judgment explicitly.

------------------------------------------------------------------------

**E.11 Manual Override (Professional Control)**

Professionals can always override automated outcomes using explicit, auditable actions.

In PowerModels:

- overrides are always permitted,

- overrides are recorded as new events,

- overrides never erase history.

This preserves:

- professional judgment,

- accountability,

- auditability.

Automation operates under professional control, not instead of it.

------------------------------------------------------------------------

**E.12 Degree of Automation (Clarified)**

Degree of automation describes how much of a workflow can be executed deterministically.

Examples of high automation:

- ingestion,

- reconciliation math,

- schedule calculations,

- recurring entries.

Examples requiring judgment:

- ambiguous classifications,

- unusual transactions,

- adjustments,

- final review.

PowerModels is designed to maximize automation where safe, while preserving professional control where required.

------------------------------------------------------------------------

**E.13 Actuals vs Pro Forma**

**Actuals** represent events that have occurred and are recorded in the General Ledger.

**Pro forma** items represent:

- planned,

- expected,

- or hypothetical events.

In PowerModels:

- pro forma items are explicitly labeled,

- never silently converted,

- require explicit approval to become actuals.

This prevents forecast drift and hindsight bias.

------------------------------------------------------------------------

**E.14 Event-Sourced Accounting Record**

An event-sourced record means:

- every change is recorded as a discrete event,

- history is preserved,

- current state is derived, not overwritten.

This differs from snapshot-based systems that overwrite balances.

Event sourcing enables:

- explainability,

- audit trails,

- future modeling without reconstruction.

------------------------------------------------------------------------

**E.15 Append-Only (Clarified)**

Append-only means:

- no data is deleted or overwritten,

- corrections are additive,

- prior history remains intact.

Corrections are visible and traceable.

------------------------------------------------------------------------

**E.16 Completeness vs Accuracy**

PowerModels distinguishes between:

- **Completeness** → Have all expected items been accounted for?

- **Accuracy** → Are classifications and judgments correct?

The 85% completion boundary prioritizes completeness first, with accuracy finalized through professional review.

------------------------------------------------------------------------

**E.17 What PowerModels Explicitly Does Not Do**

PowerModels does not:

- file tax returns,

- submit data to authorities,

- initiate payments,

- replace professional judgment,

- execute operational workflows.

These boundaries are intentional and enforced.

------------------------------------------------------------------------

**E.18 Summary**

PowerModels formalizes accounting preparation by:

- making completeness provable,

- surfacing uncertainty,

- preserving judgment,

- maintaining explainable financial history.

These definitions underpin:

- the wedge product,

- the platform architecture,

- and the long-term vision.

------------------------------------------------------------------------

**\**

## Appendix F --- Product Expansion Ladder

**Purpose:** Define the ordered, constrained path by which PowerModels expands beyond the wedge product without violating trust, scope, or execution discipline.

This appendix exists to:

- prevent feature-driven roadmap drift,

- anchor expansion to proven preparation states,

- ensure each expansion builds on the same financial record.

------------------------------------------------------------------------

**F.1 Governing Principle**

PowerModels expands **vertically through preparation maturity**, not horizontally through feature accumulation.

Every rung on the expansion ladder must:

1.  reuse the same event-sourced financial record,

2.  reinforce the 85% completion boundary,

3.  reduce downstream preparation and rework,

4.  preserve professional judgment and accountability,

5.  avoid operational execution.

If a capability does not meet **all five criteria**, it does not belong on the ladder.

------------------------------------------------------------------------

**F.2 Ladder Overview**

The expansion ladder progresses through five disciplined stages:

1.  **Accounting Preparation (Wedge)**

2.  **Persistent Accounting Models**

3.  **Expectation vs Actual Intelligence**

4.  **Portfolio & Multi-Entity Insight**

5.  **Forward-Looking Financial Planning**

Each stage is dependent on the prior stage and cannot be skipped.

------------------------------------------------------------------------

**F.3 Stage 1 --- Accounting Preparation (Wedge Product)**

This stage delivers:

- 100% transaction ingestion (or ingestion from existing accounting systems),

- reconciliation for completeness,

- explicit uncertainty via Review Accounts,

- persistent accounting schedules,

- review-ready working papers.

For multi-entity organizations, PowerModels may ingest data from existing systems of record. In these cases, PowerModels reconstructs the general ledger within its own accounting record to ensure consistency, auditability, and standardized reporting across entities.

This stage establishes:

- data integrity,

- professional accountability,

- trust in outputs.

Nothing above this stage is viable without it.

------------------------------------------------------------------------

**F.4 Stage 2 --- Persistent Accounting Models**

**What expands**

- Accounting schedules (depreciation, prepaids, deferred revenue) become long-lived objects.

- Adjustments persist across periods.

- Models update incrementally rather than being rebuilt.

**What this unlocks**

- Clean carry-forward of assumptions.

- Reduced rework each period.

- True comparability across time.

**What it explicitly does NOT do**

- No execution

- No advisory

- No filing

This stage converts preparation from a periodic task into a continuous discipline.

------------------------------------------------------------------------

**F.5 Stage 3 --- Expectation vs Actual Intelligence**

**What expands**

- Pro forma items are introduced as explicit planning objects.

- Expected vs actual variances are surfaced natively.

- Timing assumptions, renewals, and billings are tracked.

**Examples**

- ARR vs realized revenue

- Deferred revenue vs billing schedules

- Expected renewals vs actual outcomes

**Why this matters**\
Most accounting systems can report history.\
Very few can explain *why* outcomes diverged from expectations.

PowerModels can --- because expectations live in the same financial record as actuals.

------------------------------------------------------------------------

**F.6 Stage 4 --- Portfolio & Multi-Entity Insight**

**What expands**

- Multiple entities share the same event model.

- Aggregation occurs without re-ingestion.

- Role-based and multi-party access is enforced.

**Who this serves**

- Accounting firms

- Lenders

- PE / VC portfolios

- Multi-entity operators

**What this enables**

- Standardized KPIs across entities

- Portfolio-level visibility

- Oversight without bespoke reporting

**What it does NOT become**

- Not a data warehouse

- Not an operational control system

This stage increases stickiness without increasing complexity.

------------------------------------------------------------------------

**F.7 Stage 5 --- Forward-Looking Financial Planning**

**What expands**

- Cash forecasting

- Scenario modeling

- Sensitivity analysis

- Pro forma cash and revenue timelines

- Financially grounded operational reporting that explains outcomes without owning execution

**What it explicitly does NOT do**

- No operational execution

- No advisory decision-making

**Why this stage comes last**\
Forecasting only works when:

- history is complete,

- schedules are correct,

- expectations are explicit.

Most tools start here and fail.\
PowerModels earns the right to do this last.

------------------------------------------------------------------------

**F.8 What the Ladder Explicitly Excludes**

At every stage, PowerModels does not:

- execute payments,

- initiate collections,

- manage logistics,

- replace ERP systems,

- automate judgment,

- own or execute operational workflows.

Operational signals may be incorporated **only** to explain financial outcomes --- never to control operations.

Expansion occurs through **new models**, not new responsibilities.

------------------------------------------------------------------------

**F.9 Why This Ladder Is Hard to Copy**

To replicate this ladder, a competitor must:

- own the accounting record,

- preserve a complete, time-ordered event history rather than overwriting balances,

- surface incompleteness and uncertainty explicitly,

- accept professional accountability rather than hiding behind automation,

- maintain comprehensive, reconciled data across entities,

- support direct Excel-based review, analysis, and controlled adjustment,

- resist scope-driven growth pressure that compromises preparation discipline.

Most vendors fail on at least one of these.

------------------------------------------------------------------------

**F.10 Summary**

PowerModels expands by **deepening trust in the same financial record**, not by broadening feature surface area.

Each rung:

- reinforces the wedge,

- compounds value,

- increases defensibility.

This ladder defines how PowerModels grows **without breaking itself**.

------------------------------------------------------------------------

**\**

## Appendix G --- What PowerModels Is Not Building (Strategic Constraints)

**Purpose**\
Signal focus, restraint, and architectural discipline.\
This appendix exists to make clear what PowerModels will deliberately not build, even when there is market pressure to do so.\
These constraints are strategic, not temporary gaps.

------------------------------------------------------------------------

**G.1 Why Strategic Constraints Matter**\
Many platforms fail not because they lack opportunity, but because they:

- chase adjacent revenue too early,

- blur responsibility boundaries,

- accumulate execution and liability risk.

PowerModels is intentionally constrained to:

- protect professional trust,

- preserve product clarity,

- and scale without becoming services-heavy.

What we do not build is as important as what we do.

------------------------------------------------------------------------

**G.2 No SMB Self-Serve Accounting**\
PowerModels is not building:

- a self-serve accounting product for small businesses,

- a QuickBooks alternative for owners,

- or a DIY bookkeeping experience.

Why:

- SMB self-serve incentives conflict with firm-led workflows.

- Cleanup responsibility shifts back to professionals anyway.

- UX simplicity often comes at the cost of accounting rigor.

PowerModels is accountant-hosted by design.

**G.3 No Payroll (Partnered or Deferred)**\
PowerModels will not build:

- native payroll,

- payroll tax filing,

- or payroll execution.

Why:

- Payroll is execution-heavy and compliance-sensitive.

- It introduces jurisdictional complexity early.

- It distracts from preparation and modeling.

Payroll data may be ingested or referenced, but execution remains external.

------------------------------------------------------------------------

**G.4 No Inventory Systems (Initially or Lightly)**\
PowerModels will not build:

- inventory management systems,

- fulfillment workflows,

- or operational inventory control.

Clarification:

- Financial modeling of inventory impact is allowed.

- Cost recognition and working capital analysis are allowed.

- Operational inventory execution is not.

This constraint prevents ERP creep.

------------------------------------------------------------------------

**G.5 No Services-Heavy Delivery Model**\
PowerModels will not become:

- a managed bookkeeping service,

- an outsourced accounting provider,

- or a services-first business.

Why:

- Services scale linearly.

- Margins erode.

- Product discipline weakens.

PowerModels sells infrastructure and leverage, not labor.

------------------------------------------------------------------------

**G.6 No Replacement of Professional Judgment**\
PowerModels will not:

- automate judgmental decisions,

- select tax positions,

- finalize classifications without review,

- or remove accountability from professionals.

Automation is applied to deterministic work only.\
Judgment remains explicit, visible, and owned by the accountant.

------------------------------------------------------------------------

**G.7 Relationship to the Expansion Ladder**\
These constraints:

- define the outer boundary of the Product Expansion Ladder,

- prevent expansion from drifting into execution,

- ensure every new capability remains model-driven.

If a proposed feature violates this appendix, it does not ship.

------------------------------------------------------------------------

**G.8 Summary**\
PowerModels grows by:

- deepening preparation,

- expanding modeling,

- increasing insight density.

It does not grow by:

- absorbing execution,

- replacing professionals,

- chasing adjacent revenue prematurely.

These constraints are intentional and enduring.

------------------------------------------------------------------------

**\**

## Appendix H --- Client Ramp & Revenue Sensitivity (Per Firm)

**Purpose**\
PowerModels' revenue scales primarily through within-firm expansion (land → expand → standardize), not through per-client selling. This appendix models how a single accounting firm ramps client usage over time and how that translates into recurring revenue under conservative, transparent, and auditable assumptions.

This appendix is designed to be pressure-testable, not promotional.

------------------------------------------------------------------------

**H.1 Governing Principle**

The long-run revenue potential of a firm is fixed by price and client capacity.\
What varies is **time-to-expansion**, not steady-state ARR.

Faster adoption accelerates when revenue is realized.\
It does not increase the ultimate revenue per firm.

------------------------------------------------------------------------

**H.2 Locked Inputs (Applies to All Scenarios)**

The following inputs are fixed across all scenarios:

- **Pricing:** \$100 per client per month

- **Revenue per client per year:** \$1,200

- **Steady-state client cap:** 75 clients per firm

- **Steady-state ARR per firm:** \$90,000

- Initial internal rollout: 5 clients per firm at the start of active usage (assumes firms begin with a small, high-pain subset of clients once onboarding is complete)

- **Expansion mechanism:** additional clients added monthly by the firm\
  (no per-client selling by PowerModels)

Clarification:

This appendix models client expansion \*within a single firm\* after that firm has completed onboarding.

It does not model the timing of firm acquisition or beta intake, which is addressed in the operating plan.

**Definition note**\
"Clients" refers to end business entities managed under a single accounting firm.

**Constraint note**\
These scenarios assume a firm has completed its initial onboarding cycle and is operating independently. Expansion dynamics are modeled from that point forward. Firm acquisition and onboarding throughput are managed as separate, deliberate stages and are addressed explicitly elsewhere in the plan.

------------------------------------------------------------------------

**H.3 Scenario A --- Conservative Adoption (+2 Clients per Month)**

**Assumption**\
After the first quarter of active usage, the firm adds 2 additional clients per month until reaching 75.

**Client Ramp (Per Firm)**

  -----------------------------------
  **Timepoint**         **Clients**
  --------------------- -------------
  End of Month 3 (Q1)   5

  End of Month 6        11

  End of Month 9        17

  End of Month 12       23

  End of Month 18       35

  End of Month 24       47

  End of Month 30       59

  End of Month 36       75
  -----------------------------------

**ARR at Key Points**

  ----------------------------------------
  **Timepoint**   **Clients**   **ARR**
  --------------- ------------- ----------
  End of Year 1   23            \$27,600

  End of Year 2   47            \$56,400

  Steady State    75            \$90,000
  ----------------------------------------

**Interpretation**\
Represents cautious firms with slower internal change management and gradual workflow standardization.

------------------------------------------------------------------------

**H.4 Scenario B --- Moderate Adoption (+4 Clients per Month)**

**Assumption**\
After the first quarter of active usage, the firm adds 4 additional clients per month until reaching 75.

**Client Ramp (Per Firm)**

  -----------------------------------
  **Timepoint**         **Clients**
  --------------------- -------------
  End of Month 3 (Q1)   5

  End of Month 6        17

  End of Month 9        29

  End of Month 12       41

  End of Month 15       53

  End of Month 18       65

  End of Month 20       75
  -----------------------------------

**ARR at Key Points**

  ----------------------------------------
  **Timepoint**   **Clients**   **ARR**
  --------------- ------------- ----------
  End of Year 1   41            \$49,200

  End of Year 2   75            \$90,000
  ----------------------------------------

**Interpretation**\
Represents strong internal adoption constrained mainly by implementation pacing.

------------------------------------------------------------------------

**H.5 Scenario C --- Base Case Adoption (+5 Clients per Month)**

**Assumption**\
After the first quarter of active usage, the firm adds 5 additional clients per month until reaching 75.

**Client Ramp (Per Firm)**

  -----------------------------------
  **Timepoint**         **Clients**
  --------------------- -------------
  End of Month 3 (Q1)   5

  End of Month 6        20

  End of Month 9        35

  End of Month 12       50

  End of Month 15       65

  End of Month 18       75
  -----------------------------------

**ARR at Key Points**

  ----------------------------------------
  **Timepoint**   **Clients**   **ARR**
  --------------- ------------- ----------
  End of Year 1   50            \$60,000

  End of Year 2   75            \$90,000
  ----------------------------------------

**Interpretation**\
Represents firms that commit to PowerModels as default preparation infrastructure for a defined client segment.

------------------------------------------------------------------------

**H.6 Cross-Scenario Summary (Per Firm)**

  -----------------------------------------------------------------------------------------------
  **Scenario**           **Clients @ Month 12**   **Time to 75 Clients**   **Steady-State ARR**
  ---------------------- ------------------------ ------------------------ ----------------------
  Conservative (+2/mo)   23                       \~36 months              \$90,000

  Moderate (+4/mo)       41                       \~20 months              \$90,000

  Base Case (+5/mo)      50                       \~18 months              \$90,000
  -----------------------------------------------------------------------------------------------

**Key Insight**\
Steady-state ARR per firm is identical across scenarios.\
The only variable is **speed to expansion**, which affects:

- cash timing

- payback period

- funding requirements

---not long-term value.

------------------------------------------------------------------------

**H.7 What This Appendix Explicitly Does Not Assume**

This model does not assume:

- per-client selling by PowerModels

- price increases

- cross-selling of future products

- unlimited onboarding capacity

- full firm-wide adoption

Upside exists, but is intentionally excluded here.

------------------------------------------------------------------------

**H.8 Summary**

PowerModels' unit economics are driven by:

- firm-level trust,

- internal expansion,

- and standardization over time.

This appendix demonstrates that the business works under conservative assumptions, with revenue expansion governed by execution pacing rather than market demand.

------------------------------------------------------------------------

**\**

## Appendix I --- Onboarding Capacity Overlay

**Purpose**

Overlay human, process, and system capacity constraints onto the firm ramp and revenue model to ensure projections remain executable.

This appendix exists to:

- prevent unconstrained firm onboarding assumptions,

- make scaling limits explicit,

- force tradeoffs between growth rate and service quality.

------------------------------------------------------------------------

**I.1 Governing Principle**

PowerModels is not self-serve and not services-heavy, but onboarding still requires:

- structured setup,

- firm enablement,

- early-cycle support.

Therefore:

- onboarding capacity is a hard constraint, not a soft assumption,

- growth must be paced to preserve time-to-value and trust.

------------------------------------------------------------------------

**I.2 What "Onboarding" Actually Includes**

Onboarding a firm is not just account creation. It includes:

**1. Firm Setup**

- Chart of Accounts alignment

- Industry defaults selection

- Role and permission configuration

**2. Initial Client Setup**

- Selection of first high-pain clients (typically 5)

- Bank and card data source mapping

- Validation of completeness expectations

**3. Workflow Enablement**

- Reconciliation process walkthrough

- Review Account usage

- Adjustment and task workflows

**4. First-Cycle Support**

- Oversight during first reconciliation

- Adjustment review support

- Confirmation of review-ready outputs

The goal of onboarding is **self-sufficiency**, not dependence.

------------------------------------------------------------------------

**I.3 Capacity Units (What Is Actually Measured)**

Capacity is measured in **firms onboarded per month**, not clients.

Clients scale *inside* firms after onboarding is complete.

Key capacity units:

- firms onboarded per month,

- concurrent onboarding firms,

- support hours per firm during the first cycle.

------------------------------------------------------------------------

**I.4 Initial Capacity Assumptions (Pre-Seed / Early Seed)**

These assumptions are intentionally conservative.

**Team Configuration**

- Core onboarding handled by:

  - Product / Operations

  - Modeling support

  - Limited engineering assistance (as needed)

**Capacity Assumptions**

- 1--2 firms onboarded per month

- 2--3 active onboarding firms concurrently

- Onboarding duration: \~30--45 days per firm

This constrains:

- logo acquisition rate,

- early revenue realization,

- support load.

------------------------------------------------------------------------

**I.5 Capacity Expansion Levers**

Onboarding capacity expands through **process maturity**, not headcount alone.

Key levers:

- standardized onboarding playbooks,

- industry-specific Chart of Accounts libraries,

- reusable data validation templates,

- improved task automation,

- reduced first-cycle exception rates.

As these mature:

- onboarding time per firm decreases,

- concurrent onboarding increases,

- support intensity drops materially.

------------------------------------------------------------------------

**I.6 Interaction with Client Ramp (Appendix H)**

Critical constraint:

Client ramp *inside a firm* cannot begin meaningfully until onboarding is complete.

Implications:

- fast client expansion assumes successful first-cycle onboarding,

- poor onboarding delays revenue even if sales succeed,

- aggressive firm acquisition without capacity degrades outcomes.

Appendix H scenarios must be interpreted **after** applying this overlay.

------------------------------------------------------------------------

**I.7 What This Overlay Explicitly Prevents**

This appendix prevents assumptions such as:

- unlimited firm onboarding,

- zero-support deployments,

- instant client expansion,

- linear scaling without friction.

If a financial model violates this overlay, it is invalid by definition.

------------------------------------------------------------------------

**I.8 Long-Term Steady-State Capacity (Illustrative)**

With mature processes and tooling:

- 3--5 firms onboarded per month is plausible,

- support per firm drops significantly after the first cycle,

- focus shifts from onboarding to expansion monitoring.

This is an **illustrative future state**, not assumed in early models.

------------------------------------------------------------------------

**I.9 Summary**

Revenue scales through firm expansion, but growth is gated by onboarding capacity.

This overlay ensures:

- realistic pacing,

- preserved trust,

- predictable time-to-value.

Capacity is a feature of the business --- not an afterthought.

------------------------------------------------------------------------

## Appendix J --- SAM / SOM Assumptions & Sensitivity Analysis

**Purpose**

Define the addressable market rigorously, present bottom-up math, and make explicit that PowerModels' growth is governed by execution limits rather than market discovery.

This appendix exists to:

• avoid inflated TAM narratives,\
• anchor projections to realistic firm adoption dynamics,\
• support investor diligence with transparent, auditable assumptions.

------------------------------------------------------------------------

**J.1 Governing Principle**

PowerModels' growth is bounded by execution.

Specifically, outcomes are governed by:

• onboarding capacity (Appendix I),\
• firm-level expansion velocity (Appendix H),\
• and firms' willingness to standardize preparation workflows.

As a result:

• SAM and SOM are built bottom-up,\
• sensitivity is explicit and pressure-testable,\
• upside is unlocked through execution scale, not demand creation.

------------------------------------------------------------------------

**J.2 Market Definition (Who Counts)**

In Scope (Initial ICP)

• Accounting and bookkeeping firms\
• Approximately 5--30 staff (sweet spot: \~8--20)\
• Approximately 50--300 active clients\
• Meaningful share of sole proprietors and micro-businesses\
• Fixed-fee or package pricing models\
• Demonstrated preparation bottlenecks

Out of Scope (Phase 1)

• SMBs as direct buyers\
• Lifestyle firms resistant to standardization\
• Pure hourly billing shops\
• Very large national firms (Phase 2+)\
• DIY tax filers

This definition is intentionally narrow and reflects operational reality, not theoretical reach.

------------------------------------------------------------------------

**J.3 Market Size Inputs (Directional, Order-of-Magnitude)**

The following inputs are directional and conservative. They are intended to establish scale, not precision.

Estimated Small-to-Mid-Size Firm Counts

• United States: \~45,000\
• Canada: \~7,000

Targetable Subset (Fits ICP)

• Approximately 20% of total firms

Target Firms (ICP)

• United States: \~9,000\
• Canada: \~1,400\
• Total ICP firms: \~10,400

These figures reflect firms that plausibly match the operational and economic profile required for adoption.

------------------------------------------------------------------------

**J.4 SAM --- Serviceable Addressable Market**

SAM reflects steady-state penetration across the defined ICP, not year-one reach.

Per-Firm Economics (from Appendix H)

• Steady-state clients per firm: 75\
• Price: \$100 per client per month\
• ARR per firm: \$90,000

SAM Calculation

• 10,400 firms × \$90,000 ARR\
= approximately \$936M SAM

This assumes:

• partial firm penetration rather than full client bases,\
• no price expansion,\
• no additional product lines.

------------------------------------------------------------------------

**J.5 SOM --- Serviceable Obtainable Market (Execution-Bound)**

SOM reflects what is achievable under disciplined execution constraints.

Primary constraints include:

• firm onboarding throughput,\
• firm acquisition pacing,\
• successful expansion within onboarded firms.

Conservative Five-Year Scenario

• Approximately 1,000 firms onboarded cumulatively\
• Roughly 10% of ICP firms\
• Average firm ARR at maturity: \$90,000

SOM ARR

• 1,000 × \$90,000\
= \$90M ARR

This scenario reflects execution scale, not market saturation.

------------------------------------------------------------------------

**J.6 Sensitivity Analysis (Primary Levers)**

SOM is most sensitive to four variables:

1.  Number of Firms Onboarded\
    • +100 firms → +\$9M ARR at maturity\
    • Gated by onboarding capacity (Appendix I)

2.  Clients per Firm\
    • ±10 clients → ±\$12,000 ARR per firm\
    • Strong lever once PowerModels becomes default infrastructure

3.  Price per Client\
    • ±\$10 per month → ±\$9,000 ARR per firm\
    • Pricing power increases with depth of adoption and standardization

4.  Ramp Velocity\
    • Affects cash timing and payback\
    • Does not change steady-state ARR ceilings

------------------------------------------------------------------------

**J.7 Why TAM Is Intentionally De-Emphasized**

PowerModels does not compete in:

• mass SMB accounting software,\
• consumer tax products,\
• self-serve bookkeeping tools.

Those markets are large but characterized by:

• high churn,\
• low trust,\
• misaligned incentives.

PowerModels prioritizes:

• professional buyers,\
• internal rollout dynamics,\
• durable ARR per firm,\
• low churn through workflow integration.

------------------------------------------------------------------------

**J.8 Upside Scenarios (Explicitly Bounded)**

Upside does not come from:

• expanding into SMB direct sales,\
• absorbing execution workflows,\
• relaxing scope or accountability boundaries.

Upside comes from:

• deeper penetration within existing firms,\
• portfolio-level adoption by adjacent buyers,\
• expanded modeling layers built on the same financial record,\
• modest, earned price expansion over time.

These are compounding effects, not speculative leaps.

------------------------------------------------------------------------

**J.9 Summary**

• Market demand is sufficient and persistent.\
• Adoption does not depend on educating the market or changing firm workflows.\
• Revenue outcomes are governed by execution quality, not customer discovery.\
• Once the firm-led motion is proven, growth scales through parallel firm acquisition rather than incremental selling.

PowerModels operates inside a large, existing professional market where compliance-driven preparation work already exists. As execution risk is reduced, growth velocity is determined by staffing decisions, not by demand creation.

## Appendix K --- Go-To-Market Mechanics (Firm-Led)

**Purpose:** Define how PowerModels acquires firms, expands usage, and compounds revenue without relying on SMB funnels, services-heavy delivery, or high-velocity sales models.

This appendix exists to:

- make CAC dynamics explicit,

- prevent SMB-style GTM leakage,

- align sales, onboarding, and expansion into a single, disciplined motion.

------------------------------------------------------------------------

**K.1 Governing GTM Thesis**

PowerModels uses a **firm-led land-and-expand** go-to-market model.

- The economic buyer is the accounting firm.

- The usage unit is the firm's clients.

- Expansion occurs *inside* the firm, not through new logos.

- CAC is incurred once per firm, not per client.

This GTM model works because:

- firms already control client workflows,

- trust already exists between firm and client,

- PowerModels removes internal bottlenecks rather than selling abstract "software value."

------------------------------------------------------------------------

**K.2 Who We Sell To (Explicit Roles)**

**Primary Buyer**

- Firm owner / managing partner

- Accountable for margin, delivery quality, and staffing

**Primary Internal Champions**

- Senior accountants

- Reviewers

- Preparation leads

**Explicit Non-Buyers**

- SMBs directly

- Individual bookkeepers without decision authority

- Lifestyle firms resistant to standardization

If a firm cannot mandate defaults, it is not a target.

------------------------------------------------------------------------

**K.3 Land Motion (Firm Acquisition)**

**Objective**\
Land one firm and prove value quickly using a small, high-pain subset of clients.

**Land Mechanics**

- Direct outreach and referrals

- No SMB marketing

- No self-serve signup

- No free tier

**Initial Deployment**

- \~5 firm-selected clients

- Highest cleanup or preparation friction

- Full support during the first cycle

**Success Criteria**

- Clear reduction in preparation friction

- Visible proof of completeness

- Review-ready outputs within one cycle

The goal is **operational belief**, not feature admiration.

**K.4 Expand Motion (Inside the Firm)**

Expansion is not sales-driven.

Once the wedge proves itself:

- firms roll PowerModels out to similar clients,

- staff request it for new engagements,

- PowerModels becomes default preparation infrastructure.

**Expansion Drivers**

- Reduced rework

- Standardized workflows

- Cleaner reviews

- Fewer tax-season fire drills

This is why **expansion velocity** (Appendix H) matters more than logo count.

------------------------------------------------------------------------

**K.5 Standardize Motion (Default Infrastructure)**

Over time:

- new clients are onboarded directly into PowerModels,

- legacy clients migrate opportunistically,

- PowerModels becomes invisible infrastructure.

At this stage:

- churn drops materially,

- expansion becomes predictable,

- switching costs increase without contractual lock-in.

------------------------------------------------------------------------

**K.6 Why CAC Is Structurally Low**

PowerModels avoids traditional CAC drivers:

- no SMB advertising,

- no content treadmill,

- no per-client sales motion,

- no SMB churn loop.

Instead:

- one sale controls many clients,

- expansion is internal,

- trust is pre-existing.

CAC is measured **per firm**, not per client.

------------------------------------------------------------------------

**K.7 Sales Model (Early vs Scaled)**

**Early Stage**

- Founder-led sales

- High-signal conversations

- Manual ICP enforcement

**Scaled Stage**

- Limited sales headcount

- Focus on qualifying firms, not closing volume

- Onboarding capacity (Appendix I) remains the throttle

This is not a volume sales model.

------------------------------------------------------------------------

**K.8 What the GTM Model Explicitly Avoids**

PowerModels does not:

- sell directly to SMBs,

- rely on marketplaces,

- use freemium conversion,

- compete on lowest price,

- bundle services to force adoption.

These tactics undermine trust, economics, and scalability.

------------------------------------------------------------------------

**K.9 Failure Modes This GTM Prevents**

This GTM design prevents:

- high churn from poor SMB fit,

- sales-driven scope creep,

- services-heavy delivery,

- fragile unit economics.

If growth requires breaking this GTM model, the growth is not worth pursuing.

------------------------------------------------------------------------

**K.10 Summary**

PowerModels grows by:

- selling once to firms,

- expanding quietly inside them,

- becoming default infrastructure rather than a visible tool.

This GTM model is slower at the front, but stronger over time.\
It aligns with:

- the wedge product,

- onboarding capacity,

- expansion economics,

- and long-term defensibility.

## **Appendix L --- Competitive Landscape**

**(Deep Dive & Encroachment Risk)**

**Purpose:** Provide a reality-based competitive assessment that avoids strawman comparisons, clarifies encroachment risks, and explains why the gap PowerModels fills has persisted.

This appendix exists to:

- prevent underestimating incumbents,

- avoid over-claiming differentiation,

- and explain why this layer has remained unbuilt.

------------------------------------------------------------------------

**L.1 How We Define "Competition"**

PowerModels does not compete on:

- UI polish,

- SMB self-serve simplicity,

- lowest-price bookkeeping.

PowerModels competes on:

- preparation leverage,

- standardization under messy reality,

- review efficiency,

- explainability and auditability.

Therefore, the primary competitors are:

- internal labor,

- status quo workflows,

- and tools that break under real-world messiness.

------------------------------------------------------------------------

**L.2 Primary Competitors --- Internal Labor & Status Quo**

**Junior Staff & Offshore Teams**

For most firms, the default response to preparation bottlenecks is:

- hiring juniors,

- outsourcing cleanup,

- adding seasonal labor.

Why this fails structurally:

- scales linearly with cost,

- high rework and review burden,

- fragile quality,

- persistent talent constraints.

PowerModels competes directly against linear labor scaling---not against software SKUs.

------------------------------------------------------------------------

**Excel as the De Facto Preparation Layer**

Excel remains the dominant preparation tool because:

- accounting systems fail under messiness,

- adjustments are easier outside the GL,

- review workflows are informal and flexible.

Why Excel persists:

- flexible,

- forgiving,

- universally understood.

Why Excel fails:

- no system of record,

- no preserved history,

- no audit trail,

- no scalability.

PowerModels replaces Excel as **infrastructure**, not as a UI convenience.

------------------------------------------------------------------------

**L.3 SMB Accounting Platforms (QuickBooks, Xero)**

**What They Do Well**

- transaction capture,

- basic reporting,

- SMB usability,

- ecosystem reach.

**Structural Limitations**

- SMB-owned books,

- snapshot-based history,

- poor handling of incompleteness,

- flat journal entries,

- preparation externalized to Excel.

Accounting firms still export to Excel because these systems are not designed for professional preparation.

**Encroachment Risk:** Medium (6/10)

They have resources, but:

- incentives are SMB-first,

- backward compatibility constrains redesign,

- surfacing incompleteness conflicts with UX goals.

------------------------------------------------------------------------

**L.4 Engagement & Workpaper Systems**

*(Caseware, CCH Engagement, Workpapers CS)*

**What They Do Well**

- trial balance management,

- workpaper organization,

- review workflows,

- compliance documentation.

**Structural Limitations**

- document-centric,

- snapshot-based,

- do not own the GL,

- reconstruct history each period.

These systems sit *on top of* accounting systems rather than replacing preparation itself.

**Encroachment Risk:** High (7/10)

They sell to firms and understand workflows, but:

- moving to an event-sourced GL requires re-architecture,

- accepting ongoing accounting responsibility disrupts their model,

- their economics favor engagement cycles, not continuous preparation.

------------------------------------------------------------------------

**L.5 Tax Trial Balance & Cleanup Tools**

*(Tallyfor, TreeBeam, regroupTAX)*

**What They Do Well**

- address tax-season pain,

- handle broken books,

- produce tax-ready trial balances.

**Structural Limitations**

- point solutions,

- tax-centric,

- period-bound,

- no continuous financial record.

They exist because accounting systems fail at preparation---but they stop at tax.

**Encroachment Risk:** Low--Medium (4/10)\
More likely acquisition targets than long-term platform builders.

------------------------------------------------------------------------

**L.6 Embedded & Platform-Owned GLs**

*(Teal, Layer, Vertical SaaS GLs)*

**What They Do Well**

- clean transaction flows,

- modern architectures,

- developer-friendly APIs.

**Structural Limitations**

- platform-owned accounting,

- avoid messy data,

- avoid professional judgment,

- avoid accounting liability.

Their incentives directly conflict with PowerModels' thesis.

**Encroachment Risk:** Low (2/10)

------------------------------------------------------------------------

**L.7 TurboTax / Intuit Strategy**

*(Adjacent Pressure, Not Direct Competition)*

Intuit's strategy increasingly targets:

- assisted tax,

- direct SMB relationships,

- price pressure on professionals.

This creates channel tension with accounting firms.

PowerModels benefits indirectly by:

- improving firm efficiency,

- reinforcing professional value,

- enabling firms to move up-market.

**Threat vector:** pressure on firms\
**Encroachment risk:** indirect, not substitutive

------------------------------------------------------------------------

**L.8 Why This Gap Has Persisted**

This layer has not been built because it requires:

- owning messy accounting reality,

- surfacing incompleteness,

- accepting accounting accountability,

- resisting scope creep,

- building for professionals, not SMBs.

Most vendors avoid at least one of these.\
PowerModels does not.

------------------------------------------------------------------------

**L.9 Competitive Positioning Summary**

  ------------------------------------------------
  **Dimension**                 **PowerModels**
  ----------------------------- ------------------
  Primary Customer              Accounting firms

  Owns the GL                   Yes

  Designed for Messy Data       Yes

  Cleanup as First-Class        Yes

  Persistent Schedules          Yes

  Event-Sourced History         Yes

  Review & Judgment In-System   Yes

  ERP Replacement               No

  Services Model                No
  ------------------------------------------------

This position is **structurally defensible**, not feature-based.

------------------------------------------------------------------------

**L.10 Summary**

PowerModels competes against:

- linear labor scaling,

- Excel-based preparation,

- and tools that fail under messiness.

Its differentiation comes from:

- architectural choices,

- scope discipline,

- professional alignment.

Competitors can copy features.\
They cannot easily copy the tradeoffs.

## Appendix M --- Risk Register & Failure Modes

Purpose: Identify the real risks to PowerModels' success, distinguish between fatal and manageable risks, and document mitigation strategies already embedded in the design.\
This appendix exists to:\
• prevent blind spots,\
• avoid hand-wavy "execution risk" language,\
• demonstrate that speed-tradeoffs are intentional, not accidental.

------------------------------------------------------------------------

**M.1 How to Read This Risk Register**

Not all risks are equal.\
This register classifies risks as:\
• Structural risks --- inherent to the thesis\
• Execution risks --- controllable with discipline\
• Perception risks --- driven by messaging or misunderstanding

Mitigations are designed into the product, GTM, and architecture --- not deferred.

------------------------------------------------------------------------

**M.2 Structural Risks (Hard, but Intentional)**

**Risk 1: Firm Resistance to Standardization**

Description\
Some firms are culturally resistant to standardized workflows and defaults.

Why This Is Real\
• Firms often equate flexibility with quality.\
• Senior partners may resist imposed structure.

Mitigation\
• Strict ICP discipline\
• Early disqualification of "proudly manual" firms\
• Positioning PowerModels as review leverage, not rigidity

Residual Risk: Medium\
This risk is unavoidable but self-selecting.

------------------------------------------------------------------------

**Risk 2: Partial Adoption ("Tool, Not Infrastructure")**

Description\
Firms adopt PowerModels for a subset of clients but never standardize.

Why This Is Dangerous\
• Limits expansion\
• Weakens ROI\
• Undermines defensibility

Mitigation\
• Land with high-pain clients only\
• Expansion driven by internal staff demand\
• Default positioning for new clients\
• Avoid feature flags that encourage fragmentation

Residual Risk: Medium\
Controlled by onboarding discipline (Appendix H).

------------------------------------------------------------------------

**Risk 3: Misinterpretation as Bookkeeping Automation**

Description\
Market misreads PowerModels as another bookkeeping automation tool.

Why This Is Dangerous\
• Attracts wrong buyers\
• Increases churn\
• Creates pricing pressure

Mitigation\
• Explicit 85% completion boundary\
• "What We Are Not Building" appendix\
• Firm-only GTM\
• No SMB self-serve entry points

Residual Risk: Low--Medium\
Messaging discipline is critical.

------------------------------------------------------------------------

**M.3 Execution Risks (Manageable with Discipline)**

**Risk 4: Onboarding Drag Slows Time-to-Value**

Description\
Onboarding takes longer than expected, delaying expansion.

Why This Matters\
• Slows revenue realization\
• Increases support load\
• Pressures sales to oversell

Mitigation\
• Conservative onboarding capacity assumptions\
• Standardized onboarding playbooks\
• Industry CoA libraries\
• First-cycle success as a gating metric

Residual Risk: Medium\
Improves with maturity.

------------------------------------------------------------------------

**Risk 5: Engineering Overreach**

Description\
Pressure to add features beyond the ladder undermines architecture.

Why This Happens\
• Investor feature requests\
• Sales-driven edge cases\
• Competitive noise

Mitigation\
• Product Expansion Ladder (Appendix F)\
• "What We Are Not Building" constraints\
• CTO architectural veto authority

Residual Risk: Low\
Requires ongoing cultural discipline.

------------------------------------------------------------------------

**Risk 6: Over-Automation of Judgment**

Description\
Temptation to automate beyond safe DOA levels.

Why This Is Dangerous\
• Erodes trust\
• Increases liability\
• Breaks accountant alignment

Mitigation\
• Explicit MOA / DOA framework\
• Review Accounts\
• No silent overrides\
• Judgment always visible and professionally owned

Residual Risk: Low\
Architectural guardrails are already in place.

------------------------------------------------------------------------

**M.4 Market & Competitive Risks**

**Risk 7: Incumbent Encroachment**

Description\
Incumbents attempt to replicate parts of the wedge.

Reality Check\
• Features can be copied\
• Architecture and tradeoffs cannot easily be copied

Mitigation\
• Event-sourced core\
• Ownership of messy accounting reality\
• Firm-first incentives\
• Slow, trust-based expansion

Residual Risk: Medium\
Time and focus are the moat.

------------------------------------------------------------------------

**Risk 8: Pricing Pressure from SMB Tools**

Description\
SMB pricing expectations leak into firm conversations.

Mitigation\
• Price against labor, not software\
• Per-client firm licensing\
• Expansion-driven ROI narrative

Residual Risk: Low\
Buyer sophistication is decisive.

------------------------------------------------------------------------

**M.5 Financial & Operating Risks**

**Risk 9: Burn Misalignment**

Description\
Burn accelerates faster than onboarding or expansion capacity.

Mitigation\
• Controlled hiring\
• Senior-heavy team\
• Capacity-gated growth\
• Founder-led GTM initially

Residual Risk: Low\
Actively monitored.

------------------------------------------------------------------------

**Risk 10: Revenue Timing vs Cash Needs**

Description\
Ramp velocity slower than base case affects runway. The primary trade-off is not demand uncertainty but pacing: moving too fast risks onboarding quality, while moving too slowly delays ARR realization.

Mitigation\
• Conservative scenarios modeled\
• Monthly billing assumed\
• Optional annual prepay later (not assumed)

Residual Risk: Medium\
Managed through fundraising timing.

------------------------------------------------------------------------

**M.6 What Is Explicitly Not a Risk**

PowerModels is not materially exposed to:\
• SMB churn dynamics\
• Consumer marketing spend\
• Services margin compression\
• Regulatory filing liability\
• Payroll or payments risk

These risks are avoided by design.

------------------------------------------------------------------------

**M.7 Summary**

PowerModels' biggest risks are not technical.\
They are discipline risks.

The company succeeds if it:\
• maintains scope boundaries,\
• enforces ICP discipline,\
• resists feature-driven expansion,\
• prioritizes trust over speed.

Most failure modes are self-inflicted.\
The architecture and strategy already contain the mitigations.

------------------------------------------------------------------------
