---
id: "003"
title: "entrevista-auth: API router, Mangum handler, integration tests"
milestone: M1
priority: P0
estimate: 3h
blockedBy: ["002"]
blocks: ["009", "016"]
parent: null
---

## Summary

Wire the auth domain services into a FastAPI application with Mangum adapter, define the API contract (request/response schemas), add auth middleware for downstream services, and write integration tests that exercise the full HTTP stack.

## Scope

**In scope:**
- `src/schemas/auth.py` — Pydantic models: `LoginRequest`, `TokenResponse`, `RefreshRequest`, `ChangePasswordRequest`
- `src/schemas/operator.py` — `OperatorCreate`, `OperatorResponse`
- `src/middleware/auth.py` — FastAPI dependency for JWT validation on protected endpoints
- `src/router.py` — `AuthRouter` with `/auth/login`, `/auth/refresh`, `/auth/logout`, `/auth/jwks`, `/operators` (admin CRUD)
- `src/app.py` — FastAPI app factory, middleware registration, CORS
- `src/handler.py` — Mangum ASGI adapter (Lambda entry point)
- Integration tests (`tests/integration/test_auth_endpoints.py`, `tests/integration/test_operator_endpoints.py`)

**Out of scope:** Terraform / deployment (task 009).

## Deliverables

| File | Description |
|------|-------------|
| `src/schemas/auth.py` | Request/response Pydantic models with validation |
| `src/schemas/operator.py` | Operator CRUD schemas |
| `src/middleware/auth.py` | `get_current_operator` FastAPI dependency — validates Bearer token, returns `Operator` |
| `src/router.py` | All auth and operator endpoints with HTTP status codes matching `business-logic-model.md` |
| `src/app.py` | App factory with lifespan (DB connect/close), CORS, exception handlers |
| `src/handler.py` | `handler = Mangum(app, lifespan="off")` |
| `tests/integration/test_auth_endpoints.py` | GWT tests: POST /auth/login happy path + wrong password + locked; POST /auth/refresh; POST /auth/logout; GET /auth/jwks |
| `tests/integration/test_operator_endpoints.py` | GWT tests: POST /operators (create), GET /operators/:id |

## Acceptance Criteria

- [ ] `POST /auth/login` with valid credentials returns `200` with `{ access_token, refresh_token, token_type: "bearer" }`.
- [ ] `POST /auth/login` with wrong password returns `401`.
- [ ] `POST /auth/login` after 5 failed attempts returns `423` (account locked).
- [ ] `POST /auth/refresh` with valid refresh token returns new `access_token`.
- [ ] `POST /auth/logout` with valid token returns `204`; subsequent refresh with same token returns `401`.
- [ ] `GET /auth/jwks` returns valid JWKS JSON with `{ keys: [{ kty, use, alg, kid, n, e }] }`.
- [ ] Protected endpoints return `401` without Bearer token and `403` for insufficient role.
- [ ] `make test` includes integration tests and all pass (uses in-memory MongoDB via `mongomock` or `motor` with test DB).

## Test Plan

```bash
make install
make test

# Verify the Lambda handler loads
python -c "from src.handler import handler; print('Mangum handler OK')"

# Spot-check endpoint schemas
python -m pytest tests/integration/test_auth_endpoints.py -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/functional-design/business-logic-model.md` | HTTP status codes for each flow state |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/infrastructure-design/infrastructure-design.md` | API Gateway route throttling, Lambda config |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-18: Authenticate Into Dashboard — Gherkin scenarios |

## Definition of Ready

- [ ] Task 002 is complete (AuthService, TokenManager, BruteForceProtector implemented and unit-tested).
- [ ] HTTP status codes for each error state confirmed from `business-logic-model.md`.
- [ ] Test DB strategy decided (mongomock or test container).
