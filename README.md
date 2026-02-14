# PowerModels — md-planner Project Vault

Vault data for the PowerModels financial preparation & reporting platform, managed by [md-planner](https://github.com/condron/md-planner).

## Structure

```
A_facets/           16 curated facet cards (aspects/themes)
B_audiences/        10 curated audience cards (stakeholders + concerns)
E_evidence/          4 curated evidence cards (data, studies, metrics)
I_intersections/   160 computed intersection specs (facet × audience Q-analysis)
D_deliverables/      Generated audience-targeted documents
S_sources/           Raw ingested source notes
C_cards_inbox/       Normalized cards awaiting curation
_documents/          Input source material (research, business plans, interviews)
_output/             Generated analysis documents (reviews, summaries)
templates/           Card templates
prompts/             LLM prompts for structuring
```

## Usage with md-planner

This repo is designed to be linked into md-planner's `projects/` directory via a Windows junction:

```powershell
# From the md-planner repo root:
cmd /c "mklink /J projects\PowerModels C:\path\to\powermodels-md-project"
```

Then VaultTool commands work normally:

```powershell
dotnet run -- list-status --vault ../../projects/PowerModels
dotnet run -- validate --vault ../../projects/PowerModels
dotnet run -- geometry-summary --vault ../../projects/PowerModels
dotnet run -- compute-intersection --all --vault ../../projects/PowerModels
```

## Separation Rule

- `_documents/` — input material that feeds the vault pipeline; generated docs that are part of input processing stay here
- `_output/` — generated analysis and review documents (not input material)
