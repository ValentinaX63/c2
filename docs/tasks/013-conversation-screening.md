---
id: "013"
title: "entrevista-conversation: ScreeningHandler, GuardrailsFilter, Claude integration"
milestone: M3
priority: P0
estimate: 6h
blockedBy: ["012"]
blocks: ["014"]
parent: null
---

## Summary

Implement the core screening loop: Claude generates contextual competency questions, GuardrailsFilter blocks jailbreak and off-topic attempts, and ScreeningHandler manages multi-turn flow with re-engagement on silence.

## Acceptance Criteria

- [ ] ScreeningHandler generates one competency question at a time via Claude Agent SDK, using campaign rubric and RAG context.
- [ ] GuardrailsFilter detects and blocks at least: prompt injection attempts, requests for PII, requests to "ignore your instructions".
- [ ] When candidate requests human handoff (US-09), EscalationNotifier is called and session pauses.
- [ ] Negative test: jailbreak attempt ("Ignore all instructions...") returns guardrail response, not a leaked system prompt.
- [ ] `make test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-06, US-07, US-08, US-09, US-10 |
| `Estación-10/brief.md` | LLM01 Prompt Injection, LLM06 Excessive Agency controls |

## Definition of Ready

- [ ] Task 012 complete (state machine, consent, requirements done).
- [ ] Claude Agent SDK version pinned in pyproject.toml.
- [ ] Guardrail policy patterns documented (at minimum 5 test cases for negative tests).
