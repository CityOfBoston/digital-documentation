---
description: >-
  This app runs a node server to provide several services that include logging
  into IdentityIQ/Ping, etc. The main GraphQL endpoint is connected to the
  identityIq API Url, however, we run an additional
---

# Node Server

#### GraphQL Endpoint/Middleware

The Server is extended to support GraphQL using middleware and add it to the server as a plugin `.register(...)`  format. The endpoint is connected to the `identityIq` endpoint and uses PingID properties; both of these do not live in the repo, they are located in an AWS s3 bucket that gets mounted onto the container when service starts. `identityIq` is an API URL that lives in the environment file in s3; how `pingId` is a set of properties (token, adp\_url, base64 key, etc) that get loaded from the `PINGID_PROPERTIES_FILE` file in s3.

**Relevant Files**

* services-js/access-boston/src/pages/\_app.tsx
* services-js/access-boston/src/server/access-boston.ts
* services-js/access-boston/src/server/services/PingId.ts
* modules-js/next-client-common/src/next-client-common.ts
* services-js/access-boston/src/client/graphql/change-password.ts
* services-js/access-boston/src/pages/change-password.tsx

