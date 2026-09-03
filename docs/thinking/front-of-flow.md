# Front of the Flow — Requirement Levels, Artifact Chain, and Solution Gate

*Working notes, 2026-09-03. Output of a pre-planning conversation between Matt and Claude. Covers the "beginning of the flow" — how a business need becomes a signed solution and a well-formed backlog, before per-story build begins. Draft, not a mandate; every position here is open to challenge. Companion to [`vibe-engineering-platform-notes.md`](vibe-engineering-platform-notes.md) and [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md). Follow-up questions are collected at the end, deliberately unresolved — the current focus is the front of the flow, not merge/release.*

## Why this document

The 16-step pipeline in the companion process-flow doc treats "an ask" as a single unit that flows straight from scoping into per-story intake. Real enterprise work has more structure at the front: a business need is analyzed and translated by a product function, a high-level solution is designed and signed off *before* stories exist, and only then is a backlog generated. This document works out that front section and the concept it rests on — that requirements exist at different levels of precision, and moving between those levels is explicit, lossy work that needs sign-off.

---

## Part 1 — The requirement levels model (L1–L4)

### The idea in one sentence

A requirement can be expressed at four levels of precision, each matching a persona found in a typical enterprise; the system must know what level an input arrived at, so it knows how much translation work is needed and who is entitled to sign off.

### Dictionary

- **Level** (a.k.a. **altitude**). How precise and how detailed a statement of intent is. The "altitude" metaphor is borrowed from aviation and writing: fly high and you see the whole region but no detail; fly low and you see individual buildings but only a small patch. Climbing loses detail; descending must add it. "Level" and "altitude" are used interchangeably here for now — see the open question about standardizing on one.
- **Translation.** Moving a statement from one level to another. *Downward* translation (lowering) adds precision and makes intent executable. *Upward* translation (raising) abstracts and summarizes, making technical reality legible to a non-technical audience.
- **Level tag.** A marker on an individual requirement recording the level it currently sits at, and ideally the level it originated at.
- **Provenance tag.** The existing marker on a requirement recording where the fact came from — `[stated]` / `[default]` / `[inferred]` / `[unresolved]` (see [`agents/03-intake-agent.md`](agents/03-intake-agent.md)). Level tags are a sibling concept on a separate axis: provenance says *how solid* a requirement is, level says *how precise* it is.

### The chart

| Level | Persona | Language looks like | Authoritative for | Signs off that… |
|---|---|---|---|---|
| **L1** | Business / sponsor | Outcomes and direction. No technical terms. May be vague or internally contradictory. | Why this matters; what business outcome counts as success | the business need is stated correctly |
| **L2** | Product | Outcomes plus requirements, NFRs, targets, constraints. Some technical terms, but describing *what*, not *how*. | What to build; how success is measured | the product definition reflects the business need |
| **L3** | Architect | Precise. Technology choices, decision points, assumptions, interfaces, diagrams, pseudo-code. | How it is built — structure, technology, tradeoffs | the solution is sound and feasible |
| **L4** | Senior developer | Full engineering jargon for the discipline. Specific functions, data structures, control flow. | Implementation correctness | the code represents the logic correctly |

### Worked example — one requirement, descending the levels

- **L1:** "We're losing customers because changing a saved card is painful. Make it easy."
- **L2:** "Self-service payment-method update from the account page. No support contact, three steps or fewer, works on mobile web. Success: payment-related churn down 30% and 'update my card' support tickets down 50% within one quarter. PCI obligations preserved."
- **L3:** "New PaymentMethodUpdate flow. Tokenize through the existing Stripe integration so no card number touches our servers (keeps PCI SAQ-A). New endpoint `POST /account/payment-methods` behind the current auth guard. On method change, billing-service re-attempts failed invoices and emits a `PaymentMethodUpdated` event for the dunning service. Feature-flagged rollout."
- **L4:** "`PaymentMethodController.create()`: validate session; call `StripeClient.attachPaymentMethod(customerId, token)`; on success update `customers.default_payment_method_id` in a transaction and publish `PaymentMethodUpdated` to the `billing.events` topic; return 201. Handle `card_declined` → 402 with an error body. Integration test covering `BillingService.retryOpenInvoices()` on the method-change path."

