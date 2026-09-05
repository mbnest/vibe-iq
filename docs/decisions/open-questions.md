# Open Questions

*The consolidated "pick it up here" index. Supersedes the open-questions list that lived in [`../notes/platform-notes.md`](../notes/platform-notes.md). Decisions already made are in [`log.md`](log.md); this is what's still undecided.*

---

## The top blocker

**The `spec.md` schema.** How structured the core artifact is underneath its prose — free prose vs. enumerable, ID'd acceptance criteria. It is *assumed* by provenance tagging, level tagging, constraint citation, requirement-change classification, and deterministic test derivation, but never decided. Nothing that produces or consumes a real spec can be finished until this is settled. ([data-model.md](../architecture/data-model.md) states the working lean toward structured records; it is not ratified.)

Directly downstream of it:
- The handoff-validation contract (JSON Schema / Pydantic on `spec.md`) follows mechanically once the schema exists.
- Whether "declarative input" means a structured spec/DSL or natural language compiled to a structured intermediate.

## Prerequisites to the first real `spec.md`

1. The schema (above).
2. **What "the first feature" is** — the Intake Agent's own implementation (dogfooded, hand-written), or a real ask run through a hand-simulated Intake. Not chosen. The [worked example](../examples/govern-iq/) is a partial answer — a real ask hand-run through the front of the flow.
3. **`constraints.md` real content** — currently placeholder examples; Intake cites it by ID, so whichever ask is first needs its actual constraints.

## Front of the flow

- Standardize on "level" or "altitude" as the term.
- Does feature-level solutioning always happen, or only above some initiative size?
- Does solution analysis run once (initiative) or twice (initiative + feature)?
- What exactly is the solution reviewer signing — "the *right* solution" or "a *sound* solution, proceed"?
- Where does the product function sit (in-platform agent vs. outside human), and does it vary by each vertical's product coverage? *(Partly resolved — see [log.md](log.md) D-17: the L1→L2 work is an agent; prioritization stays human.)*
- How does the platform's intake integrate with, rather than duplicate, each vertical's existing backlog and prioritization process?

## Product-definition phase

The 13 phase decisions are settled ([log.md](log.md#product-definition-phase-2026-09-05)). Sub-items still open:

- The contents of the completeness checklist the agent runs before product commit (D-PD-10).
- Whether the thin-request bounce is agent-autonomous or human-confirmed first (D-PD-09).
- Where the agent sources metric baselines to fill target values (D-PD-03) — ties to the data-access bridge in [definition-agent.md](../agents/definition-agent.md).
- The concrete "informs but does not bind" mechanism for level tags in a programmatic flow (D-PD-05).

## Merge and release (parked for a dedicated session)

- Is there a feature-level sign-off gate in addition to the initiative-level Solution Sign-off?
- Branch strategy; how story work rolls up to a feature (the unit of merge).
- Does "Merge / Release Approval" split into a feature-merge gate and a separate release gate?

## Requirement-change handling

- Does the four-class taxonomy (clarification / additive / material / fundamental) survive as the classification, feed the three-response choice, or get replaced? ([requirement-change-handling.md](../policies/requirement-change-handling.md))
- How the three-response model maps onto the initiative/feature/story fan-out.
- Does a material change reopen Solution Sign-off, or only the affected feature's solution?
- What "fast automatic detection" concretely requires — where change signals enter (ticket edits, stakeholder messages, the [Project Twin](../notes/project-twin.md)).
- Real-world adoption: what makes the in-system change path lower-friction than a side channel.

## Rules and the decision engine

- Which governance decisions stabilize first and are worth formalizing in a decision engine? (Autonomy tiering is the likeliest candidate.)
- What criteria concretely drive Design's inline-vs-externalize call for a business rule — and is that itself a governance policy guiding Design?
- Does the engineering-context pack get a "volatile business rules are externalized" principle?
- Volatility scale: simple stable/volatile, or finer (long-standing / periodic / frequent / ephemeral)?
- Is the business-rule registry a standalone artifact or aggregated from `spec.md` frontmatter?
- If a decision engine is adopted for governance, does dogfooding push toward the same engine as the default for business rules in built products?

## Runtime

- Build-vs-adopt for the orchestration substrate (Temporal / Inngest / Step Functions) — **ADR-0004**, revisit when the front-of-flow build starts.
- MVP hosting — **ADR-0005**, revisit when always-on multi-user access is needed.
- Per-stage model routing — **ADR-0006**, needs eval data before it is more than "Opus 5 everywhere."
- Is the sign-off UI co-located with the agents or a separate service from the start?
- Is the Claude Agent SDK the right harness, or a plain Tool Runner loop?
- When does the web UI concretely become necessary — first non-technical (L1/L2) signer, or earlier?
- Should the gate-scoped single-step chat panel and the Project Twin share an implementation?

## External dependencies

For **each** capability in the [External Dependency Status table](../architecture/process-flow.md#external-dependency-status):

- Does it actually exist today?
- If so, is filing a ticket a fully manual action, or is submission/status programmatically reachable (API / webhook)? This materially affects how much async/resume machinery is worth building now.
- For any that turn out **not** to exist: what would building it actually take? Only the gap has been located, not sized.

Also: greenfield vs. brownfield as the first target — brownfield may be more tractable than first assumed *if* the codebase MCP servers are confirmed.

## Autonomy tiers

The per-agent tiers are starting proposals, not validated against real historical change data. Five were extrapolated for consistency rather than discussed directly — **Signal, Scoping, Planning, Review, Validation** — and need review against real cases before being trusted.

## Review / self-grading design

How different does the Review Agent's context actually need to be from the Implementation Agent's to meaningfully avoid correlated blind spots?

## The operations tail

**Incident Response, Maintenance & Iteration, Deprecation / Sunset** have no proposed agent. The scoping exercise applied to the build cycle has not been done for them.

---

## Unverified assumptions

Stated as starting points, not confirmed — each is effectively an open question:

- Each vertical has a **product function** that scopes and prioritizes asks before they reach engineering, and produces an organized backlog in a ticketing system.
- Verticals have "some level" of their own engineering resources; the depth varies and is undefined.
- The **sibling enablers** (Terraform, testing, data quality, CI/CD) exist, are ticket-based, queued, and human-fulfilled — tracked individually, not assumed collectively.
- Ticket **submission and status** (not just fulfillment) may or may not be programmatically reachable.
- The DX enabler's **MCP servers over codebases** exist and could supply brownfield context — existence, maturity, and integration effort not assessed.
- The **ticketing system** (Jira / GitHub / equivalent) is a first-class integration and serves as both the product/engineering boundary and the platform's own work queue.
- Before story tickets are generated, a **higher-level solution** is produced and signed off by an authoritative human.

---

*Maintained here going forward. When an item is decided, move it to [`log.md`](log.md) (and write an ADR if it is consequential and contestable).*
