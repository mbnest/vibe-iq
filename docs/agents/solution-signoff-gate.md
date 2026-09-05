# Solution Sign-off — Human Gate

**Role:** Solution Sign-off (human gate) · [pipeline order](README.md) · [master diagram](../architecture/process-flow.md)

Front of the flow — after solution analysis, before feature / story breakdown. The third human gate, alongside [Requirements Sign-off](requirements-signoff-gate.md) and [Merge & Release Approval](merge-release-approval-gate.md).

## Purpose

A human with authority confirms the proposed solution is sound and feasible before it becomes the constraint boundary that story generation depends on. Catching an architecture mistake here — before dozens of story tickets are written against a bad design — is where the gate earns its cost.

## Why this stays human

Whether a solution is sound is an engineering-judgment call with long-range consequences: every story in the initiative inherits it. The agent drafts the entire solution (architecture, technology choices, decision points, assumptions, alternatives considered, diagrams / pseudo-code); a human equipped to judge it — senior engineer, engineering manager, or architect — clears it. This is the autonomy framework's "autonomous prep, human-gated execution" quadrant.

## What an agent can still do here

- Produce the full proposed solution and the alternatives considered
- Surface assumptions and open decision points explicitly
- Prepare the review packet so the human's decision is fast and well-informed

## The threshold it sets

Solution sign-off is the boundary between the cheap and expensive paths for a requirement change:

- A change **before** solution sign-off re-runs the definition loop and redoes the solution draft — nothing downstream exists yet.
- A change **after** solution sign-off routes through [requirement-change-handling](../policies/requirement-change-handling.md) — blast-radius classification, then absorb / halt-and-re-solution / deliver-then-refactor.

See [`../decisions/log.md`](../decisions/log.md) (D-PD-14) and [`definition-agent.md`](definition-agent.md).

## Open questions

- What exactly is the reviewer signing — "this is the *right* solution" or "this is a *sound* solution, proceed"?
- Does solution analysis (and therefore this gate) run once per initiative, or also per feature?

## Handoffs

- **Receives from:** solution analysis (agent-drafted L3 solution spec).
- **Delivers to:** feature / story generation, then the per-story build loop starting at [Intake](intake-agent.md).
