# Roles

Status legend: see [root README](../README.md). All `REQUIRED`.

## MVP roles

| Role | Purpose | Assigned by | Modules accessible | Status |
|---|---|---|---|---|
| `CUSTOMER` | Shop: browse, cart, checkout, pay, view own orders/address | Self-registration (hardcoded) | Consumer app surfaces: catalog, cart, checkout, orders, profile, addresses | `REQUIRED` |
| `ADMIN` | Operate the store: products, inventory, orders, customers | Seeder only (never public register) | Admin console: dashboard, products, inventory, orders, customers + everything consumer (for support/testing) | `REQUIRED` |

- `CUSTOMER` and `ADMIN` are not mutually exclusive data-wise (a user can hold `CUSTOMER` implicitly), but `ADMIN` is the operating role for the seller persona.
- Single-seller MVP: all products/orders belong to the one store; there is **no** per-seller ownership column. Multi-vendor (`FUTURE`) would add `SELLER` ownership semantics — see [21-future-scope/advanced-features.md](../21-future-scope/advanced-features.md).

## Extension slots (designed in, not built)

- Role table is many-to-many (`roles`, `role_user`) so new roles (SUPPORT, FULFILLMENT, EDITOR) require no user-schema change.
- Permission checks go through named abilities (e.g. `products.manage`, `orders.update-status`) so staff roles can be granted subsets later.
- None of this exists yet — all `REQUIRED`; future roles themselves are `PLANNED`/`FUTURE`.

Cross-refs: enforcement [access-control.md](access-control.md); admin usage [14-admin/access-control.md](../14-admin/access-control.md).
