# Checkout Pricing

Status legend: see [root README](../README.md). All `REQUIRED`.

## Where price calculations happen

| Location | Computes money? | Trustworthy? |
|---|---|---|
| Flutter client | rendering only | ❌ never authoritative |
| Cart endpoints | live quote for display | ✅ (but re-priced at checkout) |
| **Checkout quote** | **canonical pre-purchase quote** | ✅ |
| **Order creation** | **final charge basis** (snapshotted) | ✅ final |
| Razorpay | charges the server-minted amount | ✅ external truth for capture |

## The pricing pipeline (quote and order-creation share it)

```
for each cart item (active products only):
    unit      = product.price × (1 − discount_percent/100)   # integer paise, floor rounding
    line      = unit × quantity
subtotal     = Σ line
tax_total    = Σ round(line × tax_percent/100)               # informational, inclusive pricing
grand_total  = subtotal                                       # shipping = 0 in MVP
```

Rules:

1. **Integer paise throughout**; floor at line level; no floats anywhere ([06-products/product-model.md](../06-products/product-model.md)).
2. Prices/discounts/taxes read from `products` **at pipeline time** — changes between quote and order-creation are picked up by the second pass (that's why order creation re-prices; see [checkout-flow.md](checkout-flow.md) stage "Order prep").
3. **Shipping is ₹0 / free in MVP** (carrier integrations FUTURE — [21-future-scope/shipping.md](../21-future-scope/shipping.md)); field exists in totals for forward-compat.
4. **No coupons/loyalty/credits in MVP** (FUTURE) — pipeline has no extra-deduction slots by design.
5. The Razorpay order `amount` = `grand_total` minted server-side at order creation ([12-payments/payment-model.md](../12-payments/payment-model.md)); client-supplied amounts are rejected structurally (never read).
6. Rounding is deterministic (floor); any future rounding policy change must re-verify refund math ([12-payments/README.md](../12-payments/README.md)).
