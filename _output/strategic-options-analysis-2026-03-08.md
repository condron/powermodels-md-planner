# PowerModels Strategic Options Analysis

Generated: 2026-03-08
Sources: Planning vault (23 facets, 11 audiences, 29 intersections, 8 source documents), Implementation vault (scan-data.json, 10 architecture docs, 6 artifact docs), vault-project-analysis-2026-03-08-v3.md, mitigation-plan-2026-03-08.md, accelerated-mitigation-plan-2026-03-08.md, business-plan.md, strategic_summary_current.md, investor-market-evaluation.md

---

## Context

PowerModels faces a strategic decision point. The company has:

- **A validated thesis**: Tax-prep-driven entry, firm-first GTM, 85/15 boundary, event-sourced architecture
- **A working product**: 174 stories shipped, 5 milestones, AI pipeline, local-first WPF+Excel dual interface
- **Early market validation**: 3 design partner firms (Liberty Tax, Stewart, Bancroft) have validated the need; all require a working demo/showcase before moving to initial trials
- **Technical debt**: God aggregate (3,557 lines, 87 Apply methods), 0/27 RestoreFromEvents tests, 46 RMs replaying on startup, no process managers, no checkpoint persistence
- **A cloud engineer** (Joao) working on cloud/security infrastructure independently of the mitigation plan
- **A detailed mitigation plan** for the existing codebase, compressible to 5–6 weeks with expert team + agents
- **Team on sweat equity** for ~2 years, collectively owning 12% of the company

Three courses of action are under consideration. This analysis evaluates each against what investors actually underwrite and what the business needs to succeed.

---

## The Investor Lens

Before evaluating options, we must establish what investors at each stage actually look for (sourced directly from the planning vault audience profiles and investor-market-evaluation.md):

### Angel ($500K) — What They Underwrite

- **Founder quality and commitment** — team working for sweat equity signals conviction
- **Problem-solution fit** — is this a real, painful problem worth solving?
- **Early traction** — customer interest, pilots, LOIs
- **Capital discipline** — scrappiness, small checks, long runway
- **Unique insight** — what do they see that others don't?
- **Path to next milestone** — can they hit proof points for seed?

### Seed/Series A ($3–5M) — What They Underwrite

- **Product-market fit evidence** — 3–5+ reference customers, ideally with expansion
- **Go-to-market repeatability** — can the company acquire customers predictably?
- **Unit economics** — CAC/LTV, payback period, does it improve with scale?
- **Competitive moats** — what's defensible?
- **Team completeness and execution** — can this team scale?
- **Capital efficiency** — how much capital to reach next inflection point?
- **Path to Series B** — line-of-sight to $5–10M ARR

### What Gets Punished (2026 Market)

- "AI will do accounting" positioning (credibility gap)
- Services creep without software margins
- Heroic onboarding that won't scale
- Past dates without status updates (instant doubt)
- Valuation disconnected from evidence
- **Thin SaaS wrappers** — investors increasingly doubt any SaaS whose value can be replicated by foundation models (Anthropic, OpenAI)

### The "Anthropic Guts SaaS" Question

There is growing market doubt about whether SaaS offerings survive foundation model disruption. Investors are asking: *"What stops Claude/GPT from replacing this product?"*

**SaaS products that are vulnerable:**
- Thin wrappers around LLM calls (summarization, content generation, chatbot builders)
- Workflow tools whose logic is prompt-engineerable
- Data transformation tools with no proprietary data or domain model
- Products where the AI IS the product

**What makes PowerModels structurally resistant:**

| Defense Layer | Why It Can't Be Replicated by a Foundation Model |
|--------------|--------------------------------------------------|
| **Event-sourced financial record** | An append-only, time-ordered ledger with deterministic replay is a data architecture, not an AI task. No prompt produces this. |
| **27 aggregates encoding accounting domain invariants** | Business rules enforced at the aggregate level (balanced entries, period boundaries, fiscal year constraints) require a persistent, stateful system — not a stateless inference call. |
| **85/15 boundary** | The explicit design choice to stop at 85% and preserve professional judgment is the opposite of "AI will do everything." It's a trust architecture. |
| **Reconciliation elimination** | Single source of truth across financial records is a structural property of the data model, not a feature that can be prompted. |
| **Regulatory accountability** | Tax preparation requires audit trails, deterministic outputs, and professional sign-off. LLMs are probabilistic — they cannot serve as the system of record. |
| **Firm-first distribution** | The GTM is institutional adoption, not consumer self-serve. Firms don't adopt LLM prompts as infrastructure. |

