---
type: evidence
id: codebase-review-feb2026
title: Codebase & GitHub Review — Feb 2026
date: 2026-02-14
method: direct code inspection + GitHub milestone/issue review
source: powermodels repo (event-systems/PowerModels) + demo-requirements.md
confidence: high
---

# Codebase & GitHub Review — Feb 2026

## Summary

Direct inspection of the PowerModels source code and GitHub project (event-systems/PowerModels) against demo requirements. 174 shoebox-track stories closed across 5 milestones. Current milestone: Shoebox Release Candidate 5 (19 open, 5 closed).

## Key Findings

### Validated by Code

- **Event-sourced architecture** — `DataStore.cs` implements full event stream storage with projections, persistence, and subscriptions via ReactiveDomain. Audit trail by design is real.
- **AI transaction classification** — `AIStep.cs` uses Azure OpenAI gpt-4o with structured JSON output constrained to Chart of Accounts enum. Confidence scoring (0.0–1.0) on every entry. Validation against CoA. This is production-quality.
- **85/15 boundary** — `UncategorizedTransactionsVm` filters transactions where confidence < 0.9 AND not accepted. This IS the 85/15 split in code. Full review UI exists (12+ WPF views rebuilt in RC4).
- **PDF bank statement ingestion** — `ExtractTransactionsFromPDFs` command + PdfPig/Tabula extraction + AI pipeline → journal entries → trial balance model. RC4 closed "PDF data ingestion" (#2006) and "trial balance solution" (#1998).
- **QuickBooks CSV import** — `GetJournalReportFromCsv()` parses QB Journal Report exports. Works in all builds. This is the only production-ready QB onboarding path.
- **Reconciliation pipeline** — Multi-step pipeline architecture: MappingStep → RuleStep → AIStep → FormatConversionStep → JournalPostingStep. Extensible via `PipelineBuilder`.
- **Semantic Kernel integration** — `ReconciliationBootstrap.cs` configures text-embedding-3-small for embeddings alongside gpt-4o for classification.

### Invalidated / Not Found in Code

- **Xero connector** — zero code, zero issues, zero references anywhere in codebase or GitHub. Strategy documents claim it's critical for onboarding scale, but nothing exists.
- **Completeness contract/report** — zero code, zero issues. No "done" artifact, no completeness metric, no report generation. The building blocks exist (acceptance status, confidence scores, expected balances) but no assembly.
- **Drift metric / Future Ledger** — zero code, zero issues. Strategy-document concept only.
- **Production QuickBooks API access** — QB connector is hardcoded to sandbox (`sandbox-quickbooks.api.intuit.com`), OAuth credentials are sandbox app, direct API provider is `#if DEBUG` only. No Intuit production app approval exists.
- **Cloud deployment** — entire codebase is local-first (MSI installer, local DataStore, Excel process). No cloud migration stories visible despite April 2026 launch target.

### Partially Validated

- **Shoebox "folder dump" experience** — the ingestion pipeline works for bank statement PDFs but the UX is per-statement upload, not "dump a folder of messy documents." Multi-format receipt/invoice parsing doesn't exist.
- **Multi-client portfolio view** — infrastructure exists (businesses, workspaces, connections) but no portfolio dashboard. Story #2023 ("tasks across multiple client workspaces") is in RC5 backlog.
- **QB onboarding via API** — the `Synchronize` handler is sophisticated (CoA mapping, vendor 1099 detection, entity creation) but sandbox-only. Represents previous product direction.

## Quantified Evidence

- **174 stories closed** across shoebox track (Beta 1 through RC4)
- **19 open stories** in current RC5 milestone
- **6 of 19 RC5 stories** are depreciation schedules (described as "secondary goals" by milestone)
- **12+ WPF views** in transaction review workflow
- **Confidence threshold: 0.9** — hardcoded boundary between auto-classified and needs-review
- **Chunk size: 80** transactions per AI batch call
- **gpt-4o** deployment for classification, **text-embedding-3-small** for embeddings

## Plaid Authorized Reseller (Added Feb 2026)

PowerModels is an **authorized Plaid reseller** with production access. Five products are enabled:
- **Transactions** (0/200 used) — 24 months of bank transaction history via direct link
- **Transactions Refresh** (0/200) — on-demand extraction + proactive notifications
- **Recurring Transactions** (0/200) — automatic recurring expense/deposit detection
- **Enrich** (0/50) — cleanse, categorize, enhance data from any source including non-Plaid
- **Liabilities** (0/200) — credit card, PayPal, student loan, mortgage data

Cloud infrastructure and security implementation is **in progress** (thefringeninja). Plaid requires cloud deployment, secure credential management, and PII handling before API integration can go live. Related prerequisite work visible in repo: PII filtering (#1604), logging infrastructure (#1593 Epic, #1605-1612). Stories for Plaid/cloud work may be tracked outside the GitHub repo.

**Strategic impact:** Plaid provides a third onboarding path that bypasses QB/Xero entirely. For shoebox clients (no accounting system, just a bank account), Plaid + Enrich + AI pipeline could deliver "link your bank → see your books in 5 minutes."

## Implications for Facets

| Facet | Impact |
|---|---|
| shoebox-offering | Claims about document ingestion are partially validated — bank statement PDFs work, receipt/invoice parsing doesn't |
| tax-preparation-wedge | AI classification pipeline validates the core value prop. QB onboarding claim needs qualifier (CSV only, not live API) |
| reconciliation-elimination | Pipeline architecture validates. Multi-step classification (rules → AI → validation) is real |
| proven-completeness | CRITICAL GAP — no code exists for completeness contract. Claims are aspirational only |
| ai-empowerment-strategy | Strongly validated — gpt-4o structured output, confidence scoring, human-in-the-loop review UI |

## References

- gap analysis: `_output/demo-gap-analysis.md`
- demo requirements: `_output/demo-requirements.md`
- repo: `event-systems/PowerModels` (GitHub)
