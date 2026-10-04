# Dependency Map — Salud Activa

## Current runtime

```mermaid
flowchart LR
    android[Android app] -->|REST / JWT| api[Single Express backend]
    web[Web portal] -->|HTTP API| api
    api --> auth[Auth routes and User model]
    api --> turnos[Turnos routes and model]
    api --> meds[Medication routes and model]
    api --> studies[Studies routes and model]
    api --> chat[Chat routes and model]
    turnos --> user[(User model / auth database connection)]
    chat --> user
    admin[Admin reset route] --> auth
    admin --> turnos
    admin --> meds
    admin --> studies
    turnos --> fcm[Firebase Cloud Messaging]
    chat --> fcm
    fcm --> android
```

All backend route modules run in one Node.js process. Domain-specific Mongoose connections
are configured in `backend/config/dbConnections.js`, while routes still have direct
cross-module imports for user notification data and administrative resets.

## Target if extraction is approved

```mermaid
flowchart LR
    clients[Android and web clients] --> edge[Optional API gateway]
    edge --> identity[Identity service]
    edge --> appointments[Appointment service]
    edge --> medication[Medication service]
    edge --> studies[Medical-studies service]
    edge --> chat[Chat service]
    appointments --> notify[Notification adapter/service]
    chat --> notify
    notify --> fcm[Firebase provider]
```

This is a **candidate topology**, not an implemented deployment. A broker is intentionally
not drawn: it should be added only after an approved event-driven use case and operational
design exist.

## Dependency review rules

- Keep dependencies directed; do not create cycles between domain owners.
- Prefer identity tokens/claims for authentication context; avoid synchronous profile lookups
  on every internal call unless a specific authorization requirement demands it.
- Define timeouts, retries, idempotency, and user-visible failure behavior for every remote call.
- Update this map when source imports, deployment units, API contracts, or data ownership change.
