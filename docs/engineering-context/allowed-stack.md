# Allowed Stack

*Approved technology choices, expressed as Technology Radar rings. Read by the Design / Solution stage; a solution that steps outside Adopt/Trial without a recorded exception is rejected at handoff validation.*

**Rings.** **Adopt** — default choice, no justification needed. **Trial** — usable with a documented reason; expect scrutiny at review. **Assess** — spike or prototype only, not in production paths. **Hold** — do not use for new work; existing use is legacy.

*Placeholder content — confirm each line or change it. Decided by [ADR-0002](../adr/0002-target-application-stack.md); reasoning for anything non-obvious added later should become its own ADR that this file links to.*

## Languages

| Technology | Ring | Notes |
|---|---|---|
| Python 3.12+ | Adopt | Backend services, data and pipeline work |
| TypeScript 5.x | Adopt | Frontend, and Node tooling |
| SQL | Adopt | PostgreSQL dialect |
| Bash | Trial | Small scripts only; reach for Python past ~30 lines |
| Go | Assess | Revisit if a service needs a performance profile Python cannot meet |
| Java | Hold | No new services |

## Backend / services

| Technology | Ring | Notes |
|---|---|---|
| FastAPI | Adopt | Default HTTP service framework |
| Pydantic v2 | Adopt | Validation, settings, boundary contracts |
| SQLAlchemy 2.x | Adopt | ORM and query builder |
| Alembic | Adopt | Schema migrations |
| uv | Adopt | Python packaging and environment management |
| Celery | Trial | Background jobs; prefer SQS-triggered Lambda for simple cases |

## Frontend

| Technology | Ring | Notes |
|---|---|---|
| React 18 | Adopt | |
| Vite | Adopt | Build tooling |
| TanStack Query | Adopt | Server state |
| Tailwind CSS | Adopt | Styling |
| Next.js | Assess | Only if server-side rendering or SEO is a real requirement |

## Data stores

| Technology | Ring | Notes |
|---|---|---|
| PostgreSQL 15+ | Adopt | Primary datastore (AWS RDS / Aurora Postgres) |
| SQLite | Trial | Local dev, tests, demos, and single-node low-volume workloads only. Never for a multi-instance service - it breaks "services own their data" at scale and the stateless-tier principle. Keep schema Postgres-compatible so a workload can graduate. |
| Redis 7 | Adopt | Cache, ephemeral state, rate limiting (AWS ElastiCache) |
| S3 | Adopt | Object storage |
| DuckDB | Assess | Local analytical work only |
| MongoDB | Hold | Use PostgreSQL JSONB instead |

## Cloud (AWS)

| Service | Ring | Notes |
|---|---|---|
| Lambda | Adopt | Event-driven and glue compute |
| ECS Fargate | Adopt | Long-running services |
| RDS / Aurora Postgres | Adopt | |
| SQS | Adopt | Async messaging between components |
| EventBridge | Trial | Cross-service events; prefer over bespoke fan-out |
| S3 | Adopt | |
| Secrets Manager | Adopt | All secrets; never in code or plain env files |
| API Gateway | Adopt | Public HTTP entry to Lambda |
| Step Functions | Trial | Durable multi-step workflows |
| CloudWatch | Adopt | Logs, metrics, alarms |

## Cross-cutting

| Technology | Ring | Notes |
|---|---|---|
| Terraform | Adopt | All infrastructure as code |
| GitHub Actions | Adopt | CI/CD |
| Docker | Adopt | Local dev and Fargate images |
| OpenTelemetry | Trial | Tracing and metrics instrumentation |

## Exceptions

To use something in Hold, or anything outside these lists: write an ADR proposing it, get it approved, then move the entry to the appropriate ring here.

*Status: starter pack, 2026-09-03. Every ring assignment is a placeholder pending confirmation.*
