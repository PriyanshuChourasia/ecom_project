# Razorpay Integration

Status legend: see [root README](../README.md). Entire integration `REQUIRED` (no razorpay SDK in `composer.json` yet; `OFFLINE/MODE=rzp_test_*` placeholders in `.env` `IMPLEMENTED` skeleton).

# RAZORPAY PROVIDER LOGIC

Everything here is the **adapter layer** only — application decisions live in [payment-model.md](payment-model.md) / [payment-flow.md](payment-flow.md).

## Provider capabilities used

| Capability | API surface | Used in |
|---|---|---|
| Order creation | `POST /v1/orders` (amount, currency, receipt=order.number) | checkout-order TX |
| Signature verification | HMAC-SHA256(`order_id|payment_id`, key_secret) | verify + webhook |
| Webhook ingestion | `payment.captured`, `payment.failed` | [webhooks.md](webhooks.md) |
| (Optional) payment fetch | `GET /v1/payments/{id}` | verify cross-check |
| Refunds | `POST /v1/payments/{id}/refund` | `PLANNED` post-MVP |

## Environment contract (`REQUIRED`)

| Var | Meaning | Notes |
|---|---|---|
| `RAZORPAY_KEY_ID` | public key, sent to client | `rzp_test_*` for demo |
| `RAZORPAY_KEY_SECRET` | server-only, never shipped to client | leaking it = catastrophic ([17-security/payment-security.md](../17-security/payment-security.md)) |
| `RAZORPAY_WEBHOOK_SECRET` | webhook HMAC secret | distinct from key secret (TBD confirm) |

## Adapter rules

1. Signature check is constant-time compared and **must** precede any state change.
2. Currency pinned `INR`; amount integer paise (Razorpay-native).
3. Idempotency: webhook/event replays detected by `provider_payment_id` + status no-op ([webhooks.md](webhooks.md) rule 4).
4. Network failures during order creation → order stays `PENDING_PAYMENT`, retriable; no orphan client charge possible because the client can't initiate without server params.
5. Razorpay's checkout SDK (Flutter) handles card/UPI data — **no payment instrument data transits our backend** (PCI scope stays with provider).
6. Test/demo mode: `rzp_test_*` keys + Razorpay's test instrument matrix in the demo script ([20-mvp/demo-script.md](../20-mvp/demo-script.md)); live keys only in prod env ([19-deployment/production.md](../19-deployment/production.md)).

Client SDK notes (Flutter): package selection TBD (`razorpay_flutter` suggested, name not pinned anywhere) — integration points listed in [13-consumer-app/api-integration.md](../13-consumer-app/api-integration.md).
