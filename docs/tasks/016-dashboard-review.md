---
id: "016"
title: "entrevista-dashboard: auth flow, ReviewQueueView, CandidateDetailView"
milestone: M4
priority: P0
estimate: 6h
blockedBy: ["003", "011"]
blocks: ["017"]
parent: null
---

## Summary

Implement the recruiter dashboard SPA (React 18 + Vite). Core views: auth (login/logout), review queue with filters, and candidate detail with HITL decision controls. These are the highest-priority flows.

## Acceptance Criteria

- [ ] Recruiter can log in via email/password; JWT stored securely (httpOnly-equivalent via memory, not localStorage).
- [ ] ReviewQueueView shows candidates paginated, filterable by status (pending, approved, rejected).
- [ ] CandidateDetailView shows executive summary with verbatim citations and score per competency.
- [ ] HITL decision buttons (Approve / Reject / Request Human Interview) call evaluation-lambda disagree endpoint.
- [ ] Accessible: primary flows pass keyboard navigation (Tab, Enter) and have visible focus indicators.
- [ ] `npm test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-15, US-16, US-18 |
| `AGENTS.md` | "Dashboard changes include accessible keyboard and screen-reader behavior for primary flows" |
| `DESIGN.md` | Color tokens, typography (Calibri/Aptos) |

## Definition of Ready

- [ ] Tasks 003 and 011 complete (auth and evaluation APIs stable).
- [ ] DESIGN.md color palette applied to component library.
