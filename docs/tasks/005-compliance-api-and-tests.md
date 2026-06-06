---
id: "005"
title: "entrevista-compliance: API router, escalation, retention, integration tests"
milestone: M1
priority: P0
estimate: 3h
blockedBy: ["004"]
blocks: ["009", "010"]
parent: null
---

## Summary

Complete `entrevista-compliance` with the escalation alert manager, NPS collector, data retention scheduler, FastAPI router, Mangum handler, and integration tests.

## Scope

**In scope:**
- `src/escalation.py` — EscalationAlertManager
- `src/nps.py` — NPSCollector
- `src/retention.py` — DataRetentionManager with `sweep()` (deletes expired consent/session data)
- `src/scheduled_handler.py` — EventBridge entry point for daily retention sweep
- `src/router.py`, `src/app.py`, `src/handler.py`
- Integration tests for all endpoints

## Acceptance Criteria

- [ ] `POST /compliance/consent` records consent; duplicate returns `409`.
- [ ] `GET /compliance/audit` returns audit events for a candidate filtered by `campaign_id`.
- [ ] `POST /compliance/escalation` creates an escalation alert.
- [ ] `POST /compliance/nps` records NPS score (1-10).
- [ ] `DataRetentionManager.sweep()` deletes records past their retention period without touching active records.
- [ ] `make test` passes.

## Test Plan

```bash
make install && make test
python -m pytest tests/integration/ -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-23, US-24, US-25, US-33 |

## Definition of Ready

- [ ] Task 004 complete (models, ConsentManager, AuditLogger).
