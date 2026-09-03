# Requirement Change Handling

*Working notes, 2026-09-03. Open topic — this document is a starting position for a later dedicated discussion, not a policy. It refines, and will eventually replace, the "Requirement Change Management" section in [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md), which was written for single-story asks. Output of a pre-planning conversation between Matt and Claude.*

## Where this stands

The process-flow doc has a v1 policy: version `spec.md`, classify a change by blast radius into clarification / additive / material / fundamental, and re-enter the pipeline at a depth matching the class. That taxonomy was written before the initiative/feature/story structure and the artifact chain (see [`front-of-flow.md`](front-of-flow.md)), and before thinking through how agentic execution changes the picture. This document holds the refined thinking.

## Two things to keep separate

- **What *should* happen** — a clean policy.
- **What actually happens in real teams** — changes arrive late, informally, mid-implementation, and often without anyone re-opening the spec. Any policy that ignores this will be routed around. The design needs to make the right path the path of least resistance, not just document it.

## Dimensions that determine the response

Blast radius alone is not enough. Four dimensions together:

1. **Blast radius** — how much existing work the change invalidates (the original dimension). Computed, not estimated — see the agentic angle below.
2. **Timing / progress** — where the pipeline is when the change lands. The same change is cheap during solutioning and expensive when most stories are merged.
3. **Reversibility of work already done** — throwing away agent-generated code that has not shipped is not the same as unwinding a migration already in production. Sunk cost is not uniform.
4. **Urgency of the change itself** — a discovered compliance gap forces action regardless of blast radius; a refinement does not.

## Three responses (not two)

- **Absorb in place.** The change lands while the affected work is still spec or solution — not yet built. Update the not-yet-started nodes and carry on. Cheapest and most common for small changes caught early. Nothing to refactor, because nothing was built wrong.
- **Halt and re-solution.** The change invalidates the direction; continuing would produce throwaway work. Rewind to the last good artifact above the affected node and replay forward.
- **Deliver original, then refactor.** Only when the original is *independently valuable* and the change is additive. **The trap:** "refactor later" is where debt goes to die. If this path is chosen, the follow-up refactor must be a committed, scheduled work item with its own ticket — not an intention. Otherwise this is just "ship it wrong and hope."

Matt's initial framing was a two-path model (halt-and-re-solution vs. deliver-then-refactor). "Absorb in place" is split out because it is genuinely distinct and is the common case for changes caught early.

## The agentic angle — it cuts both ways

- **Redo is cheap.** A re-solution that costs a human team days and morale might cost the pipeline an hour. This *lowers the threshold* for "just redo it right" — the pipeline should choose halt-and-re-solution more readily than a human org would.
- **Fan-out is fast.** By the time a change is noticed, the pipeline may have generated many story specs and several implementations. Blast radius grows faster than a human team would have produced it — so **change detection and classification need to be fast, ideally automatic**.
- **The enabling mechanism already exists in the design:**
  - the **dependency graph** lets the affected subgraph be *computed* precisely rather than estimated;
  - **durable versioned artifacts** (companion decision #2) let the pipeline rewind to the last good state and replay forward;
  - **level + provenance tags plus the artifact chain's upward traceability** let a change to an L1 need automatically flag every derived L2/L3/L4 artifact as stale.
- **Who does what:** an agent computes the impact (affected subgraph + stale artifacts + a proposed classification); a human confirms the classification and picks the response — consistent with the autonomy framework (agent proposes, human decides anything above a clarification).

## Open questions for the dedicated session

- How the three-response model maps onto the initiative/feature/story fan-out — a change can hit the initiative solution, one feature, or one story, and the response differs.
- Does a material change reopen the solution sign-off gate, or only the affected feature's solution?
- What "fast automatic detection" concretely requires — where change signals enter (ticket edits, stakeholder messages, new asks that overlap in-flight work).
- Whether the four-class taxonomy (clarification / additive / material / fundamental) survives as the classification, feeds the three-response choice, or is replaced.
- Real-world adoption: what makes the in-system change path lower-friction than a side channel.
