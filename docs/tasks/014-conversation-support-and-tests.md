---
id: "014"
title: "entrevista-conversation: EscalationNotifier, ReengagementScheduler, API router, integration tests"
milestone: M3
priority: P0
estimate: 4h
blockedBy: ["013"]
blocks: ["015"]
parent: null
---

## Summary

Complete `entrevista-conversation` with escalation, re-engagement, FastAPI router, Mangum handler, and a full integration test suite covering all Gherkin scenarios from user-stories.md.

## Acceptance Criteria

- [ ] `POST /conversations/start` creates session, starts state machine.
- [ ] `POST /conversations/{id}/message` advances state machine; returns next message.
- [ ] EscalationNotifier calls compliance-lambda when human handoff requested.
- [ ] ReengagementScheduler schedules follow-up (EventBridge) when session is abandoned.
- [ ] All Gherkin scenarios in US-02 through US-10, US-26, US-27, US-29 have passing integration tests.
- [ ] `make test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-02 through US-10, US-26-29 |
| `Estación 8/clase-8-estudiante.md` | Persona + Judge test pattern for agent testing |

## Definition of Ready

- [ ] Task 013 complete.
