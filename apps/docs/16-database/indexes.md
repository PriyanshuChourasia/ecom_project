# Indexes

Status legend: see [root README](../README.md). All `REQUIRED` (with the tables).

| Table | Index | Purpose |
|---|---|---|
| users | `UNIQUE(email)` (✅ exists) | login identity |
| users | `INDEX(status)` | admin customer filters |
| roles | `UNIQUE(name)` | role lookup |
| role_user | `UNIQUE(user_id, role_id)` | pivot integrity |
| categories | `UNIQUE(slug)` · `INDEX(parent_id, is_active)` | public nav, children lookup |
| products | `UNIQUE(sku)` · `INDEX(status, category_id)` · `INDEX(status, created_at)` · `INDEX(brand)` | browse/sort/filter |
| products | `INDEX(stock_quantity)` | low/out-of-stock admin filters |
| product_images | `INDEX(product_id, sort_order)` | gallery ordering |
| addresses | `INDEX(user_id, is_default)` | default-first fetch |
| carts | `UNIQUE(user_id)` | one active cart |
| cart_items | `UNIQUE(cart_id, product_id)` · `INDEX(cart_id)` | merge integrity |
| orders | `UNIQUE(number)` · `INDEX(user_id, created_at)` · `INDEX(status)` | history, admin queue |
| order_items | `INDEX(order_id)` · `INDEX(sku)` | order render, admin SKU search |
| payments | `UNIQUE(provider_order_id)` · `UNIQUE(provider_payment_id)` · `INDEX(order_id, status)` | webhook idempotency, reconciliation |
| sessions/cache (Laravel) | framework defaults ✅ | `IMPLEMENTED` |

Full-text search (name/description): not in MVP — LIKE + indexes suffice for demo scale; FTs `PLANNED` ([06-products/product-model.md](../06-products/product-model.md) search note).
