---
id: "006"
title: "entrevista-campaign: project scaffold, CampaignManager, RubricManager"
milestone: M1
priority: P0
estimate: 4h
blockedBy: []
blocks: ["007"]
parent: null
---

## Summary

Bootstrap `entrevista-campaign` and implement the CRUD layer for campaigns and rubrics. A campaign defines the job position, language, and rubric. A rubric contains scored competencies with weighting.

## Scope

**In scope:**
- Project scaffold
- `src/models/campaign.py`, `src/models/rubric.py`
- `src/campaign.py` — CampaignManager: create, get, list, archive, generate Telegram link
- `src/rubric.py` — RubricManager: create, get, update competencies and weights
- Unit tests

## Acceptance Criteria

- [ ] `CampaignManager.create()` persists a campaign with `campaign_id`, `tenant_id`, `title`, `language`, `status=ACTIVE`.
- [ ] `CampaignManager.generate_telegram_link()` returns a deep link in format `https://t.me/{BOT_USERNAME}?start={campaign_id}`.
- [ ] `RubricManager.create()` validates that competency weights sum to 1.0; raises `ValidationError` if not.
- [ ] `make test` passes.

## Test Plan

```bash
make install && make test
```

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md` | US-13, US-19, US-20 |

## Definition of Ready

- [ ] Campaign and rubric models understood from user stories US-13, US-19, US-20.
