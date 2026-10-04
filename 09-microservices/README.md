# 09 — Microservices and Service Boundaries

This section records Salud Activa's **current backend modules** and a cautious,
evidence-based path toward possible service extraction. It does not assert that the
system already runs as microservices.

## Current architecture

The application currently has one Node.js/Express backend process. Its routes are
mounted by `backend/server.js`; backend capabilities use domain-specific Mongoose
connections but remain in one deployable application and share the same runtime.
The architecture overview describes a modular monolith as the current style.

**Separate MongoDB connections do not, by themselves, make a system a set of
microservices.** Independent deployment, explicit service contracts, operational
ownership, and enforced data boundaries would also be required.

## Documents in this section

| Document | Purpose |
|----------|---------|
| [Service catalog](./service-catalog.md) | Current deployables, in-process modules, and proposed extraction candidates |
| [Service boundary rules](./service-boundary-rules.md) | Ownership rules and coupling to remove before extraction |
| [Communication patterns](./communication-patterns.md) | Verified current calls and future integration guidance |
| [Event catalog](./event-catalog.md) | Distinguishes existing push side effects from proposed domain events |
| [Data ownership matrix](./data-ownership-matrix.md) | Current persistence ownership and known cross-domain access |
| [Dependency map](./dependency-map.md) | Current runtime dependencies and proposed target |
| [Storage and documents](./storage-and-documents.md) | Verified persistence and file-storage gaps |
| [Service readiness checklist](./service-readiness-checklist.md) | Minimum evidence needed before an extraction can go live |

Individual service notes are in [`services/`](./services/). Proposed service notes
are explicitly marked as proposals; they are not runbooks for deployed systems.
[`_template/service/`](./_template/service/README.md) remains a generic template for
future approved services.

## Status language

- **Implemented:** verified in the source repository as part of the current application.
- **Proposed:** a possible future service boundary; not independently deployable today.
- **Unknown / not evidenced:** the repository does not establish the behavior or configuration.

## Rules for keeping this section accurate

1. Link each implementation claim to source, contract, test, or deployment evidence.
2. Do not label a module a microservice solely because it has its own database connection.
3. Do not document broker topics, service ports, production URLs, SLOs, or run commands as
   active until they exist and can be verified.
4. Update the catalog, dependency map, ownership matrix, API contracts, UML, and operations
   docs together when a boundary or deployment changes.

## Sources of truth

- Backend entry point and mounted routes: [`backend/server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js)
- Domain database connections: [`dbConnections.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/config/dbConnections.js)
- Backend route and model source: [`ProyectoDistribuidos2026/backend`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/tree/main/backend)
- Current system architecture: [`05-architecture/overview.md`](https://github.com/code-corhuila/appt-mgmt-docs/blob/main/05-architecture/overview.md)
- Domain map: [`02-domain/domain-map.md`](https://github.com/code-corhuila/appt-mgmt-docs/blob/main/02-domain/domain-map.md)
- API contracts: [`07-api/`](https://github.com/code-corhuila/appt-mgmt-docs/tree/main/07-api/)
