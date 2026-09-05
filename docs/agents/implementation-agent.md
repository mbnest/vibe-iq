# Implementation Agent(s)

**Role:** Implementation · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Build the change per the task graph, running as parallel branches where the task graph allows it.

## Inputs

- `task-graph.json`

## Outputs

- Diff(s), one or more per task-graph branch.

## Functions

- Coding against the task graph
- Local/dev-loop testing
- Style and formatting adherence (lint)
- Feature-flagging for risky changes
- Inline documentation and README updates
- Dependency/library version management
- Branch management and conflict resolution

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Code matching existing conventions and modules. | Full autonomy — implement directly. |
| Known + Risky | Changes to auth, billing, or other core data paths. | Autonomous implementation, human-gated before merge (ties into the step 10 gate). |
| Unknown + Safe | A new, isolated module with no existing dependents. | Autonomous implementation; reviewed after, not gated before. |
| Unknown + Risky | Novel and high-stakes code paths. | Manual — human implements, agent assists only. |

**Granularity note:** autonomy is assigned per task-graph node, not per request. A change that's mostly known-safe with one novel node should let the known-safe branches proceed autonomously while only the novel node — and anything downstream of it — waits on a human.

## Escalation Triggers

- Task touches a node classified Known+Risky or Unknown+Risky in the task graph
- Implementation reveals a dependency the design/planning phases didn't anticipate

## Handoffs

- **Receives from:** [Planning Agent](planning-agent.md).
- **Delivers to:** [Test Agent](test-agent.md) and [Review Agent](review-agent.md).
