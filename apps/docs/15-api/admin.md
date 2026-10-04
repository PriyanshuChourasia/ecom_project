# API — Admin Surface

Status: all `REQUIRED`. Every route below additionally requires `ADMIN` ([04-roles-permissions/access-control.md](../04-roles-permissions/access-control.md)). Detail lives in module pages; this is the consolidated `/admin` index.

| Area | Endpoints | Detail |
|---|---|---|
| Catalog | `GET/POST /admin/products`, `GET/PATCH /admin/products/{id}`, `POST/DELETE /admin/products/{id}/images(/…)`, `PATCH /admin/products/{id}/stock`, `POST /admin/products/{id}/stock/adjust` | [products.md](products.md), [06-products/api.md](../06-products/api.md) |
| Categories | `GET/POST /admin/categories`, `PATCH/DELETE /admin/categories/{id}` | [categories.md](categories.md) |
| Inventory | `GET /admin/inventory` (+filters) | [07-inventory/api.md](../07-inventory/api.md) |
| Orders | `GET /admin/orders(/…)`, `PATCH /admin/orders/{id}/status`, `POST /admin/orders/{id}/cancel` | [orders.md](orders.md) |
| Customers | `GET /admin/users(/… )`, `PATCH /admin/users/{id}/status` | [03-users/api.md](../03-users/api.md) |
| Dashboard metrics | TBD shape (counts by status) — [14-admin/dashboard.md](../14-admin/dashboard.md) | `REQUIRED` |

Notes:
- Admin identity uses the same auth endpoints as consumers (login returns roles) — [15-api/authentication.md](authentication.md).
- No role-assignment endpoint exists by design (MVP) — [14-admin/access-control.md](../14-admin/access-control.md).
- `POST /admin/products/{id}/stock/adjust` is indexed here for completeness; its canonical owner is the inventory module page.
