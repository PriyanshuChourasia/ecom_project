# 02 — Authentication

Status legend: see [root README](../README.md).

| Document | Contents |
|---|---|
| [authentication-flow.md](authentication-flow.md) | Register/login/logout/session flows |
| [api.md](api.md) | Auth endpoints (all `REQUIRED`) |
| [security.md](security.md) | Password handling, token security, middleware |

## Status

| Capability | Status |
|---|---|
| `User` model with hashed password | `PARTIALLY IMPLEMENTED` (Laravel default) |
| Register / login / logout endpoints | `REQUIRED` |
| Token issuance (Sanctum) / session auth | `REQUIRED` (package decision TBD) |
| Auth middleware wiring | `REQUIRED` (`bootstrap/app.php` middleware block is empty) |
| Role middleware (ADMIN gate) | `REQUIRED` — see [04-roles-permissions](../04-roles-permissions/README.md) |

Makeup of the decision: **Sanctum token auth is `REQUIRED` for the mobile app**; the admin console may use session cookie auth. Single-stop reference: [15-api/authentication.md](../15-api/authentication.md).
