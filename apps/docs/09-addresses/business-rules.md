# Address Business Rules

Status legend: see [root README](../README.md). All `REQUIRED`.

1. **Own-scope only** — a user sees/edits/deletes only own addresses; foreign ids yield `404`, never `403` (avoid existence leaks; consistent with [04-roles-permissions/access-control.md](../04-roles-permissions/access-control.md)).
2. **Default invariants** — at most one `is_default=true` per user; creation auto-defaults the first; switching is atomic.
3. **Checkout requires a valid owned address** — checkout with missing/foreign/unknown `address_id` → `422`/`404` per [10-checkout/validation.md](../10-checkout/validation.md).
4. **Snapshot at order time (critical)** — the order stores a full copy of receiver/phone/lines/city/state/postal/country ([order-model.md](../11-orders/order-model.md)). Later address edits/deletions must never mutate past orders. No live relation survives checkout.
5. **Label uniqueness is soft** — duplicate labels allowed; the UI disambiguates with city/line1. (TBD: enforce uniqueness per user? Default no.)
6. **Country scope** — MVP accepts only "India" (seeded default); non-IN postal flows are FUTURE.
7. **Phone format** — stored raw; validation pattern TBD (suggest 10-digit IN without country code; display with +91).
