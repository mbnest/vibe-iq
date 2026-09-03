# Merge & Release Approval — Human Gate

**Pipeline step:** 10. Merge / Release Approval · [← back to master](../sdlc-business-process-flow.md)

## Purpose

A human signs off before anything ships. This gate is forced anyway under the current assumption that enabler fulfillment is human-worked, not programmatic — but it's valuable as a checkpoint independent of that assumption too.

## Why this stays human

This is the last point before a change becomes real in production. The cost of an error here is the highest in the pipeline and, once released, is largely irreversible without triggering an incident. Regardless of how confident the upstream agents are — Implementation, Testing, Review — a human accountable for the outcome makes the final call before release.

## What an agent can still do here

- Compile a release-readiness summary: test results, review findings, risk notes, rollback plan
- Surface anything upstream agents flagged as Known+Risky or Unknown+Risky along the way
- Make the human's decision fast and well-informed, without removing their judgment from the loop

## Handoffs

- **Receives from:** [Review Agent](09-review-agent.md) (step 9).
- **Delivers to:** [Enablement Agent](11-enablement-agent.md) (step 11), on approval.
