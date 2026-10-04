# Payment Flow

Status legend: see [root README](../README.md). All `REQUIRED`.

## Phases: creation → initiation → verification → success/failure

```mermaid
sequenceDiagram
    autonumber
    participant C as Consumer (Flutter)
    participant API as Laravel API
    participant DB as Database
    participant R as Razorpay

    Note over API,DB: CREATION (inside checkout-order TX)
    C->>API: POST /checkout/order
    API->>R: create razorpay order (amount = grand_total)
    API->>DB: Payment(CREATED, provider_order_id)
    API-->>C: order + rzp params

    Note over C,R: INITIATION (client-side SDK)
    C->>R: open Razorpay checkout (params from API)
    R-->>C: payment sheet (UPI/card/netbanking)
    alt user completes
        R-->>C: razorpay_payment_id + order_id + signature
    else failure/abandon
        R-->>C: error / dismiss
    end

    Note over C,API: VERIFICATION (server decides)
    C->>API: POST /payments/verify {razorpay_*}
    API->>API: HMAC verify signature
    API->>R: (optional) fetch payment for cross-check
    alt valid
        API->>DB: Payment CAPTURED; Order PAID; stock sale
        API-->>C: 200 order confirmed
    else invalid
        API->>DB: Payment FAILED
        API-->>C: 400
    end

    Note over R,API: BACKSTOP (async truth)
    R->>API: webhook payment.captured / payment.failed
    API->>DB: reconcile (idempotent)
```

## Phase rules

1. **Creation** — server-minted amount; client never supplies it ([10-checkout/pricing.md](../10-checkout/pricing.md) rule 5).
2. **Initiation** — client only relays provider params; UI waits on Razorpay's sheet ([13-consumer-app/screen-map.md](../13-consumer-app/screen-map.md) Payment screen).
3. **Verification is server-side and mandatory** — client "success" callbacks are untrusted input; the HMAC signature (and webhook) is the only truth.
4. **Success** = `Payment.CAPTURED` + `Order.PAID` (idempotent double-writes from client-verify + webhook are handled — first write wins, second is a no-op).
5. **Failure** = `FAILED` (+ error fields) → `PAYMENT_FAILED`; user may retry within window ([11-orders/order-lifecycle.md](../11-orders/order-lifecycle.md)).
6. **Webhook as backstop** — covers app-kill mid-flow; capture without client verify still completes the order ([webhooks.md](webhooks.md)).
