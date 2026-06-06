---
id: "018"
title: "IaC Wave 4: Terraform modules for conversation, telegram-bot, dashboard (S3+CloudFront)"
milestone: M4
priority: P1
estimate: 5h
blockedBy: ["014", "017"]
blocks: []
parent: null
---

## Summary

Extend Terraform to cover the remaining services: conversation-lambda, telegram-bot Lambda, and the dashboard (S3 + CloudFront). Apply security hardening per Estación 10 brief (WAF, HTTPS, IAM least-privilege, SAST clean).

## Acceptance Criteria

- [ ] `terraform apply` deploys conversation-lambda, telegram-bot Lambda, S3 bucket, and CloudFront distribution.
- [ ] CloudFront enforces HTTPS; S3 bucket is not publicly accessible directly.
- [ ] Telegram webhook URL registered via `awslocal` call in `scripts/register-webhook.sh`.
- [ ] WAF rule attached to API Gateway for conversation-lambda (rate limit + managed SQLi/XSS rules).
- [ ] `terraform plan` on already-deployed infra shows "No changes" (idempotent).
- [ ] SAST (Semgrep) passes on all IaC files with no high-severity findings.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 9/clase-9-estudiante.md` | LocalStack → AWS provider swap, drift detection |
| `Estación-10/brief.md` | OWASP surface map: Edge, Infra, Identity; WAF, TLS, least-privilege |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/infrastructure-design/deployment-architecture.md` | Full deployment diagram |

## Definition of Ready

- [ ] Tasks 014 and 017 complete.
- [ ] Task 009 IaC modules are reusable templates.
