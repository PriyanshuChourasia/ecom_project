# 16 — Database

Status legend: see [root README](../README.md).

| Document | Contents |
|---|---|
| [entities.md](entities.md) | Every entity: fields, keys, audit, status |
| [relationships.md](relationships.md) | Cardinalities, scoping rules |
| [indexes.md](indexes.md) | Index plan per table |
| [erd.md](erd.md) | Mermaid ER diagram |

## Current state

`IMPLEMENTED` tables: `users` (+password_reset_tokens, sessions), `cache`, `jobs` (Laravel defaults). Engine: SQLite (dev).

Every e-commerce entity below is `REQUIRED` — documented as the migration blueprint. Names are used consistently across all docs (single-terminology rule).

| MVP entity | Status |
|---|---|
| User | `PARTIALLY IMPLEMENTED` (exists; needs phone/status/roles) |
| Role (+role_user) | `REQUIRED` |
| Category, Product, ProductImage | `REQUIRED` |
| Inventory (→ stock on Product) | `REQUIRED` (mapped design — see entities note) |
| Address, Cart, CartItem | `REQUIRED` |
| Order, OrderItem, Payment | `REQUIRED` |
| Permission | `PLANNED` (roles suffice for MVP; extensibility via named abilities documented in [04](../04-roles-permissions/README.md)) |
