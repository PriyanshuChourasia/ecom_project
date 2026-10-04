# 14 — Admin / Seller

Status legend: see [root README](../README.md). Everything `REQUIRED` — no admin console exists (Laravel serves only the welcome page today).

| Document | Contents |
|---|---|
| [dashboard.md](dashboard.md) | Landing metrics |
| [product-management.md](product-management.md) | Products, images, pricing |
| [inventory-management.md](inventory-management.md) | Stock operations |
| [order-management.md](order-management.md) | Order queue + status updates |
| [customer-management.md](customer-management.md) | Customer views + block |
| [access-control.md](access-control.md) | Admin login + authorization |

## Seller workflow (MVP)

```
Login → Dashboard → Products → Inventory → Orders → Customers
```

The admin console is the seller's single surface; the seller persona = `ADMIN` role ([04-roles-permissions/roles.md](../04-roles-permissions/roles.md)). Form factor TBD (Blade vs SPA) per [01-architecture/frontend-architecture.md](../01-architecture/frontend-architecture.md).
