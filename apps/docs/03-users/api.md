# Users API

Status legend: see [root README](../README.md). All `REQUIRED` unless stated.

| # | Method | URL | Auth | Purpose |
|---|---|---|---|---|
| 1 | GET | `/api/v1/users/me` | Bearer | Own profile (alias parity with `GET /auth/me` — one implementation, two routes or one canonical; TBD resolve duplication) |
| 2 | PATCH | `/api/v1/users/me` | Bearer | Update own profile (`name`, password w/ confirmation; `phone` editability TBD) |
| 3 | GET | `/api/v1/admin/users` | Bearer + ADMIN | Customer list (paginated, filter by status) |
| 4 | GET | `/api/v1/admin/users/{id}` | Bearer + ADMIN | Customer detail incl. order summary |
| 5 | PATCH | `/api/v1/admin/users/{id}/status` | Bearer + ADMIN | Block/unblock customer |

Notes:

- Customer address endpoints live in [09-addresses/api.md](../09-addresses/api.md); orders in [11-orders/api.md](../11-orders/api.md).
- Admin user pages back [14-admin/customer-management.md](../14-admin/customer-management.md).
- Cross-reference: full request/response examples follow the conventions in [15-api/README.md](../15-api/README.md) once endpoints are implemented; this page holds the contract.
