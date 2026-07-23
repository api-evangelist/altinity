---
name: Authenticate to the Altinity Cloud Manager API
description: Obtain and use an ACM API token (password, 2FA, or Auth0 SSO) and call the API with the X-Auth-Token header.
api: openapi/altinity-acm-openapi-original.json
operations: [Login, LoginVerify, GetSingleAuth, DoSingleAuth, SystemProbe]
---

# Authenticate to the Altinity Cloud Manager (ACM) API

Base URL: `https://acm.altinity.cloud/api/`
Auth: every request carries an `X-Auth-Token` header (ACM API key or a login token).

## Steps

1. **Get a token.** Either:
   - Generate a long-lived ACM API key in the UI (My Account > API Access; default 24h expiry, adjustable), or
   - Call `Login` (`POST /login`) with the user's login + password to receive a token.
2. **Handle 2FA if enabled.** If login requires a second factor, call `LoginVerify` (`POST /login/verify`) with the verification code to finalize the token.
3. **(Optional) Auth0 SSO.** For single sign-on, call `GetSingleAuth` (`GET /singleauth`) to obtain the Auth0 target URL, complete the Auth0 flow, then exchange the Auth0 token via `DoSingleAuth` (`POST /singleauth`).
4. **Verify connectivity.** Call `SystemProbe` (`GET /probe`) with the `X-Auth-Token` header to confirm the token is valid and the system is reachable.
5. **Use the token.** Send `X-Auth-Token: <token>` on all subsequent ACM API calls.

## Rules

- Tokens/API keys expire (default 24h); re-authenticate on a `401 Unauthorized`.
- Errors come back as `{"error": "<message>", "code": <int>}` — inspect `code` (401 = auth, 404 = not found). See `errors/altinity-problem-types.yml`.
- No idempotency-key header is supported; do not assume safe automatic retries on writes. See `conventions/altinity-conventions.yml`.
