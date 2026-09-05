# Autonomy Tiering

*How much any agent may do on its own. The canonical statement of the model; each [agent doc](../agents/) works out what the quadrants mean concretely for that step.*

## Two axes, not one

- **Is the path known?** — has this exact (or close enough) pattern been done before, with an established, reviewed precedent (an approved Terraform module, a standard test suite, a familiar architectural pattern)?
- **Is it safe?** — regardless of precedent, what is the blast radius if the agent is wrong, and how easily is it reversed?

## The 2×2

The two mixed quadrants need **opposite** handling — not identical "partial autonomy."

| | **Safe** | **Risky** |
|---|---|---|
| **Known** | **Full autonomy.** Execute directly. | **Autonomous prep, human-gated execution.** The agent drafts and stages the full change; a human approves before it goes live — not because the agent is uncertain, but because the cost of being wrong is high enough to warrant a second set of eyes regardless of confidence. |
| **Unknown** | **Autonomous execution, human audit after.** The agent acts; review is an audit, not a gate, because mistakes here are cheap to catch and cheap to undo. | **Manual.** Default to a human; the agent assists (drafting, summarizing, prepping) but does not act. |

## Granularity: the task-graph node, not the request

A single request is rarely uniformly risky. Gating the whole thing on its riskiest component wastes the autonomy everything else earned. The right unit is the task-graph node: everything whose dependency closure does not touch the novel/risky piece proceeds autonomously; only the novel node — and anything downstream of it — routes to a human.

This applies across every agent: Implementation can build the known-safe branches while a novel branch is gated; Testing can run standard suites autonomously while flagging only the new code path; Enablement can apply catalog modules while a novel module routes to a ticket.

## Inputs to the classification

`path-known?`, `safe?`, blast radius, data classification, environment (dev/prod), reversibility. In the MVP this is a pure function — `classify_autonomy(facts) → Tier` — not a system to deploy. See [`rules-and-policy.md`](rules-and-policy.md) for when that becomes a decision engine.

## Related

- The three **human gates** ([Requirements Sign-off](../agents/requirements-signoff-gate.md), [Solution Sign-off](../agents/solution-signoff-gate.md), [Merge & Release Approval](../agents/merge-release-approval-gate.md)) stay human regardless of tier ([`../decisions/log.md`](../decisions/log.md), D-10).
- The [Definition Agent](../agents/definition-agent.md) adds a per-function *demarcation* on top of this — what an agent can reason on now, what needs data access, and what stays permanently human — because the L1→L2 hop's limits are about information availability, not just blast radius.
