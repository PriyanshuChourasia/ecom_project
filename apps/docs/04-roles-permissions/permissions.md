# Permissions

Status legend: see [root README](../README.md). Entire matrix is `REQUIRED` for MVP (no gates/policies exist).

Legend: ✅ allowed, ❌ denied, 👁 own-only (scoped to own records).

## Capability matrix

| Capability (named ability) | `CUSTOMER` | `ADMIN` |
|---|:---:|:---:|
| `catalog.view-active` (browse/search products & categories) | ✅ | ✅ |
| `products.manage` (CRUD, images, activate/deactivate) | ❌ | ✅ |
| `inventory.update` (stock changes) | ❌ | ✅ |
| `cart.manage` (own cart CRUD) | 👁 own | ✅ (for support tooling) |
| `addresses.manage` (own addresses) | 👁 own | ❌ (except support views `PLANNED`) |
| `checkout.create` (quote/order prep) | 👁 own | ✅ |
| `orders.view` | 👁 own | ✅ all |
| `orders.create` | ✅ | ✅ |
| `orders.update-status` | ❌ | ✅ |
| `orders.cancel-own` (pre-payment / policy window) | 👁 own | ✅ any |
| `payments.verify` | ❌ (server-side only) | ✅ (via webhook/verify paths) |
| `payments.refund` | ❌ | ❌ `PLANNED` post-MVP |
| `users.view`/`users.manage` (customers) | ❌ | ✅ |
| `roles.assign` | ❌ | ❌ (seeder/CLI only in MVP) |

## Module ownership

Each module doc re-states only its slice: catalog ops → [06-products/business-rules.md](../06-products/business-rules.md); stock ops → [07-inventory/api.md](../07-inventory/api.md); order ops → [11-orders/business-rules.md](../11-orders/business-rules.md); customer ops → [03-users/api.md](../03-users/api.md).

## Rules

1. Deny by default; every admin route carries an explicit `ADMIN` requirement.
2. Own-scope (👁) is enforced in queries (`user_id` match), not just controller checks.
3. Future staff roles get subsets of this table without structural change ([roles.md](roles.md)).
