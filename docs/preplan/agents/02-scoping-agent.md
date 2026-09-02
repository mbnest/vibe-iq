# Scoping Agent

**Pipeline step:** 2. Prioritization & Scoping · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Turn a candidate opportunity or direct stakeholder ask into a scoped, prioritized ask ready for engineering intake. The agent drafts the decision inputs; the prioritization decision itself remains a business call owned by the vertical's product function.

## Inputs

- Opportunity report (from the Signal Agent) or a direct stakeholder request
- Current roadmap and OKRs
- Budget and staffing constraints
- Cross-team dependency map

## Outputs

- Scoped-ask summary: effort/ROI estimate, dependency map, roadmap-fit assessment.

## Functions

- Business case / ROI estimation drafting
- Effort sizing (t-shirt sizing, rough estimation)
- Roadmap placement and sequencing analysis
- Cross-team dependency mapping
- OKR/goal alignment check
- Budget and staffing allocation drafting

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Sizing and roadmap-fit analysis using established estimation models and existing roadmap data. | Full autonomy — draft and publish the scoped-ask summary directly. |
| Known + Risky | The ask would reprioritize other teams' committed work (cross-team resourcing conflict). | Autonomous drafting of the tradeoff analysis; the reprioritization decision itself is human-gated. |
| Unknown + Safe | A small, isolated, low-cost experiment with no roadmap conflict. | Autonomous drafting and provisional scoping. |
| Unknown + Risky | Budget, legal, or compliance impact is unclear. | Manual — human scopes it. |

## Escalation Triggers

- Ask would reprioritize another team's committed work
- Budget impact exceeds a defined threshold
- Conflicting stakeholder priorities surface during scoping

## Handoffs

- **Receives from:** [Signal Agent](01-signal-agent.md) (step 1), or a direct stakeholder ask entering the process independently.
- **Delivers to:** [Intake Agent](03-intake-agent.md) (step 3), once the product function has made the prioritization decision.
