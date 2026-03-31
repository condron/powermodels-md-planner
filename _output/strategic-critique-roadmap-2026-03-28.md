# Strategic Critique: Is This the Right Roadmap?

**Date:** 2026-03-28
**Subject:** Honest examination of the unified roadmap v2 against business objectives
**Input:** Unified Roadmap v2, Strategic Options Analysis (2026-03-08), Gap & Conflict Analysis

---

## What the Roadmap Gets Right

1. **Human-then-agent sequencing** — Front-loading pattern establishment so agents replicate is the correct compounding strategy.
2. **Weekly visible output** — Stakeholders need proof of velocity; infrastructure-only weeks kill confidence.
3. **Integration over build** — Using existing rd-agentic-flow assets compresses infrastructure timeline.
4. **No hacking constraint** — Proper ES-DDD patterns protect the architectural moat.

These are sound. The question is whether the roadmap is pointed at the **right target**.

---

## Strategic Gap 1: Reports Are Table Stakes, Not the Moat

The roadmap progression is: **7 → 11 → 13 → 15 reports.** Every weekly headline is report count.

But the strategic options analysis identifies the moat as:
- Event-sourced financial record (no prompt replicates this)
- 85/15 boundary (AI handles 85%, human reviews 15%)
- Reconciliation elimination (single source of truth)
- Regulatory accountability (audit trail, deterministic outputs)

**Reports are output. Every accounting tool has reports.** QuickBooks, Xero, FreshBooks all produce P&L, Balance Sheet, Cash Flow. Report count is not a differentiator.

**What IS a differentiator:**
- The AI classification pipeline with human review gate (the 85/15 boundary in action)
- The event-sourced audit trail (every classification decision tracked with ruleId + ruleVersion)
- The reconciliation workflow (statement vs extracted totals, eliminating manual reconciliation)

**The gap:** The human review gate — the most defensible feature — is buried in Week 3 (item 3.4). The reconciliation workflow is Week 4. The first two weeks are about making existing reports prettier (monthly breakdown, export, year selector).

**Question to answer:** Should the 85/15 demo (classify → review → post) be the Week 1 headline instead of Excel export?

---

## Strategic Gap 2: No Cloud Convergence

The strategic options analysis says:

> *"Two parallel tracks converge: Mitigation team fixes the domain → Joao makes it cloud-deployable → April launch."*

The v2 roadmap has **zero cloud items.** No API surface, no integration points with Joao's work, no cloud deployment milestone. The roadmap ends at "Production MVP" in a desktop WPF app.

The design partner firms need a **cloud-hosted demo**, not a desktop app. Stewart, Liberty Tax, and Bancroft aren't going to install a WPF application.

**The gap:** When does domain work meet cloud deployment? Where are the API endpoints that the cloud UI consumes? Is Joao's infrastructure ready for the domain changes being planned?

**Risk:** We build 15 reports in a desktop app that can't be demonstrated to design partners who need a cloud product.

---

## Strategic Gap 3: Design Partner Readiness vs Feature Count

Three firms are gated on a "working demo/showcase":
- **Stewart:** "100 hours unbillable setup" → needs to see shoebox pipeline reduce that
- **Liberty Tax:** Tax preparation workflow → needs to see tax-prep-to-trial-balance
- **All 3:** Reconciliation elimination → need to see single source of truth

None of them asked for 15 reports. They need to see **the workflow works end-to-end for their use case.**

**A compelling design partner demo is:**
1. Drop a shoebox of PDFs/statements
2. Watch AI classify 85% of transactions
3. Human reviews the remaining 15%
4. Journal entries post automatically
5. Trial balance, P&L, Balance Sheet generate
6. Export to Excel for the firm's existing tools
7. Reconcile against bank statement — zero variance

This is achievable with the **current 7 reports** plus the classification pipeline and review gate. The missing piece isn't more reports — it's the classification/review workflow.

**Question to answer:** What does the design partner demo actually need to show? Is it the 15-report breadth, or the end-to-end pipeline depth?

---

## Strategic Gap 4: The Agent Pipeline as Investor Narrative

The roadmap treats agent delivery as a development methodology. But "49% of features delivered by AI agents" is itself a **powerful investor story:**

