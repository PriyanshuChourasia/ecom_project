# API — Payments

Status: all `REQUIRED`. Canonical detail: [12-payments/api.md](../12-payments/api.md).

| Method | URL | Auth | Purpose |
|---|---|---|---|
| POST | `/api/v1/payments/verify` | Bearer | verify Razorpay triple (order_id, payment_id, signature) |
| GET | `/api/v1/orders/{id}/payment` | Bearer (own) | payment summary |
| POST | `/api/v1/payments/webhook/razorpay` | none (HMAC-verified) | provider webhook ([12-payments/webhooks.md](../12-payments/webhooks.md)) |

Rules: server-side verification only — client assertions never flip state; idempotent verify/webhook; no refund endpoints in MVP (`PLANNED`) ([12-payments/README.md](../12-payments/README.md)).
