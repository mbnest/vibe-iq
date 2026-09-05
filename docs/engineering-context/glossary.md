# Glossary — Ubiquitous Language

*The shared vocabulary: one term, one meaning, across business conversation, specs, data model, and code. Read at every stage; central to translating requirements between levels without meaning drifting.*

## Convention

| Column | Meaning |
|---|---|
| **Term** | The word as used in business and product conversation |
| **Definition** | What it means here, precisely. Note if it differs from common usage. |
| **Code name** | The identifier used in code, schema, and API, if different from the term |
| **Notes** | Boundaries, common confusions, related terms |

A term enters the glossary the first time an ambiguity about it costs someone time. Adding a term does not need an ADR.

## Platform terms

| Term | Definition | Code name | Notes |
|---|---|---|---|
| Initiative | A prioritized business ask; the unit that enters the pipeline. One business brief, one product definition. | `initiative` | See [`front-of-flow.md`](../architecture/front-of-flow.md) |
| Feature | A coherent slice of an initiative carved out by product. The unit of merge. | `feature` | |
| Story | An implementation-sized work item. The unit of spec-and-build. | `story` | |
| Spec | The `spec.md` artifact for a story — requirements and acceptance criteria, tagged with provenance and level. | `spec` | Distinct from the product definition (L2, initiative-wide) |
| Provenance tag | Marker on a requirement: `stated` / `default` / `inferred` / `unresolved`. How solid it is. | — | |
| Level tag | Marker on a requirement: L1 to L4. How precise it is, and which persona's language. | — | See [`front-of-flow.md`](../architecture/front-of-flow.md) |

## Domain terms

*(Empty. Fills in as the first real initiative is worked.)*

*Status: starter, 2026-09-03. Platform terms seeded from the thinking docs; domain section intentionally empty.*
