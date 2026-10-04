# Admin Dashboard

Status legend: see [root README](../README.md). All `REQUIRED`.

## Purpose

Seller's landing screen after login: the store's pulse for today, oriented around the demo loop (products on sale → orders to process).

## Data blocks

| Block | Content | Backing data (endpoint TBD inside admin routes) |
|---|---|---|
| New orders | count of `PAID`/`CONFIRMED` awaiting action | orders by status |
| Pending payment | `PENDING_PAYMENT` within window | orders by status |
| Low/out-of-stock | products at/below threshold (threshold TBD) | products.stock_quantity |
| Catalog size | total/active/inactive products | products by status |
| Recent orders | last 10 with buyer + total + status | orders latest |
| Quick actions | → Add product, → Orders, → Inventory | navigation |

## States

- Loading: skeletons.
- Empty store: onboarding CTA "Create your first product" (the demo starts here — [20-mvp/demo-script.md](../20-mvp/demo-script.md)).
- Error: retry banner.

Non-goals: charts/analytics beyond simple counts are FUTURE ([21-future-scope/advanced-features.md](../21-future-scope/advanced-features.md)).
