# Category Business Rules

Status legend: see [root README](../README.md). All `REQUIRED`.

1. **Activation** — deactivated categories disappear from public listing (customers get 404 on slug), while admin tooling still sees them.
2. **Deactivation is not deletion** — products keep their `category_id`; a deactivated category's products:
   - remain live and reachable via direct link/search,
   - still appear in category-filtered admin reports,
   - become unreachable from public category browsing.
   - Customer-facing product cards show category naming as-is (TBD: hide category chip when inactive).
3. **Deletion guard** — delete only if zero products reference it; otherwise `409 Conflict` with product count. (Move-products-then-delete is the admin workflow.)
4. **Slug immutability** — once a category has products, its `slug` never changes to protect client deep-links; rename changes `name` only.
5. **Depth cap** — `parent_id` must reference a top-level category; nesting beyond one level rejected with `422`.
6. **Seeded demo taxonomy** — Stationery, Electronics, Laptops, Desktops, CCTV seeded by seeder at setup (see [19-deployment/local-development.md](../19-deployment/local-development.md)); CCTV is hardware-only ([06-products/README.md](../06-products/README.md)).
