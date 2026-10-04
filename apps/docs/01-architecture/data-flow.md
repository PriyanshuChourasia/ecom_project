# Data Flow

Status legend: see [root README](../README.md). All flows below are **TARGET MVP** (`REQUIRED`) — none exists in code yet; diagrams define the contract the implementations must satisfy.

## Purchase flow (the money path)

```mermaid
sequenceDiagram
    autonumber
    participant C as Consumer (Flutter)
    participant API as Laravel API (/api/v1)
    participant DB as Database
    participant R as Razorpay

    C->>API: GET /products (browse)
    API->>DB: read active products
    API-->>C: product list
    C->>API: GET /products/{id}
    API-->>C: product detail (images, price, stock)
    C->>API: POST /cart/items {product_id, qty}
    API->>DB: validate stock, read server price
    API-->>C: cart (server-computed totals)
    C->>API: POST /checkout/quote {address_id}
    API->>DB: re-validate stock + re-price from DB
    API-->>C: quote (items, totals, address)
    C->>API: POST /orders {address_id, payment method}
    API->>DB: create order PENDING_PAYMENT (price snapshots) + reserve stock
    API->>R: create payment order (server-side)
    API-->>C: order + razorpay order params
    C->>R: complete payment (Razorpay SDK)
    R-->>C: payment_id + signature
    C->>API: POST /payments/verify {order_id, payment_id, signature}
    API->>R: verify signature (server-side)
    API->>DB: mark PAID, confirm stock decrement
    API-->>C: order confirmed
```

## Seller order-management flow

```mermaid
sequenceDiagram
    autonumber
    participant A as Admin console
    participant API as Laravel API/Admin
    participant DB as Database
    participant C as Consumer (Flutter)

    A->>API: login (ADMIN role)
    A->>API: POST /admin/products (create product)
    API->>DB: persist product DRAFT/ACTIVE + images + stock
    A->>API: PATCH /admin/products/{id} (activate)
    API->>DB: product ACTIVE → visible to consumers
    C->>API: (consumer places order — see above)
    A->>API: GET /admin/orders
    API->>DB: list orders w/ items, payments, buyers
    A->>API: PATCH /admin/orders/{id}/status
    API->>DB: status transition CONFIRMED→PROCESSING→SHIPPED→DELIVERED
    API-->>C: order status visible in My Orders
```

## Key rules encoded in these flows

1. **Prices always come from the database** at quote/checkout time — never from client-sent amounts ([08-cart/business-rules.md](../08-cart/business-rules.md), [10-checkout/pricing.md](../10-checkout/pricing.md)).
2. **Stock is re-validated at checkout** even though the cart validated it earlier ([07-inventory/stock-flow.md](../07-inventory/stock-flow.md)).
3. **Order stores price snapshots** so later catalog changes don't alter historical orders ([11-orders/order-model.md](../11-orders/order-model.md)).
4. **Payment verification is server-side** — the client never authenticates a payment by asserting success ([17-security/payment-security.md](../17-security/payment-security.md)).
5. Address is **snapshotted onto the order** at creation ([09-addresses/business-rules.md](../09-addresses/business-rules.md)).
