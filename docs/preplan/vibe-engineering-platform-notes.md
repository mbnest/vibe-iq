# Vibe Engineering Platform — Architecture Notes (Draft v0.4)

*Working notes from an ongoing pre-planning conversation between a human and Claude. v0.1 was the original pre-planning sketch; v0.2 generalized it into a full business-process lifecycle with an agent per step, an autonomy-tiering framework, and requirement-change handling; v0.3 unified the architecture diagram into one business-process-rooted view with every external dependency individually flagged; v0.4 removes that diagram (and its External Dependency Status table) from this file entirely and links to it in the companion doc instead, so there's exactly one copy to keep current. Nothing here is final; treat the decisions below as current working positions, not commitments.*

**If you're an LLM resuming this conversation:** the "Key decisions" section is what's been agreed so far and why (1–7 are from v0.1, 8–11 are from v0.2, 12 is from v0.3); "Open questions" is the most useful place to pick up next. Two companion documents carry detail this file summarizes and links to: [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md) — the sole home of the architecture diagram and External Dependency Status table, plus the step reference table, autonomy tiering framework, and requirement change management policy — and [`agents/`](agents/) (one file per agent/gate — purpose, inputs, outputs, functions, autonomy tiering, escalation triggers, handoffs). Don't treat any of this as locked in — it's the output of one extended conversation, not a reviewed design doc.

---

## Context

This platform would come from a horizontal enablement team, serving business verticals that each have some (variable, not yet defined) level of their own engineering resources. Several sibling horizontal enablers are understood to exist: cloud build / Terraform, quality & testing, data quality, and developer experience (CI/CD, MCP servers over codebases) — though that existence is tracked as an unconfirmed, per-capability assumption rather than a given. Each vertical also has a product function that scopes and prioritizes engineering asks before they reach engineering execution.

The working assumption (explicitly a starting guess, not confirmed) is that the existing enablers, to the extent they exist, are consumed via ticket-based requests into a queue, fulfilled by humans on those teams — not via synchronous, programmatic APIs. See the External Dependency Status table in [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md) for where each of these stands.

## Core thesis

The platform's value is orchestration and composition, not reimplementation. It takes an already-prioritized ask from a vertical and runs it through a spec-driven pipeline with real SDLC gates (requirements → design → plan → implement → test → review → release), while delegating anything infra-, testing-, data-quality-, or CI/CD-related to the sibling enablers — existence and maturity per capability not yet confirmed, see the companion doc — rather than rebuilding that tooling. The differentiator versus "just run Claude Code well" is that this pipeline encodes the org's golden paths once, centrally, so every vertical inherits them automatically instead of re-learning how to correctly invoke every other platform team's tooling.

## Evolution: v0.1 → v0.4

**v0.2 added:**
- Generalized the single spec → design → plan → implement → test → review → release pipeline into a 16-step, end-to-end business-process lifecycle spanning Opportunity Identification through Deprecation/Sunset, with a 13-step "build/extend cycle" segregated from a 3-step ongoing-operations tail.
- Gave each step in the build cycle a proposed agent with defined inputs, outputs, functions, and escalation triggers.
- Defined an autonomy-tiering framework (Known/Unknown × Safe/Risky) governing how much independence each agent gets, applied at the task-graph-node level rather than per request.
- Defined requirement change management: `spec.md` is versioned, and a change is classified by blast radius to determine how far back into the pipeline it re-enters.

**v0.3 added:**
- Unified the diagram into one business-process-rooted, self-contained architecture view, replacing the separate orchestration-engine view.
- Broke the single "existing enablers" assumption into five individually-flagged external dependencies (DX brownfield/MCP context, plus the four sibling enablers), each marked "unconfirmed" and tracked in an External Dependency Status table.
- Reworded Context, Core Thesis, and decision #4 to stop presupposing the sibling enablers exist.

**v0.4 added:**
- Removed the diagram and status table from this file entirely — v0.3 kept them duplicated here and in the companion doc "for narrative completeness," but flagged that as a drift risk. v0.4 resolves it by making the companion doc the single home for both; this file links out instead of embedding a copy.

