# Cart Business Rules

Status legend: see [root README](../README.md). All `REQUIRED`.

## Source of truth (non-negotiable)

1. **Backend computes all money.** Client sends `product_id` + `quantity` only. Any client-computed total is display-only and must be overwritten by server responses.
2. **Prices are live-quoted**, read from `products` at every cart read/quote — never persisted as authoritative ([cart-model.md](cart-model.md) `price_at_add` is a hint only).
3. **Stock truth lives in checkout**: cart warnings are advisory; only checkout/quote can hard-block ([07-inventory/stock-flow.md](../07-inventory/stock-flow.md), [10-checkout/validation.md](../10-checkout/validation.md)).

## Persistence & lifecycle

4. Cart requires authentication — no guest carts in MVP (guest checkout `PLANNED` post-MVP; impacts registration funnel, note kept for reference).
5. One active cart per user; created lazily; cleared (rows deleted) on successful order conversion.
6. Cart survives sessions/devices (server-side rows); merging multiple device carts is inherent (single cart row).
7. Abandoned carts: rows persist indefinitely in MVP; periodic pruning `PLANNED`.

## Item integrity

8. Duplicate product adds merge rows and sum quantity (cap per line TBD ≤ 99).
9. Items referencing deactivated/deleted products are auto-excluded from pricing and surfaced via `issues[]` at quote; cart read keeps them flagged `price_changed`/stale (final UX TBD in [cart-flow.md](cart-flow.md) rule 5).
10. Quantity bounds: ≥ 1 after merge; 0 via PATCH deletes.

## Cross-module contracts

- → Checkout consumes the cart read-only, then converts ([10-checkout/checkout-flow.md](../10-checkout/checkout-flow.md)).
- → Addresses are **not** part of cart state; chosen at checkout ([09-addresses/README.md](../09-addresses/README.md)).
- → Products define `effective_price` inputs ([06-products/product-model.md](../06-products/product-model.md)).
