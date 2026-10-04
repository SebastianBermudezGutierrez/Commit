# Appointment Service — Extraction Proposal

> **Status: Proposed boundary, not an independent service.** Appointment routes and the
> `Turno` model currently run inside the shared backend.

## Current scope and evidence

The current `/api/turnos` routes list a patient's appointments, check slot availability,
create appointments, and cancel them. The `Turno` model uses `turnosConnection`.

- [`turnos.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/turnos.js)
- [`Turno.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Turno.js)
- [Merged availability-test PR](https://github.com/BrayanYara1/ProyectoDistribuidos2026/pull/1)

## Candidate owner

Own appointment lifecycle, availability, reservation integrity, cancellation, and appointment
history. Patient identity should be represented by an identifier and authorized claims/API,
not by direct reads from the Identity database.

## Open requirements before extraction

- Make availability and reservation concurrency-safe; a check followed by a later save is not
  an atomic reservation guarantee.
- Define slot/resource uniqueness, time zone, valid state transitions, cancellation rules,
  and idempotent creation.
- Replace direct `User` model access for push tokens with an explicit notification contract.
- Preserve endpoint compatibility and add integration/contract tests before moving routes.

The current API contract is maintained under [`07-api/`](https://github.com/code-corhuila/appt-mgmt-docs/tree/main/07-api/). Do not assign an
independent port, database deployment, event topic, or production status until implemented.
