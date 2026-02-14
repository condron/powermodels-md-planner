# Vault System README

This vault implements a **Source Notes → Cards → Intersection Specs → Deliverables** workflow for creating consistent, audience-targeted documents.

## Artifact Flow

```
Source Notes (raw)
      ↓
    Cards (structured)
      ↓
Intersection Specs (computed)
      ↓
  Deliverables (generated)
```

## Folder Structure

| Folder | Purpose |
|--------|---------|
| `S_sources/` | Raw source notes (unstructured Markdown/text) |
| `C_cards_inbox/` | Card candidates awaiting structuring or curation |
| `A_facets/` | Curated Facet Cards (aspects/topics) |
| `B_audiences/` | Curated Audience Cards (stakeholder segments) |
| `E_evidence/` | Curated Evidence Cards (supporting data) |
| `I_intersections/` | Computed Intersection Specs (aspect × audience) |
| `D_deliverables/` | Generated deliverables (1-pagers, emails, etc.) |
| `templates/` | Templates for creating new Cards |
| `prompts/` | LLM prompts for structuring and generation |

## Commands

### Initialize a vault
```bash
vaulttool init --vault <path>
```

### Ingest source notes
```bash
vaulttool ingest-sources --in <source-dir> --vault <path>
```

### Normalize cards
```bash
vaulttool normalize-cards --vault <path> [--in <dir>]
```

### Compute intersection
```bash
vaulttool compute-intersection --vault <path> --aspect <id> --audience <id>
```

### List status
```bash
vaulttool list-status --vault <path>
```

## Workflow

### 1. Capture Source Notes
Add raw notes to `S_sources/` or use `ingest-sources` to import them.

### 2. Structure into Cards
Run `normalize-cards` to create Card candidates in `C_cards_inbox/`.
Then either:
- Manually edit to add proper YAML front matter
- Use the LLM prompt in `prompts/structuring_prompt.md`

### 3. Curate Cards
Move structured Cards from `C_cards_inbox/` to the appropriate folder:
- Facet Cards → `A_facets/`
- Audience Cards → `B_audiences/`
- Evidence Cards → `E_evidence/`

### 4. Compute Intersections
Run `compute-intersection` for each aspect × audience combination you need.

### 5. Generate Deliverables
Use the Intersection Spec with `prompts/targeted_writeup_prompt.md` to generate deliverables.
Save results in `D_deliverables/`.

## Naming Conventions

- **IDs**: Use kebab-case (e.g., `menu-design`, `potential-investors`)
- **Files**: Name files after their ID (e.g., `menu-design.md`)
- **Intersections**: Named `<aspect>_x_<audience>.md`

## Card Types

### Facet Card
Represents an aspect, topic, or feature. Contains claims, synonyms, and links to related facets.

### Audience Card
Represents a stakeholder segment. Contains concerns, vocabulary preferences, and risk tolerance.

### Evidence Card
Represents supporting data for claims. Links to the claims it supports.

### Intersection Spec
Computed analysis of how a facet relates to an audience. Contains hooks (connection points), q-holes (gaps), and suggested bridges.