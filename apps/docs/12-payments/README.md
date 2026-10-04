# 12 — Payments

Status legend: see [root README](../README.md). Integration is `REQUIRED` for MVP; nothing exists (no Razorpay package in `composer.json`).

| Document | Contents |
|---|---|
| [payment-model.md](payment-model.md) | `Payment` entity + application-side rules |
| [payment-flow.md](payment-flow.md) | Create → initiate → verify → outcome |
| [razorpay.md](razorpay.md) | **Razorpay provider logic only** |
| [api.md](api.md) | Payment endpoints |
| [webhooks.md](webhooks.md) | Webhook contract + trust model |

## Architecture rule: two clean layers

1. **Application payment logic** — owns `Order` ↔ `Payment` linkage, statuses, verification decisions, restock outcomes. Gateway-agnostic interfaces. (This repo's domain.)
2. **Razorpay provider logic** — an adapter: API calls (order creation), signature verification, webhook parsing. Nothing Razorpay-specific leaks into domain logic beyond the adapter ([razorpay.md](razorpay.md)).

```
Checkout ──► Order(PENDING_PAYMENT) ──► PaymentService ──► PaymentGateway (interface)
                                                              │
                                                       RazorpayGateway (adapter)
```

Related: order lifecycle ([11-orders/order-lifecycle.md](../11-orders/order-lifecycle.md)), security ([17-security/payment-security.md](../17-security/payment-security.md)).
