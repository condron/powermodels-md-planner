---
type: facet
id: agentic-development
label: Agentic Development Workflow
synonyms:
  - multi-agent system
  - AI-powered development
  - agent-driven workflow
  - skill dispatching
  - GitHub Copilot agents
  - automated code review
  - workflow-as-code
links_to_facets:
  - ai-empowerment-strategy
  - event-sourced-architecture
  - accounting-reports
claims:
  - Multi-agent system is the primary development model — not an enhancement, the core workflow
  - 15+ specialized agents covering the full development lifecycle from issue creation through 4-way parallel review
  - Skills encode domain knowledge — reactive-domain-es-ddd, accounting-reports, business-loader, test-review
  - report-builder agent can implement a complete accounting report from spec to passing tests autonomously
  - Two-tier epic branch model integrates with agent workflow — draft PRs, fast-forward merges, CI gates
  - 4-way parallel review — correctness, code quality, security, and architecture reviewers run simultaneously
  - Agent system leverages the same event-sourced architecture knowledge that makes the codebase predictable
evidence_refs:
  - technical-codebase-review
owner: technical-team
status: active
---

# Agentic Development Workflow

## Description

PowerModels uses a multi-agent system as its primary development model. Agent definitions in `.github/agents/` and skills in `.github/skills/` encode domain-specific knowledge about the ES/CQRS architecture, enabling AI agents to plan, implement, review, and ship features with minimal human intervention. This is not an AI enhancement to a human workflow — it is the workflow.

## Key Points

- **15+ agents**: feature-builder (coordinator), planner, plan-architect, implementer, implementation-reviewer, 4 specialized reviewers, report-builder, test-refactor, dependency-tracker, proposal-decomposer, work-decomposer
- **11 skills**: reactive-domain-es-ddd, csharp-xunit-reactive-domain, accounting-reports, business-loader, test-review, skill-creator, and more
- **Full lifecycle**: issue → plan → implement → 4-way parallel review → merge
- **Report-builder agent**: Can implement a complete accounting report (RM + tests + XAML + FlaUI verification) from a spec

## Agent Architecture

```
Human creates Issue
  → feature-builder (coordinator)
    → dependency-tracker (checks prerequisites)
    → planner (creates implementation plan)
    → plan-architect (validates against codebase patterns)
    → implementer (writes code)
    → implementation-reviewer (coordinates parallel review)
      ├── correctness-reviewer
      ├── code-quality-reviewer
      ├── security-reviewer
      └── architecture-reviewer
    → merge + close
```

## Branching Integration

Two-tier epic branch model:
- `main` → `epic/{epic-slug}` (Draft PR) → `{username}/{issue-slug}` (no PR, rebase + ff merge)
- CI Full skips Draft PRs; CI Accounting runs on all PRs including drafts
- Agents work on issue branches, human coordinates epic-level merges

## Implementation Reference

Agents in `.github/agents/`. Skills in `.github/skills/`. Workflow documentation in `docs/agentic-workflow/`.
