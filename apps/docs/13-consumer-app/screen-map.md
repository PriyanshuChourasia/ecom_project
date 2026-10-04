# Screen Map

Status legend: see [root README](../README.md). All screens `REQUIRED`. Shared conventions below the table.

| # | Screen | Purpose | Data required | API required | Key user actions | Navigation |
|---|---|---|---|---|---|---|
| 1 | Splash | brand + session bootstrap | stored token validity | `GET /auth/me` if token | none (auto) | → Login or Home |
| 2 | Login | credential entry | email, password | `POST /auth/login` | submit, forgot pw, go register | → Home |
| 3 | Register | account creation | name, email, password | `POST /auth/register` | submit | → Home (auto-login) |
| 4 | Home | entry surface | categories, featured/new products | `GET /categories`, `GET /products` | tap category/product, search | → Category, Product, Search |
| 5 | Categories | browse taxonomy | category tree (1 level) | `GET /categories` | tap category | → Product listing |
| 6 | Product listing | grid of products | paginated products + filters | `GET /products` | filter/sort/paginate, tap product | → Product details |
| 7 | Product details | full product view | product + images + stock | `GET /products/{slug}` | qty picker, add to cart | → Cart |
| 8 | Search | find products | query results | `GET /products?q=` | type, open result | → Product details |
| 9 | Cart | review selection | cart with live totals | `GET/POST/PATCH/DELETE /cart*` | change qty, remove, clear, checkout | → Address, → Product details |
| 10 | Address | manage addresses | address list | `GET/POST/PATCH/DELETE /addresses*` | add/edit/delete, pick default | → Checkout |
| 11 | Checkout | review order | quote + chosen address | `POST /checkout/quote` | review totals, confirm | → Payment |
| 12 | Payment | pay via Razorpay | rzp params | `POST /checkout/order`, `POST /payments/verify` | pay, retry on fail | → Order confirmation |
| 13 | Order confirmation | success receipt | order summary | `GET /orders/{id}` | view order, continue shopping | → My orders, → Home |
| 14 | My orders | order history | paginated orders | `GET /orders` | tap order, cancel (window) | → Order details |
| 15 | Order details | single order | order + items + status | `GET /orders/{id}` | cancel (window) | → My orders |
| 16 | Profile | account settings | profile | `GET/PATCH /users/me`, `POST /auth/logout` | edit profile, logout, addresses | → Address, → Login |

## Shared screen states (apply to every screen)

| State | Display | Source |
|---|---|---|
| Loading | skeleton/spinner; buttons disabled | in-flight request |
| Empty | contextual message + action (e.g. "Cart is empty → Start shopping") | server empty payload |
| Error | message + retry; auth errors redirect to Login | 4xx/5xx mapping per [api-integration.md](api-integration.md) |

Screen-specific empties worth calling out: Product listing (no results → clear-filters CTA), Cart (empty → Home CTA), My orders (no orders → browse CTA), Address (none → add-first-address CTA, required before checkout [09-addresses](../09-addresses/README.md)).
