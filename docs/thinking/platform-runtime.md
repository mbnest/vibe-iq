# Platform Runtime — Orchestration, Hosting, Model Selection

*Working notes, 2026-09-04. Output of a pre-planning conversation between Matt and Claude. Covers how the pipeline is orchestrated, where it runs for testing and demos, and which model tier fits which stage. Draft, not a mandate. Companion to [`vibe-engineering-platform-notes.md`](vibe-engineering-platform-notes.md) and [`rules-and-policy.md`](rules-and-policy.md). Model prices are Anthropic first-party rates as of the `claude-api` skill's 2026-06-24 cache — re-check before relying on a number.*

---

## Part 1 — Orchestration

### The pipeline flow is deterministic code, not LLM-routed

Established in decision #1 and [`rules-and-policy.md`](rules-and-policy.md): what step runs next, retries, timeouts, resume after a multi-day gate wait, and the fan-out from one initiative to N stories are all programmatic. The LLM reasons *inside* a step and produces an artifact; it never decides the path. Branches exist (gate or no gate, requirement-change re-entry point, catalog vs. novel path) but they are decided by the governance decision logic — rules and functions on explicit facts — not by a model.

### MVP: no orchestrator

The MVP is two agents and one gate. That is a Python script with a thin state machine, not an orchestration problem. Adopting Temporal now optimizes a problem that does not exist yet and steals attention from the agents.

**Discipline that keeps it swappable:** each agent is `inputs -> artifact` (pure-ish), and the "what runs next" logic is separate and thin. A real orchestrator drops in later without touching the agents.

### Options, for when one is actually needed

Triggers: front-of-flow fan-out, multi-day gate waits, retries at scale, compensation logic.

| Option | Fit | Cost |
|---|---|---|
| **Temporal** | Industry standard for long-running, human-in-the-loop, agentic pipelines. Code-first (Python/TS), durable by default, workflows sleep for days. Most precedent for agent orchestration. | Steep learning curve (determinism rules in workflow code); heavy to self-host, or pay for Temporal Cloud |
| **Inngest** | ~80% of the value for far less operational burden. Durable step functions, `waitForEvent` for human approval, single-binary dev server, managed free tier. | TS-first; Python SDK newer. Less battle-tested at scale |
| **AWS Step Functions** | Already the target cloud. Zero servers, native Lambda integration, wait-for-callback for human gates. | Clunky authoring (ASL JSON / CDK); thinner agent-ecosystem precedent |
| **n8n** | Wrong layer. Visual integration automation — good for *glue* (Jira webhook in, Slack out), weak as a durable-execution core for code-heavy logic. | Fights anything that wants to be code |

**Recommendation:** when the front-of-flow gets built, prototype on **Inngest** or **Step Functions** — both teach the durable-orchestration shape cheaply. Reach for **Temporal** only on hitting their ceiling. Use **n8n** only as a separate visual glue layer for external-system events, never the core.

---

## Part 2 — Hosting

### What actually has to run (MVP)

Python for the agent logic, an outbound LLM API call, a Postgres-compatible DB, artifact storage, and a thin UI for the sign-off loop. All boring and portable.

