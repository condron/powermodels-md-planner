---
type: facet
id: project-dependency-architecture
label: Project Dependency Architecture
synonyms:
  - layer diagram
  - dependency graph
  - project reference rules
  - package distribution
  - hexagonal architecture
  - domain isolation
  - layer enforcement
links_to_facets:
  - event-sourced-architecture
  - isolation-acl-pattern
  - message-bus-architecture
claims:
  - Three-layer architecture with strict dependency direction enforced by project references
  - ModelServer has zero upstream dependencies — pure domain layer
  - UI layer cannot reference ModelServer directly — all access through SpreadsheetAdapter
  - Only ModelServer and LocalDataStore reference ReactiveDomain directly
  - SpreadsheetAdapter is the sole bridge between UI and domain
  - WPF project contains no domain logic — XAML views only
  - Package versions are controlled per layer to prevent leakage
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Project Dependency Architecture

## Description

PowerModels enforces a three-layer architecture (UI → ACL → Domain) through project references. The domain layer (ModelServer) has zero dependencies, the anti-corruption layer (SpreadsheetAdapter) bridges UI to domain, and UI projects never reference domain directly.

## Key Points

- **Domain layer**: ModelServer (aggregates, services, messages) + LocalDataStore (event store) — no upstream deps
- **ACL layer**: SpreadsheetAdapter + Excel.Toolkit — translates between UI and domain
- **UI layer**: UIBehavior (ViewModels) → WPF (XAML views) → App (entry point)
- **Dependency direction**: Always inward — UI → ACL → Domain, never reversed

## Enforcement Rules

| Rule | Mechanism |
|------|-----------|
| UI cannot access aggregates | No ProjectReference from UIBehavior → ModelServer |
| Domain has no upstream deps | ModelServer.csproj has zero ProjectReferences |
| ReactiveDomain scoped to domain | Only ModelServer + LocalDataStore reference it |
| SpreadsheetAdapter is sole bridge | All other projects reach domain through it |
| WPF has no domain logic | Only references UIBehavior |

## Package Distribution

ReactiveDomain 0.13.0 (domain only), ReactiveUI 22.3.1 (UI layer), SemanticKernel 1.72.0 (ACL + UI), DynamicData 9.4.1 (SpreadsheetAdapter).

## Implementation Reference

Full dependency graph and package matrix in `implementation/project-architecture.md`.
