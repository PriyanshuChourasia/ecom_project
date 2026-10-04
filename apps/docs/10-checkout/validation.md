# Checkout Validation

Status legend: see [root README](../README.md). All `REQUIRED`.

Executed at **both** quote and order-creation (order-creation is authoritative; quote is the early-warning pass).

| # | Check | Failure | Recovery path |
|---|---|---|---|
| 1 | Authenticated | `401` | login → resume checkout |
| 2 | Cart non-empty (active items) | `422` `cart_empty` | browse → add |
| 3 | Every item's product `ACTIVE` | item flagged in `issues[]` (quote) / excluded + `422` (order) | remove item → re-quote |
| 4 | Stock ≥ quantity per item | `409` `insufficient_stock` (with available qty) | reduce qty / drop item |
| 5 | `address_id` present, exists, **owned** | `404` foreign/unknown · `422` missing | pick/create address |
| 6 | Address complete (all required fields) | `422` | edit address |
| 7 | Product price/stock re-read fresh | (enables 3–4) | — |
| 8 | Idempotency: duplicate order-creation while pending | return existing pending order (`200` + idempotency-key TBD) | resume payment |

## Stock race handling

Two users check out the last unit simultaneously:

- Order creation wraps validation + reservation in a DB transaction with conditional decrement (`stock_quantity >= qty` else rollback).
- The loser receives `409` with `insufficient_stock` and current availability — never a silent partial order.

## Failure payload contract

```json
{
  "error": "insufficient_stock",
  "message": "Some items are no longer available",
  "issues": [
    { "product_id": 42, "sku": "CCTV-DOME-2MP", "requested": 5, "available": 3 }
  ]
}
```

Quote responses carry the same `issues[]` shape in-band (`200`) so the UI can fix before attempting payment.

## Payment-window revalidation

A pending order expiring its payment window is cancelled + stock restored ([11-orders/business-rules.md](../11-orders/business-rules.md)); checkout always creates fresh orders (never resumes stale payment sessions).
