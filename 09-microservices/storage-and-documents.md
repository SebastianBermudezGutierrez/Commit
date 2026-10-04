# Storage and Documents — Salud Activa

## Verified storage

| Data | Current location | Status / notes |
|------|------------------|----------------|
| User/account and profile records | MongoDB through the `authConnection` Mongoose model | Current backend model is `User`; it includes credentials, health/profile fields, and FCM token |
| Appointments | MongoDB through `turnosConnection` | Current `Turno` model and route |
| Medication records | MongoDB through `medicamentosConnection` | Current `Medicamento` model and route |
| Medical study records | MongoDB through `estudiosConnection` | Current `Estudio` metadata model and route |
| Chat messages | MongoDB through `chatConnection` | Current `Message` model and route |
| Offline client data | Android Room | Local client persistence/cache; synchronization semantics should be verified per feature |
| Push delivery | Firebase Cloud Messaging | External notification provider, not durable application document storage |

The backend also opens a base `MONGODB_URI` connection from `backend/server.js` in addition
to the domain-specific Mongoose connections created by `dbConnections.js`.

## File/blob storage status

The inspected server routes and model definitions do not establish a dedicated object-storage
contract or bucket lifecycle for medical-study files. Do not document S3, MongoDB GridFS,
local disk persistence, encryption, retention, or backup guarantees as active without
deployment/configuration evidence.

Before storing real healthcare documents, define:

- storage owner and metadata-to-object reference model;
- authorization for upload, download, deletion, and signed URL issuance;
- encryption in transit and at rest, key ownership, audit logging, and access review;
- malware/content validation, file-size/type limits, retention and deletion policy;
- backup/restore and disaster-recovery procedure;
- how offline clients cache files and reconcile deletions.

Medical-study metadata and binary document storage may have separate owners; the contract
must make that distinction explicit.
