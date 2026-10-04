# Medical Studies Service — Extraction Proposal

> **Status: Proposed boundary, not an independent service.** The current `/api/estudios`
> routes and `Estudio` model are part of the shared backend.

## Current scope and evidence

The routes list, create, and delete authenticated medical-study records. The model uses
`estudiosConnection`.

- [`estudios.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/routes/estudios.js)
- [`Estudio.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/models/Estudio.js)

## Candidate owner

Own medical-study metadata, access rules, and the lifecycle of any associated document files.

## Open requirements before extraction

- The inspected source does not establish a dedicated object-storage service or file lifecycle.
- Specify secure upload/download, file validation, encryption, retention, deletion, audit, and
  backup/restore before storing real medical documents.
- Define whether binary storage is part of this service or an external storage adapter.
- Preserve patient ownership checks and specify offline-client behavior.

No bucket, S3 configuration, service port, or production deployment is claimed here.
