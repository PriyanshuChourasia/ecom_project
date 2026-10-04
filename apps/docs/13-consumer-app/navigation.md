# Navigation

Status legend: see [root README](../README.md). All `REQUIRED`; router package TBD ([01-architecture/frontend-architecture.md](../01-architecture/frontend-architecture.md)).

```mermaid
flowchart LR
    SPLASH[Splash] -->|"token valid"| HOME[Home]
    SPLASH -->|"no token"| LOGIN[Login]
    LOGIN --> HOME
    LOGIN --> REG[Register]
    REG --> HOME
    HOME --> CATS[Categories]
    HOME --> LIST[Product listing]
    HOME --> SRCH[Search]
    CATS --> LIST
    LIST --> PD[Product details]
    SRCH --> PD
    PD --> CART[Cart]
    CART --> ADDR[Address]
    ADDR --> CHK[Checkout]
    CHK --> PAY[Payment]
    PAY -->|"success"| CONF[Order confirmation]
    PAY -->|"failure"| PAY
    CONF --> ORDERS[My orders]
    CONF --> HOME
    HOME --> ORDERS
    ORDERS --> OD[Order details]
    HOME --> PROF[Profile]
    PROF --> ADDR
    PROF -->|"logout"| LOGIN
```

## Rules

1. **Auth guard** — Cart/Address/Checkout/Payment/Orders/Profile require a token; unauthenticated → Login (return-to-intent TBD at implementation).
2. **Checkout stack** — back from Payment→Checkout→Cart is allowed until payment succeeds; after success, back lands on Home/Orders (no re-entering a paid checkout).
3. Deep-link/product share (`ecom_mob` scheme) is `PLANNED`; only in-app navigation in MVP.
4. Bottom tabs suggested (Home/Categories/Cart/Profile) — exact IA TBD with design.
