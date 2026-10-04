# Communication Patterns — Salud Activa

## Current communication

| From | To | Mechanism | Evidence / status |
|------|----|-----------|-------------------|
| Android client | Backend | HTTP REST requests with JWT authentication; Retrofit is present in the Android build | Implemented |
| Web portal | Backend | Browser requests to the API; static assets are served by Express | Implemented in `backend/server.js` |
| Backend route modules | MongoDB | Mongoose models and configured domain connections | Implemented; same backend process |
| Turnos / chat routes | Firebase Cloud Messaging | Backend calls the Firebase Admin notification helper | Implemented as an external push integration |
| Firebase Cloud Messaging | Android client | Firebase Messaging SDK/service | Present in Android source |
| Backend service to backend service | — | No independently deployed internal service call evidenced | Not applicable today |
| Backend modules over a broker | — | No Kafka/RabbitMQ broker or event consumer/producer evidenced | Not implemented |

## Current API prefixes

The Express application mounts `/api/auth`, `/api/turnos`, `/api/medicamentos`,
`/api/estudios`, `/api/chat`, and `/api/admin` in the same Node process. See
[`backend/server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js).

## Guidance if services are extracted

- **Synchronous REST:** use for user-facing commands and queries requiring an immediate
  response. Keep contracts in OpenAPI and set explicit timeouts; do not make long chains
  of synchronous service calls.
- **Asynchronous events:** introduce only for a real decoupling need (for example, a
  committed appointment triggering notification delivery). Select a broker, schema/version
  policy, retry/dead-letter behavior, idempotency, and outbox strategy in an ADR before
  implementation.
- **Authentication:** preserve server-side authorization at each owner boundary. A gateway
  may perform edge concerns, but must not be treated as a replacement for service-level
  authorization.
- **Service discovery:** unnecessary while one backend process owns the routes. If separate
  services are deployed, document discovery and network security in the deployment design.
- **Push notifications:** FCM is an external provider, not a message broker for internal
  domain events. Record delivery/retry semantics separately from business persistence.

## Failure and consistency requirements for future work

- A notification provider outage should not report that a persisted appointment was rolled
  back. Define retry/reconciliation behavior and communicate delivery state where required.
- Appointment availability and creation currently use separate database operations; do not
  assume they are atomic. Resolve concurrent booking before promising a reservation guarantee.
- Any asynchronous consumer must tolerate duplicate delivery and record failures explicitly.

## Evidence

- [`server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js)
- [`turnos.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/turnos.js)
- [`chat.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/chat.js)
- [`notification.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/middleware/notification.js)
- [`dbConnections.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/config/dbConnections.js)
