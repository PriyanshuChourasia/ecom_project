# Product Images

Status legend: see [root README](../README.md). Storage directory exists (`storage/app/public`, Laravel `IMPLEMENTED`); the `public/storage` link and image feature are `REQUIRED`.

## Model: `ProductImage`

| Field | Type | Notes |
|---|---|---|
| `id` | bigint PK | |
| `product_id` | FK | |
| `path` | string, relative | `products/{product_id}/{filename}` |
| `sort_order` | int | 0 = primary |
| `created_at`, `updated_at` | timestamps | audit |

## Rules

1. First image (`sort_order = 0`) is the **primary image** used on listing cards.
2. Uploads: JPEG/PNG/WebP, ≤ 5MB each; max 8 per product (excess → `422`).
3. Server generates: original + resized `card` (600px) + `thumb` (200px) variants; original never served directly (TBD variant pipeline choice — GD/Intervention).
4. Uploads go through admin-authenticated endpoints only ([api.md](api.md)) — never customer-writable.
5. Deleting a product's image updates `sort_order` chain; product with zero images is allowed but shows placeholder.
6. Storage: Laravel `public` disk via Storage facade; `php artisan storage:link` is part of setup ([19-deployment/local-development.md](../19-deployment/local-development.md)) — currently NOT LINKED (`IMPLEMENTED` gap noted in skeleton status).
7. URLs in API responses are absolute (APP_URL-based); local dev = `http://localhost:8000/storage/...` (`IMPLEMENTED` env value).
