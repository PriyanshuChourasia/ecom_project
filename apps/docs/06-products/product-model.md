# Product Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Entity: `Product`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `category_id` | bigint FK | ✅ | — | one category (null would break browse — disallowed) |
| `sku` | string | ✅ | ✅ | admin-assigned; unique globally (stock lives here) |
| `name` | string | ✅ | — | display name |
| `brand` | string, nullable | — | — | free text for MVP (brand table `PLANNED`) |
| `description` | text | ✅ | — | |
| `price` | integer (paise) | ✅ | — | MRP/current selling price |
| `discount_percent` | integer 0–100, default 0 | — | — | drives `effective_price` |
| `tax_percent` | integer 0–100, default 0 | — | — | inclusive; see rules |
| `stock_quantity` | integer ≥ 0 | ✅ | — | lives on this row — single-stock MVP ([07](../07-inventory/README.md)) |
| `status` | enum: `DRAFT`/`ACTIVE`/`INACTIVE` | ✅ | — | default `DRAFT`; see lifecycle |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |
| `deleted_at` | timestamp, nullable | — | — | soft-delete; decision TBD |

Derived (computed, never stored): `effective_price = price × (1 − discount_percent/100)`; `tax_amount`; `in_stock = stock_quantity > 0`.

## Relationships

| Related | Cardinality | Doc |
|---|---|---|
| Category | many-to-one | [05](../05-categories/README.md) |
| ProductImage | one-to-many | [product-images.md](product-images.md) |
| CartItem | one-to-many (validated at add) | [08-cart](../08-cart/README.md) |
| OrderItem | one-to-many (price snapshot copied) | [11-orders](../11-orders/README.md) |

## Field notes

- **Currency INR, integer paise** — never float. TBD confirm with payment docs ([12-payments/payment-model.md](../12-payments/payment-model.md)).
- **Discount & tax** are simple per-product fields for MVP (no price tables, no per-category tax).
- **Specifications**: structured per-vertical spec tables are `PLANNED`; MVP stores specs as readable text/JSON `specs` column — **TBD** exact shape before migration freeze.
- **Search** (`REQUIRED` basic): name/SKU/brand LIKE + category filter; relevance ranking `PLANNED`.
