---
type: facet
id: cloud-migration-strategy
label: Cloud-Based Platform Migration
synonyms:
  - cloud deployment
  - local to cloud transition
  - cloud-first architecture
  - April 2026 launch
links_to_facets:
  - event-sourced-architecture
  - quickbooks-xero-onboarding
  - firm-first-gtm
claims:
  - April 2026 go-to-market is cloud-based not local-first
  - Current codebase is local-first and requires architecture migration
  - Event-sourced architecture preserves core competency through migration
  - Multi-tenant firm-level tenancy enables team collaboration
  - Cloud-to-cloud QB and Xero integration replaces local connectors
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Cloud-Based Platform Migration

## Description

PowerModels is migrating from a **local-first architecture** (MSI installer, local event store, desktop-only) to a **cloud-based platform** for the April 2026 go-to-market launch. The event-sourced architecture is preserved as the core competency while adding cloud storage, multi-tenant firm-level isolation, and cloud-to-cloud integrations with QuickBooks and Xero.

## Key Points

- **Current state**: Local-first (WPF desktop app, Excel add-in, local event store, MSI installer)
- **Target state**: Cloud-hosted (multi-tenant, firm-level tenancy, cloud event store)
- **Timeline**: April 2026 cloud launch
- **Preserved**: Event-sourced architecture, WPF + Excel dual interface, AI capabilities
- **Added**: Cloud storage, team collaboration as first-class, cloud-to-cloud integrations

## Migration Scope

### What Changes
- Event store → cloud-hosted persistence
- Multi-tenant architecture (firm = tenant)
- Authentication and authorization for cloud access
- Cloud-to-cloud QB/Xero integration
- Team features become primary (not optional sync)

### What's Preserved
- Event-sourced domain model (ReactiveDomain)
- WPF primary interface
- Excel for advanced modeling
- AI capabilities (Semantic Kernel)
- Audit trail and reproducibility

## Strategic Importance

- **April 2026 is GTM deadline**: Cloud deployment is critical path
- **Firms are teams**: Multi-tenant architecture enables firm-level collaboration
- **QB/Xero integration**: Cloud-to-cloud is simpler than local-to-cloud
- **Scalability**: Cloud enables association-driven distribution at scale

## Risks

- **Aggressive timeline**: 2 months to migrate (Feb-April 2026)
- **Architecture complexity**: Local-first to cloud-first is not trivial
- **Data migration**: Existing local data must transition cleanly
- **Performance**: Cloud latency must not degrade user experience
