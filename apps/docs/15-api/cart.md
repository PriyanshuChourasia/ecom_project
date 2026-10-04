# API — Cart

Status: all `REQUIRED`. Canonical detail: [08-cart/api.md](../08-cart/api.md).

| Method | URL | Auth | Purpose |
|---|---|---|---|
| GET | `/api/v1/cart` | Bearer | live-priced cart (empty → empty payload, not 404) |
| POST | `/api/v1/cart/items` | Bearer | add/merge item `{product_id, quantity}` |
| PATCH | `/api/v1/cart/items/{itemId}` | Bearer | set quantity (0 = remove) |
| DELETE | `/api/v1/cart/items/{itemId}` | Bearer | remove item |
| DELETE | `/api/v1/cart` | Bearer | clear (idempotent) |

Key rules: client sends only product_id + quantity; all totals server-computed live; foreign cart resources → 404; soft stock warnings in-band; hard `409` only at checkout ([08-cart/business-rules.md](../08-cart/business-rules.md)).
