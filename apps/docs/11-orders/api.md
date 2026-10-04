# Orders API

Status legend: see [root README](../README.md). All `REQUIRED`. Bearer auth.

## Consumer

| # | Method | URL | Purpose |
|---|---|---|---|
| 1 | GET | `/api/v1/orders` | Own orders (newest first, paginated) |
| 2 | GET | `/api/v1/orders/{id}` | Own order detail (items, payment, address snapshot) |
| 3 | POST | `/api/v1/orders/{id}/cancel` | Cancel own order — only while `PENDING_PAYMENT` |

## Admin (`/api/v1/admin/orders`, `ADMIN`)

| # | Method | URL | Purpose |
|---|---|---|---|
| 4 | GET | `/api/v1/admin/orders` | All orders; filters: `status`, `search` (number/phone/SKU), date range |
| 5 | GET | `/api/v1/admin/orders/{id}` | Full detail incl. buyer contact + payment record |
| 6 | PATCH | `/api/v1/admin/orders/{id}/status` | Lifecycle transition (`{status: "CONFIRMED"}`) |
| 7 | POST | `/api/v1/admin/orders/{id}/cancel` | Admin cancel (pre-shipment; reason required) |

## 1) List — response

```json
{
  "data": [
    {
      "id": 5001, "number": "ORD-2026-5001",
      "status": "PAID",
      "grand_total": 449820, "currency": "INR",
      "items_count": 2,
      "placed_at": "2026-10-04T12:05:33Z",
      "payment": { "status": "captured", "method": "razorpay" }
    }
  ],
  "meta": { "current_page": 1, "last_page": 1, "total": 1 }
}
```

## 2) Detail — adds

- `items[]`: `sku`, `product_name`, `unit_price`, `quantity`, `line_total` (snapshots)
- `shipping_address`: full snapshot fields
- `payment`: provider record summary ([12-payments/payment-model.md](../12-payments/payment-model.md))
- `totals`: subtotal/tax/shipping/grand
- `history`: `PLANNED` (absent in MVP)

## 3) Cancel (own)

`202`/`200` with updated order (`CANCELLED`) — synchronous in MVP. Errors: `409` if status ∉ {`PENDING_PAYMENT`}; foreign id `404`. Post-payment user cancellation is out of MVP (admin-only) — see [business-rules.md](business-rules.md).

## 6) Status update

```json
{ "status": "SHIPPED" }
```
Illegal transition → `422` with current status echo ([order-lifecycle.md](order-lifecycle.md) rules). Success `200` with updated order.

## 7) Admin cancel

`{ "reason": "Out of stock after verification" }` — required; restock applied per rules.

## Errors (shared)

`401` unauth; `403` admin-only paths as customer; `404` foreign/unknown; `409` status conflict; `422` validation/illegal body. Convention details: [15-api/README.md](../15-api/README.md).
