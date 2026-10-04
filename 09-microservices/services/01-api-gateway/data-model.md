# API Gateway Data

**Status:** Not applicable to the current system. A separate gateway is only a proposal,
and no gateway-owned database or cache is evidenced.

Do not introduce Redis or token/session persistence at the gateway without an approved
architecture decision. The current backend uses MongoDB/Mongoose; see
[`services/00-current-backend-api/data-model.md`](../00-current-backend-api/data-model.md).
