# PowerModels — Project Vault

Vault data for the PowerModels financial preparation & reporting platform, managed by [md-planner](https://github.com/condron/md-planner).

## Structure

```
powermodels-md-project/
├── planning/          ← md-planner Q-analysis (STRATEGIC: what + who + why)
│                        Facets, audiences, intersections, deliverables
│                        Consumed by: work-decomposer agent
│
└── implementation/    ← Graph vault (STRUCTURAL: what exists in code)
                         Aggregates, messages, handlers, read models
                         Consumed by: planner, implementer, reviewers
```

### planning/

Source Notes → Cards → Intersection Specs → Deliverables workflow. Contains facet cards, audience cards, evidence cards, intersection specs, and generated deliverables. Answers **"what should we build and for whom?"**

### implementation/

Programmatically generated codebase index of ES/CQRS artifacts and their relationships. Agents consume this vault as pre-indexed context instead of grepping the codebase every run. Answers **"what have we already built and how is it structured?"**

## Usage with md-planner

This repo is designed to be linked into md-planner's `projects/` directory via a Windows junction:

```powershell
# From the md-planner repo root:
cmd /c "mklink /J projects\PowerModels C:\path\to\powermodels-md-project"
```

Then VaultTool commands work normally (from `src/VaultTool`):

```powershell
# Planning vault commands
dotnet run -- list-status --vault ../../projects/PowerModels/planning
dotnet run -- validate --vault ../../projects/PowerModels/planning
dotnet run -- geometry-summary --vault ../../projects/PowerModels/planning
dotnet run -- compute-intersection --all --vault ../../projects/PowerModels/planning

# Implementation vault: refresh scan + drift detection
dotnet run -- project-refresh --config ../../projects/PowerModels/graph-vault.json
```

### Junction Constraint

`graph-vault.json` uses relative paths (e.g., `../../implementation-vault/PowerModels-src`) that resolve correctly only when accessed through the md-planner junction at `projects/PowerModels/`. Running the config directly from this repo's root will resolve paths incorrectly. Always invoke VaultTool commands from `md-planner/src/VaultTool`.
