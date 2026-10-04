# Authentication Security

Status legend: see [root README](../README.md).

| Concern | Status | Notes |
|---|---|---|
| bcrypt hashing, `hashed` cast | `IMPLEMENTED` (env: `BCRYPT_ROUNDS=12`) | Laravel default |
| Credentials never logged | `REQUIRED` | policy + code review gate |
| Token storage on device | `REQUIRED` | secure storage (keystore/keychain), never plaintext prefs — Flutter side |
| HTTPS-only transport | `REQUIRED` | everywhere in prod; local dev may use http |
| Rate limit login/register | `REQUIRED` | Laravel throttling, config TBD |
| Anti-enumeration | `REQUIRED` | login errors must not reveal whether email exists (pairs with TBD convention in [authentication-flow.md](authentication-flow.md)) |
| Mass-assignment protection | `IMPLEMENTED` (fillable on `User`) | keep minimal fillables when extending roles |
| Role escalation via register API | `REQUIRED` to prevent | `/auth/register` must hardcode role `CUSTOMER` |

Deep coverage (payment security, env vars, sanitization) lives in [17-security](../17-security/README.md).
