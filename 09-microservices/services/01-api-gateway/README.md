# API Gateway — Extraction Proposal

> **Status: Proposed.** The current project does not have a separately deployed API
> gateway. `backend/server.js` mounts all route modules in one Express process and serves
> the web portal assets.

## Possible responsibility

If independently deployed backend services are introduced, an edge gateway could route
client traffic, apply cross-cutting transport limits, and provide a consistent external
entry point. Business authorization and data ownership must remain with the owning service.

## Not current behavior

- No dedicated gateway product, process, port, repository, Redis cache, or gateway deployment
  was verified.
- No evidence supports the old example routes, JWT header injection, or Redis rate-limit
  design in this folder's original template.
- Do not run `docker compose up api-gateway` or use a fabricated health URL; no such
  implementation is documented.

## Decision gate

Keep direct Express routing while the backend is one deployable. Revisit a gateway only
after service extraction is approved, with an ADR covering routing, authentication,
authorization, rate limits, TLS termination, observability, and failure behavior.

## Current source

- [`backend/server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js)
- [Service catalog](../../service-catalog.md)
