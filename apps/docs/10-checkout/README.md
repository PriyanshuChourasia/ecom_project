# 10 — Checkout

Status legend: see [root README](../README.md). Everything `REQUIRED`.

| Document | Contents |
|---|---|
| [checkout-flow.md](checkout-flow.md) | The full orchestration, end to end |
| [pricing.md](pricing.md) | Where and how money is computed |
| [validation.md](validation.md) | Every check, failure code, recovery |
| [api.md](api.md) | Quote + order-prep endpoints |

## One-line definition

Checkout turns a server-priced cart + a selected address into a payment-backed order — with **all** validation and pricing re-done server-side at that moment. Upstream: cart ([08](../08-cart/README.md)), addresses ([09](../09-addresses/README.md)); downstream: payments ([12](../12-payments/README.md)), orders ([11](../11-orders/README.md)).
