---
id: "S6-02"
title: "feat(ia): motor de reglas — 12 reglas base sector restaurantero (F4.2)"
milestone: S6
priority: P0
estimate: 5h
blockedBy: ["S6-01"]
blocks: ["S6-04"]
---

## Summary

Implementar el motor de reglas determinístico con al menos 12 reglas base del sector restaurantero colombiano. El motor carga reglas desde un archivo de configuración versionado, evalúa condiciones y retorna la regla que aplica con su nivel de confianza.

## Scope

- `apps/api/src/ai-agents/rules/restaurante-rules.json` — 12+ reglas con: id, descripcion, condicion, accion, confianza, fuente_puc
- `apps/api/src/ai-agents/rules-engine.service.ts` — `RulesEngineService.evaluate(descripcion): RuleMatch | null`
- `apps/api/src/ai-agents/rules-engine.service.spec.ts` — tests GWT
- `ia.service.ts` usa el motor para `sugerir-puc`

## Acceptance Criteria

- [ ] Motor carga reglas desde JSON sin hardcoding en código.
- [ ] Reglas cubren al menos: ventas comidas, bebidas, propinas, domicilios, descuentos, IVA, impoconsumo, nómina, materia prima, inventario mermas, servicios externos, gastos operativos.
- [ ] `RulesEngineService.evaluate("venta hamburguesa")` retorna match con código PUC correcto.
- [ ] `RulesEngineService.evaluate("descripcion sin match")` retorna null.
- [ ] Tests GWT: regla exacta, regla parcial, múltiples candidatos (elige mayor confianza), sin match.
- [ ] `npx nx test api --testPathPattern="rules-engine"` pasa.

## Test Plan

```bash
npx nx test api --testPathPattern="rules-engine"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/api/src/ai-agents/ia.service.ts` | Consumidor del motor (post S6-01) |
| `backlog.md F4.2` | Story source — "12+ reglas base sector restaurantero" |
| `CLAUDE.md P5` | Confianza siempre explícita en la respuesta |
