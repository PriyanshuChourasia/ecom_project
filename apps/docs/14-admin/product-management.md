# Product Management (Admin)

Status legend: see [root README](../README.md). All `REQUIRED`.

## Capabilities (the seller's product loop)

| Capability | Screen element | API | Notes |
|---|---|---|---|
| Add product | create form | `POST /admin/products` | defaults `DRAFT` or `ACTIVE` if flagged |
| Edit product | detail form | `PATCH /admin/products/{id}` | all fields incl. specs |
| Upload images | image manager | `POST /admin/products/{id}/images` | ≤8, first = primary; drag-order TBD |
| Remove image | image manager | `DELETE /admin/products/{id}/images/{imageId}` | re-chain sort order |
| Change price | price fields | `PATCH /admin/products/{id}` | paise input w/ rupee helper |
| Change discount | discount field | same | live effective-price preview |
| Change stock | inline in list or inventory screen | `PATCH …/stock`, `POST …/stock/adjust` | see [inventory-management.md](inventory-management.md) |
| Activate/Deactivate | status toggle | `PATCH /admin/products/{id}` (`status`) | lifecycle rules ([06-products/product-lifecycle.md](../06-products/product-lifecycle.md)) |
| View all | list w/ filters | `GET /admin/products` | status/category/stock filters, search |

## Product form (create/edit)

Sections: Basics (name, SKU auto-suggest, brand, category picker, description) → Pricing (price, discount, tax — with computed effective price) → Inventory (stock qty) → Images → Specs (JSON-backed key/value UI) → Status (draft/active/inactive).

Validation mirrors the API contract ([06-products/api.md](../06-products/api.md)); errors inline (`422` mapping).

## List behaviors

- Row: thumb, name/SKU, category, effective price, stock chip (green/amber/red by threshold TBD), status chip.
- Bulk actions: `PLANNED` post-MVP.
- Duplicate-SKU attempt → `409` surfaced inline.

Demo usage: creating the demo product happens here first ([20-mvp/demo-script.md](../20-mvp/demo-script.md) Act 1).
