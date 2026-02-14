# Beta Demo Requirements — Derived from Vault Geometry
**Date:** 2026-02-14  
**Method:** Aggregated q-holes and proof_preferences across 17 facet × 10 audience intersections + codebase validation  
**Goal:** Define what the demo must show to engage both users (CPAs) and buyers (firm partners)

---

## Two Audiences, Different Concerns

| | CPA / Tax Preparer | Firm Partner |
|---|---|---|
| **Primary fear** | "Will this break my workflow?" | "Will this hurt my margins?" |
| **Primary hope** | "Will this reduce my workload?" | "Will this grow my capacity?" |
| **Proof they need** | Working review screen, judgment preserved | Economics, scalability, quality |
| **Trust trigger** | "I can still review and override" | "Every turned-away client becomes profitable" |

---

## Demo Moments (7)

### Moment 0: "The Worst Client" — Shoebox Ingestion

**Show:** A folder of messy bank statements (PDFs). Upload them. Watch the system extract transactions and generate a trial balance.

**Codebase:** 🟢 **EXISTS** — ExtractTransactionsFromPDFs → AIStep → trial balance model. Needs multi-file UX wrapping.

**Beta validation:** Bancroft's worst client was onboarded via this semi-automated pipeline. Work since then has been improving UX and fully automating the process.

**Hooks:** shoebox-offering (5 hooks to firm-partners, 4 to CPAs, 4 to bookkeepers)

**Partner takeaway:** "How many clients did you turn away last year? Every one is revenue you left on the table."  
**CPA takeaway:** "The client I dread most just became the easiest one to onboard."

---

### Moment 1: "Zero to Client in 5 Minutes" — Data Onboarding

**Show:** Import client financial data. Three paths available:

| Path | Method | Status |
|---|---|---|
| **A (today)** | Import QB Journal Report CSV via file picker | 🟢 Works |
| **B (when ready)** | Link bank account via Plaid → 24mo enriched transactions | 🟡 Authorized reseller, infra in progress |
| **C (future)** | Live QB/Xero API connection | 🔴 QB sandbox-only, Xero nothing |

**Hooks:** quickbooks-xero-onboarding → rapid client transition, platform onboarding, Plaid integration, bank account linking

**Partner takeaway:** Portfolio onboarding at scale. Shoebox clients who don't even have QB can be onboarded via bank link.  
**CPA takeaway:** I didn't have to learn anything new yet. My data is already here.

---

### Moment 2: "The 85% — Watch It Prepare" — Tax Preparation Wedge

**Show:** System processes ingested data, categorizes transactions with AI, prepares books to 85% completeness.

**Codebase:** 🟢 **EXISTS** — AIStep.cs with gpt-4o structured output, confidence scoring, validation against Chart of Accounts.

**Hooks:** tax-preparation-wedge → preparation to 85 percent, tax season adoption

**Partner takeaway:** "85% done automatically. Staff focus on the 15% that needs judgment."  
**CPA takeaway:** "It did in minutes what used to take me hours."

---

### Moment 3: "The 15% — Your Judgment Matters" — 85/15 Boundary

**Show:** System surfaces uncertain items (confidence < 0.9) with explicit flags. CPA reviews, accepts, or overrides each one.

**Codebase:** 🟢 **EXISTS** — confidence threshold at 0.9, UncategorizedTransactionsVm, 12+ WPF review views rebuilt in RC4.

**Hooks:** professional-judgment-preservation → 85/15 boundary, explicit uncertainty, professional review

**Partner takeaway:** "AI doesn't replace the CPA. It shows exactly where judgment is needed."  
**CPA takeaway:** "I know exactly what to review. No guessing."

---

### Moment 4: "The Review Screen" — WPF Interface

**Show:** CPA works through flagged items in the review interface. Accept categorizations, override where needed, add notes.

