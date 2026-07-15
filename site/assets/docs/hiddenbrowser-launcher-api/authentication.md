# Authentication

Every endpoint except `GET /health` requires a **Bearer JWT** (HS256, signed with the launcher's stored secret):

```
Authorization: Bearer <token>
```

The permanent token is shown in *Settings → Automation API* (copy it from there). Pressing **Regenerate token** rotates the signing secret live — the new token works immediately and every previously issued token is invalidated at once. There is no separate API key and no token-minting endpoint: the token itself is the credential.

Missing/invalid token → `401` with an empty body.
