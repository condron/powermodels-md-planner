---
type: facet
id: domain-model-structure
label: Domain Model Structure
synonyms:
  - entity hierarchy
  - aggregate relationships
  - counterparty model
  - business creation chain
  - temporal model
  - accounting system ownership
  - domain entity graph
links_to_facets:
  - event-sourced-architecture
  - isolation-acl-pattern
  - proven-completeness
claims:
  - Firm-Customer-Business-AccountingSystem hierarchy enforces per-client data isolation
  - Counterparties operate at two levels with shared global contacts and business-specific role entities
  - ChartOfAccounts is 1:1 with AccountingSystem sharing the same GUID
  - No explicit fiscal year aggregates — temporal logic lives in read models
  - Business creation follows a deterministic chain from workspace through accounting system to chart of accounts
  - DataSource and EntrySet separate external feeds from manual journal entries
  - ServerFinancialModel is a God Aggregate at 3500+ lines requiring expansion gating
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Domain Model Structure

## Description

The PowerModels domain model is an ES/CQRS aggregate graph centered on the Firm → Customer → Business → AccountingSystem hierarchy. Each business gets its own isolated event store with aggregates for chart of accounts, counterparties, data sources, journal entries, and financial models.

## Key Points

- **Entity hierarchy**: Firm (implicit) → Customer (ClientWorkspace) → Business → AccountingSystem → child aggregates
- **Counterparty dual model**: Global Counterparty aggregate (shared contact info) + per-AccountingSystem role entities (Vendor, Customer, Employee, Contractor)
- **1:1 identity sharing**: AccountingSystem and ChartOfAccounts share the same GUID — creation is tightly coupled
- **Temporal gap**: No fiscal year or period aggregates; period logic deferred to read models and application layer
- **Data source separation**: DataSource (external feeds) vs EntrySet (manual entries) — journal entries don't decompose to debits/credits at the domain level
- **God Aggregate**: ServerFinancialModel (3,557 lines, 87 Apply methods) handles financial model structure, formulas, worksheets, templates — new features should be separate aggregates

## Structural Invariants

- Every Business has exactly one AccountingSystem
- Every AccountingSystem has exactly one ChartOfAccounts
- Accounts form a parent-child hierarchy within a single ChartOfAccounts
- Counterparty role flags are additive (bitwise Flags enum)
- Business isolation is enforced by separate DataStore instances per business

## Implementation Reference

Detailed structural data in `implementation/domain-model.md` and `implementation/scan-data.json`.
