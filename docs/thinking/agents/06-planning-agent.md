# Planning Agent

**Pipeline step:** 6. Planning / Breakdown · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Decompose an approved design into a sequenced, assignable task graph — the mechanical bridge between "what we're building" and "what work happens in what order."

## Inputs

- `design.md`

## Outputs

- `task-graph.json` — tasks, estimates, dependencies, parallelization plan.

## Functions

- Task decomposition
- Per-task estimation
- Dependency sequencing and critical-path identification
- Parallelization strategy (what can run concurrently)
- Risk identification and mitigation planning

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Decomposing a design that follows established patterns, using historical estimation data. | Full autonomy — produce the task graph directly. |
| Known + Risky | Planning around a high-risk shared dependency (e.g., a library many services depend on). | Autonomous drafting; sequencing/rollout plan reviewed before it drives real merges. |
| Unknown + Safe | Planning for an isolated new module with no existing dependents. | Autonomous drafting; reviewed after, not gated before. |
| Unknown + Risky | Novel architecture with an unclear dependency graph. | Manual — human plans it, agent assists with drafting only. |

## Escalation Triggers

- Dependency graph touches shared or critical infrastructure outside this change's normal domain
- Estimation confidence is low due to lack of precedent

## Handoffs

- **Receives from:** [Design Agent](05-design-agent.md) (step 5).
- **Delivers to:** [Implementation Agent(s)](07-implementation-agent.md) (step 7), per task-graph node.
