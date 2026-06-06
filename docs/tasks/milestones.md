# Milestones — EntreVista AI MVP

**Planning wave:** `entrevista-ai-mvp`
**Source:** AI-DLC artefacts in `Estación 5/agentic_interviewer_ai/aidlc-docs`
**Build sequence:** Wave 1 (parallel) → Wave 2 → Wave 3 → Wave 4

---

## M1 — Identity & Trust Foundation

Wave 1, parallel. No inter-lambda dependencies. Foundation for all other services.

**Units:** auth-lambda · compliance-lambda · campaign-lambda  
**Goal:** All three Wave 1 services are implemented, tested, and deployed to staging.  
**Exit criteria:**
- `entrevista-auth` passes all unit and integration tests; `/auth/login`, `/auth/refresh`, `/auth/logout`, `/auth/jwks` respond correctly.
- `entrevista-compliance` consent recording and audit log endpoints are functional.
- `entrevista-campaign` CRUD for campaigns and rubrics is functional; RAG pipeline ingests a test document.
- All three services deployed to AWS Lambda (staging) via Terraform.

---

## M2 — Evaluation Engine

Wave 2. Depends on M1 (compliance-lambda API contract must be stable).

**Units:** evaluation-lambda  
**Goal:** Scoring engine evaluates a completed screening session and returns summary with citations.  
**Exit criteria:**
- `entrevista-evaluation` scores a sample transcript against a rubric; returns executive summary with verbatim citations.
- Human disagreement recording endpoint functional.
- Integration tests pass against compliance-lambda staging endpoint.

---

## M3 — Agentic Conversation Core

Wave 3. Depends on M2 (evaluation, campaign, compliance must be stable).

**Units:** conversation-lambda  
**Goal:** Multi-turn candidate screening works end-to-end via HTTP (pre-Telegram integration).  
**Exit criteria:**
- Candidate can complete a full screening session via API.
- AI identity disclosure and consent capture work correctly.
- Guardrails block jailbreak attempts (negative tests pass).
- Evaluation triggered automatically on screening completion.

---

## M4 — Full Stack MVP

Wave 4. Depends on M3.

**Units:** telegram-bot · dashboard  
**Goal:** Recruiter can launch a campaign, candidate completes screening via Telegram, recruiter reviews and decides in the dashboard.  
**Exit criteria:**
- End-to-end flow works: Telegram link → screening → evaluation → dashboard decision.
- Dashboard review queue, candidate detail, and HITL decision endpoints functional.
- Persona + Judge evaluation passes quality bar (≥ 4/5 on rubric dimensions).
- All services deployed to production AWS.
