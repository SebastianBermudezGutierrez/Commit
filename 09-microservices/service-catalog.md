# Salud Activa — Service and Module Catalog

> **Architecture status:** the repository currently deploys one Node.js/Express backend.
> The domain rows below describe modules within that backend, not independently
> deployable microservices. The extraction candidates are proposals, not running services.

## Current deployable units

| Unit | Responsibility | Runtime / entry point | Data connections | Status |
|------|----------------|-----------------------|------------------|--------|
| Android client | User interface, local Room persistence, REST calls, FCM reception | Android application (`app/`) | Room is a client-side cache; not a backend source of truth | Implemented |
| Web portal | Patient web interface | Static files in `web/`, served by Express | None directly | Implemented |
| Salud Activa backend API | Authentication and the turnos, medication, studies, chat, and admin HTTP routes | One Node.js/Express process, `backend/server.js`; `PORT` or default `10000` | Mongoose connections configured in `backend/config/dbConnections.js`; a base `MONGODB_URI` connection is also opened by `server.js` | Implemented as a modular monolith |
| Firebase Cloud Messaging | External push-delivery provider | Firebase Admin integration in backend; Firebase Messaging client in Android | Provider-managed | External dependency |

## Backend modules (same process, not microservices)

| Module / bounded context | Current HTTP surface | Current model / connection | Source |
|--------------------------|----------------------|----------------------------|--------|
| Identity and access | `/api/auth` | `User` / `authConnection` | [`AuthRoutes.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/src/infrastructure/adapters/http/express/AuthRoutes.js), [`User.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/User.js) |
| Appointment management (Turnos) | `/api/turnos` | `Turno` / `turnosConnection` | [`turnos.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/turnos.js), [`Turno.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Turno.js) |
| Medication management | `/api/medicamentos` | `Medicamento` / `medicamentosConnection` | [`medicamentos.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/medicamentos.js), [`Medicamento.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Medicamento.js) |
| Medical studies | `/api/estudios` | `Estudio` / `estudiosConnection` | [`estudios.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/estudios.js), [`Estudio.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Estudio.js) |
| Chat | `/api/chat` | `Message` / `chatConnection` | [`chat.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/chat.js), [`Message.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Message.js) |
| Administration | `/api/admin` | Imports several domain models | [`admin.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/admin.js) |
| Push notifications | No standalone API; invoked from turnos and chat routes | Uses a user's FCM token and Firebase Admin | [`notification.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/middleware/notification.js) |

## Candidate extraction order (proposed, not deployed)

| Candidate | Source module today | Initial boundary | Status |
|-----------|--------------------|-----------------|--------|
| Identity and access service | `/api/auth`, `User` | Credentials, account identity, authentication and profile ownership must be explicitly separated before extraction | Proposed |
| Appointment service | `/api/turnos`, `Turno` | Appointment lifecycle and slot reservation; needs atomic booking before scaling independently | Proposed |
| Medication service | `/api/medicamentos`, `Medicamento` | Patient-owned medication records and schedule data | Proposed |
| Medical-studies service | `/api/estudios`, `Estudio` | Patient-owned study metadata and secure document lifecycle | Proposed |
| Communication/notification service | `/api/chat`, FCM calls | Chat records and notification delivery have different responsibilities; split only if justified | Proposed |
| API gateway | Routes currently mounted by `backend/server.js` | A separate edge proxy is not present; introduce only when independent services require it | Proposed |

No service-specific port, database, broker topic, production URL, or deployment status is assigned to a proposed extraction. Those require an approved ADR and implementation evidence.

## Current request routing

`backend/server.js` mounts route modules directly in Express:

| Prefix | Route module |
|--------|--------------|
| `/api/auth` | `src/infrastructure/adapters/http/express/AuthRoutes.js` |
| `/api/turnos` | `routes/turnos.js` |
| `/api/medicamentos` | `routes/medicamentos.js` |
| `/api/estudios` | `routes/estudios.js` |
| `/api/chat` | `routes/chat.js` |
| `/api/admin` | `routes/admin.js` |

This is in-process routing, not an API gateway proxying to independent services.

## Related documents

- [Boundary rules](./service-boundary-rules.md)
- [Communication patterns](./communication-patterns.md)
- [Event catalog](./event-catalog.md)
- [Data ownership matrix](./data-ownership-matrix.md)
- [Dependency map](./dependency-map.md)
- [Storage and documents](./storage-and-documents.md)
- [Service readiness checklist](./service-readiness-checklist.md)
- [Current backend API](./services/00-current-backend-api/README.md)
- [Proposed identity service](./services/02-auth-service/README.md)
- [Proposed appointment service](./services/03-appointment-service/README.md)
- [Proposed medication service](./services/04-medication-service/README.md)
- [Proposed medical-studies service](./services/05-medical-studies-service/README.md)
- [Proposed communication capabilities](./services/06-communication/README.md)
- [Proposed gateway](./services/01-api-gateway/README.md)
