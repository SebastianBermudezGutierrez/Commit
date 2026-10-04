# Service Boundary Rules — Salud Activa

## Architectural decision in force

The current implementation is a single backend application with route/model modules.
The service boundaries below are **design boundaries inside that application** until
separate deployment and ownership are demonstrated.

## Boundary rules

1. **One business capability owns each write model.** Other capabilities must not mutate
   that model directly. Shared database connections do not grant shared ownership.
2. **Use stable contracts across boundaries.** Inside the current process, prefer application
   services and typed/validated DTOs. After extraction, use versioned REST contracts or
   documented asynchronous events; do not share Mongoose models.
3. **Keep identity separate from patient profile deliberately.** The current `User` model
   combines credentials, contact/medical profile fields, and the FCM token. Do not split
   these fields across services until ownership, authorization, migration, and privacy rules
   are agreed.
4. **Appointment reservation is a single business operation.** Availability check and
   reservation must become atomic or use a concurrency-safe reservation mechanism before
   independent horizontal scaling is considered.
5. **Notifications are side effects, not owners of appointment or chat state.** Their failure
   must not silently roll back a committed appointment/message; delivery retries and status
   handling need an explicit design.
6. **Do not create a service for every collection or route.** Extract only where independent
   scaling, team ownership, release cadence, security boundary, or domain autonomy has a
   measurable benefit that outweighs distributed-system cost.
7. **No cross-service database reads.** This is a target constraint, not a claim that the
   current code already enforces it.
8. **Administrative operations must respect ownership.** `routes/admin.js` currently imports
   multiple domain models to reset records. In a multi-service design, replace this with a
   controlled administrative workflow that each owner authorizes and audits.

## Known current coupling to resolve

| Source | Observed coupling | Before extraction |
|--------|-------------------|-------------------|
| `backend/routes/turnos.js` | Appointment routes import `User` to obtain/clear FCM token | Move notification preference/token access behind an interface or notification contract |
| `backend/routes/chat.js` | Chat routes import `User` to access/clear FCM token | Same as above; keep message persistence owned by Chat |
| `backend/routes/admin.js` | One route deletes records across several models | Replace cross-model writes with owner-controlled operations |
| `backend/server.js` | All HTTP routes are mounted in one Express process | Define independent configuration, health, deployment, and release lifecycle |
| `backend/config/dbConnections.js` | Multiple Mongoose connections are constructed in the same application | Isolate credentials, migrations, backup/restore, and access per deployable |

## Service extraction gate

Before approving a boundary, document:

- the bounded context and owner in [`02-domain/domain-map.md`](https://github.com/code-corhuila/appt-mgmt-docs/blob/main/02-domain/domain-map.md);
- its API and compatibility strategy in [`07-api/`](https://github.com/code-corhuila/appt-mgmt-docs/tree/main/07-api/);
- authoritative data, migration, and rollback plan;
- inbound/outbound dependencies and failure behavior;
- operational ownership and a service-specific deployment/runbook;
- an ADR that explains why a separate deployable is preferable to another module.
