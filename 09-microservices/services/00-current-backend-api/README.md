# Current Deployable — Salud Activa Backend API

> **Status: Implemented as one backend application. This is not a microservice.**

## Responsibility

The Node.js/Express backend serves the web assets and exposes authentication,
appointment, medication, medical-study, chat, administration, and status routes.

## Location in the architecture

| Field | Value |
|-------|-------|
| Source repository | [BrayanYara1/ProyectoDistribuidos2026](https://github.com/BrayanYara1/ProyectoDistribuidos2026) |
| Entry point | [`backend/server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js) |
| Runtime | Node.js / Express |
| Local port | `PORT`, default `10000` |
| Deployment unit | One Express application; independently deployed domain services not evidenced |
| Persistence | MongoDB via Mongoose, with domain connections in `backend/config/dbConnections.js` |

## Mounted routes

| Prefix | Capability |
|--------|------------|
| `/api/auth` | Register, login, profile, FCM token |
| `/api/turnos` | List, check availability, create, cancel appointments |
| `/api/medicamentos` | Authenticated medication CRUD |
| `/api/estudios` | Authenticated medical-study metadata operations |
| `/api/chat` | Authenticated message history and patient message creation |
| `/api/admin` | Protected database reset operation |
| `/api/status` | Process status response |
| `/` | Backend status page |

The Express process also serves static files from `web/`. Route mounting is direct
in-process routing, not proxying through a separate gateway.

## Local verification

From the `backend/` directory, use the repository's `package.json` scripts and
environment setup. Configure valid local environment variables without committing
secrets. The source exposes `GET /api/status`; this endpoint reports application
status and is not a full dependency-readiness check.

Do not use production credentials or run the admin reset endpoint against shared data.

## Extraction notes

The candidate service notes under this section describe possible future boundaries.
Before any extraction, address the cross-module imports and administrative operation
documented in [boundary rules](../../service-boundary-rules.md), and pass the
[readiness checklist](../../service-readiness-checklist.md).
