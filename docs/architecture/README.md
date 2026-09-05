# Architecture

How the platform is put together. Start with [`../overview.md`](../overview.md) for the whole picture in one read; come here for detail on a specific concern.

| Doc | Covers |
|---|---|
| [`process-flow.md`](process-flow.md) | The end-to-end lifecycle: the 13-step build cycle, the master diagram, external dependencies the pipeline assumes, and the autonomy framework's home |
| [`front-of-flow.md`](front-of-flow.md) | The front section in detail: the L1–L4 requirement-levels model, the derived artifact chain, the initiative → feature → story hierarchy, and the fan-out |
| [`data-model.md`](data-model.md) | Artifacts as data: request / change_request, versioning, provenance + level tags, the artifact store, and the sidecar-metadata pattern for external tooling |
| [`runtime.md`](runtime.md) | Orchestration substrate, hosting tiers, per-stage model selection, and the CLI-vs-web interface |

**Status:** these are promoted from the working notes and carry conversational framing in places. They are the current design position; the reasoning trail is in [`../notes/`](../notes/). Nothing here is reviewed or committed to build.

## Cross-cutting, documented elsewhere

- **Autonomy tiering** — the Known/Unknown × Safe/Risky model: [`../policies/autonomy-tiering.md`](../policies/autonomy-tiering.md)
- **Requirement change** — what happens when a signed artifact changes: [`../policies/requirement-change-handling.md`](../policies/requirement-change-handling.md)
- **Rules** — pipeline-governance logic vs. business rules in the built product: [`../policies/rules-and-policy.md`](../policies/rules-and-policy.md)
- **Engineering context pack** — the standing rules agents read at each stage: [`../engineering-context/`](../engineering-context/)
