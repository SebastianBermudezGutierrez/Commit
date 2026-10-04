# Gateway Events

**Current status:** no independent gateway and no gateway-produced or gateway-consumed
domain events are evidenced.

An edge gateway should not become the publisher of business events such as appointment
creation. Those events, if introduced, belong to the domain service that commits the
business change. See the [system event catalog](../../event-catalog.md).
