# Product Business Rules

Status legend: see [root README](../README.md). All `REQUIRED`.

## Pricing & discount

1. `effective_price` is always server-computed from `price` and `discount_percent` — never client-supplied ([10-checkout/pricing.md](../10-checkout/pricing.md)).
2. Prices/discounts take effect immediately on save; no scheduled pricing in MVP.
3. Cart and order **never** store a live-derived price: carts re-quote at checkout; orders store snapshots ([11-orders/order-model.md](../11-orders/order-model.md)).
4. Discount display: show `price` (MRP) struck-through when `discount_percent > 0`, plus `effective_price`.
5. Zero `price` is allowed only in `DRAFT`; activation with `price = 0` rejected `422`.

## Tax

6. `tax_percent` defaults 0 for MVP; `inclusive` pricing: `effective_price` is the customer-paid total; tax amount is informational on order summaries only (no separate tax line added). GST-ready structure `PLANNED`.

## Stock coupling

7. See [07-inventory/stock-flow.md](../07-inventory/stock-flow.md) for the single source of stock rules; product surfaces `in_stock` only.

## Visibility & integrity

8. Customers see `ACTIVE` only — any other state yields `404` on public endpoints.
9. `sku` unique **globally** and immutable after first order referencing it.
10. Deleting products with order history is prohibited (soft-delete TBD in [product-model.md](product-model.md)); use `INACTIVE`.
11. Category changes allowed; category-notch display follows [05-categories/business-rules.md](../05-categories/business-rules.md).
12. Demo verticals (stationery, electronics, laptops, desktops, CCTV hardware) need **no** special workflow — they are plain products with these same rules; CCTV service workflows are FUTURE ([21-future-scope/cctv-services.md](../21-future-scope/cctv-services.md)).
