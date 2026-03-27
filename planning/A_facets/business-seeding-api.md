---
type: facet
id: business-seeding-api
label: Business Seeding API
synonyms:
  - headless business creation
  - SeedTestBusiness
  - Business Loader
  - test fixtures
  - seed data
  - deterministic testing
  - demo data
links_to_facets:
  - domain-model-structure
  - data-ingestion-pipeline
  - event-sourced-architecture
claims:
  - Headless business creation via domain commands without UI — enables deterministic test fixtures and demos
  - SeedTestBusiness CLI creates fully-functioning businesses with CoA, counterparties, products, and journal entries
  - BusinessLoader orchestrates the full business creation workflow programmatically (504 lines)
  - Seed data format is JSON — business.json, accounts.json, counterparties.json, products.json, journal-entries.json
  - Adventure Services dataset provides realistic test data with 30 accounts and 615 journal entries
  - Reset capability allows clean-slate testing by deleting and recreating workspace data
  - Validates seed data integrity before loading via SeedDataValidator
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Business Seeding API

## Description

The business seeding infrastructure enables headless creation of fully-functioning test businesses via domain commands, bypassing the UI workflow entirely. This is critical for deterministic testing, CI pipelines, demo environments, and development workflows where a known business state is needed.

## Key Points

- **CLI tool**: `dotnet run --project src/Utilities/SeedTestBusiness -- --dataset adventure-services --reset`
- **Datasets**: `adventure-services` (default, ~30 accounts, ~615 journal entries) or `minimal` (1 account)
- **Components**: BusinessLoader (504 lines), SeedDataLoader (300), SeedDataValidator (106), BusinessSeedData (83), SeedResult (20)
- **Seed data location**: `SpreadsheetAdapter.Tests/seed-data/{dataset-name}/` as JSON files
- **Reset mode**: `--reset` deletes existing workspace data before loading for clean-slate testing

## Seeding Flow

```
SeedTestBusiness CLI
  → SeedDataLoader reads JSON files into BusinessSeedData
  → SeedDataValidator validates integrity (accounts reference valid parents, etc.)
  → BusinessLoader executes domain commands:
    1. CreateClientWorkspace
    2. AddBusiness
    3. CreateAccountingSystem + CreateChartOfAccounts
    4. AddRootAccount / AddChildAccount (with opening balances)
    5. CreateCounterparty / CreateVendor / CreateCustomer / etc.
    6. CreateProduct
    7. InitializeJournalDataSource
    8. CreateJournal + CreateJournalEntry (615 entries for adventure-services)
```

## Strategic Value

Enables the "try before you buy" demo experience — potential customers can see a fully-populated business with realistic data in seconds, without manual data entry. Also enables automated UI testing via UITestRunner.

## Implementation Reference

CLI in `src/Utilities/SeedTestBusiness/`. Seeding components in `src/ModelServer/SpreadsheetAdapter/Seeding/`. Seed data in `src/ModelServer/SpreadsheetAdapter.Tests/seed-data/`.
