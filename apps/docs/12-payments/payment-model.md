# Payment Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Entity: `Payment`

| Field | Type | Required | Unique | Notes |
|---|---|:---:|:---:|---|
| `id` | bigint PK | ✅ | ✅ | |
| `order_id` | bigint FK | ✅ | — (one active) | 1:1 in MVP ([retry rows `PLANNED`]) |
| `provider` | string | ✅ | — | "razorpay" (constant in MVP) |
| `provider_order_id` | string | ✅ | ✅ | Razorpay `order_*` id |
| `provider_payment_id` | string, nullable | — | ✅ | `pay_*` id after attempt |
| `amount` | integer | ✅ | — | paise; mirrors `order.grand_total` |
| `currency` | string | ✅ | — | "INR" |
| `status` | enum | ✅ | — | `CREATED` → `INITIATED` → `CAPTURED` / `FAILED`; `REFUNDED` `PLANNED` |
| `method` | string, nullable | — | — | upi/card/netbanking (informational) |
| `error_code`, `error_description` | string, nullable | — | — | from provider on failure |
| `verified_at` | timestamp, nullable | — | — | server verification time |
| `created_at`, `updated_at` | timestamps | ✅ | — | audit |

## APPLICATION PAYMENT LOGIC (gateway-agnostic)

1. `Payment` is created in the same transaction as the `Order` (`CREATED` + Razorpay order minted via gateway interface).
2. **Only the backend decides** `CAPTURED` — after server-side verification (signature check and/or webhook), never from client assertions ([payment-flow.md](payment-flow.md) rule 3).
3. On `CAPTURED`: order `PENDING_PAYMENT → PAID`; stock reservation finalized ([07-inventory/stock-flow.md](../07-inventory/stock-flow.md)).
4. On `FAILED`/expiry: order → `PAYMENT_FAILED`/`CANCELLED` + stock restored ([11-orders/business-rules.md](../11-orders/business-rules.md)).
5. Amount integrity: payment `amount` must equal `order.grand_total` — mismatch (corrupt state) blocks capture and alerts (log).
6. No card/bank data ever touches this app — tokenized checkout happens in Razorpay's surface ([razorpay.md](razorpay.md) rule 5, [17-security/payment-security.md](../17-security/payment-security.md)).

## RAZORPAY PROVIDER LOGIC (adapter)

- Creates provider orders; verifies signatures (HMAC); parses webhooks. Full detail: [razorpay.md](razorpay.md).

## Refunds (context only)

`status = REFUNDED` + `refunded_at` are **`PLANNED` post-MVP**; refund orchestration requires Razorpay refund APIs and order-state additions — out of MVP scope ([21-future-scope/advanced-features.md](../21-future-scope/advanced-features.md)).
