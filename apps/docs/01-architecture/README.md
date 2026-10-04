# 01 — Architecture

Status legend: see [root README](../README.md).

| Document | Contents |
|---|---|
| [system-architecture.md](system-architecture.md) | System context, CURRENT vs TARGET MVP |
| [backend-architecture.md](backend-architecture.md) | Laravel app structure, layers, conventions |
| [frontend-architecture.md](frontend-architecture.md) | Flutter app + admin console structure |
| [data-flow.md](data-flow.md) | Purchase data flow, sequence diagrams |

## Summary

- **CURRENT (`IMPLEMENTED`):** two framework skeletons in a Turborepo — Laravel 13 (`apps/lara_ecom`) with SQLite and a Flutter 3.38 app (`apps/ecom_mob`). No API routes, no domain code, no UI beyond templates.
- **TARGET MVP (`REQUIRED`):** Laravel REST API (`/api/v1`) consumed by the Flutter consumer app and an admin web console; Razorpay for payments; SQLite for local dev, MySQL/PostgreSQL TBD for production.
