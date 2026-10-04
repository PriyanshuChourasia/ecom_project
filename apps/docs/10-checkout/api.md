# Checkout API

Status legend: see [root README](../README.md). All `REQUIRED`. Bearer auth.

| # | Method | URL | Purpose |
|---|---|---|---|
| 1 | POST | `/api/v1/checkout/quote` | Pre-purchase price + validation (read-only) |
| 2 | POST | `/api/v1/checkout/order` | Create order (TX) + reserve stock + mint Razorpay order |

## 1) POST /checkout/quote

Request:
```json
{ "address_id": 3 }
```
Response `200`:
```json
{
  "data": {
    "address": { "id": 3, "label": "Home", "receiver_name": "Asha Kumar", "line1": "12 MG Road", "city": "Bengaluru", "state": "Karnataka", "postal_code": "560001" },
    "items": [
      { "product_id": 42, "sku": "CCTV-DOME-2MP", "name": "Indoor Dome Camera 2MP", "quantity": 2, "unit_price": 224910, "line_total": 449820 }
    ],
    "totals": { "subtotal": 449820, "tax_total": 0, "shipping": 0, "grand_total": 449820, "currency": "INR" },
    "issues": []
  }
}
```
Errors: `422` (`cart_empty`, missing `address_id`), `404` foreign address, `409` `insufficient_stock` with `issues[]` ([validation.md](validation.md)).

## 2) POST /checkout/order

Request:
```json
{ "address_id": 3, "idempotency_key": "c4f9…(TBD header-based)" }
```
Success `201`:
```json
{
  "data": {
    "order": { "id": 5001, "number": "ORD-2026-5001", "status": "PENDING_PAYMENT", "grand_total": 449820, "currency": "INR" },
    "payment": {
      "provider": "razorpay",
      "razorpay_order_id": "order_Nxxxxxxxxx",
      "amount": 449820,
      "currency": "INR",
      "key_id": "rzp_test_xxxxxxxx"
    }
  }
}
```
The client passes these into the Razorpay SDK ([12-payments/payment-flow.md](../12-payments/payment-flow.md), [13-consumer-app/api-integration.md](../13-consumer-app/api-integration.md)).

Errors: same matrix as quote plus `409` on lost stock race; `500` if Razorpay is unreachable (order stays `PENDING_PAYMENT`, retriable via verify/retry path TBD).

## Explicit non-endpoints

- No client endpoint sets order totals, prices, or amounts — structurally impossible by design ([pricing.md](pricing.md) rule 5).
- Cart mutation during checkout uses cart endpoints ([08-cart/api.md](../08-cart/api.md)) then re-quotes.
