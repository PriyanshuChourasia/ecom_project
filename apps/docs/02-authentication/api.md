# Authentication API

Status legend: see [root README](../README.md). **All endpoints on this page are `REQUIRED`** — `routes/api.php` does not exist yet. This page is the owner; [15-api/authentication.md](../15-api/authentication.md) mirrors it without duplicating detail.

| # | Method | URL | Auth | Purpose |
|---|---|---|---|---|
| 1 | POST | `/api/v1/auth/register` | none | Create consumer account (role `CUSTOMER`) |
| 2 | POST | `/api/v1/auth/login` | none | Exchange credentials for token |
| 3 | POST | `/api/v1/auth/logout` | Bearer token | Revoke current token |
| 4 | GET | `/api/v1/auth/me` | Bearer token | Current user + roles |
| 5 | POST | `/api/v1/auth/password/forgot` | none | Send reset link (mail driver currently `log` — `IMPLEMENTED` limitation) |
| 6 | POST | `/api/v1/auth/password/reset` | none | Reset with token |

## 1) Register

Request:
```json
{
  "name": "Asha Kumar",
  "email": "asha@example.com",
  "password": "s3cret-plain",
  "device_name": "Pixel 8"
}
```

Response `201`:
```json
{
  "user": { "id": 1, "name": "Asha Kumar", "email": "asha@example.com", "roles": ["CUSTOMER"] },
  "token": "1|xxxxxxxx..."
}
```

Validation (`REQUIRED`): `name` required string; `email` required, unique `users.email`, email format; `password` required, confirmed, min length per [authentication-flow.md](authentication-flow.md); `device_name` optional string.

Errors: `422` validation; email taken → `422` on `email`.

## 2) Login

Request:
```json
{ "email": "asha@example.com", "password": "s3cret-plain", "device_name": "Pixel 8" }
```
Response `200`: same shape as register. Errors: invalid credentials → `422`/`401` (convention TBD, documented in [authentication-flow.md](authentication-flow.md)).

## 3) Logout

Auth required. Response `204`. Revokes only the presented token (per-device logout).

## 4) Me

Auth required. Response `200` with serialized user incl. roles; `password`/`remember_token` never serialized (`IMPLEMENTED` hidden attributes).

## 5–6) Password reset

`REQUIRED` for MVP usability (users will forget passwords), but the demo flow does not depend on it — lowest priority of the six. Delivery rides on the current `log` mail driver locally (`IMPLEMENTED`); production SMTP is [FUTURE/PLANNED — see 19-deployment/production.md](../19-deployment/production.md).
