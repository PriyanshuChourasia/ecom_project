# MVP Scope

Status legend: see [root README](../README.md).

## Primary Demo Flow (must remain the demo)

```
Seller creates product
  ↓
Consumer discovers product
  ↓
Consumer views product
  ↓
Consumer adds product to cart
  ↓
Consumer checks out
  ↓
Consumer pays
  ↓
Consumer places order
  ↓
Seller receives order
  ↓
Seller manages order
```

Every step above is `REQUIRED` — none is implemented yet (see [feature-status.md](feature-status.md)).

## In Scope (MVP)

| Capability | Status |
|---|---|
| Registration / login / logout (session or token based) | `REQUIRED` |
| Roles: `CUSTOMER`, `ADMIN` (single seller) | `REQUIRED` |
| Category management (flat or one-level hierarchy) | `REQUIRED` |
| Product management with images, price, stock | `REQUIRED` |
| Inventory = stock quantity on product | `REQUIRED` |
| Cart (persisted server-side per user) | `REQUIRED` |
| Addresses with default + order snapshot | `REQUIRED` |
| Checkout with server-side price/stock validation | `REQUIRED` |
| Orders with price snapshots and status lifecycle | `REQUIRED` |
| Razorpay payment + server-side verification | `REQUIRED` |
| Flutter consumer app: browse → order flow screens | `REQUIRED` |
| Admin web console: products/inventory/orders | `REQUIRED` |

## Out of Scope (MVP)

| Capability | Status |
|---|---|
| CCTV installation/service booking, technicians | `FUTURE` — [21-future-scope/cctv-services.md](../21-future-scope/cctv-services.md) |
| SMS / WhatsApp notifications | `FUTURE` — [21-future-scope/notifications.md](../21-future-scope/notifications.md) |
| Shipping carrier integrations | `FUTURE` — [21-future-scope/shipping.md](../21-future-scope/shipping.md) |
| Reviews, coupons, loyalty | `FUTURE` — [21-future-scope/advanced-features.md](../21-future-scope/advanced-features.md) |
| Multi-vendor marketplace | `FUTURE` |
| Advanced SEO / marketing automation | `FUTURE` |
| Multi-warehouse, multi-currency | `FUTURE` |

## MVP Acceptance (summary)

The demo proves: **SELLER → creates product; CONSUMER → sees product, adds to cart, checks out, pays, places order; SELLER → sees order, manages order.** Full criteria: [20-mvp/acceptance-criteria.md](../20-mvp/acceptance-criteria.md) and [18-testing/acceptance-criteria.md](../18-testing/acceptance-criteria.md).
