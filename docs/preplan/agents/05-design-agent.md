# Design Agent

**Pipeline step:** 5. Design · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Turn an approved spec into a technical design ready for planning.

## Inputs

- `spec.md`

## Outputs

- `design.md` — architecture, API/schema design, documented tradeoffs (ADRs).

## Functions

- System/architecture design
- API and interface design
- Data schema design
- Threat modeling / security design pass
- Scalability and performance design
- Tradeoff documentation (ADRs)
- UX/UI design, where applicable

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Extending an established architectural pattern. | Full autonomy — draft `design.md` directly. |
| Known + Risky | A new service touching shared or critical infrastructure. | Autonomous drafting, human-gated approval before it drives planning. |
| Unknown + Safe | An isolated, experimental component with no shared dependents. | Autonomous drafting; reviewed after, not gated before. |
| Unknown + Risky | Novel architecture with unclear failure modes. | Manual — human designs it, agent assists with research and drafting only. |

## Escalation Triggers

- Design implies new external exposure or a new data classification
- Failure modes of the proposed architecture are not well understood
- Design touches infrastructure shared by other teams/verticals

## Handoffs

- **Receives from:** [Requirements Sign-off Gate](04-requirements-signoff-gate.md) (step 4).
- **Delivers to:** [Planning Agent](06-planning-agent.md) (step 6).