**PowerModels uses AI correctly — as a tool inside a trusted architecture:**
- AI classifies transactions (the 85% mechanical work)
- AI extracts data from shoebox documents
- AI suggests mappings in the reconciliation pipeline
- But the financial record, the accounting rules, the audit trail, and the professional review workflow are all deterministic, event-sourced, and human-governed

**The investor pitch on this point:** *"We use AI to accelerate the mechanical 85% of accounting preparation. But the financial record itself — the thing firms trust, regulators audit, and lenders rely on — is a deterministic, event-sourced system that no foundation model can replace. We're not a wrapper around AI. We're an accounting infrastructure platform that uses AI as one input among many."*

This positioning is strengthened, not threatened, by foundation model advances. Better models make the 85% faster and cheaper while the 15% professional judgment layer and the underlying architecture remain PowerModels' moat.

---

## Option A: Fix What We Have

### Description

Execute the accelerated mitigation plan on the existing codebase. 5–6 weeks with the expert team (2 ES experts, principal, 3 support) + AI agents. Cloud migration continues in parallel (Joao). Ship cloud-based product for April 2026 GTM.

### What This Preserves

| Asset | Status | Investor Value |
|-------|--------|---------------|
| 174 shipped stories across 5 milestones | Proven | Execution evidence |
| Event-sourced architecture (ReactiveDomain) | Working | Technical moat — incumbents can't retrofit |
| WPF + Excel dual interface | Working | Professional-grade UI for tax accountants |
| AI pipeline (Semantic Kernel, gpt-4o) | Working | AI empowerment positioning |
| Shoebox-to-trial-balance pipeline | Validated (tested against disorganized client records) | Core value prop demonstration |
| Bank statement PDF ingestion | Working | Completeness proof artifact |
| Reconciliation pipeline (MappingStep → RuleStep → AIStep → FormatConversion) | Working (with defects) | End-to-end preparation cycle |
| Design partner pipeline (3 firms) | Active | Angel round proof |
| QB/Xero import capability | Working | Onboarding scalability answer |
| 85/15 boundary implementation | Working | "Unusually smart" investor advantage |
| Domain knowledge embedded in 27 aggregates, 80 read models | 5+ years of work | Irreplaceable institutional knowledge |

### What the Mitigation Fixes

| Problem | Fix | Timeline |
|---------|-----|----------|
| No RestoreFromEvents tests → replay safety risk | 30/30 coverage | Weeks 1–5 |
| God aggregate (3,557 lines) → compound defect risk | SFM extraction: 87 → ~67 Apply, 31 → ~11 events | Weeks 4–5 |
| No checkpoint persistence → startup replay amplification | 46/46 RMs checkpointed, <2s startup at 15K events | Weeks 1–5 |
| No process managers → implicit sagas → 29% reconciliation defect rate | 2 explicit PMs (Reconciliation, PeriodClose), <10% defect rate | Weeks 3–5 |
| Missing accounting aggregates (Journal, ReconciliationState, PeriodClose) | 3 new aggregates designed and implemented | Weeks 2–5 |
| No caching for SFM GetById | CachingRepository with persisted snapshot tier | Week 2 |

### Investor Scorecard

| Investor Criterion | Score | Evidence |
|-------------------|-------|----------|
| Product-market fit evidence | ★★★★☆ | 3 firms (Liberty, Stewart, Bancroft) validated need; all awaiting working demo for next step; shoebox pipeline proven |
| Go-to-market repeatability | ★★★☆☆ | Firm-first model defined; 3 firms validated need but all gated on demo; association channel not yet proven at scale |
| Unit economics | ★★★★☆ | $20/mo validated at micro tier; $48K–$87K ARR/firm modeled; CAC per firm not per client |
| Competitive moats | ★★★★★ | Event-sourced architecture, 85/15 boundary, reconciliation elimination — all validated by code |
| Team execution | ★★★★★ | 5+ years together, ES/DDD/CQRS experts, 174 stories shipped, detailed mitigation plan shows self-awareness |
| Capital efficiency | ★★★★★ | ~2 years on sweat equity, detailed plan shows exactly what capital buys |
| Path to next round | ★★★★☆ | Clear: mitigate → cloud launch → design partners → seed metrics |
| Time to revenue | ★★★☆☆ | April 2026 cloud launch → tax season → first revenue Q2–Q3 2026 |
| Technical risk | ★★★☆☆ | God aggregate and implicit sagas are real risks; mitigation plan addresses them directly |