Detail is lost climbing up and added — along with assumptions — coming down. The added-coming-down is where translation errors and unstated assumptions enter, which is why every descent is a sign-off point.

### Translation is bidirectional

The current pipeline only does downward translation. Real orgs constantly do both:

- **Downward (lowering) — add precision.** L1→L2→L3→L4. Stakeholder to product to architect to engineer. Each step makes intent more executable and introduces assumptions that need confirming.
- **Upward (raising) — abstract and roll up.** L4→L3→L2→L1. Engineer to manager to product to stakeholder. Status, "what does this blocker mean for the business," gate summaries, release notes. The pipeline barely does this today — only post-release reporting and gate-prep summaries.

Each agent therefore has two working levels: a **produce level** (the level of the artifact it hands downstream) and a **report level** (the level it speaks at when escalating or reporting upward to a human).

### The three hops are not symmetric

Treating "translate" as one uniform operation is wrong:

- **L1→L2** *creates* information that was not there — precision, targets, NFRs the business never stated. Highest risk, most human involvement, most assumptions introduced.
- **L2→L3** is translation plus genuine design decisions.
- **L3→L4** is mostly mechanical elaboration within decisions already made.

An agent's confidence, autonomy, and need for human confirmation should differ per hop.

### Per-requirement level tags

Real asks are mixed-level — some sentences are business, some are already architect. So the level tag belongs on the individual requirement, not the whole ask (the same granularity principle the companion docs apply to autonomy and to provenance). "How much lifting is needed" is then the distribution of levels across the requirement set, and translation effort targets the least-precise items.

---

## Part 2 — The artifact chain

Discrete artifacts are needed — each is a transaction between distinct parties and gets signed off separately. What is avoided is *hand-maintained parallel documents that drift*. The artifacts form a derivation chain: each is generated from the one above, every line traces upward, each is versioned.

| # | Artifact | Level | Drafted by | Confirmed by | Purpose |
|---|---|---|---|---|---|
| 1 | **Business brief** | L1 | agent, from business intake | business sponsor | captures need and requirements from the business |
| 2 | **Product definition** | L2 | agent + product function | business sponsor (validation loop), then feeds solutioning | mirrors the need back with added depth, clarity, risk, NFRs, targets |
| 3 | **Solution spec** | L3 | agent | authoritative engineer / architect / manager | mirrors the product definition into *how it is built*; initiative-level, and feature-level where the ask needs it |
| 4 | **Story specs / tickets** | L3→L4 | agent, decomposed from #3 | delivery team refines | well-formed stories, generated automatically into Jira / GitHub / equivalent |

### The disciplines that keep this from becoming a drift problem

- **Derivation, not authoring.** Each artifact is generated from the one above it. A human reviews and signs a generated artifact; a human never copies content between documents.
- **Upward traceability.** Every product requirement links to a business need, or is explicitly flagged "product-introduced." Every solution element links to a product requirement. Every story links to a solution element.
- **Versioning + staleness propagation.** Change artifact #1 and the system flags exactly what in #2, #3, #4 is now stale. This is the mechanism the requirement-change work depends on — see [`requirement-change-handling.md`](requirement-change-handling.md).
- **Views within a level, not extra artifacts.** When a stakeholder wants "a BRD" in a familiar format, render a view of the relevant artifact and freeze that view against the sign-off. The frozen view is a snapshot, not a living document.

### The validation loop on artifact #2

"Product definition mirrors the need back… then feeds solutioning" is two motions: product (or the agent) restates at L2 what it heard at L1, the business sponsor confirms "yes, that is what I meant," and only then does the confirmed L2 artifact become the intake for solutioning. The mirror-back is upward translation; the confirmation is the gate.

