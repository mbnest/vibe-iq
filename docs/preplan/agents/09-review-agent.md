# Review Agent

**Pipeline step:** 9. Review · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Independent review of spec + diff, run from different context than the implementer, specifically to catch what testing and self-review miss and to avoid correlated blind spots between an agent and the agent checking its own work.

## Inputs

- `spec.md`, diff, test report

## Outputs

- Review findings / approval recommendation

## Functions

- Code review (correctness, readability, style)
- Design/architecture conformance check
- Spec-conformance check (does the diff actually satisfy the spec)
- Documentation review
- Static analysis / automated review gates

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Changes to code paths following established conventions, no security-sensitive surface. | Full autonomy — review and recommend directly. |
| Known + Risky | Changes to auth, billing, or other high-stakes-but-familiar paths. | Full independent review completed autonomously; final approval still requires human sign-off. |
| Unknown + Safe | An isolated, low-stakes new module. | Autonomous review; audited after, not gated before. |
| Unknown + Risky | Novel architecture or logic with unclear correctness criteria. | Manual — agent review used as supplementary input only; a human reviewer leads. |

## Escalation Triggers

- Change touches a security-sensitive surface
- Agent's own confidence in its review is low
- Spec-conformance check fails or is ambiguous

## Handoffs

- **Receives from:** [Test Agent](08-test-agent.md) (step 8).
- **Delivers to:** [Merge & Release Approval Gate](10-merge-release-approval-gate.md) (step 10).
