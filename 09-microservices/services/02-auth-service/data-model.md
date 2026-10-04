# Identity Data — Current Model and Future Questions

## Current source model

The current `User` Mongoose schema is on the `authConnection`. It includes:

- name, email, phone, and password;
- verification flag and verification code;
- FCM token;
- blood type, allergies, conditions, emergency contact;
- creation date.

See [`backend/models/User.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/User.js)
for field definitions, constraints, and defaults. No separate roles collection, Redis token
blacklist, refresh-token collection, or PostgreSQL identity schema was verified.

## Ownership questions before extraction

1. Should medical profile fields remain part of Identity, or be owned by a patient-profile
   capability?
2. Which service owns FCM device-token registration and revocation?
3. What is the identifier and lifecycle policy for patient identity across services?
4. How are data export, correction, deletion, and migration handled?
5. Which fields may appear in JWT claims, logs, events, or downstream caches?

Do not copy the generic SQL schema in the original scaffold; the implemented backend uses
Mongoose/MongoDB.
