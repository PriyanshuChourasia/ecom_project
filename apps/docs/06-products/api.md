# Products API

Status legend: see [root README](../README.md). All `REQUIRED`.

## Public (customer-facing)

| # | Method | URL | Auth | Purpose |
|---|---|---|---|---|
| 1 | GET | `/api/v1/products` | none | List/search ACTIVE products |
| 2 | GET | `/api/v1/products/{slug}` | none | Active product detail w/ images |
| 3 | GET | `/api/v1/products/{slug}/related` | none | Same-category, same brand first (PLANNED ordering; basic same-category for MVP) |

**1) Query params:** `q` (search name/SKU/brand), `category` (slug), `brand`, `min_price`/`max_price` (paise), `in_stock` bool, `sort` (`newest` default | `price_asc` | `price_desc`), `page`, `per_page` (max 50).

```json
{
  "data": [
    {
      "id": 42, "slug": "indoor-dome-camera-2mp", "sku": "CCTV-DOME-2MP",
      "name": "Indoor Dome Camera 2MP", "brand": "SecureEye",
      "price": 249900, "discount_percent": 10, "effective_price": 224910,
      "effective_price_display": "₹2,249.10", "in_stock": true, "stock_quantity": 7,
      "category": { "slug": "cctv", "name": "CCTV" },
      "primary_image": "products/42/dome-front.jpg"
    }
  ],
  "meta": { "current_page": 1, "last_page": 3, "total": 57 }
}
```

**2) Detail** adds: full `description`, `specs`, all `images[]`, tax fields.

Errors: `404` unknown slug or non-`ACTIVE` product (customers never see drafts).

## Admin (`/api/v1/admin/products`, Bearer + `ADMIN`)

| # | Method | URL | Purpose |
|---|---|---|---|
| 4 | GET | `/api/v1/admin/products` | All statuses; filters status/category/stock |
| 5 | POST | `/api/v1/admin/products` | Create (default `DRAFT` or `ACTIVE` if `is_active=true`) |
| 6 | GET | `/api/v1/admin/products/{id}` | Any status detail |
| 7 | PATCH | `/api/v1/admin/products/{id}` | Update fields incl. `status` (lifecycle rules apply), price, stock, specs |
| 8 | PATCH | `/api/v1/admin/products/{id}/stock` | Dedicated stock update ([07](../07-inventory/README.md)) |
| 9 | POST | `/api/v1/admin/products/{id}/images` | Add image(s) |
| 10 | DELETE | `/api/v1/admin/products/{id}/images/{imageId}` | Remove image |

**5) Create request:**

```json
{
  "category_id": 3,
  "sku": "CCTV-DOME-2MP",
  "name": "Indoor Dome Camera 2MP",
  "brand": "SecureEye",
  "description": "1080p indoor dome camera with IR night vision.",
  "price": 249900,
  "discount_percent": 10,
  "tax_percent": 0,
  "stock_quantity": 10,
  "is_active": true,
  "specs": { "resolution": "1080p", "ir_range_m": 20 }
}
```

Validation: `category_id` exists; `sku` required unique; `price` required integer ≥ 0; `discount_percent` 0–100; `tax_percent` 0–100; `stock_quantity` ≥ 0 integer; `specs` JSON ≤ 4KB; `name`/`description` required. Errors `409` duplicate SKU, `422` validation.

Admin screens: [14-admin/product-management.md](../14-admin/product-management.md).
