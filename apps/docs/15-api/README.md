# 15 — API Reference

Status legend: see [root README](../README.md). Base URL: `/api/v1` — **the entire API is `REQUIRED`** (no `routes/api.php` exists).

| Document | Module owner (canonical detail) |
|---|---|
| [authentication.md](authentication.md) | [02-authentication/api.md](../02-authentication/api.md) |
| [products.md](products.md) | [06-products/api.md](../06-products/api.md) |
| [categories.md](categories.md) | [05-categories/api.md](../05-categories/api.md) |
| [cart.md](cart.md) | [08-cart/api.md](../08-cart/api.md) |
| [addresses.md](addresses.md) | [09-addresses/api.md](../09-addresses/api.md) |
| [checkout.md](checkout.md) | [10-checkout/api.md](../10-checkout/api.md) |
| [orders.md](orders.md) | [11-orders/api.md](../11-orders/api.md) |
| [payments.md](payments.md) | [12-payments/api.md](../12-payments/api.md) |
| [admin.md](admin.md) | [14-admin](../14-admin/README.md) + module api pages |

Module pages hold full request/response examples; these pages give the endpoint index + anything API-global. No duplication beyond cross-links (rule: single source per fact).

## Global conventions

| Concern | Convention |
|---|---|
| Versioning | `/api/v1` prefix |
| Auth | `Authorization: Bearer <token>` (Sanctum `REQUIRED`) |
| Content type | `application/json` in/out |
| Money | integer paise + `currency: "INR"`; display formatting client-side |
| Pagination | `?page=&per_page=` → `meta {current_page, last_page, total}` |
| Errors | problem-style JSON: `{"error": "code", "message": "…", "errors"?: {field: [..]}, "issues"?: [..]}` |
| Status codes | 200/201/204 success · 400 bad request · 401 unauth · 403 forbidden · 404 not-found (incl. foreign resources) · 409 conflict/state · 422 validation · 500 server |
| ID refs | numeric ids + human `number`/`slug` where documented |

## Full endpoint inventory (all `REQUIRED`)

| Module | Endpoints |
|---|---|
| Auth | register, login, logout, me, password forgot/reset (6) |
| Users | me GET/PATCH, admin users list/detail/status (5) |
| Categories | public list/detail (2), admin CRUD (3) |
| Products | public list/detail/related (3), admin list/create/detail/update/stock/images (7) |
| Cart | get, add, update, remove, clear (5) |
| Addresses | list/create/update/delete/default (5) |
| Checkout | quote, order (2) |
| Orders | consumer list/detail/cancel (3), admin list/detail/status/cancel (4) |
| Payments | verify, order-payment, razorpay webhook (3) |
| System | `GET /up` health (`IMPLEMENTED`) |

Total: 45 planned endpoints (1 implemented).
