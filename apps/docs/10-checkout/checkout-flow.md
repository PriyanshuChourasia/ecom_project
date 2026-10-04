# Checkout Flow

Status legend: see [root README](../README.md). All `REQUIRED`.

## The complete process

```
Cart (server-priced) → Address selection → Quote (price + validate) → Order preparation → Payment → Order creation confirmed
```

```mermaid
sequenceDiagram
    autonumber
    participant C as Consumer (Flutter)
    participant API as Laravel API
    participant DB as Database
    participant R as Razorpay

    C->>API: GET /cart
    API-->>C: live-priced cart
    C->>API: GET /addresses (pick or create)
    C->>API: POST /checkout/quote {address_id}
    API->>DB: reload cart items + products (price, stock, status)
    API->>DB: load address (verify ownership)
    API-->>C: quote {items, totals, address, issues[]?}
    alt issues present
        C->>API: fix cart (remove/adjust) → re-quote
    end
    C->>API: POST /checkout/order {address_id}
    API->>DB: TX: re-validate → create Order (PENDING_PAYMENT, snapshots) → reserve stock
    API->>R: POST create razorpay order (server-side)
    API-->>C: order + razorpay {order_id, key, amount, currency}
    C->>R: pay via Razorpay checkout SDK
    R-->>C: payment_id + signature
    C->>API: POST /payments/verify
    API->>R: verify signature server-side
    API->>DB: order PAID; stock sale confirmed
    API-->>C: order confirmed (status PAID)
```

## Stage detail

| Stage | Input | Server actions | Output | Failure mode |
|---|---|---|---|---|
| Cart review | — | live pricing | cart payload | stale items flagged ([08-cart/cart-flow.md](../08-cart/cart-flow.md)) |
| Address | `address_id` | ownership check | — | `404`/`422` ([validation.md](validation.md)) |
| Quote | `address_id` | re-price + re-validate + issues[] | quote object | `409` stock, `422` empty/invalid |
| Order prep | `address_id` | **TX:** re-validate → order + items + address snapshot + price snapshots → reserve stock | order `PENDING_PAYMENT` + rzp params | `409` race; rollback TX + stock |
| Payment | rzp params | (see [12-payments/payment-flow.md](../12-payments/payment-flow.md)) | — | pay-fail path |
| Confirmation | verify payload | signature check → status `PAID` | confirmed order | `400` bad signature |

## Where calculations happen (summary)

**Everywhere on the server.** Client renders only server numbers. Full contract: [pricing.md](pricing.md).

## Handoff to payment

Order creation returns Razorpay checkout parameters; the client never creates orders with amounts of its own — the `amount` in the Razorpay params is server-minted. On verify success the order enters the lifecycle at `PAID` ([11-orders/order-lifecycle.md](../11-orders/order-lifecycle.md)).
