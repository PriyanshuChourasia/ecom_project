# 03 — Users

Status legend: see [root README](../README.md).

| Document | Contents |
|---|---|
| [user-model.md](user-model.md) | Entity, fields, lifecycle, relationships |
| [customer-flow.md](customer-flow.md) | Customer journey states |
| [api.md](api.md) | User endpoints |

## Status

| Capability | Status |
|---|---|
| `User` model (`name`, `email`, `password`) | `PARTIALLY IMPLEMENTED` (Laravel default) |
| Role linkage (see [04](../04-roles-permissions/README.md)) | `REQUIRED` |
| Profile view/update endpoints | `REQUIRED` |
| Admin customer listing | `REQUIRED` |

Model ownership: [16-database/entities.md](../16-database/entities.md) documents the `User` entity table.
