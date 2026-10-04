# Customer Management (Admin)

Status legend: see [root README](../README.md). All `REQUIRED`.

## Scope (MVP: view + block only)

| Capability | API | Notes |
|---|---|---|
| Customer list | `GET /admin/users` | name, email, phone (if set), status, joined, orders count; paginated + status filter + search |
| Customer detail | `GET /admin/users/{id}` | profile + order history summary (totals, statuses, last order) |
| Block / unblock | `PATCH /admin/users/{id}/status` | with confirm dialog; reason logged (TBD audit storage) |
| Edit customer profile | ❌ not in MVP | |
| Delete customer | ❌ not in MVP (privacy/audit — `FUTURE` review) | |

## Block semantics

Blocked user ([03-users/user-model.md](../03-users/user-model.md) lifecycle): login rejected; new checkout `403`; existing orders intact; pending-payment orders run their natural window. Unblock restores everything.

## Notes

- Customer's full address book is **not** shown (privacy) — only shipping snapshots inside orders ([14-admin/order-management.md](../14-admin/order-management.md)).
- Demo: the demo consumer appears here after placing the demo order ([20-mvp/demo-script.md](../20-mvp/demo-script.md) Act 3).
