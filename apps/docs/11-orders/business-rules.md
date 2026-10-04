# Order Business Rules

Status legend: see [root README](../README.md). All `REQUIRED`.

## Creation integrity

1. Orders are created **only** via checkout-order ([10-checkout/api.md](../10-checkout/api.md)) — never from client-supplied totals; all amounts snapshotted server-side.
2. Creation is transactional: validation → order + items + snapshots → stock reservation; any failure rolls back everything ([10-checkout/validation.md](../10-checkout/validation.md)).
3. Numbering `ORD-YYYY-<id>` is display-only; `id` is the join key.

## Payment window

4. `PENDING_PAYMENT` orders expire after `PAYMENT_WINDOW_MIN` (**TBD**, suggested 30 min) → auto-`CANCELLED` + stock restored. Scheduler via Laravel cron (`IMPLEMENTED` framework capability; the job itself `REQUIRED`).
5. Retry after `PAYMENT_FAILED` re-enters `PENDING_PAYMENT` only inside the same window (retry-count cap **TBD**, suggested 3).

## Stock coupling

6. Reserve at creation; definitive sale at `PAID`; restore on fail/expiry/cancel ([07-inventory/stock-flow.md](../07-inventory/stock-flow.md)).
7. Admin cancellation (pre-shipment only) restores stock; `SHIPPED`+ orders cannot be cancelled in MVP (returns process = `FUTURE`/`PLANNED` post-MVP).

## History & immutability

8. Snapshotted fields (prices, product name/SKU, address) are immutable after creation ([order-model.md](../11-orders/order-model.md)).
9. No hard delete of orders ever (financial record).
10. Consumer sees own orders only (query-scoped); admin sees all ([04-roles-permissions/permissions.md](../04-roles-permissions/permissions.md)).

## Cancellation matrix (summary)

| Current status | Consumer | Admin |
|---|:---:|:---:|
| `PENDING_PAYMENT` | ✅ (also auto-expiry) | ✅ |
| `PAID`…`PROCESSING` | ❌ | ✅ + restock + reason |
| `SHIPPED`+ | ❌ | ❌ (no returns in MVP) |

## History surfaces

- Consumer: My Orders list + detail ([13-consumer-app/screen-map.md](../13-consumer-app/screen-map.md)).
- Admin: orders dashboard + detail ([14-admin/order-management.md](../14-admin/order-management.md)).
- Status-change audit log: `PLANNED`.
