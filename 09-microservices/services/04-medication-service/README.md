# Medication Service — Extraction Proposal

> **Status: Proposed boundary, not an independent service.** Medication CRUD routes and the
> `Medicamento` model currently live in the shared backend.

## Current scope and evidence

The `/api/medicamentos` routes provide authenticated listing, creation, update, and deletion.
The model is configured on `medicamentosConnection`.

- [`medicamentos.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/medicamentos.js)
- [`Medicamento.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Medicamento.js)

## Candidate owner

Own patient medication records and medication-specific schedule/reminder data if that is
confirmed in the implemented domain and acceptance criteria.

## Open requirements before extraction

- Document actual fields, validation, schedule semantics, reminder behavior, and lifecycle.
- Define patient authorization, privacy, deletion/export, and mobile offline conflict behavior.
- Clarify whether reminders are an internal module or a separate notification capability.
- Add a versioned contract and tests before assigning a standalone runtime or database.
