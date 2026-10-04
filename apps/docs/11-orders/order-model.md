# Order Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Entity: `Order`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `number` | string | ✅ | ✅ | human ref `ORD-YYYY-<id>` |
| `user_id` | bigint FK | ✅ | — | buyer |
| `status` | enum (lifecycle) | ✅ | — | see [order-lifecycle.md](order-lifecycle.md) |
| **Address snapshot** | | | | |
| `ship_name` | string | ✅ | — | copied from Address at creation |
| `ship_phone` | string | ✅ | — | snapshot |
| `ship_line1` / `ship_line2` | string / nullable | ✅ | — | snapshot |
| `ship_city`, `ship_state`, `ship_postal_code`, `ship_country` | string | ✅ | — | snapshot |
| **Totals (paise, snapshot at creation)** | | | | |
| `subtotal` | integer | ✅ | — | |
| `tax_total` | integer | ✅ | — | informational (inclusive) |
| `shipping_total` | integer | ✅ | — | 0 in MVP |
| `grand_total` | integer | ✅ | — | charged amount |
| `currency` | string | ✅ | — | "INR" |
| `placed_at` | timestamp | ✅ | — | |
| `cancelled_at`, `cancellation_reason` | timestamp/string, nullable | — | — | |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |

## Entity: `OrderItem`

| Field | Type | Required | Notes |
|---|---|:---:|---|
| `id` | bigint PK | ✅ | |
| `order_id` | bigint FK | ✅ | |
| `product_id` | bigint FK | ✅ | live link (nullable `PLANNED` if products deletable — currently blocked by [06-products/business-rules.md](../06-products/business-rules.md) rule 10) |
| `sku` | string | ✅ | snapshot |
| `product_name` | string | ✅ | snapshot |
| `unit_price` | integer | ✅ | **price snapshot** (effective unit price at purchase) |
| `quantity` | integer | ✅ | |
| `line_total` | integer | ✅ | unit_price × quantity |
| `created_at`, `updated_at` | timestamps | ✅ | audit |

## Why snapshots (the integrity model)

- **Price snapshots** (`unit_price`, totals): catalog edits never rewrite history — a 50%-off sale today doesn't change yesterday's invoices ([06-products/business-rules.md](../06-products/business-rules.md) rule 3).
- **Address snapshot**: receiver edits/deletes don't touch past orders ([09-addresses/business-rules.md](../09-addresses/business-rules.md) rule 4).
- **Product snapshot** (name/SKU): renames don't corrupt order lines.

## Relationships

User 1—N Order 1—N OrderItem; Order 1—1 Payment ([12-payments/payment-model.md](../12-payments/payment-model.md)); Order N—1 Address (source only — data copied, not live).

## Cancellation model

Fields `cancelled_at` + `cancellation_reason`; status `CANCELLED` per lifecycle; restock rules in [business-rules.md](business-rules.md).
