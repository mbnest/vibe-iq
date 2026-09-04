# ADR-0002: Target application stack for the MVP

## Status

Accepted - 2026-09-03

## Context

The pipeline's Design and Implementation agents generate code, and they need a
defined target stack to generate *for* - `allowed-stack.md` cannot be populated
without one. The choice should be a well-trodden stack with abundant training
data, so the coding agents perform well, and one a small team can operate with a
light footprint. This is the stack the platform's agents build in; it is not a
statement about what the platform itself is implemented in.

## Decision

We will target:

- **Languages.** Python 3.12+ for services and data work; TypeScript 5.x for
  frontend and Node tooling.
- **Backend.** FastAPI, Pydantic v2, SQLAlchemy 2.x, Alembic; `uv` for packaging.
- **Frontend.** React 18 with Vite.
- **Data.** PostgreSQL 15+ as the primary datastore; Redis for cache and
  ephemeral state; S3 for objects.
- **Cloud.** AWS - Lambda and ECS Fargate for compute, RDS / Aurora Postgres,
  SQS, API Gateway, Secrets Manager, CloudWatch.
- **Cross-cutting.** Terraform for all infrastructure; GitHub Actions for CI/CD.

Ring assignments and detail live in
[`../engineering-context/allowed-stack.md`](../engineering-context/allowed-stack.md).

## Consequences

- The coding agents operate in a mainstream stack with strong model support.
- Choices outside this set now require an explicit exception (a new ADR) rather
  than being made ad hoc.
- Ties the MVP to AWS specifically; a multi-cloud or different-cloud future would
  need `allowed-stack.md` reworked and this ADR superseded.
- Go and Next.js are deliberately parked at Assess - available for spikes, not
  production paths, without further discussion.

**Revisit when:** a real workload cannot be met on this stack, or the platform
needs to target a second stack for a different vertical.
