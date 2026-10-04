# Identity and Access Service — Extraction Proposal

> **Status: Proposed boundary; not a separately deployed service.** Authentication routes
> and the `User` model currently run inside the shared backend application.

## Current implementation

The mounted route module provides registration, login, profile update, FCM token update,
and compatibility endpoints for verification. It uses the application use-cases,
`MongooseUserRepository`, JWT, Express rate limiting, and the `User` model on
`authConnection`. The repository also contains `backend/routes/auth.js`; it is not the
router mounted in `server.js`, so the mounted router is the source of truth for the
current HTTP surface.

Evidence:

- [`AuthRoutes.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/src/infrastructure/adapters/http/express/AuthRoutes.js)
- [`AuthController.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/src/infrastructure/adapters/http/express/AuthController.js)
- [`backend/models/User.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/User.js)
- [`backend/config/dbConnections.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/config/dbConnections.js)

## Candidate responsibility

Own account credentials and identity lifecycle, authentication, and token issuance. Decide
separately whether contact/health profile data and device notification tokens belong here
or in dedicated profile/notification capabilities; the current `User` model combines them.

## Current route behavior (same backend)

| Method | Path | Current operation |
|--------|------|-------------------|
| POST | `/api/auth/register` | Register a user |
| POST | `/api/auth/login` | Authenticate and issue a JWT |
| PUT | `/api/auth/profile` | Update profile fields |
| POST | `/api/auth/fcm-token` | Store the caller's FCM token |
| POST | `/api/auth/verify` | Compatibility response |
| POST | `/api/auth/resend-code` | Compatibility response |

These are current backend routes, not a separately versioned service contract. Use
[`07-api/`](https://github.com/code-corhuila/appt-mgmt-docs/tree/main/07-api/) for API documentation and verify it against code.

## Extraction blockers / requirements

- Separate account identity from medical profile and FCM-token ownership.
- Replace any insecure development fallback for JWT signing configuration before production
  extraction; keep signing secrets out of source control.
- Define token lifetime, key rotation, logout/revocation, verification, and password policies
  in the contract and tests.
- Remove cross-module direct access to the `User` model; expose minimal authorized operations.
- Do not copy the template's PostgreSQL, Redis, refresh-token, or RBAC design as existing
  behavior: those are not evidenced by the current model/routes.

## Related documents

- [Data ownership matrix](../../data-ownership-matrix.md)
- [Boundary rules](../../service-boundary-rules.md)
- [Service readiness checklist](../../service-readiness-checklist.md)
