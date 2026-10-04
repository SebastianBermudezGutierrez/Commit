# Event Catalog — Salud Activa

## Current event-broker status

No internal message broker, event producer/consumer, or published domain-event contract
was found in the inspected backend source. **The events below are not currently emitted
or consumed by the application.**

The backend sends push notifications through Firebase after selected appointment and chat
operations. These are direct external side effects, not evidence of a durable event bus:
the routes call `sendPushNotification` and handle a subset of invalid-token responses.

## Existing externally visible notifications

| Trigger | Current behavior | Durability / caveat |
|---------|------------------|---------------------|
| Appointment created | Turnos route saves the record, then requests an FCM push when the user has a token | Direct best-effort call; no outbox/retry queue evidenced |
| Appointment cancelled | Turnos route deletes the record, then requests an FCM push | The current implementation deletes the record; direct best-effort call, with no durable delivery record evidenced |
| Chat message | Chat route saves the patient's message; schedules a simulated doctor reply and may send FCM | The simulated reply uses an in-process timer, not a durable queue |

## Candidate domain events (proposals only)

| Proposed event | Potential owner | Possible consumers | Status |
|----------------|-----------------|-------------------|--------|
| `AppointmentScheduled` | Appointment management | Notification delivery, patient history | Not implemented |
| `AppointmentCancelled` | Appointment management | Notification delivery, patient history | Not implemented |
| `MedicationRecorded` | Medication management | Reminder scheduling | Not implemented |
| `MedicalStudyStored` | Medical studies | Patient timeline | Not implemented |
| `MessageCreated` | Chat | Push notification delivery | Not implemented |

Names and payloads must be approved against [`02-domain/domain-events.md`](https://github.com/code-corhuila/appt-mgmt-docs/blob/main/02-domain/domain-events.md)
before they are treated as contracts. Do not publish credentials, JWTs, medical details, or
unnecessary personally identifiable information in event payloads.

## Requirements before introducing a broker

- Select broker and ownership model through an ADR.
- Define versioned schemas, event identifiers, timestamps, correlation/causation identifiers,
  and data minimization.
- Specify delivery guarantee, retries, dead-letter handling, replay policy, and idempotency.
- Use a transactional outbox or equivalent where database commit and event publication must
  remain consistent.
- Add contract and integration tests plus alerting for stuck/failed messages.