**Codebase:** 🟢 **EXISTS** — comprehensive WPF review interface, rebuilt in RC4, standalone app in progress (#2017).

**Hooks:** wpf-excel-interface-strategy → review efficiency, familiar workflow, professional control

**Partner takeaway:** Staff can review efficiently, managers can supervise, output is audit-ready.  
**CPA takeaway:** This fits my workflow.

---

### Moment 5: "The Trial Balance" — Proven Completeness

**Show:** The trial balance report — the standard accounting completeness proof. All transactions classified (85% AI + 15% professional review), reconciled, and balanced.

**Codebase:** � EXISTS. Three-part mechanism already in code:
1. Automated classification (AIStep.cs, gpt-4o, confidence scoring) = the 85%
2. Reconciliation UI + adjustment UI (12+ WPF views, accept/override) = the 15%
3. Trial balance report (closed in RC4 #1998) = the completeness proof

No separate "completeness contract" needed — the trial balance IS the proof.

**Hooks:** proven-completeness → trial balance report, accounting completeness, preparation quality standard

**Partner takeaway:** Standard accounting proof. Defensible output. The books balance.  
**CPA takeaway:** I know when I'm done. The trial balance tells me."

---

### Moment 6: "The Second Client" — Portfolio Scale

**Show:** Repeat the process for a second client. Show that the firm can manage many clients efficiently. Show the monthly update cadence.

**Hooks:** unit-economics → 3x client capacity, staff leverage, small account flywheel

**Key message:** Monthly updates prevent work explosions. $20/mo per client is cheap insurance against 100-hour surprises.

**Partner takeaway:** "My small accounts just became profitable."  
**CPA takeaway:** I can actually handle this volume. Monthly updates keep things manageable.

---

## Design Partner Pipeline — Demo Must Work for These Firms

| Firm | Demo Urgency | Key Requirement |
|---|---|---|
| **Stewart** | 🟡 2 trial clients identified | Demo must process real client data end-to-end |
| **Liberty Tax** | 🟡 Onboarding conversation next week | Demo must work for seasonal tax prep workflow |
| **Bancroft** | ✅ Already demoed (semi-automated) | UX improvements and full automation in progress |

---

## What Must Work

### Non-Negotiable (demo fails without these)
1. **Document ingestion → trial balance** — Moment 0. 🟢 EXISTS. Needs UX wrapping.
2. **QB CSV import or Plaid bank link** — Moment 1. 🟢 QB CSV works / 🟡 Plaid in progress.
3. **85/15 boundary UI** — Moment 3. 🟢 EXISTS.
4. **Trial balance report** — Moment 5. � EXISTS (closed RC4 #1998).

### Important (demo is weak without these)
5. **WPF review interface** — Moment 4. 🟢 EXISTS.
6. **Multi-client view** — Moment 6. 🟡 Story #2023 in RC5 backlog.
7. **Plaid integration** — Moment 1 upgrade. 🟡 IN PROGRESS.

### Cut from Near-Term Demo
8. **Drift/Future Ledger** — Zero code. Phase 2+ vision. Will deliver via client dashboard.
9. **Xero connector** — Nothing exists.

### Nice to Have
10. **Client dashboard** — Monthly book visibility for end-clients
11. **AI explanation UI** — Shows "why" behind categorization
12. **Excel export** — Familiar format for power users

---

## Pricing in the Demo

| Tier | Show | Message |
|---|---|---|
| **Micro/1099 ($20/mo)** | Current focus — demo this | "Same as QuickBooks. Pass it through to the client." |
| **Solo/Partnership ($50-100/mo)** | Mention | "As the product grows, so does the value." |
| **SMB ($200/mo)** | Vision | "Full books, forecasting, drift metrics — Phase 2." |

---

## The One-Sentence Demo Script

> "Your worst client just emailed you a folder of receipts and bank statements. We turn that into a trial balance in minutes — no data entry, no 100-hour setup. Import their QuickBooks data or link their bank account directly. We prepare the books to 85%, show you exactly where your judgment is needed for the last 15%, and hand you a trial balance that proves completeness. Do the same for the next 30 clients — every turned-away shoebox client becomes profitable. $20 a month, same as QuickBooks."
