# API — Authentication

Status: all `REQUIRED`. Canonical detail (payloads, validation, examples): [02-authentication/api.md](../02-authentication/api.md).

| Method | URL | Auth | Purpose |
|---|---|---|---|
| POST | `/api/v1/auth/register` | — | create customer, returns token |
| POST | `/api/v1/auth/login` | — | exchange credentials for token |
| POST | `/api/v1/auth/logout` | Bearer | revoke current token |
| GET | `/api/v1/auth/me` | Bearer | current user + roles |
| POST | `/api/v1/auth/password/forgot` | — | issue reset link |
| POST | `/api/v1/auth/password/reset` | — | reset via token |

Notes:
- Register hardcodes role `CUSTOMER` (no privilege escalation — [02-authentication/security.md](../02-authentication/security.md)).
- Admin accounts are seeded; they use the same login endpoint.
- Error conventions (incl. the anti-enumeration stance): [02-authentication/authentication-flow.md](../02-authentication/authentication-flow.md).
