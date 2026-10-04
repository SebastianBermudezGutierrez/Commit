# Data Ownership Matrix — Salud Activa

> This matrix maps the current model/connection arrangement. The current code is still one
> backend application; the target ownership rules do not imply separately deployed services.

| Data / model | Current owner module | Current persistence | Evidence | Other current access / notes |
|--------------|----------------------|---------------------|----------|-----------------------------|
| Credentials, account profile, contact data, FCM token | Identity / Auth | `User` model on `authConnection` | [`User.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/User.js) | `turnos`, `chat`, and `admin` routes also import `User`; split ownership needs design |
| Appointment | Turnos | `Turno` model on `turnosConnection` | [`Turno.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Turno.js) | Appointment route owns list, availability, create, and delete behavior |
| Medication | Medication management | `Medicamento` model on `medicamentosConnection` | [`Medicamento.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Medicamento.js) | Authenticated CRUD routes use the patient/user identifier |
| Medical study metadata | Medical studies | `Estudio` model on `estudiosConnection` | [`Estudio.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Estudio.js) | The reviewed route stores model data; a dedicated object-storage service was not established |
| Chat message | Chat | `Message` model on `chatConnection` | [`Message.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Message.js) | Chat route also reads user FCM token |
| Push token / delivery | Identity stores token; Firebase delivers push | `User.fcmToken` plus Firebase provider | [`notification.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/middleware/notification.js) | Ownership is coupled; define token lifecycle and notification responsibility before splitting |
| Local mobile cache | Android client | Room | Android `app/` source | Client cache, not authoritative backend data |

## Target rules

1. One service owns writes and schema changes for each business entity.
2. Other services receive only the minimum necessary data through explicit API/event contracts.
3. Do not use a shared Mongoose model package to bypass service boundaries.
4. A user ID in another domain is an identifier, not a cross-database foreign-key relationship.
5. Cross-owner administrative actions must be authenticated, authorized, audited, and routed
   through the data owner.

## Known exception to resolve

[`backend/routes/admin.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/admin.js)
imports multiple models and deletes across domains through a single reset endpoint. Retain this
only as a tightly controlled development/admin capability; it is not a pattern to carry into
independently deployed services.
