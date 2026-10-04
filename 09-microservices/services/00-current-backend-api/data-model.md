# Current Backend Data Map

> All models are accessed by the same backend application today. The configured
> Mongoose connections are useful domain separation, but are not separate services.

| Model | Current Mongoose connection | Fields / responsibility at a glance |
|-------|-----------------------------|--------------------------------------|
| `User` | `authConnection` | Account credentials, contact and health profile fields, verification state, FCM token |
| `Turno` | `turnosConnection` | User identifier, patient name, date/time, reason, specialty, doctor, state |
| `Medicamento` | `medicamentosConnection` | Patient-owned medication records |
| `Estudio` | `estudiosConnection` | Patient-owned medical-study records |
| `Message` | `chatConnection` | User identifier, sender, message text, date |

The model schemas and connection setup in the source repository are authoritative.
This summary intentionally does not invent field types or indexes not established by
those schemas. `backend/server.js` additionally opens a base `MONGODB_URI` connection.

## Known cross-module access

- Turnos and Chat routes import `User` to read or clear the FCM token.
- The admin reset route imports several models and can delete data across domains.
- These are current same-process dependencies and need explicit redesign before the
  modules become isolated services.

## Sources

- [`backend/models/`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/tree/main/backend/models)
- [`backend/config/dbConnections.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/config/dbConnections.js)
- [`backend/server.js`](https://github.com/BrayanYara1/ProyectoDistribuidos2026/blob/main/backend/server.js)
