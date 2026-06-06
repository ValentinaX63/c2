---
id: "011"
title: "entrevista-evaluation: API router, integration tests, Claude scoring prompts"
milestone: M2
priority: P0
estimate: 4h
blockedBy: ["010"]
blocks: ["012", "016"]
parent: null
---

## Summary

Wire evaluation services into FastAPI + Mangum. Tune Claude scoring prompts with a golden test set. Add integration tests.

## Acceptance Criteria

- [ ] `POST /evaluations` accepts `{ session_id, transcript, rubric_id }` and returns `EvaluationResult` with scores and citations.
- [ ] `POST /evaluations/{id}/disagree` records human disagreement.
- [ ] `GET /evaluations/{id}` returns full evaluation with summary.
- [ ] `make test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-11, US-12, US-14 |
| `Estación 8/clase-8-estudiante.md` | Persona + Judge pattern for agent evaluation |

## Definition of Ready

- [ ] Task 010 complete.
