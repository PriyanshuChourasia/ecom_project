# 04 — Roles & Permissions

Status legend: see [root README](../README.md).

| Document | Contents |
|---|---|
| [roles.md](roles.md) | Role definitions (MVP + extension slots) |
| [permissions.md](permissions.md) | Permission matrix per module |
| [access-control.md](access-control.md) | Enforcement model, middleware, rules |

## Status: everything here is `REQUIRED`

The users table has no role column; no role tables, middleware, or policies exist. The only entity today is the default `User` model.

MVP roles: **`CUSTOMER`** and **`ADMIN`** (sole seller persona). The design must stay extensible for future staff roles (fulfillment staff, support, content editor) without schema rework — but those roles are **FUTURE/PLANNED**, not MVP.

Canonical permission matrix: [permissions.md](permissions.md). API enforcement: [access-control.md](access-control.md).
