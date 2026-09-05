# Policies

Cross-cutting rules that every agent and gate is subject to. Distinct from the [engineering-context pack](../engineering-context/), which is the standing rules for the *products the platform builds* — these are the rules for how the *platform itself* operates.

| Doc | Covers |
|---|---|
| [`autonomy-tiering.md`](autonomy-tiering.md) | The Known/Unknown × Safe/Risky model that governs how much any agent may do on its own, applied per task-graph node |
| [`requirement-change-handling.md`](requirement-change-handling.md) | What happens when a signed artifact changes: dimensions, the three-response model, the agentic angle, the request / change_request model |
| [`rules-and-policy.md`](rules-and-policy.md) | Two unrelated "rules" concerns: pipeline-governance logic (autonomy tiering, constraint selection, gap routing, change classification) vs. business rules that live in the built product |

**Status:** promoted from working notes; the reasoning trail is in [`../notes/`](../notes/). MVP stance for all three: implement each decision point as a small pure function behind a clean interface; adopt a decision engine only once the logic stabilizes against real cases and a non-engineer needs to own it.
