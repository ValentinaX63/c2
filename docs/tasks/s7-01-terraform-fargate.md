---
id: "S7-01"
title: "iac(terraform): módulos Fargate API + workers DIAN — LocalStack primero (E9)"
milestone: S7
priority: P1
estimate: 6h
blockedBy: ["S5-03"]
blocks: []
---

## Summary

Crear los módulos Terraform para desplegar YARO en AWS: API NestJS en Fargate On-Demand + worker DIAN en Fargate Spot + RDS Multi-AZ. Validar primero contra LocalStack, luego aplicar en staging.

## Scope

- `infra/modules/fargate-service/` — módulo reutilizable ECS Fargate (task def, service, IAM, CloudWatch)
- `infra/modules/rds-postgres/` — módulo RDS PostgreSQL Multi-AZ
- `infra/staging/main.tf` — instancia módulos para api, dian-worker, rds
- `infra/providers-local.tf` / `infra/providers-aws.tf`
- `docker-compose.localstack.yml` — ya existe, verificar servicios necesarios
- `infra/steering.md` — convenciones naming, tagging, seguridad

## Acceptance Criteria

- [ ] `terraform validate` pasa sin errores.
- [ ] `terraform apply -var-file=local.tfvars` contra LocalStack crea: ECS cluster, 2 task definitions (api + worker), RDS instance, ALB, CloudWatch log groups.
- [ ] IAM roles siguen least-privilege: api-role solo accede a RDS + Secrets Manager; worker-role solo accede a SQS + Secrets Manager.
- [ ] Fargate API: On-Demand, 0.5 vCPU, 1GB RAM. Worker: SPOT, 0.25 vCPU, 512MB RAM.
- [ ] `terraform plan` sobre infra ya aplicada muestra "No changes".

## Test Plan

```bash
docker compose -f docker-compose.localstack.yml up -d
cd infra/staging
terraform init && terraform validate
terraform apply -var-file=local.tfvars -auto-approve
awslocal ecs list-clusters
awslocal ecs list-task-definitions
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `arquitectura.md ADR-07` | AWS Fargate On-Demand API + SPOT workers |
| `Estación 9/clase-9-estudiante.md` | Terraform workflow, LocalStack, MCPs |
| `apps/api/Dockerfile.dev` | Base para task definition |
