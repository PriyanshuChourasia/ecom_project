# Business Requirements

Status legend: see [root README](../README.md).

## Business Objective

`IMPLEMENTED` (as a goal) — Launch a working e-commerce MVP that demonstrates the complete purchase cycle end to end: a seller lists physical products, consumers discover and buy them, and the seller receives and manages the resulting orders.

The MVP is a **demo-ready vertical slice**, not a full production platform. It must be provable in a live demo from product creation to order management.

## Target Users

### Consumer (`REQUIRED`)

- Browses products by category, searches, views product details
- Adds products to a cart, manages quantities
- Checks out with a saved/entered address
- Pays online (Razorpay)
- Views order history and order status

Platform: **Flutter mobile app** (`apps/ecom_mob`) — currently a skeleton, `REQUIRED`.

### Seller / Admin (`REQUIRED`)

- Creates and edits products, uploads images
- Manages prices, stock, product activation
- Views incoming orders and customer/order information
- Updates order status through fulfillment

Platform: **web admin console** on the Laravel backend — `REQUIRED` (does not exist yet).

## Core Shopping Flow (Consumer) — `REQUIRED`

```
Browse products → View product → Add to cart → Checkout → Payment → Place order
```

## Product-Selling Flow (Seller) — `REQUIRED`

```
Create product → Publish/activate → Manage inventory → Receive orders → Update order status
```

## Non-Goals (for the current demo)

CCTV service booking, technician management, SMS/WhatsApp, advanced SEO, and marketing automation are **FUTURE** — see [21-future-scope](../21-future-scope/README.md). CCTV hardware may still be sold as a normal physical product (see [Products](../06-products/README.md)).

## Cross-references

- MVP boundaries: [mvp-scope.md](mvp-scope.md)
- Consumer screens: [13-consumer-app](../13-consumer-app/README.md)
- Seller workflow: [14-admin](../14-admin/README.md)
