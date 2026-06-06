---
id: "017"
title: "entrevista-dashboard: CampaignManagerView, KnowledgeBaseManagerView, AnalyticsDashboard"
milestone: M4
priority: P1
estimate: 5h
blockedBy: ["016"]
blocks: ["018"]
parent: null
---

## Summary

Complete the dashboard with campaign creation, rubric editor, knowledge base document upload, and analytics views.

## Acceptance Criteria

- [ ] Recruiter can create a campaign, define rubric competencies, and generate/copy the Telegram link.
- [ ] Recruiter can upload a PDF to the campaign knowledge base; upload progress indicator shown.
- [ ] AnalyticsDashboard shows total screenings, pass rate, and average scores per competency.
- [ ] AbandonmentAnalytics view shows drop-off rate per session stage (US-28).
- [ ] `npm test` passes.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-13, US-17, US-19, US-20, US-21, US-22, US-28 |

## Definition of Ready

- [ ] Task 016 complete.
