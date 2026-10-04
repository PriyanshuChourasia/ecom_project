# Cart API

Status legend: see [root README](../README.md). All `REQUIRED`. All endpoints require Bearer auth (`CUSTOMER` or `ADMIN` acting for own cart).

| # | Method | URL | Purpose |
|---|---|---|---|
| 1 | GET | `/api/v1/cart` | Current cart with live pricing |
| 2 | POST | `/api/v1/cart/items` | Add item (or merge quantity) |
| 3 | PATCH | `/api/v1/cart/items/{itemId}` | Set quantity (0 = remove) |
| 4 | DELETE | `/api/v1/cart/items/{itemId}` | Remove item |
| 5 | DELETE | `/api/v1/cart` | Clear cart |

## 1) GET /cart — response

```json
{
  "data": {
    "id": 7,
    "items": [
      {
        "id": 31, "product_id": 42, "sku": "CCTV-DOME-2MP",
        "name": "Indoor Dome Camera 2MP",
        "quantity": 2,
        "unit_price": 224910, "line_total": 449820,
        "in_stock": true, "stock_quantity": 7,
        "price_at_add": 224910, "price_changed": false
      }
    ],
    "item_count": 2,
    "subtotal": 449820
  }
}
```
All money fields are server-computed **live** at response time (paise). Empty cart → `{"data": {"items": [], "item_count": 0, "subtotal": 0}}` (never a 404).

## 2) POST /cart/items — request

```json
{ "product_id": 42, "quantity": 2 }
```
Validation: `product_id` required, exists, `ACTIVE`; `quantity` required, integer, ≥ 1, ≤ 99 (max-per-line TBD).
Errors: `404` unknown/inactive product; `422` validation; soft stock warning returned in-band:
```json
{ "data": { "…item…": "…" }, "warnings": ["only_3_left"] }
```

## 3) PATCH — request `{ "quantity": 5 }`; quantity 0 removes the row (returns `200` with updated cart, or `204` — convention TBD, fix once in code).

## 4/5) DELETE — `204`; clearing an already-empty cart is a no-op `204` (idempotent).

## Error handling

`401` no/expired token; cart of another user → `404` (scope rule from [04-roles-permissions/access-control.md](../04-roles-permissions/access-control.md)); `409` only at checkout (not cart) for hard stock conflicts.

Client consumption: [13-consumer-app/api-integration.md](../13-consumer-app/api-integration.md).
