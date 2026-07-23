---
name: Create an Altinity.Cloud trial account
description: Start a free Altinity.Cloud trial by email and confirm the signup.
api: openapi/altinity-acm-openapi-original.json
operations: [SignupQuick, SignupCheck, SignupConfirm]
---

# Create an Altinity.Cloud trial account

Base URL: `https://acm.altinity.cloud/api/`

## Steps

1. **Start signup.** Call `SignupQuick` (`POST /signup-email`) with only the user's email to create a trial account. Altinity.Cloud emails a confirmation link with a signup token.
2. **Check the token.** Call `SignupCheck` (`GET /signup/confirm`) to verify the signup token is still active before finishing.
3. **Finish signup.** Call `SignupConfirm` (`POST /signup/confirm`) to complete account setup.
4. **Authenticate.** Once confirmed, get a token via the authenticate skill (`altinity-authenticate.md`) and call the API with `X-Auth-Token`.

## Rules

- Signup tokens expire; if `SignupCheck` reports the token inactive, restart with `SignupQuick`.
- Errors use the `{"error", "code"}` envelope (see `errors/altinity-problem-types.yml`).