### Risks

- **Mitigation plan execution risk**: Plan is detailed but still 5–6 weeks of intensive work; any delay pushes cloud launch
- **Cloud migration coordination**: Must align mitigation output with James's cloud infrastructure work
- **Technical debt may have surprises**: SFM RestoreFromEvents test (87 Apply methods) may reveal >5 bugs requiring event versioning
- **Sweat equity fatigue**: Team has been on deferred compensation for ~2 years; another 5–6 weeks of intense execution before any revenue signal

### Capital Required

- **Angel round**: ~$500K (already planned)
- **Use**: Team continuity through mitigation + cloud launch + design partner onboarding
- **Runway to**: Seed metrics (10+ firms, expansion data, CAC payback evidence)

---

## Option B: Rewrite a Smaller Product From Scratch

### Description

Build a new, focused product from scratch — likely targeting the shoebox-to-trial-balance pipeline only, or just the micro/1099 tier. Cloud-native from day one. Potentially use a modern stack (web-based, not WPF).

### What This Abandons

| Asset Lost | Replacement Cost | Impact |
|-----------|-----------------|--------|
| 174 shipped stories (5 milestones) | 12–24 months of rewrite | All execution evidence gone |
| Event-sourced architecture (ReactiveDomain) | Must re-implement or choose different architecture | Technical moat weakened or abandoned |
| 27 aggregates modeling accounting domain | 6–12 months domain modeling | 5+ years of domain knowledge lost |
| 80 read models (46 ReadModelBase + 34 TransientSubscriber) | 3–6 months to rebuild | Query layer expertise lost |
| AI pipeline (Semantic Kernel integration) | 2–4 months rebuild | Delay to AI empowerment story |
| Reconciliation pipeline (4-step, working) | 2–3 months rebuild | Core value prop delayed |
| WPF + Excel dual interface | Decision: rebuild in WPF? or pivot to web? | Either way, months of UI work |
| Design partner validation (3 firms) | Must re-validate with new product | Pipeline reset |
| QB/Xero import pipeline | Must rebuild | Onboarding story delayed |
| ProcessManager infrastructure (ready for integration) | Must re-implement | Infrastructure cost repeated |
| Checkpoint/persistence infrastructure (ready) | Must re-implement | Same |

### What This Gains