- **Capital efficiency:** "A 2-person team delivers at the velocity of a 6-person team using agent-assisted development"
- **Competitive moat:** "We can add a new report type in hours, not weeks — no competitor can match this velocity"
- **Scalability answer:** Investors always ask "can this small team scale?" The agent pipeline is the answer.

**The gap:** The roadmap buries this as a success metric. It could be a **demo-able capability** shown to investors. Imagine showing an investor: "Watch — I give the agent a spec for a new report, and it builds it, tests it, and delivers it in one session."

**Opportunity:** The agent pipeline is both a development tool AND a product narrative. Treat it as both.

---

## Strategic Gap 5: WPF Investment vs Long-Term Direction

Every XAML change (fiscal year dropdown, export buttons, review gate UI) is work that lives in `PowerModels.WPF` — a project that the strategic direction says will eventually be replaced by a web/cloud UI.

**However — this is less of a problem than it appears:**
- Monthly breakdown logic is in the RM (domain layer) — portable
- Export logic is headless (`IAccountingReport → .xlsx/.pdf`) — portable
- Fiscal year logic is in the VM — portable
- Only the XAML binding is truly WPF-specific — trivial

**The real question:** Is WPF the right demo surface for design partner presentations? If design partners need a cloud URL, WPF demos are a dead end regardless of how polished they are.

**If cloud demo is needed:** The effort might be better spent on a minimal web viewer for `IAccountingReport` (the data model is already JSON-serializable) rather than WPF polish.

---

## Strategic Gap 6: Reconciliation Process Manager (The 29% Defect Rate)

The strategic analysis says reconciliation has a **29% defect rate** from implicit sagas. The roadmap has:
- Process manager infrastructure (Week 2, invisible) ✅
- ReconciliationRm report (Week 4) ✅
- Reconciliation **process manager** (not on the roadmap) ❌

The report shows reconciliation results. The process manager FIXES the 29% defect rate. These are different things. The roadmap builds the dashboard but not the engine.

**The gap:** An explicit Reconciliation ProcessManager using the newly integrated `ProcessAggregateBase<TState>` + `ProcessManagerBase` should be on the roadmap. It's the single highest-impact reliability improvement.

---

## Alternative Framings

### Alternative A: Pipeline-Depth-First

Instead of report breadth, prove the end-to-end workflow deeply for one client scenario.

| Week | Headline |
|------|----------|
| 1 | Shoebox → AI classify → human review → post → TB + P&L + BS with Excel export |
| 2 | Second client scenario + reconciliation + year selector |
| 3 | Agent builds 4 new reports from spec (velocity proof) |
| 4 | Classification rules persistent + enhanced + audit trail |
| 5 | Polish, cloud integration points, design partner demo prep |

**Pros:** Proves the moat (pipeline), not just the output (reports). More compelling for design partners. Tests the full stack earlier.
**Cons:** Fewer "new feature" metrics in early weeks. Requires classification/review UI earlier.

### Alternative B: Cloud-Convergent

Align with Joao's cloud work so Week 5 is a cloud-hosted demo, not a desktop demo.

| Week | Headline |
|------|----------|
| 1 | Current plan (demo features on desktop) |
| 2 | Headless report API + export endpoints (no WPF dependency) |
| 3 | Agent-delivered reports + review gate |
| 4 | Cloud deployment dry run with Joao |
| 5 | Cloud-hosted design partner demo |

**Pros:** Actually delivers the strategic milestone (cloud launch). Design partners get a URL, not an .exe.
**Cons:** Depends on Joao's readiness. Higher coordination overhead.

### Alternative C: Moat-Demonstration Sequence

Order features by contribution to the investor narrative:

| Week | Moat Layer Demonstrated |
|------|------------------------|
| 1 | **85/15 boundary**: AI classification → human review → post (+ Excel export for immediate utility) |
| 2 | **Audit trail**: Event-sourced classification decisions visible, rules reports, rule hits |
| 3 | **Reconciliation elimination**: Statement import → reconciliation report → zero-variance proof |
| 4 | **Scale proof**: Agent builds 4+ reports + enhanced rules (velocity demonstration) |
| 5 | **Production readiness**: Performance, full export, end-to-end demo |

