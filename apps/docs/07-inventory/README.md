# 07 — Inventory

Status legend: see [root README](../README.md). Everything `REQUIRED`.

| Document | Contents |
|---|---|
| [inventory-model.md](inventory-model.md) | Stock-on-product design, fields |
| [stock-flow.md](stock-flow.md) | Reservation/decrement/restore rules |
| [api.md](api.md) | Admin stock endpoints |

## Design position (MVP)

**Inventory = `stock_quantity` on the `Product` row** — one global stock, no warehouses, no locations, no per-variant tracking, no separate ledger. Multi-warehouse is FUTURE ([21-future-scope/advanced-features.md](../21-future-scope/advanced-features.md)).

Demo verticals (CCTV hardware included) follow the same rules: single stock pool per product.

Related: product stock field ([06-products/product-model.md](../06-products/product-model.md)), order impact ([11-orders/README.md](../11-orders/README.md)).
