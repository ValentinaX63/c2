---
id: "012"
title: "entrevista-conversation: state machine, ConsentHandler, RequirementsHandler"
milestone: M3
priority: P0
estimate: 5h
blockedBy: ["008", "011"]
blocks: ["013"]
parent: null
---

## Summary

Implement the conversation state machine core: session lifecycle, AI identity disclosure, consent capture, and basic requirements check. These three states must complete before the competency screening loop begins.

## Scope

- Project scaffold for `entrevista-conversation`
- `src/orchestrator.py` — ConversationOrchestrator: state machine (INIT → DISCLOSURE → CONSENT → REQUIREMENTS → SCREENING → COMPLETED)
- `src/handlers/consent.py` — ConsentHandler: send disclosure, receive affirmative consent, call compliance-lambda
- `src/handlers/requirements.py` — RequirementsHandler: verify minimum job requirements (structured form via Claude)
- Session model (`src/models/session.py`) with state transitions
- Unit tests for state machine and handlers

## Acceptance Criteria

- [ ] State machine starts at INIT; transitions to DISCLOSURE on session start.
- [ ] ConsentHandler sends AI disclosure text; moves to CONSENT only after affirmative reply.
- [ ] ConsentHandler rejects ambiguous or negative consent responses with clarification prompt (max 2 retries, then graceful exit).
- [ ] ConsentHandler calls compliance-lambda `POST /compliance/consent` on affirmative consent.
- [ ] RequirementsHandler checks all requirements before moving to SCREENING.
- [ ] Unit tests cover happy path and consent-refused edge case.
- [ ] `make test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-02, US-03, US-04, US-05 |
| `AGENTS.md` | "Candidate-facing flows must disclose that the interviewer is an AI and record affirmative consent before evaluation" |

## Definition of Ready

- [ ] Tasks 008 and 011 complete (campaign and evaluation APIs stable).
- [ ] Disclosure text template confirmed.
