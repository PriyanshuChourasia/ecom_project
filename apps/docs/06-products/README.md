# 06 — Products

Status legend: see [root README](../README.md). Everything `REQUIRED` unless noted.

| Document | Contents |
|---|---|
| [product-model.md](product-model.md) | Entity: SKU, brand, price, discount, tax, stock |
| [product-lifecycle.md](product-lifecycle.md) | DRAFT → ACTIVE → INACTIVE |
| [api.md](api.md) | Public read + admin CRUD endpoints |
| [business-rules.md](business-rules.md) | Pricing, discount, display rules |
| [product-images.md](product-images.md) | Image upload/management |

## Scope note

Products are **physical goods** in any vertical: stationery, electronics, laptops, desktops, CCTV cameras/DVRs. A CCTV *installation/service* is **not** a product or product-service workflow here — it is FUTURE ([21-future-scope/cctv-services.md](../21-future-scope/cctv-services.md)).

Related: category ([05](../05-categories/README.md)), stock ([07](../07-inventory/README.md)), cart pricing ([08](../08-cart/README.md)).
