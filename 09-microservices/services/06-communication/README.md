# Communication and Notifications — Boundary Proposal

> **Status: Current capabilities are modules/integrations in one backend, not an independent
> service.** Chat persistence and FCM push delivery are distinct responsibilities and may not
> need to share a service boundary.

## Current capabilities

- Chat API at `/api/chat`, backed by the `Message` model and `chatConnection`.
- Push notification calls from Turnos and Chat routes through a Firebase Admin helper.
- Android receives push messages through Firebase Messaging.
- Chat currently creates a simulated doctor reply with an in-process timer; no durable queue
  or real clinician messaging workflow is established by that code.

Evidence: [`chat.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/chat.js),
[`Message.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Message.js),
[`notification.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/middleware/notification.js).

## Candidate boundaries

- **Chat capability:** owns message history, sender, recipient/conversation semantics, and
  authorization.
- **Notification adapter/service:** delivers approved notification requests through FCM;
  should not own appointment or chat business records.

These are design candidates only. Before extraction, specify delivery durability, retries,
invalid-token lifecycle, privacy-safe payloads, real chat participants, and whether an
asynchronous broker is justified.
