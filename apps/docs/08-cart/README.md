# 08 — Cart

Status legend: see [root README](../README.md). Everything `REQUIRED`.

| Document | Contents |
|---|---|
| [cart-model.md](cart-model.md) | Cart + CartItem entities |
| [cart-flow.md](cart-flow.md) | Item lifecycle within a cart |
| [api.md](api.md) | Cart endpoints |
| [business-rules.md](business-rules.md) | Pricing, stock, persistence rules |

## Prime directive

**The backend is the single source of truth for price and stock.** The client sends only `product_id` and `quantity`; it never sends prices, and it never computes totals it trusts. See [business-rules.md](business-rules.md) and [10-checkout/pricing.md](../10-checkout/pricing.md).

Related: products ([06](../06-products/README.md)), stock flow ([07](../07-inventory/stock-flow.md)), checkout handoff ([10](../10-checkout/README.md)).