---

## Part 3 — Work hierarchy and the solution gate

### Initiative → Feature → Story

- **Initiative** — the prioritized business ask. One business brief and one product definition.
- **Feature** — a coherent slice the product function carves out of the initiative. **The unit of merge.**
- **Story** — an implementation-sized work item. The unit of spec-and-build.

### Two layers of solutioning

- **Initiative-level solution** — overall architecture, technology, shape. Signed once.
- **Feature-level solution** — how a given feature realizes that architecture. Signed per feature where the ask and product need it.

Whether feature-level solutioning always happens, or only for larger initiatives, is a follow-up (below).

### Solution analysis: agent drafts, human approves

- The agent produces the proposed solution: architecture, technology choices, decision points, assumptions, alternatives considered, diagrams / pseudo-code.
- A human with authority reviews and approves — senior engineer, engineering manager, or architect; someone equipped to judge the solution is sound.
- This is the existing autonomy framework's "autonomous prep, human-gated" quadrant: the agent does the entire draft; a human clears it before it becomes the constraint boundary that story generation depends on.

### How this changes the current pipeline

1. **A solution sign-off gate before story breakdown** — a third human gate, alongside Requirements Sign-off and Merge/Release Approval. Catching an architecture mistake here, before dozens of story tickets are written against a bad design, is where the gate earns its cost.
2. **The pipeline fans out.** "The ask" stops being one unit. One initiative runs through solution sign-off, then fans out into N story pipelines, each inheriting the signed solution as a constraint boundary. Steps 1–2 plus solution analysis run per initiative (and per feature); per-story intake through validation runs per story.
3. **The backlog is the integration seam.** Jira / GitHub / equivalent is the boundary between product and engineering and the platform's own work queue. A story ticket is generated from the solution spec, and its per-story spec is attached back to the ticket.

### Relationship to the existing steps

Roughly, the front of the flow expands from:

`1 Signal → 2 Scoping → 3 Intake (spec.md) → 4 Requirements Sign-off → 5 Design → …`

to:

`Signal → Scoping → [product function: analyze, ROI, prioritize] → Business brief → Product definition (+ business validation) → Solution analysis (agent) → Solution sign-off (human) → Feature / story generation → backlog → per story: Intake → Requirements Sign-off → …`

The existing Design step (5) becomes the per-story / per-feature elaboration *within* the signed solution, not the place the architecture is first decided.

---

## Follow-up questions (deliberately unresolved)

**Front-of-flow:**
- Standardize on "level" or "altitude" as the term?
- Does feature-level solutioning always happen, or only above some initiative size?
- Does solution analysis run once (initiative) or twice (initiative + feature)?
- What exactly is the solution reviewer signing — "this is the *right* solution" or "this is a *sound* solution, proceed"?
- Where does the product function sit — inside the platform (agent) or outside (human) — and does that vary with each vertical's product coverage? Does the existing "prioritization is a non-goal" position soften to "draft, don't decide"?
- How structured does each artifact need to be underneath its prose (enumerable, ID'd requirements vs. prose alone)? Ties to the existing "what does declarative mean" open question in the companion notes.

**Merge and release (parked — for a later session):**
- Is there a feature-level sign-off gate in addition to the initiative-level one?
- Merge mechanics — branch strategy, how story work rolls up to a feature.
- Release — how and when a merged feature actually goes live; whether the current "Merge / Release Approval" step splits into a feature-merge gate and a separate release gate.

**Cross-cutting:**
- How the level model and the artifact chain interact with each vertical's *existing* backlog and prioritization process, rather than duplicating it.

---

*Doc status: new in this session (2026-09-03). Not reviewed by anyone else. The diagram in [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md) does not yet reflect the initiative/feature/story fan-out or the solution gate — that integration is pending. Best next step is probably resolving the term ("level" vs "altitude") and the feature-layer questions, or moving on to merge/release as its own topic.*
