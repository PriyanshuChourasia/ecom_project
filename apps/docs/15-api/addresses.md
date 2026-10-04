# API — Addresses

Status: all `REQUIRED`. Canonical detail: [09-addresses/api.md](../09-addresses/api.md).

| Method | URL | Auth | Purpose |
|---|---|---|---|
| GET | `/api/v1/addresses` | Bearer | own addresses (default first) |
| POST | `/api/v1/addresses` | Bearer | create (first auto-defaults) |
| PATCH | `/api/v1/addresses/{id}` | Bearer | update |
| DELETE | `/api/v1/addresses/{id}` | Bearer | delete (snapshots unaffected) |
| PATCH | `/api/v1/addresses/{id}/default` | Bearer | switch default (atomic) |

Rules: own-scope (foreign → 404); single-default invariant; country "India" only in MVP ([09-addresses/business-rules.md](../09-addresses/business-rules.md)).
