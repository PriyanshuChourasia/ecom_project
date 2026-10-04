# Addresses API

Status legend: see [root README](../README.md). All `REQUIRED`. Bearer auth; own-scope only (`ADMIN` has no address CRUD of others in MVP — support views `PLANNED`).

| # | Method | URL | Purpose |
|---|---|---|---|
| 1 | GET | `/api/v1/addresses` | List own addresses (default first) |
| 2 | POST | `/api/v1/addresses` | Create address |
| 3 | PATCH | `/api/v1/addresses/{id}` | Update |
| 4 | DELETE | `/api/v1/addresses/{id}` | Delete |
| 5 | PATCH | `/api/v1/addresses/{id}/default` | Make default |

## 1) Response

```json
{
  "data": [
    { "id": 3, "label": "Home", "receiver_name": "Asha Kumar", "phone": "9876543210",
      "line1": "12 MG Road", "line2": "Near Metro", "city": "Bengaluru",
      "state": "Karnataka", "postal_code": "560001", "country": "India",
      "is_default": true }
  ]
}
```

## 2) Create — request

```json
{ "label": "Home", "receiver_name": "Asha Kumar", "phone": "9876543210",
  "line1": "12 MG Road", "line2": "Near Metro", "city": "Bengaluru",
  "state": "Karnataka", "postal_code": "560001", "is_default": true }
```
Validation: all required fields per [address-model.md](address-model.md); `postal_code`/`phone` pattern TBD; `is_default` optional bool. First-ever address becomes default even without the flag. Errors: `422`; foreign id → `404`.

## 4) Delete

`204`. Order snapshots unaffected (rule 4 in [business-rules.md](business-rules.md)). Deleting the default promotes oldest remaining (TBD confirm in [address-model.md](address-model.md)).

## 5) Default

`200` with the updated list. Clears previous default atomically.

Client usage: checkout address picker ([13-consumer-app/shopping-flow.md](../13-consumer-app/shopping-flow.md)).
