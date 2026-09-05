# Test Agent

**Role:** Testing · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

## Purpose

Verify the implementation against the spec, independent of the diff — so the check doesn't grade the implementation against itself.

## Inputs

- `spec.md`
- Diff (from Implementation)

## Outputs

- Test report

## Functions

- Unit tests
- Integration tests
- Regression tests
- Data quality tests
- Formatting/lint gate adherence
- Coverage analysis

*Note:* user acceptance testing, compliance testing, and testing categories beyond standard SAST/DAST (e.g., penetration testing) typically route to humans or to the existing sibling enablers rather than to this agent — see Escalation Triggers.

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Standard test suites against known code paths. | Full autonomy — run and report directly. |
| Known + Risky | Tests for a Known+Risky implementation node (auth, billing, etc.). | Autonomous test execution; results reviewed as part of the human-gated merge decision. |
| Unknown + Safe | Tests for genuinely new, low-stakes code. | Autonomous execution; reviewed after, not gated before. |
| Unknown + Risky | Novel and high-stakes code paths. | Manual — human-designed test plan, agent assists with execution only. |

## Escalation Triggers

- Requires compliance/regulatory test categories
- Requires actual user judgment (UAT)
- Requires testing beyond standard SAST/DAST (e.g., penetration testing)

## Handoffs

- **Receives from:** [Implementation Agent(s)](implementation-agent.md).
- **Delivers to:** [Review Agent](review-agent.md).
