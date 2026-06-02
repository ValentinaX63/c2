# AGENTS.md

Persistent implementation context for agents working in this repository.

## Project Overview

This workspace contains the Hardcore AI 30X station materials and the EntreVista AI planning artifacts. The active implementation planning package is for:

- Project: EntreVista AI
- Planning wave: `entrevista-ai-mvp`
- Source artifacts: `Estación 5/agentic_interviewer_ai/aidlc-docs`
- Task package: `docs/tasks/task-package.yaml`

EntreVista AI is a greenfield, agentic interviewer platform for high-volume hiring in Latin America. Candidates complete conversational screening through Telegram. Recruiters manage campaigns, review AI-generated evaluations with citations, and make all final hiring decisions through a dashboard.

## Non-Negotiable Constraints

- Do not put generated application code inside `Estación 5/agentic_interviewer_ai/aidlc-docs`; that directory is source documentation only.
- Preserve the AI-DLC source artifacts as historical planning inputs. Add implementation planning under `docs/`.
- The MVP is a polyrepo-style system, represented here as implementation tasks for seven service repositories:
  - `entrevista-auth`
  - `entrevista-compliance`
  - `entrevista-campaign`
  - `entrevista-evaluation`
  - `entrevista-conversation`
  - `entrevista-telegram-bot`
  - `entrevista-dashboard`
- All backend services must enforce `tenant_id` isolation on every query and API path that touches tenant-owned data.
- The AI must never make final hiring decisions. It may score, summarize, cite evidence, and recommend. Human approval or rejection is mandatory.
- Every evaluation score must include transcript citations.
- Candidate-facing flows must disclose that the interviewer is an AI and record affirmative consent before evaluation.
- Candidate screening is text-only. Do not introduce biometric, emotional, facial, or voice analysis.
- SECURITY-01 through SECURITY-15 from the AI-DLC security baseline are blocking constraints.

## Technology Stack

- Telegram gateway: Node.js 20, TypeScript, Telegraf 4.x, AWS Lambda
- Backend services: Python 3.12, FastAPI, Mangum, AWS Lambda
- Dashboard: React 18, TypeScript, Vite, S3, CloudFront
- Primary database: MongoDB Atlas
- RAG/vector store: Pinecone
- Document storage: AWS S3
- Secrets: AWS Secrets Manager
- AI orchestration: Anthropic Claude / Claude Agent SDK
- Auth: email/password, Argon2id, RS256 JWT, refresh token rotation, email OTP MFA for admin login

## Repository Conventions

- Shared implementation docs live in `docs/`.
- Task files live in `docs/tasks/`; the manifest is the source of truth.
- Architecture decisions live in `docs/decisions/`.
- Service code should be created in the service repository named in each task, not in `aidlc-docs`.
- Use English for code, identifiers, commit messages, and generated service README files. Spanish is acceptable for candidate-facing and recruiter-facing product copy.

## Commands

There is no single root build because the target system is polyrepo. Each generated service should include its own standard commands:

- Python services: `make install`, `make test`, `make lint`, `make build`
- TypeScript services and dashboard: `npm install`, `npm test`, `npm run lint`, `npm run build`

Before publishing or converting tasks, validate:

```bash
find docs/tasks -maxdepth 1 -type f | sort
```

## Key Source Documents

- `Estación 5/agentic_interviewer_ai/aidlc-docs/aidlc-state.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/requirements/requirements.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/user-stories/stories.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/application-design/components.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/application-design/services.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/inception/application-design/unit-of-work.md`
- `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/`

## PR Requirements

Before submitting implementation work:

1. Required service tests pass.
2. Formatting and lint checks pass.
3. New API contracts have integration or contract tests.
4. Security-sensitive flows have negative tests.
5. Dashboard changes include accessible keyboard and screen-reader behavior for primary flows.
6. Docs and `.env.example` are updated for new configuration.
