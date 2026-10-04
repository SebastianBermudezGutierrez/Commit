# Service Readiness Checklist — Salud Activa

> A domain module is not production-ready as an independent microservice just because it has
> a route prefix or a separate MongoDB connection. Complete this checklist for a proposed
> extraction and attach evidence before describing it as deployed.

## Boundary and ownership

- [ ] Approved ADR states the measurable reason for extraction and alternatives considered.
- [ ] Bounded context, service owner, responsibilities, and out-of-scope areas are documented.
- [ ] One authoritative owner exists for each entity; cross-service database access is removed.
- [ ] Cross-domain admin/reset operations are replaced by audited owner-controlled operations.
- [ ] Personal and health data minimization, authorization, retention, and deletion are reviewed.

## API and compatibility

- [ ] Versioned OpenAPI contract exists and is validated in CI.
- [ ] Authentication and resource-level authorization are tested at the service boundary.
- [ ] Timeouts, error codes, pagination, idempotency, and compatibility policy are documented.
- [ ] Consumer/provider contract tests run in CI.

## Persistence and migration

- [ ] Service owns independent database credentials and deployment/configuration.
- [ ] Data migration, verification, rollback, backup, and restore procedures are tested.
- [ ] Indexes, retention, encryption, and sensitive-data handling are documented.
- [ ] Concurrent appointment reservation is safe if this is the Turnos service.

## Reliability and operations

- [ ] Health/readiness endpoints distinguish process health from dependency health.
- [ ] Logs, metrics, traces, dashboards, alerts, SLOs, RTO, and RPO have verified owners/values.
- [ ] Resource limits, scaling policy, rate limits, and failure behavior are tested.
- [ ] Runbook uses commands for the actual deployment platform (not placeholder Kubernetes commands).
- [ ] Incident, rollback, and secret-rotation procedures are reviewed.

## Delivery evidence

- [ ] Independent build/deploy workflow and environment configuration exist.
- [ ] Automated unit, integration, security, and contract checks pass.
- [ ] Deployment URL and current release are verifiable.
- [ ] Service catalog, dependency map, data ownership, API docs, and UML are updated.

**Current status:** this checklist is a gate for future extraction. It does not assert that
any proposed service has passed it.
