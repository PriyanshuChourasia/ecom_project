# ERD

Status legend: see [root README](../README.md). MVP schema target — all `REQUIRED` except User (`PARTIALLY IMPLEMENTED`). Inventory = stock column on Product; Permission table = `PLANNED`.

```mermaid
erDiagram
    USER ||--o{ ROLE : "role_user (M:N)"
    USER ||--o{ ADDRESS : owns
    USER ||--|| CART : "one active"
    USER ||--o{ ORDER : places

    CART ||--o{ CART_ITEM : contains
    PRODUCT ||--o{ CART_ITEM : "in carts"

    CATEGORY ||--o{ PRODUCT : contains
    CATEGORY ||--o{ CATEGORY : "parent_id (depth<=1)"
    PRODUCT ||--o{ PRODUCT_IMAGE : has
    PRODUCT ||--o{ ORDER_ITEM : "snapshotted in"

    ORDER ||--o{ ORDER_ITEM : contains
    ORDER ||--|| PAYMENT : "one active"
    ORDER ||--o| ADDRESS : "snapshot copy (no FK)"

    ORDER_ITEM }o--|| PRODUCT : references

    USER {
        bigint id PK
        string name
        string email UK
        string password "hashed"
        string phone "REQUIRED"
        enum status "REQUIRED"
        timestamps timestamps
    }
    ROLE {
        bigint id PK
        string name UK "CUSTOMER|ADMIN"
    }
    CATEGORY {
        bigint id PK
        string name
        string slug UK
        bigint parent_id FK "nullable"
        boolean is_active
    }
    PRODUCT {
        bigint id PK
        bigint category_id FK
        string sku UK
        string name
        string brand "nullable"
        text description
        int price "paise"
        int discount_percent
        int tax_percent
        int stock_quantity "MVP inventory"
        enum status "DRAFT|ACTIVE|INACTIVE"
        json specs "nullable"
    }
    PRODUCT_IMAGE {
        bigint id PK
        bigint product_id FK
        string path
        int sort_order
    }
    ADDRESS {
        bigint id PK
        bigint user_id FK
        string label
        string receiver_name
        string phone
        string line1
        string line2 "nullable"
        string city
        string state
        string postal_code
        string country
        boolean is_default
    }
    CART {
        bigint id PK
        bigint user_id FK "unique"
    }
    CART_ITEM {
        bigint id PK
        bigint cart_id FK
        bigint product_id FK
        int quantity
        int price_at_add "hint only"
    }
    ORDER {
        bigint id PK
        string number UK
        bigint user_id FK
        enum status
        string ship_name "snapshot"
        string ship_phone "snapshot"
        string ship_line1 "snapshot"
        string ship_city "snapshot"
        string ship_state "snapshot"
        string ship_postal_code "snapshot"
        int subtotal "paise"
        int grand_total "paise"
        string currency "INR"
        timestamp placed_at
        timestamp cancelled_at "nullable"
    }
    ORDER_ITEM {
        bigint id PK
        bigint order_id FK
        bigint product_id FK
        string sku "snapshot"
        string product_name "snapshot"
        int unit_price "snapshot paise"
        int quantity
        int line_total "snapshot paise"
    }
    PAYMENT {
        bigint id PK
        bigint order_id FK
        string provider "razorpay"
        string provider_order_id UK
        string provider_payment_id UK "nullable"
        int amount "paise"
        enum status "CREATED|INITIATED|CAPTURED|FAILED"
        string method "nullable"
        timestamp verified_at "nullable"
    }
```

Reading notes: dashed intent (snapshot on address) is represented by the "snapshot copy" label — order holds copied fields, not a live relation. Permission entity intentionally omitted (`PLANNED`, [entities.md](entities.md)).
