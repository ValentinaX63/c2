# EntreVista AI Implementation Planning

This workspace contains the implementation planning package for EntreVista AI, generated from the AI-DLC artifacts in `Estación 5/agentic_interviewer_ai/aidlc-docs`.

EntreVista AI is a greenfield platform for agentic candidate screening over Telegram. The MVP combines a Telegram gateway, Python/FastAPI Lambda services, MongoDB Atlas, Pinecone-backed knowledge bases, and a React recruiter dashboard.

## Planning Package

- Canonical manifest: `docs/tasks/task-package.yaml`
- Milestone index: `docs/tasks/milestones.md`
- Architecture: `docs/architecture.md`
- Decisions: `docs/decisions/`

The current planning wave is `entrevista-ai-mvp`.

## Implementation Shape

The AI-DLC source artifacts define a polyrepo MVP with seven service repositories:

1. `entrevista-auth`
2. `entrevista-compliance`
3. `entrevista-campaign`
4. `entrevista-evaluation`
5. `entrevista-conversation`
6. `entrevista-telegram-bot`
7. `entrevista-dashboard`

This repository keeps the shared planning package and the original course/example artifacts. Application code should be created in the target service repositories named by each task.

## Source Artifacts

Read these first when executing tasks:

- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/requirements/requirements.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/application-design/`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/`

## Validation

The planning package is intended for review and later conversion to Linear. The manifest task list is the source of truth.

```bash
find docs/tasks -maxdepth 1 -type f | sort
```