**Pros:** Every week's demo directly supports the investor pitch. Design partners see the VALUE, not just features.
**Cons:** Requires review gate UI in Week 1 (more Week 1 work). Agent pipeline benefits delayed to Week 4.

### Alternative D: Current Approach + Strategic Patches

Keep the v2 roadmap but fix the gaps:

1. Move human review gate from Week 3 to Week 2 (visible alongside new reports)
2. Add Reconciliation ProcessManager to Week 3/4 (not just the report)
3. Add cloud convergence checkpoints (API surface in Week 3, cloud dry run in Week 5)
4. Frame weekly demos around moat layers, not report count
5. Make agent velocity a demo-able capability (Week 4 investor narrative)

---

## Questions That Change the Answer

These are the questions whose answers would significantly alter the optimal path:

### 1. Who is the audience for weekly demos?

| If... | Then... |
|-------|---------|
| Internal stakeholders / co-founders | Report count and velocity metrics matter. Current roadmap is fine. |
| Investors | Moat demonstration matters. Reorder around 85/15 boundary, audit trail, reconciliation. |
| Design partner firms | End-to-end workflow matters. Pipeline depth > report breadth. Cloud access needed. |

### 2. Is Joao's cloud infrastructure ready for integration?

| If... | Then... |
|-------|---------|
| Ready by Week 3 | Build headless API in Week 2–3, cloud demo by Week 5 |
| Not ready until after Week 5 | Desktop demos are fine for now; cloud convergence is Phase 2 |
| Unknown | Need to find out immediately — this changes the entire delivery surface |

### 3. What's the next funding gate?

| If... | Then... |
|-------|---------|
| Angel round pitch in 4–6 weeks | Every demo must tell the investor story. Moat demonstration > feature count. |
| Design partner trial in 4–6 weeks | End-to-end pipeline for their scenario. Cloud access required. |
| Internal confidence milestone | Current roadmap (feature velocity proof) is appropriate. |

### 4. Is the ingestion pipeline working well enough to demo?

| If... | Then... |
|-------|---------|
| Seeder data is sufficient for demos | Current plan works — no need to touch ingestion |
| Need real PDF/bank statement ingestion | Front-load pipeline robustness over report breadth |

### 5. Does the human review gate UI exist at all?

| If... | Then... |
|-------|---------|
| No review UI exists | This is a significant build item (not agent-deliverable for v1). Must be sequenced carefully. |
| Exists but basic | Enhance as part of the 85/15 demo story |

---

## My Assessment

The v2 roadmap is **tactically sound but strategically underleveraged.** It treats PowerModels as a "reporting tool that needs more reports" when the strategic analysis clearly positions it as an "accounting infrastructure platform with an AI-augmented pipeline."

**The optimal path depends on the answers above**, but my current read is:

1. **Week 1 (Monday demo) is fine as-is** — Excel export, monthly breakdown, year selector are high-impact, low-risk, and establish patterns. Don't change this.

2. **Weeks 2–3 should front-load the moat features** — The review gate (85/15 boundary in action) and classification persistence are more strategically valuable than reports 8–11. Reports can be agent-delivered in parallel.

3. **Cloud convergence must be addressed** — Even if Joao isn't ready, the API surface should be built. `IAccountingReport` serializes cleanly. A `/api/reports/{name}?year=2024` endpoint is a day of work and unlocks cloud UI.

4. **The Reconciliation ProcessManager is missing** — The report is on the roadmap; the process that fixes the 29% defect rate is not.

5. **Agent pipeline should be demo-able** — It's both a development tool and an investor narrative. Build one report live in a demo.

---

## Related Documents

- [Unified Roadmap v2](unified-roadmap-v2-2026-03-28.md) — current plan
- [Strategic Options Analysis](../../implementation-vault/PowerModels-src/docs/Roadmap/Research/strategic-options-analysis-2026-03-08.md) — investor lens
- [Gap & Conflict Analysis](gap-conflict-analysis-2026-03-28.md) — technical gaps + rd-agentic-flow
- [Monday Demo Plan](monday-demo-plan-2026-03-28.md) — Week 1 detail