| Tier | What | Good for | Effort |
|---|---|---|---|
| **Local (Mac)** | CLI or local FastAPI + simple web UI; SQLite (ADR-0003's use case) | **Testing** — fast iteration, zero cost/ops | none |
| **One PaaS box** | Render / Railway / Fly.io — deploy from repo + managed Postgres | **Demo** with a shareable always-on URL | ~1 hr, ~$10/mo |
| **AWS serverless** | Lambda + API Gateway + Aurora Serverless + S3 — the allowed-stack destination | The full platform, once orchestration exists | days (Terraform, IAM) |
| **ECS Fargate + RDS + orchestrator** | The real thing | Later | — |

### Recommendation

- **Testing -> local.** Mac, SQLite, no hosting decision. Where the learning happens.
- **Demo -> depends how you demo.** Screen-share from local if that is how demos go; otherwise one service on **Render** (simplest) or **Fly.io** with managed Postgres.
- **Not AWS yet.** It is the destination per ADR-0002, but adopting it now means time on IAM and Terraform instead of agents. Step Functions / Temporal-on-AWS pulls you there naturally when orchestration lands.

### Not a one-way door

Local, PaaS, and AWS all satisfy the runtime needs. Pick the cheapest thing that unblocks the current need; move when a real requirement forces it. The **Claude Agent SDK** choice (it supplies the agent loop + built-in file/bash tools; runs anywhere Python runs) is independent of the hosting choice.

---

## Part 3 — Model selection per stage

### Principles

1. **Effort before model.** `output_config.effort` (`low` -> `max`) trades thoroughness against spend within one model. `xhigh` is the sweet spot for coding/agentic work on the current generation; `low`/`medium` for simple stages; `max` only where correctness outweighs cost. Tune this per stage before switching models.
2. **Measure the simplest thing first.** The most capable model at lower effort often beats a prior-tier model at high effort. Do not pre-build a multi-model cascade — start stages on one model, then promote/demote where eval data justifies it.
3. **Caches are model-scoped.** The engineering-context pack is a stable cache prefix across every agent call; a cascade across many models forfeits that reuse. Fewer models = better cache economics.
4. **Batch the non-interactive stages.** Message Batches run async at 50% cost — fits Signal scanning and bulk story generation, not the interactive Intake/sign-off loop.
5. **Governance logic and orchestration cost zero model spend** — they are code.

### Tiers

| Tier | Model | ~$/1M in-out | Use for |
|---|---|---|---|
| Deep reasoning | `claude-opus-5` | $5 / $25 | judgment, architecture, catching subtle errors |
| Balanced | `claude-sonnet-5` | $2 / $10 | code generation, decomposition, test authoring — the volume stages |
| Fast / cheap | `claude-haiku-4-5` | $1 / $5 | extraction, tagging, formatting, structural checks |
| Frontier | `claude-fable-5-1` | $10 / $50 | only the single hardest reasoning task, and only if Opus 5 at `max` proves insufficient |

### Stage -> tier

| Stage | Tier | Effort | Why |
|---|---|---|---|
| Signal (scan sources, draft candidates) | Fast, some Balanced | low | mostly extraction and pattern-matching; batchable |
| Scoping | Balanced | medium | drafting decision inputs, not deep design |
| **Intake agent** (resolve ambiguity, find the gaps a senior engineer would) | **Deep** | high | genuine judgment — "what is missing" is hard |
| Requirements sign-off (draft the gate summary) | Fast | low | summarization for a human |
| Solution analysis (initiative architecture) | **Deep**, Frontier for a large initiative | xhigh / max | the hardest reasoning in the pipeline; wrong here is expensive |
| **Design agent** (per-story/feature design within the signed solution) | **Deep** | high / xhigh | architecture decisions, bounded by the solution |
| Planning / breakdown (design -> task-graph) | Balanced | medium | mechanical decomposition of a settled design |
| Implementation | Balanced, promote hard nodes to Deep | xhigh | strong at coding; the volume stage, so cost matters; escalate only the novel nodes |
| Testing (derive tests from spec) | Balanced | high | systematic, spec-driven |
| Review (catch subtle correctness issues) | **Deep** | high / max | correlated-blind-spot avoidance; correctness outweighs cost |
| Story / ticket generation from a signed solution | Balanced, Fast for formatting | low | mechanical; batchable |
| Handoff validation (structural / referential) | mostly not an LLM | — | JSON Schema / Pydantic; only the semantic-consistency check needs a model (Fast) |
| Per-audience view rendering of a spec | Fast | low | reformatting one source |
| Requirement-change impact analysis | **Deep** | high | blast-radius reasoning over the dependency graph |
| Governance decision logic | none | — | pure functions / decision engine |

### MVP

Both MVP agents — Intake and Design — are deep-reasoning stages. Run both on **`claude-opus-5`**, adaptive thinking on (default), effort `high`. One model, one cache namespace, no routing logic. Revisit when cost or eval data says a stage can drop to Sonnet 5, or that Solution analysis (when built) needs `max` or Fable.

---

## ADR flags (not decided yet)

- **ADR-0004 — Orchestration substrate.** Revisit when: front-of-flow build starts, or a multi-day workflow is needed.
- **ADR-0005 — MVP hosting.** Revisit when: always-on multi-user access needed, or the orchestration layer lands.
- **ADR-0006 — Model routing policy.** Revisit when: eval data exists to justify per-stage model choices beyond "Opus 5 everywhere."

## Open questions

- Does the sign-off UI live in the same service as the agents, or separate from the start?
- Is the Claude Agent SDK the right harness for these agents, or a plain Tool Runner loop? (SDK gives built-in file/bash tools; the agents are file-heavy, so probably yes.)
- For Implementation, what decides which task-graph nodes escalate from Sonnet to Opus — a governance rule on node novelty/risk?
- Batch vs. interactive boundary — Signal and story-generation are batchable; does the MVP even have them? (No — deferred.)

*Status: new 2026-09-04. Not reviewed. Orchestration and hosting are deferred for the MVP; the model-selection table is a starting hypothesis to test against real runs, not a commitment.*
