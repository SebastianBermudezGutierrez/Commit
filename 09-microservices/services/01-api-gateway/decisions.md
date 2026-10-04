# Gateway Decisions — Proposed

No gateway-specific implementation decision is active.

## Open decision

Before adding a gateway, compare:

- retaining Express as the single application entry point;
- a managed or self-hosted edge proxy for transport/routing needs;
- a gateway introduced only when multiple independently deployed services exist.

The decision must be based on operational requirements and include authentication versus
authorization responsibilities, rate-limit storage, TLS/network boundaries, observability,
availability, cost, and rollback. Do not assume that a gateway should verify tokens and
forward trusted identity headers; services remain responsible for protecting their own
resources.
