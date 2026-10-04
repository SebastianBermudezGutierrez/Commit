# Identity Service Decisions

> This file records extraction questions. It does not supersede project-level ADRs.

## Current state

Authentication is implemented as routes within the shared Express backend. The code uses
the existing `User` model and does not evidence a separate auth deployment, Redis-backed
session/blacklist, refresh-token lifecycle, or gateway token verification.

## Open decisions

- **Identity versus patient profile:** decide which fields Identity owns and which belong to
  a patient-profile capability before moving data.
- **Token and key lifecycle:** specify supported algorithms, expiration, rotation, revocation,
  and client storage based on the live contract and security requirements.
- **Notification token ownership:** remove the current direct `User` model imports from
  Turnos and Chat by defining an authorized interface.
- **Migration and rollback:** define how existing users and credentials move without exposing
  or resetting secrets.

Record an accepted decision with date, rationale, alternatives, consequences, and evidence
before implementing a separate service.
