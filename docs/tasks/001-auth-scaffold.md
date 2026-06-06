---
id: "001"
title: "entrevista-auth: project scaffold and domain models"
milestone: M1
priority: P0
estimate: 3h
blockedBy: []
blocks: ["002"]
parent: null
---

## Summary

Bootstrap the `entrevista-auth` repository with the project structure, configuration, database layer, and domain models required by subsequent tasks. All file paths and model names must follow the language in `domain-entities.md`.

## Scope

**In scope:**
- Repository scaffold (`pyproject.toml`, `Makefile`, `.env.example`, `README.md`)
- `config.py` — Pydantic Settings loading env vars and Secrets Manager references
- `secrets.py` — lazy-loaded AWS Secrets Manager cache (RS256 private/public keys, MongoDB URI)
- `db/mongo.py` — Motor async client with `maxPoolSize=1`, TTL index on `refresh_tokens.expires_at`
- Domain models: `models/operator.py` (`Operator`, `HashedPassword` value object) and `models/token.py` (`RefreshToken`, `RevokedToken`)

**Out of scope:** Services, routes, tests (covered in tasks 002 and 003).

## Deliverables

| File | Description |
|------|-------------|
| `pyproject.toml` | uv-managed dependencies: fastapi, mangum, motor, pydantic-settings, argon2-cffi, python-jose[cryptography], boto3 |
| `Makefile` | Targets: `install`, `test`, `lint`, `build` |
| `.env.example` | All required env vars documented |
| `src/config.py` | Settings class: `MONGO_URI`, `AWS_REGION`, `SECRET_NAME_RS256_PRIVATE`, `SECRET_NAME_RS256_PUBLIC`, `JWT_ALGORITHM=RS256`, `ACCESS_TOKEN_EXPIRE_MINUTES=15`, `REFRESH_TOKEN_EXPIRE_DAYS=7`, `BRUTE_FORCE_MAX_ATTEMPTS=5`, `BRUTE_FORCE_WINDOW_MINUTES=15` |
| `src/secrets.py` | `SecretsManager` with lazy get + in-memory cache per Lambda instance |
| `src/db/mongo.py` | `get_db()` async motor client, TTL index setup, `close_db()` |
| `src/models/operator.py` | `Operator` dataclass with `activate()`, `deactivate()`, `update_password()` |
| `src/models/token.py` | `RefreshToken` with `revoke()`, `is_expired()`; `RevokedToken` |

## Acceptance Criteria

- [ ] `make install` completes without errors in a clean Python 3.12 virtualenv.
- [ ] `from src.models.operator import Operator` imports without error; `Operator` has all attributes from `domain-entities.md`.
- [ ] `from src.models.token import RefreshToken` imports without error; `RefreshToken.is_expired()` returns `True` when `expires_at < now`.
- [ ] `src/config.py` loads settings from environment without crashing when `MONGO_URI` is set.
- [ ] All model field names match the language in `domain-entities.md` exactly (e.g., `operator_id`, `hashed_password`, `jti`).
- [ ] `.env.example` documents every variable consumed by `config.py`.

## Test Plan

```bash
make install
python -c "from src.models.operator import Operator; print('OK')"
python -c "from src.models.token import RefreshToken; import datetime; t = RefreshToken(jti='x', operator_id='y', token_hash='h', expires_at=datetime.datetime(2000,1,1), is_revoked=False); print(t.is_expired())"
```

Expected: second command prints `OK`, third prints `True`.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/functional-design/domain-entities.md` | Entity/VO definitions, field names |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/nfr-requirements/nfr-requirements.md` | bcrypt factor 10, RS256, TTL index |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/nfr-design/nfr-design-patterns.md` | Secrets Manager pattern, JWT strategy |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/application-design/unit-of-work.md` | Repository structure for Unit 6 |

## Definition of Ready

- [ ] `domain-entities.md` read and understood.
- [ ] `nfr-requirements.md` read (bcrypt, JWT, Secrets Manager constraints confirmed).
- [ ] Python 3.12 + uv available in the execution environment.
- [ ] AWS credentials with `secretsmanager:GetSecretValue` available (or mocked for unit tests).
