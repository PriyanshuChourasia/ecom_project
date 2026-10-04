# Cart Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Entity: `Cart`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `user_id` | bigint FK → users | ✅ | ✅ (one active cart per user) | |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |

One active cart per user, created lazily on first add. Guest carts are **not** in MVP — the cart requires authentication ([business-rules.md](business-rules.md)).

## Entity: `CartItem`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `cart_id` | bigint FK | ✅ | — | |
| `product_id` | bigint FK | ✅ | ✅ (with cart_id) | one row per product per cart |
| `quantity` | integer ≥ 1 | ✅ | — | |
| `price_at_add` | integer (paise) | ✅ | — | **informational only** — display hint for "price changed" UX; never authoritative ([business-rules.md](business-rules.md)) |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |

## Derived totals (computed per request, never stored)

```
item_total   = effective_price(product) × quantity        # server-computed, live
cart_total   = Σ item_total
```

`effective_price` from [06-products/product-model.md](../06-products/product-model.md). If `product` is no longer `ACTIVE` or is deleted → item is pruned or flagged at read/quote time ([cart-flow.md](cart-flow.md)).

## Relationships

- User 1—1 Cart 1—N CartItem N—1 Product.
- Checkout converts the cart into an `Order` with snapshots — the cart itself is **not** snapshotted; see [10-checkout/README.md](../10-checkout/README.md).
