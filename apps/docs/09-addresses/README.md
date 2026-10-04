# 09 — Addresses

Status legend: see [root README](../README.md). Everything `REQUIRED`.

| Document | Contents |
|---|---|
| [address-model.md](address-model.md) | Entity fields, default behavior |
| [api.md](api.md) | Address endpoints |
| [business-rules.md](business-rules.md) | Defaults, snapshots, deletion rules |

Addresses belong to customers, are selectable at checkout, and are **copied (snapshotted)** onto orders so historical orders remain accurate when users edit/delete addresses. Related: [10-checkout](../10-checkout/README.md), [11-orders/order-model.md](../11-orders/order-model.md).
