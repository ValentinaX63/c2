---
id: "004"
title: "entrevista-compliance: project scaffold, ConsentManager, AuditLogger"
milestone: M1
priority: P0
estimate: 4h
blockedBy: []
blocks: ["005"]
parent: null
---

## Summary

Bootstrap `entrevista-compliance` and implement the write-once consent recording and immutable audit log. Both components are critical for GDPR/privacy compliance and must enforce append-only writes.

## Scope

**In scope:**
- Project scaffold (pyproject.toml, Makefile, .env.example, README.md)
- `src/models/consent.py` — ConsentRecord model with `recorded_at`, `candidate_id`, `campaign_id`, `consent_text_hash`
- `src/models/audit_event.py` — AuditEvent model with event_type, actor, resource, timestamp, immutable flag
- `src/consent.py` — `ConsentManager`: record consent, verify consent exists, reject duplicate for same candidate+campaign
- `src/audit.py` — `AuditLogger`: append-only write, query by candidate/campaign/event_type, no update/delete operations
- Unit tests for ConsentManager and AuditLogger

**Out of scope:** Escalation, NPS, retention (task 005).

## Acceptance Criteria

- [ ] `ConsentManager.record()` creates a consent record; calling it again for the same `(candidate_id, campaign_id)` raises `ConsentAlreadyRecordedError`.
- [ ] `AuditLogger.append()` inserts a new document; no update or delete method exists on the class.
- [ ] Consent records have a `recorded_at` timestamp that cannot be modified after creation.
- [ ] `make test` passes all unit tests.

## Test Plan

```bash
make install && make test
python -m pytest tests/unit/test_consent.py tests/unit/test_audit.py -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-03, US-23, US-24 |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/requirements/requirements.md` | Privacy and consent requirements |

## Definition of Ready

- [ ] Consent and audit models understood from user stories.
- [ ] Append-only constraint strategy confirmed (MongoDB: no updateOne/deleteOne on consent/audit collections).
