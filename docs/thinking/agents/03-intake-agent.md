# Intake Agent

**Pipeline step:** 3. Intake & Requirements Clarification · [← back to master](../sdlc-business-process-flow.md)

## Purpose

Resolve engineering-level ambiguity in an already-prioritized ask and produce a spec ready for sign-off. This includes not just answering questions but identifying *who* can actually answer each one — an ask's ambiguity isn't uniform, and not every gap belongs back with whoever submitted the request.

## Input Structure & Routing

Not all raw asks are equally workable, and not every gap in an ask should route back to the same person. This defines what's expected of an ask before Intake does deep work on it, and where gaps go when the original submitter isn't the right source to fill them.

### Minimum intake scaffold

A lightweight checklist, not a rigid form — before Intake proceeds past a first pass, the ask should address each of the following, explicitly marked as answered, not applicable, or unknown:

- Problem statement — what's being solved, and for whom
- Target users / affected personas
- Success criteria — how "done" will be recognized
- Known constraints — deadlines, budget, systems it must integrate with
- Explicit out-of-scope — what this ask is not trying to do
- Related precedent — links to prior tickets, similar features, existing docs

An ask missing most of these isn't ready for deep Intake work. It routes back to Prioritization & Scoping (step 2) for further scoping rather than having Intake attempt to manufacture a comprehensive spec from too little raw material. This is a fast pre-check, not the full clarification loop — it exists to stop Intake from spinning effort on an ask that was never ready to leave Scoping.

### Gap routing

When Intake identifies a gap, it classifies what *kind* of gap it is before deciding who resolves it. Looping back to the original submitter by default is often routing the question to someone who was never going to be able to answer it:

| Gap type | Example | Routed to |
|---|---|---|
| Business intent | What outcome matters, priority tradeoffs | Original submitter / product function |
| Non-functional requirement | Expected scale, latency, availability target | A technical stakeholder, not the business submitter |
| Compliance / legal constraint | Data handling, regulatory obligation | Compliance/legal owner |
| Existing-system behavior | Current SLA, established pattern, prior design decision | Brownfield/MCP context first (see below) — often answerable without asking a person at all |
| Genuinely unknown | Nobody currently in the loop has the answer | Flagged `[unresolved]` and carried forward as visible risk — never silently defaulted |

### Brownfield context as an intake source, not only an implementation source

The DX brownfield/MCP dependency (flagged as unconfirmed in the master diagram's External Dependency Status) has a second use beyond supplying the Implementation Agent with codebase context: it can answer intake-time questions the business/product submitter was never positioned to answer — current behavior, existing constraints, patterns already encoded in the system. Where this dependency exists and is reachable, Intake queries it before escalating a gap to a human, and tags what it finds `[inferred]` (see below).

## Provenance Tagging

Every requirement, constraint, and acceptance criterion in `spec.md` carries a provenance tag, so a reader — human or agent — can tell at a glance which parts of the spec are solid and which are exposure:

- **`[stated]`** — given directly by an authoritative source (the submitter, a technical stakeholder, compliance) for this specific ask.
- **`[default]`** — Intake applied a reasonable default where nothing was specified. Correctable at Requirements Sign-off without being treated as a change request — see Requirement Change Management in the companion process-flow doc.
- **`[inferred]`** — derived from existing-system context (brownfield/MCP) rather than asked of a person.
- **`[unresolved]`** — a real gap with no authoritative source yet identified. A spec can still proceed to sign-off carrying `[unresolved]` items, but the human reviewer sees them flagged rather than buried in confident-sounding prose.

This isn't a new file format requirement on top of `spec.md` — it's a convention applied within its existing structure (e.g., an inline tag after each requirement or acceptance-criterion line), consistent with the still-open question of how structured `spec.md`'s format ultimately needs to be.

**Sibling concept — level tags.** [`front-of-flow.md`](../front-of-flow.md) (v0.5) adds a second per-requirement tag on a separate axis: the precision *level* (L1 business / L2 product / L3 architect / L4 senior dev) a requirement sits at. Provenance says how solid a requirement is; level says how precise it is. Both are per-requirement, applied the same way within `spec.md`'s structure.

## Inputs

- Scoped ask (from the Scoping Agent), checked against the minimum intake scaffold above
- Existing system/brownfield context (e.g., via codebase MCP servers, where reachable)
- Responses from whichever stakeholder a gap was routed to (business, technical, or compliance)

## Outputs

- `spec.md` — data model, edge cases, non-functional requirements, acceptance criteria, each carrying a provenance tag (`stated` / `default` / `inferred` / `unresolved`).

## Functions

- Minimum-scaffold pre-check, routing back to Scoping (step 2) when unmet
- Stakeholder clarifying-question rounds
- Gap classification and routing to the appropriate authority — not always the original submitter
- Edge-case enumeration
- Data model definition
- Non-functional requirements (performance, scale, SLAs)
- Compliance/legal/security constraint gathering
- Integration and dependency identification
- Acceptance criteria drafting
- Provenance tagging of every requirement and criterion
- `spec.md` authoring

## Autonomy Tiering

| Quadrant | What it looks like here | Behavior |
|---|---|---|
| Known + Safe | Standard requirement patterns, common data models; scaffold pre-checks and gap routing itself (mechanical classification). | Full autonomy — draft the spec directly. |
| Known + Risky | Requirements touch PII or other regulated data. | Autonomous drafting, with the sensitive sections flagged for review before sign-off. |
| Unknown + Safe | Clarifying questions on a genuinely new feature area with low stakes. | Autonomous drafting; reviewed after, not gated before. |
| Unknown + Risky | Ambiguous requirements with legal or compliance exposure, or a gap with no identified authoritative source. | Manual — human resolves the ambiguity directly; the spec carries it as `[unresolved]` in the meantime rather than being blocked entirely. |

## Escalation Triggers

- Ambiguity implicates legal or compliance exposure
- Stakeholders give conflicting answers to the same clarifying question
- Requirements touch regulated data categories
- A gap is classified as genuinely unknown, with no authoritative source identified anywhere in the loop
- An ask fails the minimum intake scaffold pre-check

## Handoffs

- **Receives from:** [Scoping Agent](02-scoping-agent.md) (step 2).
- **Delivers to:** [Requirements Sign-off Gate](04-requirements-signoff-gate.md) (step 4). Also escalates directly to the appropriate technical or compliance stakeholder mid-process when a gap is routed to them, rather than always resolving gaps through the original submitter first.
