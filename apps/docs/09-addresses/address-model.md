# Address Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Entity: `Address`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `user_id` | bigint FK → users | ✅ | — | owner; all reads scoped to owner |
| `label` | string | ✅ | — | "Home", "Office" |
| `receiver_name` | string | ✅ | — | may differ from account name |
| `phone` | string | ✅ | — | format validation TBD (10-digit IN) |
| `line1` | string | ✅ | — | house/flat/street |
| `line2` | string, nullable | — | — | landmark/area |
| `city` | string | ✅ | — | |
| `state` | string | ✅ | — | |
| `postal_code` | string | ✅ | — | pattern TBD (6-digit IN) |
| `country` | string | ✅ | — | default "India"; other countries out of MVP |
| `is_default` | boolean | ✅ | — | see default rules |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |

## Relationships

- User 1—N Address (own-scope enforced per [04-roles-permissions/access-control.md](../04-roles-permissions/access-control.md)).
- Order 1—1 **Address snapshot** — embedded copy on the order ([order-model.md](../11-orders/order-model.md)); never a live FK.

## Default address rules

1. Exactly one default per user (first address auto-defaults).
2. Setting a new default clears the old one (single transaction).
3. Deleting the default promotes the oldest remaining (policy TBD — confirm).
4. `null` default is allowed only when the user has zero addresses; checkout then requires explicit selection/creation ([10-checkout/checkout-flow.md](../10-checkout/checkout-flow.md)).

## Checkout address selection

The client sends `address_id` at checkout; the server verifies ownership and **copies** the fields onto the order (snapshot). Address edits after checkout never mutate existing orders ([business-rules.md](business-rules.md) rule 4).
