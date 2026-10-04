# API — Categories

Status: all `REQUIRED`. Canonical detail: [05-categories/api.md](../05-categories/api.md).

| Method | URL | Auth | Authz | Purpose |
|---|---|---|---|---|
| GET | `/api/v1/categories` | — | public | active categories (+products_count) |
| GET | `/api/v1/categories/{slug}` | — | public | detail |
| POST | `/api/v1/admin/categories` | Bearer | ADMIN | create |
| PATCH | `/api/v1/admin/categories/{id}` | Bearer | ADMIN | update/activate/deactivate |
| DELETE | `/api/v1/admin/categories/{id}` | Bearer | ADMIN | delete (only when empty → else 409) |

Rules: one-level hierarchy cap; slug immutable once products attach ([05-categories/business-rules.md](../05-categories/business-rules.md)).
