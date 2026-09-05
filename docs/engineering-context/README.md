# Engineering Context Pack

*The organization's standing engineering rules — what is true for every project regardless of the specific ask. The pipeline's agents read these files at the stages noted below. These are living documents: they always reflect the current rules. The reasoning behind them lives in [`../adr/`](../adr/) (the append-only decision log); these files are the current-state view derived from it.*

## Files

| File | What it defines | Read at |
|---|---|---|
| [`allowed-stack.md`](allowed-stack.md) | Approved languages, frameworks, datastores, cloud services | Design / Solution |
| [`architecture-principles.md`](architecture-principles.md) | How systems are structured here | Design / Solution |
| [`constraints.md`](constraints.md) | Regulatory and policy obligations, each with an ID and an applies-when rule | Intake (selects the ones relevant to an ask) |
| [`coding-standards.md`](coding-standards.md) | Code-craft expectations that linters can't enforce | Implementation / Review |
| [`glossary.md`](glossary.md) | Ubiquitous language — business term to code/data term | All stages; especially requirement translation |

## How agents use it

- Every generated artifact (`spec.md`, `design.md`, ...) records in its frontmatter which versions of these files it was built against. When a file changes, downstream artifacts built against the old version are flagged stale.
- `constraints.md` is the only file agents *query* rather than just read — the Intake agent selects constraints whose applies-when rule matches an ask and cites them by ID in `spec.md`.

## Editing

A consequential change is recorded as an ADR first, then the relevant file is updated to match. Trivial additions (a glossary term, a clarifying sentence) do not need an ADR.

*Status: starter pack, 2026-09-03. Placeholder content is marked inline. Not reviewed.*
