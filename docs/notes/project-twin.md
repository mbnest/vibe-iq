# Project Twin — Chat Bot Concept

*Working notes, 2026-09-04. Idea surfaced in a conversation between Matt and Claude, captured here as a build-ready concept for after the MVP — not scheduled, not started. Companion to [`architecture-napkin.md`](architecture-napkin.md) (where it eventually plugs in) and [`requirement-change-handling.md`](../policies/requirement-change-handling.md) (the change-management thread that motivated most of the design). Draft, not a mandate.*

## What it is

A conversational agent, aware of project status through the backlog/project board and the codebase, that stakeholders, product, and engineers (including coding agents) can talk to. Two directions:

- **Query (read).** "What's the status of X," "what's blocking Y," "what's still outstanding." Answered by reading the artifact store, backlog, ADR log, engineering-context pack, and (once confirmed to exist) brownfield codebase context — nothing here needs new data, it's a conversational layer over facts the platform already tracks.
- **Intake (write).** A channel for notes, revisions, and feedback to enter the workflow — most concretely, capturing stakeholder reactions during UAT and routing them into requirement-change handling rather than a side-channel Slack thread.

## Why it matters (motivating scenario)

A story or feature reaches UAT. The twin tells the stakeholder it's ready to review. They look, and want a layout tweak, a functionality change, or a new business rule. Today that lands in email or Slack and has to be manually re-entered into the pipeline, if it's re-entered at all. The twin's job is to make the in-system path lower-friction than the side channel — capturing the ask conversationally, in the moment, instead of requiring the stakeholder to open a ticket.

## Position in the platform

- **Not a new pipeline stage.** It's a UI/agent layer over existing components: [architecture-napkin.md](architecture-napkin.md)'s Backlog, Artifact store, ADR log, and Engineering context pack, plus (once confirmed to exist) brownfield MCP context.
- **Not a new decision-maker.** It does not classify changes or decide responses. It hands reasoning off to the impact/classification agent already described in [requirement-change-handling.md](../policies/requirement-change-handling.md) — same division of labor whether a change signal arrives via the twin, a ticket edit, or a stakeholder message.
- **Not the system of record.** Status, linkable/dependent requests, and approvals live in the backlog/artifact store it reads from — it's the notification and conversation surface over that store, not a parallel ticketing system.
- **Reuses existing gates.** A classified change re-enters through the requirements sign-off or solution sign-off gates that already exist, at a depth the classification implies (absorb in place / halt-and-re-solution / deliver-then-refactor) — no new gate type invented.
- **MVP stance: later.** The MVP is Intake -> `spec.md` -> requirements sign-off -> Design -> `design.md`, single reviewer, greenfield. The twin assumes a working backlog, artifact store, and change-classification agent already exist, none of which are in MVP scope.

## Functionality summarized from discussion

**Read side**
- Answers status, roadblock, and outstanding-question queries for stakeholder, product, and engineer (including coding agents) audiences.
- Reads: backlog, artifact store, ADR log, engineering-context pack, brownfield codebase context (external dependency, existence unconfirmed).

**Notification**
- Proactively surfaces state changes as conversational notices, e.g.:
  - "This is ready for you to review" (UAT-ready)
  - "We built a formal intake form for you"
  - "This hit a roadblock on X"
  - "The team needs you to answer Y"

**Intake / change capture**
- Captures a raw ask (feedback, revision, new rule) conversationally.
- Tags it with provenance — which initiative/feature/story it targets — so it can be handed to the impact/classification agent as a well-formed input, not free text.
- Does not classify or decide itself.

**Approval flow**
- Classification agent computes impact and a proposed classification (blast radius, timing, reversibility, urgency) and response (absorb / halt-and-re-solution / deliver-then-refactor).
- Requestor gets exactly one confirm-type approval: did the twin capture their intent correctly. Not a decision gate.
- Product holds final decision authority — they own the backlog and are accountable for it — and is made aware of the change and its status regardless of timing, so they can decide whenever they choose to.

## Related: the Definition Agent's chat sidekick (2026-09-05)

The [Definition Agent](../agents/definition-agent.md)'s product-definition form has a **chatbot sidekick** scoped to one artifact and one field at a time — the same conversational-grounding pattern as the twin, at the narrowest scope (one field, not one artifact, not the whole backlog). Decision there: the sidekick may write to form fields directly, with the user confirming on submit. Worth keeping the twin, the gate-scoped single-step chat panel ([platform-runtime.md](../architecture/runtime.md) Part 4), and this field-scoped sidekick as one mechanism at three scopes rather than three builds.

## Open items

1. **Sync vs. async notification mechanics.** Whether requestor and product are notified concurrently or sequentially by default; not decided.
2. **Disagreement resolution.** If a requestor confirms intent was captured correctly but product declines the change, product's call governs (a different kind of approval) — leaning, not committed.
3. **Blocking behavior while awaiting approval.** Whether only the affected subgraph blocks, or more — ties to the dependency-graph mechanism already described in [requirement-change-handling.md](../policies/requirement-change-handling.md).
4. **Change-intake agent.** Whether capturing and provenance-tagging a change signal needs its own agent (structurally a sibling of the MVP Intake agent, but for change signals rather than original asks), or is folded into the twin itself.
5. **Where UAT sits in the pipeline.** [architecture-napkin.md](architecture-napkin.md) doesn't yet name a UAT step distinct from "Post-release validation" (`VLD`) — worth reconciling once this is built.
6. **Audience-scoped access.** Stakeholder, product, and engineer/coding-agent queries may need different visibility or answer scope from the twin — not discussed yet.
7. **Friction budget.** Per [requirement-change-handling.md](../policies/requirement-change-handling.md)'s own warning, any confirm/notice step heavy enough will get routed around — the concrete UX for each notice type and the confirm step needs to stay near-zero-friction.

## Needing further discussion

- Whether this can be scoped and built in parallel with the front-of-flow work (v0.5), since its read side needs little beyond a working backlog/artifact store, or should strictly wait until after both MVP and front-of-flow land.
- How "neighbor" agents (the classification agent, a possible change-intake agent) are structured relative to the twin — new agents, or extensions of existing ones.
- Decision logging: per [ADR-0001](../adr/0001-record-architecture-decisions.md), once pieces of this settle (e.g., "product has final say," "requestor gets exactly one confirm-type approval") they should be written up as ADRs — the bar is "consequential and contestable," not "is this architecture." None of the above is settled enough yet.

*Status: new 2026-09-04. Not started, not scheduled — captured for the post-MVP backlog. Revisit once the front-of-flow and change-classification agent are real enough to build against.*
