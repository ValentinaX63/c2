---
id: "S6-01"
title: "feat(ia): endpoint POST /ia/sugerir-puc — motor determinístico (US-AI-011)"
milestone: S6
priority: P0
estimate: 4h
blockedBy: []
blocks: ["S6-02"]
---

## Summary

Crear el endpoint `POST /api/v1/ia/sugerir-puc` que, dado un texto de ítem de factura, sugiere el código PUC (Plan Único de Cuentas) más apropiado usando el motor de reglas determinístico. La sugerencia debe incluir fuente y nivel de confianza (P5: IA verificable, nunca prescriptiva).

## Scope

- `apps/api/src/ai-agents/` — nuevo `ia.controller.ts`, `ia.service.ts`, `ia.module.ts`
- `apps/api/src/ai-agents/rules/` — archivo JSON con reglas base (cargadas por el motor)
- Respuesta: `{ sugerencia: string, codigoPUC: string, confianza: number, fuente: string }`
- El motor en F1 usa solo reglas determinísticas (sin Claude) — Claude se activa en F2

## Acceptance Criteria

- [ ] `POST /ia/sugerir-puc` con `{ descripcion: "Venta de hamburguesas" }` retorna sugerencia + código PUC + confianza 0-1 + fuente.
- [ ] Respuesta < 200ms (reglas en memoria, sin llamadas externas).
- [ ] Respuesta nunca usa lenguaje prescriptivo ("deberías", "te recomendamos") — P5.
- [ ] Tests: spec controller (5 tests) + spec service (motor carga reglas, match exacto, match parcial, sin match).
- [ ] `npx nx test api --testPathPattern="ia"` pasa.

## Test Plan

```bash
npx nx test api --testPathPattern="ia.controller|ia.service"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/api/src/ai-agents/agent-interaction.service.ts` | Logging service existente |
| `apps/api/src/ai-agents/ai-agents.module.ts` | Módulo a extender |
| `backlog.md F4.4, US-AI-011` | Story source |
| `CLAUDE.md P5` | IA verificable — nunca prescriptiva |
