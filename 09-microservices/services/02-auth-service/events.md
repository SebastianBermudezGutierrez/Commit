# Identity Events

**Current status:** no identity-domain event publisher or consumer is evidenced in the
current authentication route/model. Registration and login are HTTP operations; they do
not currently publish through a broker.

## Possible future event

`UserRegistered` may be considered if another independently deployed capability needs to
react to a successful account registration. Before adoption, define a privacy-minimized,
versioned schema, delivery guarantees, idempotency, retry/dead-letter behavior, and a
transactional outbox or equivalent.

Do not treat the sample `user.registered` JSON, login-failure event, or password-change
events from the generic template as project contracts.

See the [system event catalog](../../event-catalog.md).
