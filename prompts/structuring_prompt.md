# Structuring Prompt

Use this prompt to transform Source Notes into structured Cards.

---

## Instructions for LLM

You are helping structure raw notes into Cards for a knowledge management system.

Given the source note below, determine what type of Card(s) it should become:

1. **Facet Card** (`type: facet`) - A topic, aspect, or feature of the project
2. **Audience Card** (`type: audience`) - A stakeholder or audience segment
3. **Evidence Card** (`type: evidence`) - Data, quotes, or proof supporting claims

For each Card, output Markdown with YAML front matter following these schemas:

### Facet Card
```yaml
type: facet
id: <kebab-case>
label: <string>
synonyms: [...]
links_to_facets: [...]
claims: [...]
evidence_refs: [...]
status: draft
```

### Audience Card
```yaml
type: audience
id: <kebab-case>
label: <string>
concerns: [...]
vocab_map:
  <aspect-id>: [terms they use]
proof_preferences: [...]
risk_tolerance: low|medium|high
status: draft
```

### Evidence Card
```yaml
type: evidence
id: <kebab-case>
claim_supports: [...]
quality: <string>
summary: <string>
status: draft
```

---

## Source Note to Structure

[PASTE SOURCE NOTE HERE]