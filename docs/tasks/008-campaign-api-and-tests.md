---
id: "008"
title: "entrevista-campaign: API router, RAG search endpoint, integration tests"
milestone: M1
priority: P0
estimate: 3h
blockedBy: ["007"]
blocks: ["009", "012"]
parent: null
---

## Summary

Wire campaign, rubric, knowledge base, and RAG services into the FastAPI router with Mangum handler. Add integration tests covering all endpoints.

## Acceptance Criteria

- [ ] `POST /campaigns` creates campaign; returns `201` with `campaign_id`.
- [ ] `POST /campaigns/{id}/documents` triggers ingestion pipeline; returns `202 Accepted`.
- [ ] `POST /campaigns/{id}/rag-search` returns top-k chunks for a query.
- [ ] `GET /campaigns/{id}/telegram-link` returns Telegram deep link.
- [ ] All endpoints require valid Bearer token (auth middleware from task 003 pattern).
- [ ] `make test` passes.

## Test Plan

```bash
make install && make test
python -m pytest tests/integration/ -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-13, US-19, US-20, US-21, US-22 |

## Definition of Ready

- [ ] Tasks 006 and 007 complete.
