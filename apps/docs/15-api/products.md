# API — Products

Status: all `REQUIRED`. Canonical detail: [06-products/api.md](../06-products/api.md).

| Method | URL | Auth | Authz | Purpose |
|---|---|---|---|---|
| GET | `/api/v1/products` | — | public | list/search ACTIVE products |
| GET | `/api/v1/products/{slug}` | — | public | detail (404 unless ACTIVE) |
| GET | `/api/v1/products/{slug}/related` | — | public | same-category suggestions |
| GET | `/api/v1/admin/products` | Bearer | ADMIN | all statuses |
| POST | `/api/v1/admin/products` | Bearer | ADMIN | create |
| GET | `/api/v1/admin/products/{id}` | Bearer | ADMIN | any status detail |
| PATCH | `/api/v1/admin/products/{id}` | Bearer | ADMIN | update fields/status |
| PATCH | `/api/v1/admin/products/{id}/stock` | Bearer | ADMIN | set stock |
| POST | `/api/v1/admin/products/{id}/images` | Bearer | ADMIN | upload images |
| DELETE | `/api/v1/admin/products/{id}/images/{imageId}` | Bearer | ADMIN | remove image |
| POST | `/api/v1/admin/products/{id}/stock/adjust` | Bearer | ADMIN | delta adjust (inventory module) |

Key rules: server-only `effective_price`; paise integers; SKU unique/immutable post-orders; lifecycle-gated visibility ([06-products/business-rules.md](../06-products/business-rules.md)).
