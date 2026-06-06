---
id: "S6-04"
title: "test(agents): Persona+Juez para agente IA YARO — golden dataset (E8)"
milestone: S6
priority: P1
estimate: 4h
blockedBy: ["S6-02"]
blocks: []
---

## Summary

Implementar el patrón Persona+Juez de la Estación 8 para evaluar el agente IA de YARO (endpoint `/ia/sugerir-puc`). El agente Persona simula a un contador que hace preguntas sobre PUC. El agente Juez evalúa la respuesta contra un rubric.

## Scope

- `apps/api/tests/agents/rubrics/puc-evaluacion.md` — rubric con dimensiones: precisión código PUC, claridad explicación, ausencia prescripción, fuente incluida
- `apps/api/tests/agents/datasets/golden-puc-dataset.json` — 5 pares (input, expected_puc) de referencia
- `apps/api/tests/agents/test-persona-juez.ts` — orquestador: Persona hace N preguntas, Juez evalúa
- Reporte en Linear (issue automático si scorecard < 4/5)

## Acceptance Criteria

- [ ] Golden dataset con 5 items cargado y documentado.
- [ ] Juez evalúa 4 dimensiones: precisión (0-5), claridad (0-5), no-prescripción (0-5), fuente (0/5).
- [ ] Scorecard promedio ≥ 4/5 sobre el golden dataset.
- [ ] `npm run test:agents` genera reporte en `agent-eval-report.json`.
- [ ] Si alguna dimensión < 3, se crea issue en Linear automáticamente.

## Test Plan

```bash
npm run test:agents
cat agent-eval-report.json | jq '.scorecard'
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/api/src/ai-agents/ia.service.ts` | Agente bajo evaluación |
| `Estación 8/clase-8-estudiante.md` | Patrón Persona+Juez, golden datasets |
| `CLAUDE.md P5` | Criterio de no-prescripción |
