---
id: "009"
title: "IaC Wave 1: Terraform modules for auth, compliance, campaign lambdas"
milestone: M1
priority: P1
estimate: 5h
blockedBy: ["003", "005", "008"]
blocks: ["018"]
parent: null
---

## Summary

Create Terraform modules to deploy all three Wave 1 Lambda services to AWS. Each module covers Lambda function, API Gateway HTTP API, IAM role, CloudWatch log group, and Secrets Manager references. Validate against LocalStack before applying to staging.

## Scope

**In scope:**
- `infra/modules/lambda-python/` — reusable module for Python Lambda + API Gateway HTTP + IAM + CloudWatch
- `infra/wave1/main.tf` — instantiates module for auth, compliance, campaign
- `infra/wave1/variables.tf`, `infra/wave1/outputs.tf`
- `infra/providers-local.tf` — LocalStack provider
- `infra/providers-aws.tf` — AWS provider (staging)
- `docker-compose.yml` — LocalStack container
- `scripts/validate.sh` — `terraform validate && terraform plan`
- Steering file `.claude/skills/iac.md`

**Out of scope:** evaluation, conversation, telegram-bot (later waves).

## Acceptance Criteria

- [ ] `terraform validate` passes with no errors for all modules.
- [ ] `terraform apply -var-file=local.tfvars` against LocalStack creates all 3 Lambdas, API Gateways, IAM roles, and log groups.
- [ ] `awslocal lambda list-functions` shows `entrevista-auth`, `entrevista-compliance`, `entrevista-campaign`.
- [ ] Each Lambda has env vars matching service `.env.example` (no hardcoded secrets; secrets via Secrets Manager ARN).
- [ ] IAM roles follow least-privilege: auth role has `secretsmanager:GetSecretValue` only on `entrevista/auth/*`; compliance/campaign scoped similarly.
- [ ] `make test` on each service still passes after IaC applies (no env regression).

## Test Plan

```bash
docker compose up -d localstack
cd infra/wave1
terraform init
terraform validate
terraform apply -var-file=local.tfvars -auto-approve

# Verify
awslocal lambda list-functions --query 'Functions[].FunctionName'
awslocal apigateway get-rest-apis
```

Expected: three Lambda functions listed, three API gateways.

## Context

| Document | Relevance |
|----------|-----------|
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/infrastructure-design/infrastructure-design.md` | Service map, Lambda config (arm64, 512MB, 30s), VPC, Secrets Manager |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/infrastructure-design/deployment-architecture.md` | Mermaid deployment diagram |
| `Estación 5/agentic_interviewer_ai/aidlc-docs/construction/auth-lambda/nfr-design/nfr-design-patterns.md` | ADR-U1-01 (degraded mode), ADR-U1-03 (rate limiting per endpoint) |
| `Estación 9/clase-9-estudiante.md` | Terraform workflow, LocalStack setup, MCPs |

## Definition of Ready

- [ ] Tasks 003, 005, 008 complete (all Wave 1 services have passing tests and build artifacts).
- [ ] Docker + Terraform installed in execution environment.
- [ ] AWS credentials configured for staging account.
