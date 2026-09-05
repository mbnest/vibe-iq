# Rules and Policy — Two Concerns, Not One

*Current design position (pre-planning, 2026-09-04). Draft, open to challenge. The platform touches "rules" in two unrelated ways that are easy to conflate: the **governance decisions that run the pipeline**, and the **business rules that end up inside the product being built**. This doc separates them and places each. Draft, not a mandate. Companion to [`platform-notes.md`](../notes/platform-notes.md). Triggered by the question of whether a business-rule management system such as GoRules fits the architecture.*

## Three kinds of logic in the platform

| Kind | Example | Right tool | Where it runs |
|---|---|---|---|
| Judgment | Is this spec complete? Is this design sound? | LLM | inside a pipeline step |
| Orchestration | What runs when; retries, timeouts, resume after a pause | Durable workflow engine | the control plane (decision #1) |
| Policy / classification | What autonomy tier? Which constraints apply? How to route this gap? How to classify this change? | A decision engine (rules-engine shaped) | called from inside a step, same slot as an LLM call |

Judgment and orchestration are already covered by decisions in the main notes. This doc is about the third — and about a fourth concern that is not platform logic at all.

## Concern 1 — Governance rules (how the pipeline governs itself)

These decide how the platform runs each ask. They are the decision logic of the golden paths.

| Decision point | Inputs | Output |
|---|---|---|
| Autonomy tiering | path-known?, safe?, blast radius, data classification, environment | full-autonomy / prep+gate / act+audit / manual |
| Requirement-change response | blast radius, pipeline progress, reversibility, urgency | absorb / halt-and-re-solution / deliver-then-refactor + re-entry point |
| Constraint selection | ask attributes (PII?, payments?, EU users?, ...) | applicable constraint IDs |
| Gap routing (Intake) | gap type | business / technical / compliance / brownfield / unresolved |
| Allowed-stack enforcement | proposed technology, context | permitted / exception-required / denied |
| Catalog vs. novel path (Enablement) | requested module, parameter set | agentic apply / ticket to enabler |

**Properties.** Owned by platform / governance people, not buried in code; should be versioned, testable, auditable; changes without a code deploy; every evaluation logged as inputs -> output -> which rule fired — which is the transparency story the platform already wants.

**MVP stance.** Implement each as a small pure function behind a clean interface — `classify_autonomy(facts) -> Tier`, `select_constraints(ask) -> list[ConstraintId]`. The autonomy 2x2 is four outcomes, not a system to deploy. And the logic is still draft — encoding unstable rules into a formal decision model is premature.

**Swap in a rules engine (e.g. GoRules) when** the logic has stabilized against real asks, *and* a non-engineer needs to own and change it without a deploy. The clean interface means nothing upstream changes.

## Concern 2 — Domain / business rules (what goes into the built product)

A separate concern entirely. These are rules like "Gold-plan customers get free shipping over $50," "applications from under-18s are auto-rejected," "tax is calculated as ...". They live in the *product the platform builds*, not in the platform.

**They enter the pipeline as requirements.** At Intake, a business rule is a functional requirement with acceptance criteria — captured in `spec.md` like any other requirement. Business rules are the strongest argument for `spec.md` acceptance criteria being enumerable, ID'd, and table-shaped rather than prose (ties to the open "how structured does spec.md need to be" question).

**Design makes an inline-vs-externalize call** for each business rule:

| | Inline in code | Externalize to a rules service |
|---|---|---|
| When | Stable rule, no external owner, unlikely to change, not separately audited | Changes often, needs a business owner, must be independently auditable, reused across features |
| Cost | Rule change = code change = full pipeline run | Another runtime component to provision and operate |

This could become an architecture principle in the engineering-context pack: *volatile business rules are externalized to a decision service, not inlined.*

### Rules carry their own classification

The inline-vs-externalize call is not a one-time decision that disappears into "it is in the code now." It is a dimension of the rule — recorded and queryable regardless of where the rule physically lives. Two orthogonal attributes:

- **Volatility** — expected change frequency (long-standing / periodic / frequent / ephemeral). Intrinsic to the rule; set at Intake or Design.
- **Placement** — where the rule currently lives (`inline:<module path>` or `external:<service>.<decision>`). A fact that changes over time.

Keeping them separate makes drift detectable: `volatility=frequent, placement=inline` is a flag that the rule has outgrown its placement. Without the classification recorded, no one notices a stable-classified rule that has started changing monthly.

A third attribute is often useful: **owner** — business-owned vs. engineering-owned. Correlates with volatility but not always (a stable regulatory rule can still be compliance-owned).

### A rule registry

This implies a lightweight registry: every business rule has an entry — ID, the spec acceptance criterion it derives from, volatility, placement, owner, last-changed / change count, whether it needs independent audit. Same discipline as `constraints.md` — stable IDs, cited from `spec.md`, one source of truth. In data terms it is a dimension table for business rules, and the join key between the spec, the code, and any rules service.

**MVP.** No registry, no rules service. But every business rule in `spec.md` carries its volatility tag from day one, even while placement is always inline. Cheap now (a tag), and the data to decide what to externalize later already exists — same pattern as tagging gap type while routing stays manual.

**Brownfield rule discovery.** In an existing product, business rules are already encoded. A new feature may interact with them. This is the "existing-system behavior" gap type in the Intake agent doc — brownfield / MCP context surfaces the rules already in the area. If those rules are externalized in a decision service, the platform can read the decision model directly instead of reverse-engineering it from code.

**What the platform does and does not do with business rules.** It *captures* them precisely (Intake), *decides how to implement* them (Design), and *tests every branch* (Test agent, derived from the spec). It does not reason about them semantically — it is not an expert system.

## The recursion: same technology, two roles

A decision engine such as GoRules could appear twice in the full picture, as separate deployments:

1. **The platform's own governance decision engine** — internal tooling for Concern 1.
2. **A component the platform provisions into the products it builds** — part of the golden path / allowed stack for apps that have volatile business rules (Concern 2).

Same technology, unrelated instances, different owners.

## MVP stance (both concerns)

Neither needs a rules engine now. Both need clean interfaces so one can be added later:

- Concern 1: pure functions with typed inputs and outputs.
- Concern 2: `spec.md` captures business rules as structured acceptance criteria; Design records the inline-vs-externalize call (ADR-worthy per rule class, not per rule).

## Open questions

- What criteria drive the inline-vs-externalize call, concretely — and is that itself a Concern 1 policy guiding Design?
- What is the volatility scale — simple stable/volatile, or a finer long-standing / periodic / frequent / ephemeral?
- Is the rule registry a standalone artifact, or aggregated from `spec.md` frontmatter?
- Is volatility/placement drift detected automatically — a flag when a "stable" rule changes repeatedly?
- Does the engineering-context pack get a "volatile business rules are externalized" principle?
- For governance rules: which stabilize first and are worth formalizing? Autonomy tiering is the likeliest candidate.
- If the platform adopts a decision engine for Concern 1, does dogfooding push toward the same engine as the default for Concern 2?

*Status: new 2026-09-04. Not reviewed. Both concerns are deferred for the MVP — the value here is separating them and defining the interface boundary.*
