# Relationships

Status legend: see [root README](../README.md). All `REQUIRED` except where noted.

| Relation | Cardinality | Notes |
|---|---|---|
| User ↔ Role | M:N via role_user | ADMIN seeded; CUSTOMER default |
| User → Address | 1:N | own-scope reads |
| User → Cart | 1:1 (unique) | lazy-created active cart |
| Cart → CartItem | 1:N | unique(cart_id, product_id) |
| Product → CartItem | 1:N | validated at add; pruned at quote if inactive |
| Category → Product | 1:N | required FK; deactivate ≠ detach |
| Category → Category | 1:N self | depth cap 1 |
| Product → ProductImage | 1:N | sort_order primary |
| User → Order | 1:N | buyer |
| Order → OrderItem | 1:N | cascade with order |
| Order → Product (via OrderItem) | N:M | **snapshot copy** — name/SKU/unit_price duplicated, product_id kept live |
| Order → Address | snapshot | no live FK; full field copy ([09-addresses/business-rules.md](../09-addresses/business-rules.md)) |
| Order → Payment | 1:1 (MVP) | one active payment; retries update the row (`PLANNED`: history rows) |
| User → Payment | indirect via Order | never direct |

## Integrity rules

1. **Snapshot columns are write-once** (order creation); updates forbidden by policy/tests ([11-orders/business-rules.md](../11-orders/business-rules.md) rule 8).
2. FKs `ON DELETE`:
   - Category→Product `RESTRICT` (empty-only delete, [05-categories/business-rules.md](../05-categories/business-rules.md)).
   - Cart→CartItem `CASCADE`; CartItem→Product `RESTRICT` (product delete blocked with history anyway).
   - Order→OrderItem `CASCADE`; Order→Payment `CASCADE`.
   - Address delete is soft-by-design (rows simply removed; orders unaffected — snapshots).
3. Stock decrements are conditional atomic updates against `products.stock_quantity` ([07-inventory/stock-flow.md](../07-inventory/stock-flow.md)).
