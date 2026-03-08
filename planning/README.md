# PowerModels — Planning Vault

Vault data for the PowerModels financial preparation & reporting platform, managed by [md-planner](https://github.com/condron/md-planner).

Implements a **Source Notes → Cards → Intersection Specs → Deliverables** workflow for creating consistent, audience-targeted documents.

## Structure

```
A_facets/           23 curated facet cards (aspects/themes)
B_audiences/        11 curated audience cards (stakeholders + concerns)
E_evidence/          7 curated evidence cards (data, studies, metrics)
I_intersections/   253 computed intersection specs (facet × audience Q-analysis)
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

Then VaultTool commands work normally (from `src/VaultTool`):

```powershell
dotnet run -- list-status --vault ../../projects/PowerModels/planning
dotnet run -- validate --vault ../../projects/PowerModels/planning
dotnet run -- geometry-summary --vault ../../projects/PowerModels/planning
dotnet run -- compute-intersection --all --vault ../../projects/PowerModels/planning
```

## Workflow

1. **Capture Source Notes** — Add raw notes to `S_sources/` or use `ingest-sources` to import them.
2. **Structure into Cards** — Run `normalize-cards` to create candidates in `C_cards_inbox/`. Edit YAML front matter or use `prompts/structuring_prompt.md`.
3. **Curate Cards** — Move from `C_cards_inbox/` to `A_facets/`, `B_audiences/`, or `E_evidence/`.
4. **Compute Intersections** — Run `compute-intersection` for each facet × audience pair.
5. **Generate Deliverables** — Use intersection specs with `prompts/targeted_writeup_prompt.md`. Save in `D_deliverables/`.

## Naming Conventions

- **IDs**: kebab-case (e.g., `menu-design`, `potential-investors`)
- **Files**: Named after their ID (e.g., `menu-design.md`)
- **Intersections**: Named `<facet>_x_<audience>.md`

## Card Types

- **Facet Card** — An aspect, topic, or feature. Contains claims, synonyms, and links to related facets.
- **Audience Card** — A stakeholder segment. Contains concerns, vocabulary preferences, and risk tolerance.
- **Evidence Card** — Supporting data for claims. Links to the claims it supports.
- **Intersection Spec** — Computed analysis of how a facet relates to an audience. Contains hooks (connection points), q-holes (gaps), and suggested bridges.

## Separation Rule

- `_documents/` — input material that feeds the vault pipeline
- `_output/` — generated analysis and review documents (not input material)
