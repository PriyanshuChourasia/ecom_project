# Entities

Status legend: see [root README](../README.md). Money = integer paise everywhere. Timestamps/audit on all tables.

## User — `PARTIALLY IMPLEMENTED`
Exists (users table). Fields: id PK · name ✅ · email ✅ unique · email_verified_at ✅ · password ✅ (hashed) · remember_token ✅ · `phone` ❌ REQUIRED (nullable) · `status` ❌ REQUIRED (enum active/blocked, default active) · timestamps ✅.
Detail: [03-users/user-model.md](../03-users/user-model.md).

## Role — `REQUIRED`
id PK · name (unique: `CUSTOMER`, `ADMIN`) · description nullable · timestamps. Pivot `role_user` (user_id, role_id, unique pair).
Detail: [04-roles-permissions/roles.md](../04-roles-permissions/roles.md).

## Permission — `PLANNED`
Not an MVP table. Named abilities live in code/policies; a `permissions` table + `permission_role` pivot is the post-MVP extension path ([04-roles-permissions/permissions.md](../04-roles-permissions/permissions.md)).

## Category — `REQUIRED`
id PK · name · slug unique · parent_id FK self nullable (depth ≤ 1) · description nullable · is_active bool default true · timestamps.
Detail: [05-categories/category-model.md](../05-categories/category-model.md).

## Product — `REQUIRED`
id PK · category_id FK (required) · sku unique · name · brand nullable · description text · price int (paise) · discount_percent int 0–100 default 0 · tax_percent int 0–100 default 0 · stock_quantity int ≥ 0 (**this is the MVP "Inventory"**) · status enum DRAFT/ACTIVE/INACTIVE default DRAFT · specs json nullable · deleted_at nullable (soft-delete TBD) · timestamps.
Detail: [06-products/product-model.md](../06-products/product-model.md), stock semantics [07-inventory/inventory-model.md](../07-inventory/inventory-model.md).

## ProductImage — `REQUIRED`
id PK · product_id FK · path · sort_order int default 0 · timestamps. (No unique constraint; sort 0 = primary by convention.)
Detail: [06-products/product-images.md](../06-products/product-images.md).

## Inventory — mapped
Not a separate MVP table: `products.stock_quantity` is the inventory. A future `inventories`/`stock_ledger` table is `PLANNED` ([07-inventory/inventory-model.md](../07-inventory/inventory-model.md)).

## Address — `REQUIRED`
id PK · user_id FK · label · receiver_name · phone · line1 · line2 nullable · city · state · postal_code · country default "India" · is_default bool · timestamps. (Snapshot fields on Order are copies, not FKs.)
Detail: [09-addresses/address-model.md](../09-addresses/address-model.md).

## Cart — `REQUIRED`
id PK · user_id FK **unique** (one active cart) · timestamps.
Detail: [08-cart/cart-model.md](../08-cart/cart-model.md).

## CartItem — `REQUIRED`
id PK · cart_id FK · product_id FK · quantity int ≥1 · price_at_add int (hint only) · timestamps · unique(cart_id, product_id).
Detail: [08-cart/cart-model.md](../08-cart/cart-model.md).

## Order — `REQUIRED`
id PK · number unique (`ORD-YYYY-<id>`) · user_id FK · status enum (PENDING_PAYMENT/PAID/CONFIRMED/PROCESSING/SHIPPED/DELIVERED/COMPLETED/PAYMENT_FAILED/CANCELLED) · ship_name · ship_phone · ship_line1 · ship_line2 nullable · ship_city · ship_state · ship_postal_code · ship_country · subtotal · tax_total · shipping_total · grand_total (all int paise) · currency default INR · placed_at · cancelled_at nullable · cancellation_reason nullable · timestamps.
Detail: [11-orders/order-model.md](../11-orders/order-model.md), lifecycle [11-orders/order-lifecycle.md](../11-orders/order-lifecycle.md).

## OrderItem — `REQUIRED`
id PK · order_id FK · product_id FK · sku · product_name · unit_price · quantity · line_total (all snapshots; int paise) · timestamps.
Detail: [11-orders/order-model.md](../11-orders/order-model.md).

## Payment — `REQUIRED`
id PK · order_id FK · provider default "razorpay" · provider_order_id unique · provider_payment_id nullable unique · amount int · currency · status enum CREATED/INITIATED/CAPTURED/FAILED (REFUNDED planned) · method nullable · error_code nullable · error_description nullable · verified_at nullable · timestamps.
Detail: [12-payments/payment-model.md](../12-payments/payment-model.md).

## Status & audit field conventions

- **Status fields** carry enum values documented in their module (product status, order status, payment status) — changed only through their state machines.
- **Audit fields**: `created_at`/`updated_at` everywhere; optional `soft deletes` only on Product (TBD); user-level attribution for stock ops is response-logged (ledger `PLANNED`).
