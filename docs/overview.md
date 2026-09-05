# VibeIQ — Overview

*The whole idea in one read. For detail, follow the links; for the reasoning trail, see [`notes/`](notes/). Everything here is a working position from pre-planning, not a reviewed or committed design.*

---

## The problem

A horizontal enablement team serves many business verticals. Each vertical has its own (variable) engineering capacity and its own product function, and each keeps re-learning how to correctly invoke every *other* platform team's tooling — provisioning, testing, data quality, CI/CD. Golden paths exist in principle but are re-discovered per team, per project.

## The thesis

Encode the organization's SDLC golden paths **once, centrally**, as a spec-driven pipeline of agents with real human gates, so every vertical inherits them automatically. The platform's value is **orchestration and composition, not reimplementation** — it takes an already-prioritized ask and runs it through requirements → design → plan → implement → test → review → release, delegating infra / testing / data-quality / CI-CD work to sibling enabler teams rather than rebuilding their tooling.

The differentiator over "just run a coding agent well" is that the pipeline is the golden path, encoded and governed.

## The pipeline

A 13-step build/extend cycle, plus an ops tail that is not yet scoped. Each step has a proposed agent; three steps are human gates.

```
FRONT OF FLOW  (per initiative)
  Signal → Scoping → [product function: prioritize]
    → Definition Agent (business brief L1 → product definition L2)
    → Solution analysis (L2 → solution spec L3) → SOLUTION SIGN-OFF (human)
    → feature / story generation → backlog

BUILD LOOP  (per story)
  Intake (spec.md) → REQUIREMENTS SIGN-OFF (human)
    → Design (design.md) → Planning (task-graph.json)
    → Implementation (diff) → Test (report) → Review (findings)
    → MERGE / RELEASE APPROVAL (human)
    → Enablement (provision / ticket) → Deployment → Post-release Validation

OPS TAIL  (no agents yet)
  Incident Response · Maintenance · Deprecation
```

Full diagram and step-by-step: [`architecture/process-flow.md`](architecture/process-flow.md). One spec per agent: [`agents/`](agents/).

## Five concepts that hold it together

1. **Requirement levels (L1–L4).** A requirement can be stated at business (L1), product (L2), architect (L3), or senior-dev (L4) precision. Moving between levels is explicit, lossy work that needs sign-off. Each requirement is tagged with its level. → [`architecture/front-of-flow.md`](architecture/front-of-flow.md)

2. **The artifact chain.** Business brief → product definition → solution spec → story specs. Each is *derived* from the one above, versioned, and upward-traceable — never hand-maintained in parallel. Change one, and exactly what is now stale downstream is flagged. → [`architecture/data-model.md`](architecture/data-model.md)

3. **Autonomy tiering.** How much an agent may do alone is a 2×2 of *known-path?* × *safe?*, applied per task-graph node — not per request. The two mixed quadrants get opposite handling. → [`policies/autonomy-tiering.md`](policies/autonomy-tiering.md)

4. **Three human gates, by design.** Requirements Sign-off, Solution Sign-off, Merge/Release Approval stay human regardless of how capable the agents get. Agents prepare the packet; humans make the call. → [`agents/README.md`](agents/README.md)

5. **Governance is code, not a model.** What runs next, retries, gate waits, fan-out, and policy classification are deterministic logic. The LLM reasons *inside* a step and produces an artifact; it never decides the path. → [`policies/rules-and-policy.md`](policies/rules-and-policy.md)

## The front of the flow

Real enterprise work has structure before stories exist: a business need is analyzed by a product function, a product definition is validated back with the business, a high-level solution is signed off — *then* a backlog is generated and the per-story loop fans out. The [Definition Agent](agents/definition-agent.md) owns the L1→L2 translation (the riskiest hop — it creates precision the business never stated). Its phase is worked out in detail across [`agents/definition-agent.md`](agents/definition-agent.md), the [13 phase decisions](decisions/log.md#product-definition-phase-2026-09-05), and [enterprise-tooling integration options](notes/product-definition-integration-options.md).

## MVP scope

Two agents and one gate: **Intake → `spec.md` → Requirements Sign-off → Design → `design.md`**. Single technical reviewer, greenfield, GitHub issue in / docs out. Runs as a skill / prompt-loop on the Claude Agent SDK — no orchestrator, git for the artifact store, pure functions for governance logic, all behind clean interfaces to swap later. Both agents run on `claude-opus-5`. → [`architecture/runtime.md`](architecture/runtime.md), [`notes/architecture-napkin.md`](notes/architecture-napkin.md)

## What is and isn't settled

- **Decided (working):** 17 platform decisions + 14 product-definition decisions — [`decisions/log.md`](decisions/log.md). Three ADRs accepted — [`adr/`](adr/).
- **Not decided:** the full index is [`decisions/open-questions.md`](decisions/open-questions.md).
- **The top blocker:** the `spec.md` schema itself (prose vs. enumerable ID'd criteria) is undecided, and much depends on it.
- **Deferred:** orchestration substrate, hosting, per-stage model routing, merge/release mechanics, the ops-tail agents, and every external-dependency assumption (sibling enablers, brownfield MCP context) — tracked individually, not assumed.
- **Non-goals:** product discovery/prioritization (the vertical's product function owns it; the platform may *draft* but not *decide*), and rebuilding tooling that sibling enablers already provide.

## Navigating the rest

| You want… | Go to |
|---|---|
| The executive "so what" — value per feature, build order | [`product-vision.md`](product-vision.md) |
| The lifecycle and master diagram | [`architecture/process-flow.md`](architecture/process-flow.md) |
| A specific agent | [`agents/`](agents/) |
| The front-of-flow / levels / artifact chain | [`architecture/front-of-flow.md`](architecture/front-of-flow.md) |
| How artifacts are stored and versioned | [`architecture/data-model.md`](architecture/data-model.md) |
| Orchestration, hosting, models, UI | [`architecture/runtime.md`](architecture/runtime.md) |
| Autonomy, change handling, rules | [`policies/`](policies/) |
| Every decision, one line each | [`decisions/log.md`](decisions/log.md) |
| What's still undecided | [`decisions/open-questions.md`](decisions/open-questions.md) |
| The standing rules agents read | [`engineering-context/`](engineering-context/) |
| A concrete example run through the chain | [`examples/govern-iq/`](examples/govern-iq/) |
| The reasoning behind any of it | [`notes/`](notes/) |
