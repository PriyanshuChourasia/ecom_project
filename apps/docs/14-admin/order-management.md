# Order Management (Admin)

Status legend: see [root README](../README.md). All `REQUIRED`.

## The seller's order loop

```
View orders → open order → verify payment → update status (CONFIRMED → PROCESSING → SHIPPED → DELIVERED)
```

## Screens

### Orders list (`GET /admin/orders`)

| Column | Source |
|---|---|
| number, placed_at | order |
| customer | user snapshot (name/phone) |
| items count + total | order |
| payment status chip | payment record |
| status chip (color-coded by lifecycle stage) | order.status |

Filters: status tabs (`PAID` first — action queue), search by number/phone/SKU, date range. Pagination standard.

### Order detail (`GET /admin/orders/{id}`)

- **Customer + shipping snapshot** (name, phone, full address) — read-only ([09-addresses/business-rules.md](../09-addresses/business-rules.md) rule 4).
- **Items** with SKU/name/unit price/qty/line totals (snapshots).
- **Totals** block.
- **Payment block**: provider status, `pay_` id, amount, verified time — the "verify payment" step means confirming this shows `captured` and amount == `grand_total` ([12-payments/README.md](../12-payments/README.md)).
- **Status actions**: legal next-states only (buttons render per current status; illegal transitions impossible — [11-orders/order-lifecycle.md](../11-orders/order-lifecycle.md) rule 1).
- **Cancel** (pre-shipment only) with mandatory reason ([11-orders/business-rules.md](../11-orders/business-rules.md)).

## Update-status contract

`PATCH /admin/orders/{id}/status` `{ "status": "PROCESSING" }` → `200` updated order; `422` on illegal transition; `409` concurrent-update race (TBD optimistic-locking via `updated_at` check).

## Customer/order information visible to seller

Allowed: order contents, totals, shipping snapshot, buyer name/phone/email (fulfillment needs), payment record. Not exposed: buyer's other addresses, password fields obviously, other orders without navigating (linked view `PLANNED`).

Gate: every screen/endpoint behind `ADMIN` ([access-control.md](access-control.md)).
