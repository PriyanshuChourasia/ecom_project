# Payments API

Status legend: see [root README](../README.md). All `REQUIRED`. Bearer auth (consumer).

| # | Method | URL | Purpose |
|---|---|---|---|
| 1 | POST | `/api/v1/payments/verify` | Client-relayed Razorpay verification |
| 2 | GET | `/api/v1/orders/{id}/payment` | Payment summary of own order |
| 3 | POST | `/api/v1/payments/webhook/razorpay` | Provider webhook — **no auth, signature-verified** ([webhooks.md](webhooks.md)) |

## 1) POST /payments/verify

Request:
```json
{
  "razorpay_order_id": "order_Nxxxxxxxxx",
  "razorpay_payment_id": "pay_Nyyyyyyyyy",
  "razorpay_signature": "9ef4…"
}
```
Process: locate Payment by `provider_order_id` (must belong to caller's order) → HMAC verify → cross-check amount → set `CAPTURED` + order `PAID`.

Responses:
- `200` `{ "data": { "order_id": 5001, "status": "PAID" } }`
- `400` invalid signature (`FAILED` recorded)
- `404` unknown/foreign `provider_order_id`
- `409` already-verified (idempotent success echo, not an error state — returns existing result)

## 2) GET /orders/{id}/payment

```json
{ "data": { "status": "captured", "provider": "razorpay", "provider_payment_id": "pay_Nyyy…", "amount": 449820, "currency": "INR", "method": "upi", "verified_at": "2026-10-04T12:06:01Z" } }
```
Own-order scope only (`404` otherwise).

## 3) Webhook endpoint

Documented fully in [webhooks.md](webhooks.md); listed here for API completeness. Unauthenticated by design; verified by HMAC; never exposes state to the caller (`200` always accepted-and-processed).

## Anti-patterns (blocked by design)

- No endpoint accepts "payment succeeded" booleans from clients.
- No endpoint accepts amounts.
- Refund endpoints: none in MVP (`PLANNED`).
