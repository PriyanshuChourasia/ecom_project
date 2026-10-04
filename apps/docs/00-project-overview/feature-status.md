# Feature Status Matrix

Status legend: see [root README](../README.md). Snapshot date: **2026-10-04**.

> Ground truth: the backend contains only the Laravel skeleton (one `User` model, default migrations, `web.php` welcome route, no `routes/api.php`). The Flutter app is the default counter template. Everything e-commerce-specific below is therefore `REQUIRED`.

## Platform / Tooling

| Feature | Status | Notes |
|---|---|---|
| Turborepo monorepo, pnpm workspaces | `IMPLEMENTED` | `turbo.json`, `pnpm-workspace.yaml` |
| Laravel 13 app (`apps/lara_ecom`) | `IMPLEMENTED` | PHP ^8.3, SQLite, database sessions/cache/queue |
| App key, migrations run | `IMPLEMENTED` | users/cache/jobs tables exist |
| Health endpoint `/up` | `IMPLEMENTED` | registered in `bootstrap/app.php` |
| Flutter app (`apps/ecom_mob`) | `IMPLEMENTED` | default counter template, all platforms scaffolded |
| Turbo build caching (Vite + APK outputs) | `IMPLEMENTED` | `public/build/**`, `build/app/outputs/**` |

## Authentication & Users

| Feature | Status | Notes |
|---|---|---|
| `User` model | `PARTIALLY IMPLEMENTED` | Laravel default; hashed password cast, `name/email/password` |
| Registration API | `REQUIRED` | |
| Login / logout API | `REQUIRED` | |
| Token issuing (Sanctum or similar) | `REQUIRED` | Sanctum not in `composer.json` |
| Roles on users | `REQUIRED` | users table has no role column |
| Profile management | `REQUIRED` | |
| Email verification | `PLANNED` | interface stub present in `User.php` (commented) |

## Catalog

| Feature | Status | Notes |
|---|---|---|
| Category entity + CRUD | `REQUIRED` | |
| Product entity + CRUD | `REQUIRED` | |
| Product images | `REQUIRED` | storage link not yet created (`public/storage NOT LINKED`) |
| Price / discount fields | `REQUIRED` | |
| Product active/inactive | `REQUIRED` | |
| Product search | `REQUIRED` | |

## Inventory

| Feature | Status | Notes |
|---|---|---|
| Stock quantity on product | `REQUIRED` | |
| Stock decrement on order | `REQUIRED` | |
| Out-of-stock handling | `REQUIRED` | |

## Cart / Addresses / Checkout

| Feature | Status | Notes |
|---|---|---|
| Server-side cart | `REQUIRED` | |
| Add/update/remove cart items | `REQUIRED` | |
| Stock validation in cart | `REQUIRED` | |
| Address CRUD + default address | `REQUIRED` | |
| Order address snapshot | `REQUIRED` | |
| Checkout orchestration (re-price, re-validate) | `REQUIRED` | |

## Orders & Payments

| Feature | Status | Notes |
|---|---|---|
| Order + order items with price snapshots | `REQUIRED` | |
| Order status lifecycle | `REQUIRED` | lifecycle is proposed — see [order-lifecycle](../11-orders/order-lifecycle.md) |
| Order cancellation | `REQUIRED` | |
| Razorpay integration (create/verify/webhook) | `REQUIRED` | no razorpay package in `composer.json` |
| Refunds | `PLANNED` | |

## Applications (UI)

| Feature | Status | Notes |
|---|---|---|
| Flutter consumer screens (16 screens) | `REQUIRED` | only default `MyHomePage` counter exists |
| Admin console (dashboard, products, inventory, orders, customers) | `REQUIRED` | |
| Admin login/authorization | `REQUIRED` | |

## Non-Functional

| Feature | Status | Notes |
|---|---|---|
| Backend tests beyond examples | `REQUIRED` | only `ExampleTest` stubs |
| Widget tests beyond default | `REQUIRED` | only counter smoke test |
| CI pipeline | `PLANNED` | |
| Production deployment config | `TBD` | |

## Explicit FUTURE (not MVP)

CCTV service booking, technicians, SMS, WhatsApp, shipping integrations, reviews, coupons, loyalty, multi-vendor, AI support, advanced analytics — see [21-future-scope](../21-future-scope/README.md).
