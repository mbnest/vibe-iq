# Architecture Principles

*How systems are structured here. Read by the Design / Solution stage. Each principle has a statement, why it exists, what adherence looks like, and how it is checked. A solution that violates a principle must call it out and justify it — a silent violation is a handoff-validation failure.*

*Placeholder set — confirm, cut, or add. Anything contested should get an ADR.*

## 1. Services own their data

**Statement.** A service is the only thing that writes to its database. Other services get that data through its API or its events, never by reading its tables directly.
**Why.** Shared databases turn independent services into a distributed monolith — a schema change means coordinating every reader.
**In practice.** One logical database per service. No cross-service foreign keys. Analytics and reporting read from a replica or an export, not the primary.
**Checked by.** Design review; a maintained schema-ownership map.

## 2. Asynchronous by default between services

**Statement.** Cross-service communication uses a queue or an event unless the caller genuinely cannot proceed without a synchronous answer.
**Why.** Synchronous call chains couple availability — every downstream outage becomes yours.
**In practice.** SQS or EventBridge for work that can be processed slightly later. Reserve synchronous HTTP for read paths a user is actively waiting on.
**Checked by.** Design review.

## 3. Design for failure

**Statement.** Every call to something outside the process has a timeout, a retry policy, and a defined behavior for when it ultimately fails.
**Why.** Networks and dependencies fail routinely; "it usually works" is not a design.
**In practice.** No unbounded waits. Idempotent handlers so retries are safe. An explicit fallback or a clean error — never a hang.
**Checked by.** Code review; failure testing where the risk warrants it.

## 4. Prefer managed services

**Statement.** Use the cloud provider's managed offering before self-hosting.
**Why.** Patching, backups, and failover are undifferentiated work a small team should not carry.
**In practice.** RDS over self-run Postgres; SQS over a self-run broker; Lambda or Fargate over a managed EC2 fleet.
**Checked by.** Design review; any self-hosted infrastructure needs an ADR.

## 5. Stateless application tier

**Statement.** Application processes hold no state that matters across requests. State lives in Postgres, Redis, or S3.
**Why.** Any instance can then handle any request, and any instance can be replaced at any time.
**In practice.** No in-memory sessions, no local-disk data. Sticky sessions are a smell.
**Checked by.** Design review.

## 6. Explicit contracts at boundaries

**Statement.** Every interface between components — API, event, queue message, file handoff — has a versioned, validated schema. The consumer validates inbound data; it does not assume the producer got it right.
**Why.** Boundaries are where drift and bugs hide. Catching a bad payload at the edge is cheap; catching it three hops later is forensic.
**In practice.** A Pydantic model or JSON Schema on every boundary. Schema changes are versioned and stay backward-compatible within a major version.
**Checked by.** Design review; contract tests.

## 7. Least privilege

**Statement.** Every component gets the narrowest set of permissions that lets it do its job, and nothing more.
**Why.** Limits the blast radius when something is compromised or misbehaves.
**In practice.** Per-service IAM roles scoped to specific resources. No wildcard resource ARNs. No shared credentials.
**Checked by.** Terraform review; policy-as-code where available.

## 8. Observability is part of the feature

**Statement.** A feature is not done until you can see it working in production — structured logs, the metrics that matter, and an alarm on the thing that would page someone.
**Why.** You cannot operate what you cannot see, and retrofitting observability after an incident is too late.
**In practice.** Structured JSON logs with a correlation ID. Rate, error, and duration metrics on every service. Alarms tied to user-visible symptoms, not just resource usage.
**Checked by.** Design and code review.

## 9. Infrastructure is code

**Statement.** Every piece of infrastructure is defined in Terraform and applied through the pipeline. No console changes.
**Why.** Reproducibility, review, and a real audit trail.
**In practice.** Production console access is read-only. Drift is detected and corrected.
**Checked by.** CI; drift detection.

## 10. Reversible over clever

**Statement.** Prefer the change that is easy to undo. Feature-flag risky rollouts. Migrate schemas in expand-then-contract steps.
**Why.** For a small team learning as it goes, the cost of a mistake matters more than the elegance of the solution.
**In practice.** Flags for behavior changes. Backward-compatible migrations deployed before the code that needs them. Deploys are revertible in one step.
**Checked by.** Design and code review.

*Status: starter pack, 2026-09-03. Placeholder set pending confirmation.*
