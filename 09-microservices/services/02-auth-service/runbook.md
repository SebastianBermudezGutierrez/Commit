# Identity Service Runbook

**Status:** There is no standalone Identity service to operate today. Authentication routes
run within the current backend process.

For local backend startup and status verification, use
[`services/00-current-backend-api/runbook.md`](../00-current-backend-api/runbook.md).

Do not use generic template commands for PostgreSQL, Redis, Kubernetes, or a standalone
auth port; those components and deployment procedures are not evidenced in this repository.
Create a real service runbook only after extraction, with verified dashboards, health checks,
secret rotation, database recovery, incident owner, and rollback procedure.
