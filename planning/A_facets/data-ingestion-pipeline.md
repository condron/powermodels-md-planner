---
type: facet
id: data-ingestion-pipeline
label: Data Ingestion Pipeline
synonyms:
  - CSV import
  - PDF parsing
  - transaction ingestion
  - data import
  - composable pipeline
  - column mapping
  - transaction normalization
  - classification rules
  - AI classification
  - journal posting
links_to_facets:
  - event-sourced-architecture
  - domain-model-structure
  - ai-empowerment-strategy
  - shoebox-offering
claims:
  - Composable pipeline architecture replaces ad-hoc CSV/PDF parsing with reusable typed steps
  - Pipeline steps include parse, map, normalize, classify (rule + AI), persist, and post to journal
  - DataIngestionContext provides step-to-step state passing without coupling
  - AI classification step uses SemanticKernel for account categorization suggestions
  - Pipeline feeds domain aggregates — DataSource, DataElement, Journal, JournalEntry
  - DataValidator utility provides CLI validation of imported data before commit
  - Highest write-volume operation — 1000-row CSV generates approximately 1001 events
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Data Ingestion Pipeline

## Description

The data ingestion pipeline is a composable, step-based architecture for importing external financial data (CSV files, PDF bank statements, API feeds) into the PowerModels domain. It replaces the previous ad-hoc parsing approach with typed pipeline steps that can be composed, tested, and reused across data source types.

## Key Points

- **Orchestrator**: DataIngestionPipeline composes IDataIngestionStep[] into a sequential flow
- **Pipeline steps**: CsvReaderStep → ColumnMappingStep → TransactionNormalizerStep → ClassificationRuleStep → AIClassificationStep → AddOrUpdateDataSourceStep → JournalPostingStep
- **PDF support**: PDFReaderStep for bank statement extraction
- **State passing**: DataIngestionContext carries intermediate results between steps without coupling
- **Models**: AccountTransaction, ClassificationResult, ClassificationRule, JournalSchema, StandardFields
- **Write volumes**: Highest in the system — a 1,000-row CSV generates ~1,001 events (1 DataSource + 1,000 DataElements)

## Pipeline Architecture

```
Source File (CSV/PDF)
  → CsvReaderStep / PDFReaderStep        ← parse raw data
  → ColumnMappingStep                     ← map source columns to standard fields
  → TransactionNormalizerStep             ← normalize amounts, dates, descriptions
  → ClassificationRuleStep                ← rule-based account classification
  → AIClassificationStep                  ← AI-powered suggestions (SemanticKernel)
  → AddOrUpdateDataSourceStep             ← persist to DataSource aggregate
  → JournalPostingStep                    ← post to Journal aggregates
```

## Relationship to Shoebox Offering

The data ingestion pipeline is the technical backbone of the "shoebox to trial balance" value proposition. Messy, unstructured financial documents go in; classified, normalized journal entries come out. The AI classification step is where the 85/15 boundary materializes — AI suggests, accountant reviews and approves.

## Implementation Reference

Pipeline in `src/ModelServer/SpreadsheetAdapter/Reconciliation/`. DataValidator in `src/Utilities/DataValidator/`. Detailed architecture in `implementation/saga-catalog.md`.
