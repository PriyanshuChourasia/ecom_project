# API — Orders

Status: all `REQUIRED`. Canonical detail: [11-orders/api.md](../11-orders/api.md).

| Method | URL | Auth | Authz | Purpose |
|---|---|---|---|---|
| GET | `/api/v1/orders` | Bearer | own | order history |
| GET | `/api/v1/orders/{id}` | Bearer | own | detail (items, payment, address snapshot) |
| POST | `/api/v1/orders/{id}/cancel` | Bearer | own | cancel while `PENDING_PAYMENT` |
| GET | `/api/v1/admin/orders` | Bearer | ADMIN | all orders + filters/search |
| GET | `/api/v1/admin/orders/{id}` | Bearer | ADMIN | full detail incl. buyer + payment |
| PATCH | `/api/v1/admin/orders/{id}/status` | Bearer | ADMIN | legal lifecycle transition |
| POST | `/api/v1/admin/orders/{id}/cancel` | Bearer | ADMIN | cancel pre-shipment (reason required) |

Rules: state machine enforced server-side (illegal → 422); own-scope 404; no hard delete ([11-orders/business-rules.md](../11-orders/business-rules.md)).
