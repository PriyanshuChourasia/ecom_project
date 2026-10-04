# Inventory Management (Admin)

Status legend: see [root README](../README.md). All `REQUIRED`.

## Purpose

One screen to see and correct stock across the catalog; pairs with the per-product inline stock editor in [product-management.md](product-management.md).

## Elements

| Element | Detail | API |
|---|---|---|
| Stock table | product, SKU, category, quantity, state chip, reserved count (within payment window) | `GET /admin/inventory` |
| Filters | all / out-of-stock / low (threshold TBD) / search | query params |
| Set stock | absolute set (recount, e.g. shipment arrival) | `PATCH /admin/products/{id}/stock` |
| Adjust stock | delta ± with reason (damage, theft, audit) | `POST /admin/products/{id}/stock/adjust` |
| Reservation view | quantities held by `PENDING_PAYMENT` orders | included in row data |

## Rules surfaced in UI

1. Set-below-reserved → `409` blocked with explanation ([07-inventory/api.md](../07-inventory/api.md)).
2. Adjust below zero → `422`.
3. Every set/adjust shows previous → new value confirmation.
4. Movement history per product: `PLANNED` (ledger) — MVP shows current state + last change via confirmation logs only.

Demo usage: bump stock before the consumer adds to cart ([20-mvp/demo-script.md](../20-mvp/demo-script.md)).
