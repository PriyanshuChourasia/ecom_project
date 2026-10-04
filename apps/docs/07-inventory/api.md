# Inventory API

Status legend: see [root README](../README.md). All `REQUIRED`.

| # | Method | URL | Auth | Authz | Purpose |
|---|---|---|---|---|---|
| 1 | GET | `/api/v1/admin/inventory` | Bearer | `ADMIN` | Stock report (all products) |
| 2 | GET | `/api/v1/admin/inventory?filter=out_of_stock\|low` | Bearer | `ADMIN` | Filtered views (`low` threshold TBD) |
| 3 | PATCH | `/api/v1/admin/products/{id}/stock` | Bearer | `ADMIN` | Set stock (recount semantics) |
| 4 | POST | `/api/v1/admin/products/{id}/stock/adjust` | Bearer | `ADMIN` | Delta adjust (+5 / −3) with reason |

## 3) Set stock — request/response

```json
{ "stock_quantity": 25, "reason": "New shipment received" }
```
```json
{ "product_id": 42, "sku": "CCTV-DOME-2MP", "stock_quantity": 25, "previous_quantity": 10, "adjusted_by": 1, "at": "2026-10-04T12:00:05Z" }
```
Guard: PATCH with `reserved_quantity > new value` conflict → `409` (can't set below held stock; hold-resolution TBD per [stock-flow.md](stock-flow.md)).

## 4) Adjust — request

```json
{ "delta": -3, "reason": "Damaged in storage" }
```
Response mirrors (3) with resulting quantity; never falls below 0 (`422` if delta crosses zero).

## Consumer-facing availability

There is **no separate consumer inventory endpoint** — availability is part of product payloads (list shows `in_stock`/`stock_quantity` policy; detail adds exact quantity). See [06-products/api.md](../06-products/api.md).

## Admin operations recap ([14-admin/inventory-management.md](../14-admin/inventory-management.md))

- View counts + filters, set stock on delivery arrival, decrement for damage/theft via adjust, verify reservations on the orders screen.
- Movement history per product: `PLANNED` (ledger).
