# API Integration (Flutter ↔ Laravel)

Status legend: see [root README](../README.md). All `REQUIRED`.

## Client stack (TBD choices per [01-architecture/frontend-architecture.md](../01-architecture/frontend-architecture.md))

- HTTP: dio or http + interceptor injecting `Authorization: Bearer <token>`; base URL from env (`API_BASE_URL`, e.g. `http://10.0.2.2:8000/api/v1` for Android emulator).
- Serialization: JSON ↔ typed models (codegen TBD); money fields are **integer paise** — format client-side for display only.
- Token storage: secure storage; never plaintext prefs ([02-authentication/security.md](../02-authentication/security.md)).

## Error handling contract

| HTTP | Client action |
|---|---|
| 401 | clear token → Login screen (preserve intent) |
| 403 | "not allowed" message (shouldn't occur in normal flow) |
| 404 | entity-gone handling per screen |
| 409 | stock/status conflicts → refresh + inline fix |
| 422 | field/inline errors from `errors` body |
| 5xx / timeout | generic error + retry |

## Module → endpoint map (canonical source: [15-api](../15-api/README.md))

| Client feature | Endpoints |
|---|---|
| Auth | `/auth/register`, `/auth/login`, `/auth/logout`, `/auth/me` |
| Browse | `/categories`, `/products`, `/products/{slug}` |
| Cart | `/cart`, `/cart/items`, `/cart/items/{id}`, `/cart` |
| Address | `/addresses`, `/addresses/{id}`, `/addresses/{id}/default` |
| Checkout | `/checkout/quote`, `/checkout/order` |
| Payment | `/payments/verify`, `/orders/{id}/payment`, Razorpay SDK sheet |
| Orders | `/orders`, `/orders/{id}`, `/orders/{id}/cancel` |
| Profile | `/users/me` |

## Razorpay SDK integration points

1. `POST /checkout/order` response → feed `{key_id, order_id, amount, currency}` into the Flutter Razorpay plugin (package TBD — [12-payments/razorpay.md](../12-payments/razorpay.md)).
2. SDK success callback → POST `/payments/verify` with the triple.
3. SDK failure/dismiss → remain on Payment screen; order pending server-side.
4. App-kill during payment → webhook completes the order; next app-open shows true status ([12-payments/webhooks.md](../12-payments/webhooks.md)).
