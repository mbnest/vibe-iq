# Data Model — Artifacts as Data

*How the pipeline's artifacts are stored, versioned, linked, and tagged. Consolidates decisions scattered across [`front-of-flow.md`](front-of-flow.md), [`../policies/requirement-change-handling.md`](../policies/requirement-change-handling.md), [`../agents/definition-agent.md`](../agents/definition-agent.md), and [`../policies/rules-and-policy.md`](../policies/rules-and-policy.md). Draft — the underlying schema is still an open blocker (see the end).*

## The artifact chain

Each artifact is **derived** from the one above it, never hand-copied. A human reviews and signs a generated artifact; a human never transcribes content between documents.

| # | Artifact | Level | Signed by |
|---|---|---|---|
| 1 | Business brief | L1 | Business sponsor |
| 2 | Product definition | L2 | Business sponsor (mirror-back only) + product (augmentation) |
| 3 | Solution spec | L3 | Authoritative engineer / architect |
| 4 | Story specs (`spec.md`) | L3→L4 | Delivery team refines |
| — | `design.md`, `task-graph.json`, diff, test report, deployment record | — | per the build-loop gates |

**Upward traceability is mandatory.** Every product requirement links to a business need or is flagged "product-introduced." Every solution element links to a product requirement. Every story links to a solution element. This is what makes staleness computable.

## Versioning

- Every artifact is versioned; a change is a new version (`spec.md@v2`), never an overwrite of `v1`.
- Every derived artifact records which version of its parent it was built against (in frontmatter), plus which versions of the [engineering-context pack](../engineering-context/) files it used.
- Changing an artifact flags exactly which downstream artifacts are now stale — the mechanism [requirement-change-handling](../policies/requirement-change-handling.md) depends on.

## request / change_request

One table, type-discriminated:

- **`request` (`kind = initial`)** — the raw stakeholder ask. Versioned (the stakeholder can revise it).
- **`request` (`kind = change`)** — a change request. Carries `parent_request_id` (FK to the original) and `supersedes` (which artifact version it targets).
- Every **product-definition version** points to exactly one cause row — an `initial` request version or a `change` request version. A request version may have several product-definition versions under it (draft iterations + the committed one).

Chain: `request(initial)` → `product-definition v1` → `change_request` → `product-definition v2` → …

### Two thresholds, not one

| Threshold | What it gates |
|---|---|
| **Product-definition sign-off (freeze)** | Whether a modification is *draft churn* (no record) or a *change request* (creates a `change_request` row) |
| **Solution sign-off** | How a change request is *processed*: cheap re-loop vs. full blast-radius classification ([requirement-change-handling](../policies/requirement-change-handling.md)) |

The record is created earlier (at freeze) than the expensive flow engages (at solution sign-off). The gap between the two is real and cheap to handle.

## Per-requirement tags

Requirements are structured records, not just prose (the product definition especially — a form needs fields to bind to). Each requirement carries two orthogonal tags:

- **Provenance** — how solid it is: `[stated]` / `[default]` / `[inferred]` / `[unresolved]`. Defined by the [Intake Agent](../agents/intake-agent.md); reused at L2 with a documented rebinding for the no-live-sponsor case.
- **Level** — how precise it is: L1 / L2 / L3 / L4. Real artifacts are mixed-level. **Level tags inform the layer owner; they do not bind** — the owner keeps decision autonomy, the tag is a signal, never an automated gate.

Business rules carry a third pair ([rules-and-policy.md](../policies/rules-and-policy.md)): **volatility** (expected change frequency) and **placement** (`inline:<module>` or `external:<service>`), kept separate so drift is detectable.

## The artifact store

- **MVP:** git files behind a clean interface.
- **Later:** a real versioned store with referential validation on handoff (JSON Schema / Pydantic).
- **Sidecar-metadata pattern:** when artifact *content* lives in external product tooling (Jira, Aha!, Productboard), keep the derivation links, version lineage, level/provenance tags, and staleness flags in our store, keyed to their object IDs. That is the minimum ownership that keeps the chain coherent regardless of where the prose lives. See [`../notes/product-definition-integration-options.md`](../notes/product-definition-integration-options.md).

## Open blocker

The **`spec.md` schema itself is undecided** — prose vs. enumerable, ID'd acceptance criteria — and is assumed by provenance tagging, level tagging, constraint citation, and change classification. This is the named top blocker to producing a first real spec — see [`../decisions/open-questions.md`](../decisions/open-questions.md).
