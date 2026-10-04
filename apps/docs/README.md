# E-Commerce Platform — Technical Documentation

**Project:** `ecom_proj` — a consumer e-commerce platform with a seller/admin console and a Flutter mobile application.
**Codebase:** Turborepo monorepo — Laravel 13 backend (`apps/lara_ecom`) + Flutter 3.38 mobile app (`apps/ecom_mob`).
**Documentation generated:** 2026-10-04. Status markers reflect the codebase **as inspected on this date**.

---

## Project Purpose

Build and demonstrate a working e-commerce MVP: consumers browse products, add them to a cart, check out, pay, and place orders; sellers/admins manage products, inventory, and orders.

## MVP Objective

| Actor | MVP flow |
|---|---|
| **Consumer** | Browse products → view product → add to cart → checkout → payment → place order |
| **Seller/Admin** | Manage products → manage inventory → view orders → update order status |

The critical demo (see [20-mvp/demo-flow.md](20-mvp/demo-flow.md)):

```
Seller creates product
  ↓
Consumer discovers product
  ↓
Consumer views product
  ↓
Consumer adds product to cart
  ↓
Consumer checks out
  ↓
Consumer pays
  ↓
Consumer places order
  ↓
Seller receives order
  ↓
Seller manages order
```

## Status Legend

Used consistently across all documentation:

| Status | Meaning |
|---|---|
| `IMPLEMENTED` | Exists in the codebase today and works |
| `PARTIALLY IMPLEMENTED` | Some part exists (e.g. a framework default) but not the MVP feature itself |
| `REQUIRED` | Needed for the MVP but does not exist yet — must be built |
| `PLANNED` | Designed/proposed but deliberately out of the current MVP |
| `FUTURE` | Roadmap item, explicitly not MVP (CCTV services, WhatsApp, etc.) |
| `TBD` | Unresolved decision, blocking or shaping related work |

## Current Implementation Status (summary)

| Area | Status |
|---|---|
| Monorepo & tooling (Turborepo, pnpm) | `IMPLEMENTED` |
| Laravel 13 app skeleton (SQLite, database sessions/cache/queue) | `IMPLEMENTED` |
| Flutter app skeleton (default counter app) | `IMPLEMENTED` |
| REST API (`routes/api.php`, Sanctum) | `REQUIRED` |
| Users / Roles / Categories / Products / Inventory / Cart / Addresses / Checkout / Orders / Payments | `REQUIRED` |
| Admin & consumer UI screens | `REQUIRED` |
| Razorpay integration | `REQUIRED` |
| Tests beyond framework examples | `REQUIRED` |

**Honest baseline:** the repository currently contains *framework skeletons only*. One Laravel `User` model exists; everything e-commerce-specific is documented here as `REQUIRED` or later. Nothing in these docs invents existing behavior.

## Architecture Overview

```
┌─────────────────┐        ┌──────────────────────────────┐
│  Flutter mobile │  HTTPS │   Laravel 13 backend          │
│  apps/ecom_mob  ├───────►│   apps/lara_ecom              │
│  (consumer)     │  REST  │   ├─ REST API  (api/*) REQUIRED│
└─────────────────┘        │   ├─ Admin console REQUIRED    │
                           │   ├─ Domain modules REQUIRED   │
┌─────────────────┐        │   └─ SQLite (current) / MySQL  │
│ Admin (browser) ├───────►│        (target)                │
└─────────────────┘        └──────────────┬───────────────┘
                                          │
                                    ┌─────▼─────┐
                                    │ Razorpay  │ FUTURE/REQUIRED
                                    │ (payments)│ for MVP
                                    └───────────┘
```

Details: [01-architecture/README.md](01-architecture/README.md).

## Documentation Index

| # | Module | Contents |
|---|---|---|
| 00 | [Project Overview](00-project-overview/README.md) | Business requirements, MVP scope, feature status |
| 01 | [Architecture](01-architecture/README.md) | System/backend/frontend architecture, data flow, diagrams |
| 02 | [Authentication](02-authentication/README.md) | Registration, login, tokens, middleware |
| 03 | [Users](03-users/README.md) | User entity, lifecycle, customer flow |
| 04 | [Roles & Permissions](04-roles-permissions/README.md) | Customer vs Admin/Seller, access control |
| 05 | [Categories](05-categories/README.md) | Category model, API, business rules |
| 06 | [Products](06-products/README.md) | Product model, lifecycle, images, business rules |
| 07 | [Inventory](07-inventory/README.md) | Stock model, stock flow, admin operations |
| 08 | [Cart](08-cart/README.md) | Cart model, pricing rules, persistence |
| 09 | [Addresses](09-addresses/README.md) | Address model, defaults, order snapshots |
| 10 | [Checkout](10-checkout/README.md) | Checkout flow, pricing, validation |
| 11 | [Orders](11-orders/README.md) | Order model, lifecycle, snapshots, cancellation |
| 12 | [Payments](12-payments/README.md) | Razorpay, verification, webhooks, refunds |
| 13 | [Consumer App](13-consumer-app/README.md) | Screen map, navigation, shopping flow |
| 14 | [Admin / Seller](14-admin/README.md) | Dashboard, product/inventory/order management |
| 15 | [API](15-api/README.md) | Endpoint reference (actual + required) |
| 16 | [Database](16-database/README.md) | Entities, relationships, ERD |
| 17 | [Security](17-security/README.md) | Auth security, API security, payment security |
| 18 | [Testing](18-testing/README.md) | Strategy, flow tests, acceptance criteria |
| 19 | [Deployment](19-deployment/README.md) | Local, staging, production, env vars |
| 20 | [MVP](20-mvp/README.md) | Demo flow, checklist, acceptance criteria, demo script |
| 21 | [Future Scope](21-future-scope/README.md) | CCTV services, notifications, shipping, advanced features |

## Important Assumptions

1. **Single seller for MVP** — the admin console represents the seller; multi-vendor is FUTURE.
2. **Razorpay is the sole payment gateway** for MVP.
3. **Mobile is the primary consumer surface** (Flutter); a mobile-friendly admin web console is secondary.
4. **Backend is the source of truth** for prices, stock, and order totals — the mobile app never computes chargeable amounts.
5. Physical products only (stationery, electronics, laptops, desktops, CCTV hardware) for MVP; **service booking is FUTURE**.
6. Currency: INR (Razorpay default). TBD: multi-currency is not planned.

## MVP Status

**Not yet demoable.** The demo flow in [20-mvp/demo-flow.md](20-mvp/demo-flow.md) cannot be executed against the current codebase. Every step of that flow is `REQUIRED`. See [documentation-status.md](documentation-status.md) for the precise gap list and the recommended next development step.

## Future Scope

Explicitly **not** MVP: CCTV installation/service booking, technician management, SMS/WhatsApp notifications, advanced SEO, marketing automation, shipping integrations, reviews, coupons, loyalty, multi-vendor. See [21-future-scope/README.md](21-future-scope/README.md).
