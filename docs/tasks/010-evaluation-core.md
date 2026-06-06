---
id: "010"
title: "entrevista-evaluation: EvaluationEngine, SummaryGenerator, DisagreementTracker"
milestone: M2
priority: P0
estimate: 5h
blockedBy: ["005"]
blocks: ["011"]
parent: null
---

## Summary

Implement the evaluation domain: score a completed screening transcript against a rubric using Claude, generate an executive summary with verbatim citations, and record human disagreements.

## Scope

- Project scaffold for `entrevista-evaluation`
- `src/engine.py` — EvaluationEngine: score_session(transcript, rubric) → EvaluationResult with per-competency scores and citations
- `src/summary.py` — SummaryGenerator: generate_summary(evaluation_result) → markdown summary with verbatim quotes
- `src/disagreement.py` — DisagreementTracker: record_disagreement(evaluation_id, recruiter_score, recruiter_note)
- Unit tests (AAA for score aggregation, GWT for Claude prompt behavior via mock)

## Acceptance Criteria

- [ ] `EvaluationEngine.score_session()` calls Claude with rubric + transcript; returns `EvaluationResult` with per-competency `score` (0-100) and `citations` (list of verbatim quotes).
- [ ] `SummaryGenerator.generate_summary()` returns non-empty markdown with at least one verbatim citation per scored competency.
- [ ] `DisagreementTracker.record()` persists disagreement; `get_disagreements(evaluation_id)` returns list.
- [ ] Claude client is injectable (mock for unit tests).
- [ ] `make test` passes.

## Test Plan

```bash
make install && make test
python -m pytest tests/unit/ -v
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-11, US-12, US-14 |
| `AGENTS.md` | "AI must never make final hiring decisions", "every score must include transcript citations" |

## Definition of Ready

- [ ] Task 005 complete (compliance-lambda running; audit log available).
- [ ] Claude model and max tokens for evaluation confirmed.
