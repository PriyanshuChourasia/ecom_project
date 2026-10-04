# API — Checkout

Status: all `REQUIRED`. Canonical detail: [10-checkout/api.md](../10-checkout/api.md).

| Method | URL | Auth | Purpose |
|---|---|---|---|
| POST | `/api/v1/checkout/quote` | Bearer | re-price + validate cart (read-only) |
| POST | `/api/v1/checkout/order` | Bearer | TX: create order + snapshots + reserve stock + mint Razorpay order |

Key rules:
- Quote/order share the validation matrix ([10-checkout/validation.md](../10-checkout/validation.md)); order-creation pass is authoritative.
- Success of (2) returns Razorpay params consumed by the client SDK ([12-payments/razorpay.md](../12-payments/razorpay.md)).
- No client-supplied amounts anywhere ([10-checkout/pricing.md](../10-checkout/pricing.md)).