## Diagram

The architecture diagram — the business-process-rooted, self-contained view mapping each of this platform's own 13 build-cycle steps to its agent, with every external dependency (sibling enablers, brownfield tooling) individually flagged as unconfirmed — along with its External Dependency Status table, now lives solely in [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md) (see its "Diagram" and "External Dependency Status" sections). Keeping one copy there, rather than duplicating it here, is what this file's decisions and open questions below still assume and reference.

## Key decisions (working, not final)

1. **Orchestration is deterministic, durable, and distributed; agentic reasoning lives inside individual steps, not in the control flow.** Rationale: replayability and debuggability, and the ability to build reliable retry/timeout/resume logic on top — the same split Temporal/Step Functions make between workflow code and activities. *(This is an implementation decision about how the pipeline executes; the diagram in the companion doc is intentionally business-process-rooted and doesn't visually depict the orchestration substrate — see decision #12.)*
2. **Phase handoffs are durable, versioned artifacts** (spec.md, design.md, task-graph.json, diff, test report), not conversational context passed implicitly between agents. Rationale: enables human checkpoints, auditability, and safe resumption after a multi-day pause.
3. **Test agent derives tests from the spec, independent of the implementation; review agent works from spec + diff**, ideally with different context than the implementer. Rationale: avoid self-grading and correlated blind spots between an agent and the agent checking its work.
4. **Provisioning, testing infrastructure, data quality, and CI/CD are consumed via the sibling enablers, not rebuilt.** Rationale: composition over construction; avoid duplicating sibling teams' work and standards. *(Contingent on each enabler actually existing and being reachable — where one doesn't, that capability becomes build scope for this platform instead of a given; see the External Dependency Status table in the companion doc.)*
5. **Human gates sit after the spec (cheapest point to catch a misunderstanding) and before merge/release** (forced anyway by ticket-based fulfillment, to the extent that's confirmed). Middle phases are intended to run with less oversight, though the right amount is still an open question.
6. **Realistic v1 "done" state is "PR ready to merge + enabler tickets filed with full context,"** not full autonomous deploy — because fulfillment on those tickets is assumed to be human-worked, not programmatic, under current (unconfirmed) assumptions.
7. **Intake agent's job starts after the vertical's product function has already scoped and prioritized the ask.** It resolves engineering-level ambiguity (data model, edge cases, non-functional requirements), not business prioritization.
8. **Autonomy is assigned per task-graph node using a Known/Unknown × Safe/Risky framework, not per pipeline step.** Rationale: a single request is rarely uniformly risky; gating the whole thing on its riskiest component wastes the autonomy earned by everything else in it.
9. **Infrastructure provisioning bifurcates into a catalog path and a novel path.** Pre-approved, compliance- and security-reviewed Terraform modules already in git can be applied agentically, subject to policy-as-code checks on the specific parameters used — not just the module's identity. Anything novel still routes to the sibling enablers, where those exist. Rationale: decision #4 (composition over construction) doesn't mean zero autonomy on infra — it means autonomy is earned per-module through the same review the enablers already require, not skipped.
10. **The two human gates stay human by design, not by capability gap.** Requirements Sign-off and Merge/Release Approval are checkpoints regardless of how capable the surrounding agents get — agents can prep and summarize for these gates, but not clear them.
11. **`spec.md` is versioned, and requirement changes are classified by blast radius** (clarification / additive / material / fundamental) to determine how far back into the pipeline a change re-enters, rather than treating every change as either a silent edit or a full restart.
12. **The architecture diagram is rooted in the business process, not a separate orchestration-engine view, and stands on its own.** External dependencies are called out individually as unconfirmed rather than folded into one assumption, so the diagram stays valid and complete regardless of which of those dependencies turn out to exist. As of v0.4, this diagram lives only in the companion doc — this file references it rather than keeping its own copy. Rationale: an architecture diagram whose validity depends on an unconfirmed external assumption is misleading by omission; separating "this platform's own scope" from "capabilities we're assuming but haven't verified" lets others immediately see what's confirmed versus what may need to be built — and a single copy of it means that clarity can't quietly go stale in one file while the other is updated.

## Assumptions (stated as a starting point, not verified)

- Whether each sibling enabler exists, and if so whether it's ticket-based, queued, and human-fulfilled, is unconfirmed enabler-by-enabler — tracked individually in the companion doc's External Dependency Status table, not assumed collectively.
- Each vertical has a product function that scopes and prioritizes asks before they reach engineering.
- Verticals have "some level" of engineering resources; the depth varies and hasn't been defined.
- Whether ticket *submission and status* (as opposed to fulfillment) is programmatically reachable — API or webhook — is still unknown, for whichever enablers turn out to exist. This materially affects how much async/resume machinery is worth building now.
- DX enabler's MCP servers over codebases are assumed to exist and could plausibly supply brownfield context; existence, maturity, and integration effort not yet assessed.
- The specific autonomy tier proposed for each agent (Known+Safe vs. Known+Risky, etc.) is a starting proposal, not validated against real historical changes — five of the eleven agents (Signal, Scoping, Planning, Review, Validation) had their tiers extrapolated for consistency rather than discussed directly.

## Open questions / where to pick this up next

- For each capability in the companion doc's External Dependency Status table: does it actually exist today, and if so, is even filing a ticket a fully manual, human-keyboard action, or is submission/status programmatically reachable?
- Build-vs-adopt call for the orchestration substrate: sit on an existing durable-workflow engine (Temporal, Step Functions, Argo) versus build custom state persistence.
- Review/self-grading design in more depth — how different does the review agent's context actually need to be from the implementation agent's to meaningfully avoid correlated blind spots?
- Greenfield vs. brownfield as the first target — brownfield may be more tractable than first assumed, given the existing codebase MCP servers, if that capability is confirmed.
- What "declarative" concretely means as an input format — structured spec/DSL vs. natural language compiled to a structured intermediate. Sharper now than in v0.1: if tests need to be *deterministically* derivable from spec.md and requirement changes need to be *classified* by what they invalidate, spec.md likely needs enumerable, ID'd acceptance criteria underneath the prose, not prose alone.
- How this platform's intake integrates with (rather than duplicates) each vertical's existing backlog/prioritization process.
- The five agents whose autonomy tiers were extrapolated rather than discussed directly (Signal, Scoping, Planning, Review, Validation) need review against real cases before being trusted.
- Steps 14–16 (Incident Response, Maintenance & Iteration, Deprecation/Sunset) have no proposed agent yet — the same scoping exercise applied to steps 1–13 hasn't been done for them.
- The requirement-change classification taxonomy (clarification / additive / material / fundamental) is a proposed framework, untested against a real change.
- For any capability in the External Dependency Status table that turns out not to exist: what would building it actually take? That sizing exercise hasn't been done yet, only the gap has been located.

## Non-goals (as currently scoped)

- Product discovery or prioritization — owned by the vertical's product function.
- Rebuilding provisioning, testing frameworks, data quality tooling, or CI/CD, where those already exist — owned by the sibling enablers.
- Full autonomous deploy in v1 — deferred given assumed ticket-based fulfillment.

## Companion documents

- [`sdlc-business-process-flow.md`](sdlc-business-process-flow.md) — the architecture diagram and External Dependency Status table (the sole copy of both, as of v0.4), the full step reference table, the autonomy tiering framework, and the requirement change management policy.
- [`agents/`](agents/) — one file per agent or human gate: purpose, inputs, outputs, functions, autonomy tiering specific to that step, escalation triggers, and handoffs to adjacent steps.

---

*Doc status: v0.4 removes the diagram and External Dependency Status table from this file — duplicated here and in the companion doc as of v0.3, flagged then as a drift risk — leaving `sdlc-business-process-flow.md` as their single home. All substantive content from v0.1 through v0.3 (key decisions, assumptions, open questions, non-goals) is preserved here, reworded only where the removal required a reference to point at the companion doc instead. Still not reviewed or approved by anyone else. Best next step is probably confirming one row of the companion doc's External Dependency Status table against reality, or stress-testing one of the extrapolated autonomy tiers or the change-classification taxonomy against a real historical change.*