| Benefit | Reality Check |
|---------|--------------|
| Clean architecture from day one | But the existing architecture IS clean (ES/DDD/CQRS); the debt is in one god aggregate and missing tests — fixable in 5–6 weeks |
| Cloud-native from day one | Cloud migration is already underway with Joao; the domain layer ports cleanly |
| Smaller scope = faster MVP | But MVP already exists — 3 firms have validated the need and are waiting for a working demo |
| Web-based = broader accessibility | True, but WPF is correct for tax accountant workflow; web is Phase 2+ |
| Modern stack appeal | The current stack (C#, ReactiveDomain, EventStoreDB, Semantic Kernel) IS modern and purpose-built |

### The Fundamental Problem With Option B

**You already have the MVP.** Three firms have validated the need and are waiting for a working demo/showcase to move to initial trials. The shoebox pipeline works. The reconciliation pipeline works (with fixable defects). The 85/15 boundary is implemented.

A rewrite doesn't produce a better product faster — it restarts the clock on:
- Domain modeling (the hardest part — 5+ years of accounting domain knowledge)
- Design partner trust (3 firms waiting for demo — a rewrite resets that clock)
- Proof artifacts (all investor evidence resets)
- Team momentum (morale hit from discarding 2 years of sweat equity work)

### Investor Scorecard

| Investor Criterion | Score | Evidence |
|-------------------|-------|----------|
| Product-market fit evidence | ★★☆☆☆ | Resets to zero — prior validation doesn't transfer |
| Go-to-market repeatability | ★☆☆☆☆ | No product to sell; GTM timeline pushed 6–12+ months |
| Unit economics | ★★☆☆☆ | Theoretical only — no real data possible for months |
| Competitive moats | ★★★☆☆ | Team expertise preserved, but architectural moat must be rebuilt |
| Team execution | ★★★☆☆ | Demonstrates willingness to make hard calls, but also signals prior execution failure |
| Capital efficiency | ★★☆☆☆ | ~2 years of sweat equity effectively written off; capital now funds rebuild not growth |
| Path to next round | ★★☆☆☆ | Pushed 9–15 months — must rebuild → re-validate → re-prove |
| Time to revenue | ★☆☆☆☆ | 12–18 months minimum before design partner readiness |
| Technical risk | ★★★★☆ | Cleaner start, but new risks: will the team ship faster the second time? History says no |

### The Rewrite Trap

The decision to rewrite is often driven by the feeling that "if we just started over, we'd do it right." But the evidence from the implementation vault shows:

1. **The architecture IS right.** ES/DDD/CQRS with ReactiveDomain is exactly the correct architecture for this domain. The problems (god aggregate, missing tests, no checkpoints) are fixable without rewriting.
2. **The domain model IS right.** 27 aggregates, 80 read models, domain events correctly modeled — this represents 5+ years of accounting domain expertise encoded in code.
3. **The problems are specific and addressable.** The v3 analysis identified exactly what's wrong and the mitigation plan shows exactly how to fix it in 5–6 weeks.

A rewrite says "we'll throw away what works to avoid fixing what's broken" — when fixing what's broken is faster than rewriting what works.

### Capital Required

- **Angel round**: ~$500K–$750K (more needed because timeline extends)
- **Additional funding before revenue**: Likely another $500K–$1M to reach design partner stage
- **Total to seed-ready**: $1–1.5M vs $500K for Option A
- **Time to seed-ready**: 12–18 months vs 4–6 months for Option A

---

## Option C: Slim the Team + Seek Market Validation

### Description

Reduce the team to a skeleton crew. Pause or drastically slow product development. Focus on market validation — customer interviews, firm conversations, possibly exploring Canadian market with a new team. Use minimal capital to extend runway while seeking stronger product-market fit signals.

### What This Preserves

| Asset | Status | Investor Value |
|-------|--------|---------------|
| Existing codebase (frozen) | Available for future use | Retains option value |
| Business plan + strategic frameworks | Intact | Can be pitched to new investors |
| Design partner relationships | At risk — all 3 firms (Liberty, Stewart, Bancroft) need a working demo to proceed; pausing loses them | Fragile |
| Founding team (Chris, Lisa) | Preserved | Core IP holders retained |
| Market research (12 interviews) | Available | Validation artifacts available |

### What This Risks

| Risk | Severity | Explanation |
|------|----------|-------------|
| **Design partner pipeline collapse** | CRITICAL | All 3 firms (Liberty, Stewart, Bancroft) have validated the need but require a working demo/showcase to move to initial trials. Pausing development loses the April 15 tax deadline window entirely. |
| **Team dissolution** | HIGH | Development team (12% equity, ~2 years sweat) will leave for paid work. ES/DDD/CQRS expertise is extremely rare — this team cannot be reassembled. |
| **Competitive window closes** | HIGH | Industry is converging on AI empowerment (not replacement) positioning per Accounting Podcast. Other entrants will fill the gap. |
| **April 2026 cloud launch missed** | CERTAIN | No development = no cloud launch = no GTM = no revenue signal |
| **Sweat equity devalued** | HIGH | 12% ownership for ~2 years of work becomes worthless if company stalls |
| **Canadian restart fallacy** | HIGH | Rebuilding with a new team means losing the ES/DDD/CQRS expertise that IS the moat |

### The "Further Market Validation" Question

The planning vault already contains strong validation signals:

**What's validated:**
- Tax preparation as forcing function (Liberty, Stewart, Bancroft all confirm)
- 85/15 boundary resonates with practitioners (Accounting Podcast industry consensus)
- Shoebox pain is acute and universal (Stewart: "100 hours unbillable setup")
- Firm-first model makes economic sense (CAC per firm, not per client)
- $20/mo pricing validated at micro tier
- All 3 firms need a working demo/showcase to proceed to trials — signals demand contingent on product readiness, not market doubt
- Event-sourced architecture addresses the core structural problem

**What's not yet validated:**
- Full tax season throughput with the product
- Expansion mechanics (clients per firm over time)
- Onboarding scalability (time-to-first-cycle compression)
- Pricing at Solo/Partnership and SMB tiers
- Association channel conversion rates

The gap is **execution validation, not market validation**. The market is saying yes. The question is whether the product can deliver at the quality and speed firms require. Slimming the team makes that harder, not easier.

### Investor Scorecard

| Investor Criterion | Score | Evidence |
|-------------------|-------|----------|
| Product-market fit evidence | ★★☆☆☆ | Existing interviews preserved, but no new proof generated; active pipeline at risk |
| Go-to-market repeatability | ★☆☆☆☆ | No product in market; GTM frozen |
| Unit economics | ★☆☆☆☆ | Theoretical only; no path to real data without product |
| Competitive moats | ★★☆☆☆ | Moat exists in the team's heads but is not being built into defensible IP |
| Team execution | ★☆☆☆☆ | Signals failure to execute with current team; "we need to find a different team" is a red flag |
| Capital efficiency | ★★★☆☆ | Low burn, but no progress per dollar — capital buys time, not value |
| Path to next round | ★☆☆☆☆ | No clear milestones; "seeking further validation" is not a fundable plan |
| Time to revenue | ★☆☆☆☆ | Indefinite |
| Technical risk | ★★☆☆☆ | Code freezes don't improve; they rot. Returning to a frozen codebase is harder than maintaining a live one |

### Capital Required

- **Skeleton crew**: $100–200K (low burn, founder salaries only)
- **Time to decision**: 3–6 months of conversations
- **Total to restart**: If Canadian team, $500K–$1M to rebuild + $500K to operate = $1–1.5M
- **Time to market after restart**: 18–24 months (new team, new codebase, re-validate)

---

## Side-by-Side Comparison

### Timeline to Investor Milestones

| Milestone | Option A (Fix) | Option B (Rewrite) | Option C (Slim) |
|-----------|---------------|-------------------|----------------|
| Product in market | **6–8 weeks** (mitigation + cloud launch) | 9–15 months | Indefinite |
| Design partner validation | **Q2 2026** (3 firms awaiting demo) | Q4 2026–Q1 2027 | At risk of loss |
| Angel round proof points | **April 2026** | Q1–Q2 2027 | None generated |
| Seed round readiness | **Q3–Q4 2026** | Q2–Q4 2027 | 2027+ (if restart) |
| First revenue | **Q2–Q3 2026** | Q4 2026–Q1 2027 | Indefinite |
| Path to $3–4M ARR | **12–18 months post-launch** | 24–36 months | Unknown |

### Capital Efficiency

| Metric | Option A | Option B | Option C |
|--------|----------|----------|----------|
| Capital to angel proof points | $500K | $750K–$1M | $100–200K (but no proof points) |
| Capital to seed readiness | $500K | $1–1.5M | $1–1.5M (restart) |
| Sweat equity preserved | ✅ Full value | ❌ Partially destroyed | ⚠️ At risk of total loss |
| Prior work leveraged | ✅ 174 stories, 5+ years domain | ❌ Started over | ❌ Frozen/abandoned |
| Team retention | ✅ Motivated by clear plan | ⚠️ Morale risk from discard | ❌ Team dissolves |

### Investor Narrative Strength

| Element | Option A | Option B | Option C |
|---------|----------|----------|----------|
| "We shipped" | ✅ 174 stories, design partner validated | ❌ "We're rebuilding" | ❌ "We're pausing" |
| "Customers want it" | ✅ 3 firms validated need, awaiting demo to start trials | ⚠️ "They wanted the old version" | ⚠️ "We think they still do" |
| "We know the problem" | ✅ Self-aware analysis + fix plan | ⚠️ "We had to start over" | ❌ "We're not sure yet" |
| "We're capital efficient" | ✅ 2 years sweat → working product | ❌ 2 years sweat → discarded | ❌ 2 years sweat → frozen |
| "Here's the moat" | ✅ ES architecture in production | ⚠️ "We'll rebuild the moat" | ❌ "The moat is in our heads" |
| "Here's the timeline" | ✅ 5–6 weeks to fix → April launch | ⚠️ 12–18 months to MVP | ❌ "We need time to figure it out" |

---

## The Investor Narrative for Each Option

### Option A: "We Built It, We Know What's Wrong, We're Fixing It"

> "We have a validated product with a design partner on hold for our stable demo, and a detailed, evidence-based plan to fix specific technical debt in 5–6 weeks. Our expert team — 5+ years together, world-class in the exact architecture we use — will execute this while cloud deployment continues in parallel. We'll be in market by April 15, 2026 — the tax filing deadline — with 3 design partner firms in pipeline."

**Investor reaction**: This is fundable. Self-aware founders who built something real, identified problems honestly, and have a credible fix plan. The team's domain expertise (ES at Event Store, financial systems at Wellington/Linedata/Hanover) directly addresses the technical risk. Capital buys execution, not speculation.

### Option B: "We Built It Wrong, We're Starting Over"

> "We spent 2 years building a product, but we've decided to start over with a smaller scope. We believe we can build it better the second time."

**Investor reaction**: Red flag. Why did it take 2 years to realize? What ensures the rewrite succeeds? The team already knows ES/DDD/CQRS — if the current architecture is wrong, what changes? If the architecture is right (it is), why rewrite? This narrative destroys the execution credibility that's one of the company's strongest assets.

### Option C: "We Need More Time to Validate"

> "We're slimming down to seek further market validation, possibly with a new Canadian team."

**Investor reaction**: Fatal. After 2 years of development and active beta users, asking for more validation time signals either: (a) the market isn't there (contradicts the evidence), (b) the team can't execute (contradicts the track record), or (c) the founders have lost conviction. None of these are fundable.

---

## The Cloud Engineer Factor

Joao is working on cloud hosting/security infrastructure independently. This is a **critical asset for Option A**:

- Cloud migration scope: Event store → cloud-hosted persistence, multi-tenant (firm = tenant), auth, cloud-to-cloud QB/Xero integration
- The domain layer (aggregates, events, read models, services) ports to cloud without change — ES/DDD/CQRS is deployment-agnostic
- The mitigation plan output (fixed aggregates, checkpointed RMs, explicit PMs) makes the cloud deployment more reliable, not harder
- **Two parallel tracks converge**: Mitigation team fixes the domain → Joao makes it cloud-deployable → April launch

For Options B and C, Joao's cloud work is either:
- **Option B**: Partially wasted (new codebase may need different cloud architecture)
- **Option C**: Completely wasted (nothing to deploy)

---

## The Canadian Team Question

Option C suggests possibly "restarting with a brand new Canadian team." This deserves direct analysis:

### What the current team brings that cannot be replaced

1. **ES/DDD/CQRS expertise at production scale**: Chris (CTO EventStore, Chief Architect at Linedata, Principal at PerkinElmer, Lead at Wellington), plus a development team who are "ES/DDD/CQRS experts" with 5+ years together. This concentration of event-sourcing expertise in an accounting product company is globally rare.

2. **5+ years of accounting domain modeling**: 27 aggregates, 80 read models, reconciliation pipeline, AI classification pipeline, shoebox ingestion — this domain knowledge is embedded in code and in the team's heads. A new team starts at zero.

3. **Existing customer relationships**: Liberty Tax, Stewart, and Bancroft know and trust this team. All three have validated the need and are waiting for a demo. Starting over with a new team resets these relationships.

4. **Sweat equity investment**: $0 in cash compensation for ~2 years. This signals extraordinary conviction and commitment. A new team will require cash from day one.

### What a Canadian team offers

- Canadian R&D tax credits (SR&ED) — significant (up to 35% of qualifying expenditures)
- Access to Canadian VC ecosystem (different investor pool)
- Lisa's Canadian network and fundraising experience ($100M+ raised in Canada)
- Local market entry for Canadian accounting firms (~27,000 firms, CAD $18–28B market)

### The hybrid answer

The Canadian market advantages don't require abandoning the current team. They can be captured through:
- Incorporating in Canada while retaining the current development team
- Using SR&ED credits on the existing codebase development
- Targeting Canadian associations (equivalent of PSTAP) for GTM
- Lisa's Canadian network for capital strategy

---

## Recommendation

### Option A is the clear choice, with elements of the other options captured within it.

**Why:**

1. **The fastest path to investor proof points.** April 2026 cloud launch with active design partners = angel round proof within weeks, not months or years.

2. **The most capital-efficient path.** $500K buys a complete fix + cloud launch + design partner validation. Options B and C cost 2–3× as much to reach the same milestones.

3. **The only path that preserves existing assets.** 174 stories, 5+ years of domain knowledge, 3 design partner relationships, working AI pipeline, working shoebox pipeline — all preserved.

4. **The only path that retains the team.** The development team is on sweat equity. They need to see forward momentum and a path to compensation. Option A provides that. Options B and C risk losing them permanently.

5. **The self-awareness is itself an investor asset.** The v3 analysis, mitigation plan, and accelerated plan demonstrate extraordinary technical self-awareness. Investors fund teams that know their weaknesses and have credible plans to address them — not teams that throw away work or stall for "more validation."

6. **The technical debt is fixable, not fatal.** The problems are specific: one god aggregate, missing tests, no checkpoints, implicit sagas. The solutions are known: extraction, RestoreFromEvents, ICheckpointStore, ProcessManagerBase. The team that built these solutions is available. 5–6 weeks.

### Capture Option C's market validation within Option A

- Run the accelerated mitigation (5–6 weeks) **while Lisa continues firm conversations**
- Use the cloud launch as a forcing function for Stewart and Liberty Tax conversion
- Use April 2026 as the angel round proof deadline
- Evaluate Canadian incorporation / SR&ED as a capital strategy decision independent of product execution

### The one-sentence pitch

**"We have a validated product with a design partner waiting for our stable demo, a world-class ES team that built it together over 5 years, a detailed 6-week plan to fix specific technical debt, and a cloud launch targeting April 15 — tax filing deadline — with 3 design partner firms ready to validate the tax-prep-to-85% workflow."**

This is fundable on first read.

---

## Decision Matrix Summary

| Factor | Weight | Option A: Fix | Option B: Rewrite | Option C: Slim |
|--------|--------|--------------|-------------------|----------------|
| Time to market | HIGH | ★★★★★ (6–8 weeks) | ★★☆☆☆ (12–18 months) | ★☆☆☆☆ (indefinite) |
| Capital efficiency | HIGH | ★★★★★ ($500K) | ★★☆☆☆ ($1–1.5M) | ★★★☆☆ (low burn, no value) |
| Asset preservation | HIGH | ★★★★★ (everything) | ★☆☆☆☆ (team only) | ★★☆☆☆ (frozen code) |
| Team retention | HIGH | ★★★★★ (clear plan) | ★★★☆☆ (morale risk) | ★☆☆☆☆ (dissolution) |
| Investor narrative | HIGH | ★★★★★ (self-aware builders) | ★★☆☆☆ (restart signal) | ★☆☆☆☆ (stall signal) |
| Design partner pipeline | HIGH | ★★★★★ (3 firms active) | ★★☆☆☆ (reset) | ★☆☆☆☆ (at risk) |
| Technical risk mitigation | MED | ★★★★☆ (specific plan) | ★★★★☆ (clean start) | ★☆☆☆☆ (no progress) |
| Market timing | HIGH | ★★★★★ (April 2026) | ★★☆☆☆ (2027) | ★☆☆☆☆ (window may close) |
| Canadian market access | LOW | ★★★☆☆ (via incorporation) | ★★★☆☆ (via new team) | ★★★★☆ (native) |
| **TOTAL (weighted)** | | **★★★★★** | **★★☆☆☆** | **★☆☆☆☆** |

---

## Appendix: What "Fix What We Have" Actually Means

For precision, the mitigation plan addresses these specific items (not "general cleanup"):

| Item | Current State | Post-Mitigation State | Why It Matters for Investors |
|------|-------------|---------------------|----------------------------|
| RestoreFromEvents tests | 0/27 | 30/30 | Proves ES architecture works correctly — not just claimed |
| SFM god aggregate | 3,557 lines, 87 Apply | ~1,500–2,000 lines, ~67 Apply | Removes #1 defect source; shows engineering discipline |
| RM startup replay | Full replay from position 0 | Checkpoint-based resume (46/46) | Startup <2s at 15K events — deployable at scale |
| Process managers | 0 (7 implicit sagas) | 2 explicit (Recon, PeriodClose) | Reconciliation defect rate from 29% → <10% |
| Accounting domain | Missing Journal, ReconciliationState, PeriodClose | 3 new aggregates designed and implemented | Foundation for accountant workflow features |
| FinancialModelService | 166 commands in one handler | Split: ~100 core + ~60 mapping | Reduces coordination complexity |
| Caching | No aggregate snapshots | CachingRepository for SFM | GetById: full replay → <100 events from snapshot |

Total cost: **5–6 weeks** with the team described. Total capital: **$500K angel round** (already planned).

None of this requires changing the architecture, the domain model, the product scope, or the team. It requires executing a plan that the team themselves wrote, using infrastructure they themselves designed.
