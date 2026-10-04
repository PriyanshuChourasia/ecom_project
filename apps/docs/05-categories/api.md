# Categories API

Status legend: see [root README](../README.md). All `REQUIRED`.

| # | Method | URL | Auth | Authz | Purpose |
|---|---|---|---|---|---|
| 1 | GET | `/api/v1/categories` | none | public | Active categories (customers) / all (admin via query param) |
| 2 | GET | `/api/v1/categories/{slug}` | none | public | Category detail + product count |
| 3 | POST | `/api/v1/admin/categories` | Bearer | `ADMIN` | Create |
| 4 | PATCH | `/api/v1/admin/categories/{id}` | Bearer | `ADMIN` | Update name/description/parent/activation |
| 5 | DELETE | `/api/v1/admin/categories/{id}` | Bearer | `ADMIN` | Delete — only when empty; `409` otherwise (see rules) |

## 1) List — example response

```json
{
  "data": [
    { "id": 1, "name": "Stationery", "slug": "stationery", "parent_id": null, "is_active": true, "products_count": 12 },
    { "id": 2, "name": "Electronics", "slug": "electronics", "parent_id": null, "is_active": true, "products_count": 5 },
    { "id": 3, "name": "CCTV", "slug": "cctv", "parent_id": 2, "is_active": true, "products_count": 3 }
  ]
}
```

## 3) Create — request

```json
{ "name": "Laptops", "parent_id": 2, "is_active": true, "description": "Portable computers" }
```
`slug` auto-generated from `name`; can be overridden explicitly.

## Validation

| Field | Rules |
|---|---|
| `name` | required, string, ≤255, same-parent uniqueness (case-insensitive) |
| `slug` | optional, unique, slug format |
| `parent_id` | nullable, must exist, must be top-level (depth cap) |
| `is_active` | bool |

Errors: `409` conflict (delete non-empty), `422` validation, `404` unknown id, `401/403` auth/authz — conventions per [15-api/README.md](../15-api/README.md).

Consumer-side usage: [13-consumer-app/screen-map.md](../13-consumer-app/screen-map.md); admin usage: [14-admin/product-management.md](../14-admin/product-management.md).
