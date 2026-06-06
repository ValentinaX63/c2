---
id: "002"
title: "entrevista-auth: AuthService, TokenManager, BruteForceProtector"
milestone: M1
priority: P0
estimate: 4h
blockedBy: ["001"]
blocks: ["003"]
parent: null
---

## Summary

Implement the three core domain services for `entrevista-auth`, following the flows in `business-logic-model.md` and enforcing all rules in `business-rules.md`. Each service must be independently unit-testable.

## Scope

**In scope:**
- `src/brute_force.py` — `BruteForceProtector`: in-memory counter with TTL (RULE-AUTH-01)
- `src/token.py` — `TokenManager`: RS256 JWT sign/verify, JWKS endpoint data, refresh token hash, revocation check
- `src/auth_service.py` — `AuthService`: login, refresh, logout, change-password flows
- `src/audit_logger.py` — structured JSON audit events to stdout
- Unit tests for all three services (`tests/unit/`)

**Out of scope:** FastAPI routes, Mangum handler (task 003).

## Deliverables

| File | Description |
|------|-------------|
| `src/brute_force.py` | `BruteForceProtector(max_attempts, window_minutes)` with `record_failed_attempt(email)`, `is_locked(email)`, `reset(email)` |
| `src/token.py` | `TokenManager` with `create_access_token(sub, tenant_id, role)`, `create_refresh_token(operator_id)`, `verify_access_token(token)`, `get_jwks()`, `hash_token(token)` |
| `src/auth_service.py` | `AuthService.login(email, password)`, `.refresh(refresh_token)`, `.logout(operator_id)`, `.change_password(operator_id, old_pw, new_pw)` |
| `src/audit_logger.py` | `AuditLogger.log(event_type, operator_id, tenant_id, **kwargs)` → JSON to stdout |
| `tests/unit/test_brute_force.py` | AAA tests: counter increments, lockout triggers at threshold, lockout clears after window |
| `tests/unit/test_token_manager.py` | AAA tests: token creation, verification, expiry, JWKS structure |
| `tests/unit/test_auth_service.py` | GWT tests: login happy path, wrong password, locked account, refresh, logout, change-password |

## Acceptance Criteria

- [ ] `BruteForceProtector` locks after exactly 5 failed attempts within 15 minutes (RULE-AUTH-01).
- [ ] `TokenManager.create_access_token()` returns RS256-signed JWT with claims: `sub`, `tenant_id`, `role`, `exp`, `jti`.
- [ ] `TokenManager.verify_access_token()` raises on expired token, invalid signature, and revoked `jti`.
- [ ] `AuthService.login()` returns `(access_token, refresh_token)` on valid credentials.
- [ ] `AuthService.login()` raises `AuthenticationError` on wrong password and increments brute-force counter.
- [ ] `AuthService.login()` raises `AccountLockedError` when `BruteForceProtector.is_locked()` returns `True`.
- [ ] `AuthService.logout()` marks all refresh tokens for the operator as revoked (RULE-AUTH-03).
- [ ] `AuditLogger.log()` writes a JSON line to stdout with `event_type`, `operator_id`, `timestamp`.
- [ ] `make test` runs all unit tests and passes.

## Test Plan

```bash
make install
make test  # should show all unit tests passing

# Spot-check GWT test for lockout
python -m pytest tests/unit/test_auth_service.py -k "lockout" -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/functional-design/business-rules.md` | RULE-AUTH-01 through RULE-AUTH-06 |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/functional-design/business-logic-model.md` | Login, Refresh, Logout, Change Password flows step by step |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/nfr-design/nfr-design-patterns.md` | ADR-U1-02 (RS256 + Secrets Manager), ADR-U1-03 (rate limiting) |

## Definition of Ready

- [ ] Task 001 is complete (`src/models/`, `src/config.py`, `src/secrets.py`, `src/db/mongo.py` exist).
- [ ] `business-rules.md` read: RULE-AUTH-01 (lockout threshold and window), RULE-AUTH-02 (token expiry), RULE-AUTH-03 (logout revocation) confirmed.
- [ ] `business-logic-model.md` read: all login steps and error states understood.
