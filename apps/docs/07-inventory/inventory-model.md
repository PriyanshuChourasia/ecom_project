# Inventory Model

Status legend: see [root README](../README.md). All `REQUIRED`.

## Where stock lives

There is **no separate `Inventory` entity in MVP**. Stock is the `stock_quantity` integer column on `Product` ([06-products/product-model.md](../06-products/product-model.md)) — documented here as the inventory design because the spec's `Inventory` entity maps onto it.

| Aspect | MVP design | Status |
|---|---|---|
| Storage | `products.stock_quantity` ≥ 0 integer | `REQUIRED` column on product table |
| Scope | one global pool per product | `REQUIRED` |
| Reservation rows (e.g. `Inventory`, `StockLedger`) | not in MVP; ledger `PLANNED` for audit | `PLANNED` |
| Out-of-stock threshold | quantity ≤ 0 | `REQUIRED` |
| Low-stock alert | `PLANNED` (admin badge at threshold, value TBD) | `PLANNED` |

## Availability states

| State | Condition | Customer sees | Can buy? |
|---|---|---|---|
| In stock | `stock_quantity ≥ qty` at add/quote time | quantity or "In stock" (display TBD) | ✅ |
| Out of stock | `stock_quantity = 0` | "Out of stock", product stays browsable if `ACTIVE` | ❌ |
| Insufficient | `0 < stock_quantity < requested qty` | limited-stock hint | ❌ for that qty |

## Why stock lives on the product (rationale)

- Single-seller, single-warehouse MVP: a join/ledger adds latency and complexity without benefit.
- Atomic stock updates (`UPDATE ... SET stock_quantity = stock_quantity - X WHERE stock_quantity >= X`) provide race-safe decrementing at order time.
- Moving to rows in a dedicated `inventories` table later is a contained migration (documented as the extension path, `PLANNED`).
