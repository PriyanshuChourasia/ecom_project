# Shopping Flow

Status legend: see [root README](../README.md). All `REQUIRED`.

The demo path through the UI (mirrors [00-project-overview/mvp-scope.md](../00-project-overview/mvp-scope.md)):

| Step | Screen | What happens | Success signal |
|---|---|---|---|
| 1 | Home/Categories | browse seeded catalog | products visible |
| 2 | Product listing | filter/search, open product | detail opens |
| 3 | Product details | pick qty → **Add to cart** | cart badge count +1 |
| 4 | Cart | review server totals, adjust | totals match backend |
| 5 | Address | pick default / add address | address selected |
| 6 | Checkout | quote reviewed (items, totals, address) | confirm enabled |
| 7 | Payment | Razorpay sheet → pay | verify → confirmation |
| 8 | Order confirmation | order number + status `PAID` | order id shown |
| 9 | My orders | order visible with status | status trackable |

## Failure paths in the UI

| Failure | UI behavior | Doc |
|---|---|---|
| Add-to-cart 404/422 | toast (product gone / invalid) + refresh listing | [08-cart/api.md](../08-cart/api.md) |
| Quote `409` stock | per-item availability warning; adjust-qty shortcuts | [10-checkout/validation.md](../10-checkout/validation.md) |
| Pay fail/dismiss | stay on Payment with retry + order stays `PENDING_PAYMENT` | [12-payments/payment-flow.md](../12-payments/payment-flow.md) |
| Session expiry | redirect Login, cart intact server-side | [02-authentication](../02-authentication/README.md) |

## Cart badge rule

Badge = `item_count` from `GET /cart`; refreshed on: app resume, add/update/remove, order completion. Server counts, client displays.
