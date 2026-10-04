# Local Verification — Current Backend API

> This is a minimal local verification note for the existing backend. It is not a
> production on-call runbook; production URLs, dashboards, SLOs, and escalation contacts
> are not established by the inspected repository evidence.

## Start locally

1. Install the Node.js version specified by the project and run `npm ci` from `backend/`.
2. Configure required environment variables using the repository's example file; keep
   real secrets out of source control.
3. From `backend/`, run `npm start` (the script executes `node server.js`).
4. Check `GET http://localhost:<PORT>/api/status`, where `PORT` defaults to `10000`.

The status endpoint confirms the process responds. It must not be interpreted as a
readiness check for MongoDB, Firebase, or every mounted route.

## Safe-operation warnings

- Never invoke `/api/admin/reset-database` against production or shared data.
- Do not paste database credentials, JWT secrets, or Firebase service-account data into
  logs, tickets, or documentation.
- If MongoDB connection errors appear, inspect environment configuration and application
  logs; do not report the service healthy solely because `/api/status` responds.
- Push delivery may be unavailable when Firebase Admin is not configured; the notification
  helper logs simulation mode when no service account is available.

## Production procedures

No verifiable production dashboard, deployment URL, on-call contact, tested rollback
command, or SLO is recorded here. Add those only after confirming them in the deployment
configuration and with the responsible operator.
