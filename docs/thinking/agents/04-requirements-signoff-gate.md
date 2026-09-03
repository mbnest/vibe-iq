# Requirements Sign-off — Human Gate

**Pipeline step:** 4. Requirements Sign-off · [← back to master](../sdlc-business-process-flow.md)

## Purpose

A human confirms the spec reflects what was actually wanted before real design work starts. This is deliberately positioned as the cheapest point in the pipeline to catch a misunderstanding — every downstream step compounds the cost of an error made here.

## Why this stays human

Whether a written spec matches someone's actual intent is a judgment and alignment problem, not a pattern-matching problem. No amount of agent confidence substitutes for the stakeholder confirming "yes, this is what I meant" — and because the cost of getting this wrong compounds through design, planning, implementation, testing, and review, this is intentionally the first of the pipeline's two human checkpoints.

## What an agent can still do here

- Summarize the spec for fast human review
- Flag ambiguous or underspecified sections
- Highlight deltas from prior, related specs
- Prepare the review packet so the human's decision is fast and well-informed, without removing their judgment from the loop

## Handoffs

- **Receives from:** [Intake Agent](03-intake-agent.md) (step 3) — `spec.md`.
- **Delivers to:** [Design Agent](05-design-agent.md) (step 5), on approval.
