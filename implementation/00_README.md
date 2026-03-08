# Implementation Vault — PowerModels Graph Index

Programmatically generated codebase index of the PowerModels ES/CQRS architecture. Agents consume these files as pre-indexed context instead of grepping the codebase each run.

**Source:** PowerModels codebase (via `implementation-vault/PowerModels-src` junction)
**Generator:** `dotnet run -- scan-codebase --config <path>/graph-vault.json`
**Config:** `graph-vault.json` (in project root, resolved via junction)

## Files

| File | Content | Stability |
|------|---------|-----------|
| `domain-model.md` | Logical business concepts, entity hierarchy, counterparty model | Stable — changes when business concepts change |
| `application-topology.md` | Hosts, databases, per-business isolation, runtime composition | Very stable — changes with new hosts or DB topology |
| `project-architecture.md` | Dependency graph, layer rules, package distribution | Semi-static — validate on run |
| `message-bus-topology.md` | Bus types, two-bus pattern, message flow, namespaces | Semi-static — validate on run |
| `isolation-acl.md` | Context hierarchy, read/write separation, isolation guarantees | Semi-static — validate on run |
| `data-usage-and-storage.md` | Stream types, write volumes, read amplification, persistence format, growth estimates | Semi-static — validate when new aggregates or read models added |
| `feature-provenance.md` | Feature stories → artifact mapping, milestone tracking, author distribution | Updates when new milestones complete |
| `saga-catalog.md` | Cross-aggregate coordination patterns, workflow chains, implicit process managers | Updates when new cross-context features are added |
| `defect-analysis.md` | Defects mapped to contexts, handlers, read models; recurring patterns | Updates per milestone |
| `aggregates.md` | All aggregates with locations, sizes, responsibilities | Changes with feature work — full scan each run |
| `message-map.md` | Command → handler → events routing map | Changes with feature work — full scan each run |
| `read-models.md` | Read models with event subscriptions and output shapes | Changes with feature work — full scan each run |
| `bounded-contexts.md` | Project → aggregate → service ownership map | Changes with feature work — full scan each run |
| `test-coverage.md` | Aggregate test coverage with gap tracking | Changes with feature work — full scan each run |
| `god-aggregate.md` | ServerFinancialModel tracking and expansion triggers | Changes with feature work — full scan each run |
| `generation-log.md` | Audit trail of generation runs | Append-only |

## Agent Consumption

| Agent | Primary Files | Purpose |
|-------|--------------|---------|
| planner | domain-model, aggregates, message-map, message-bus-topology | "What domain concepts exist?" |
| plan-architect | domain-model, bounded-contexts, god-aggregate, project-architecture, isolation-acl, data-usage-and-storage | Boundary enforcement, scaling risk |
| implementer | domain-model, message-map, read-models, isolation-acl, data-usage-and-storage | Handler routing, patterns, stream impact |
| architecture-reviewer | domain-model, bounded-contexts, god-aggregate, project-architecture, isolation-acl, data-usage-and-storage | Cross-boundary leakage, performance |
| data-usage-reviewer | data-usage-and-storage, aggregates, read-models, god-aggregate, defect-analysis | Stream growth, read amplification, scaling, perf defects |
| plan-architect | ..., saga-catalog, feature-provenance | Cross-context feature history, saga boundaries |
| architecture-reviewer | ..., saga-catalog, defect-analysis | Saga boundary violations, defect hotspots |
| code-quality-reviewer | aggregates, test-coverage, message-bus-topology | Pattern compliance |
| work-decomposer | domain-model, aggregates, test-coverage, bounded-contexts | EXISTS/PARTIAL/MISSING classification |

## Regeneration

```bash
# From md-planner/src/VaultTool:
dotnet run -- project-refresh --config ../../projects/PowerModels/graph-vault.json
```
