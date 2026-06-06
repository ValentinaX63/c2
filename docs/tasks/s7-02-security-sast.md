---
id: "S7-02"
title: "sec(sast): Semgrep + threat model OWASP Top 10 LLM + Agentic (E10)"
milestone: S7
priority: P1
estimate: 3h
blockedBy: []
blocks: []
---

## Summary

Ejecutar SAST con Semgrep sobre el codebase YARO y documentar el threat model cubriendo las superficies de riesgo relevantes según el brief de Estación 10: OWASP Web 2025, OWASP LLM 2025, OWASP Agentic 2026.

## Scope

- `.semgrep/yaro-rules.yaml` — reglas custom: no-direct-prisma ya existe, agregar: no-hardcoded-tenant, no-prescriptive-ai
- `docs/security/threat-model.md` — mapa de superficies (8 categorías del brief E10) + top 5 riesgos YARO
- `docs/security/semgrep-report.json` — output del análisis
- `docs/security/security-backlog.md` — quick wins + mediano plazo + estructural

## Acceptance Criteria

- [ ] Semgrep corre sobre `apps/api/src/` y `libs/` sin errores de configuración.
- [ ] 0 hallazgos HIGH en las reglas custom YARO (no-direct-prisma, no-hardcoded-hex).
- [ ] `threat-model.md` cubre las 8 superficies: Infra+plataforma, Edge+exposición, Servicios+app, Identidad+acceso, Datos, Sistema IA, SDLC+supply chain, Observabilidad+respuesta.
- [ ] Top 5 riesgos YARO identificados con severidad, evidencia y fix propuesto.
- [ ] `security-backlog.md` con al menos 3 quick wins (≤ 2 días) y criterios de aceptación.

## Test Plan

```bash
semgrep --config .semgrep/ apps/api/src/ libs/ --json > docs/security/semgrep-report.json
cat docs/security/semgrep-report.json | jq '.results | length'
# Expected: 0 HIGH findings
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `libs/eslint-rules/src/rules/` | Reglas custom existentes como referencia |
| `Estación-10/brief.md` | Mapa de 8 superficies, OWASP taxonomías |
| `CLAUDE.md P1, P3` | Integridad fiscal + aislamiento multi-tenant como superficies críticas |
