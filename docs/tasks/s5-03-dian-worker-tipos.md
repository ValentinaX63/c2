---
id: "S5-03"
title: "feat(dian-worker): soporte Tipo 01 + Tipo 03 en dian-transmit.worker"
milestone: S5
priority: P0
estimate: 3h
blockedBy: ["S5-01", "S5-02"]
blocks: ["S6-03", "S7-01"]
---

## Summary

Extender `dian-transmit.worker.ts` para procesar trabajos de Tipo 01 (FE Venta) y Tipo 03 (Contingencia) además del Tipo 04 (Tiquete POS) que ya funciona. El worker ya tiene el patrón fire-and-forget correcto (P2).

## Scope

- `apps/api/src/fiscal-compliance/dian-queue/dian-transmit.worker.ts` — agregar rama para Tipo 01 y Tipo 03
- `apps/api/src/fiscal-compliance/dian-queue/dian-transmit.worker.spec.ts` — nuevos tests
- El job payload debe incluir campo `tipoDocumento: 'TIQUETE_POS' | 'FE_VENTA' | 'CONTINGENCIA'`

## Acceptance Criteria

- [ ] Worker procesa jobs `tipoDocumento: 'FE_VENTA'` llamando a `buildFEVentaXml()`.
- [ ] Worker procesa jobs `tipoDocumento: 'CONTINGENCIA'` llamando a `buildContingenciaXml()`.
- [ ] Tipo 04 sigue funcionando sin cambios.
- [ ] `fiscal_documents` con tipo FE_VENTA se crean con `tipo: 'FE_VENTA'` en BD.
- [ ] Tests: spec unitario para cada tipo de documento procesado.
- [ ] `npx nx test api --testPathPattern="dian-transmit"` pasa.

## Test Plan

```bash
npx nx test api --testPathPattern="dian-transmit.worker"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/api/src/fiscal-compliance/dian-queue/dian-transmit.worker.ts` | Archivo a extender |
| `libs/xml-builder/src/index.ts` | Builders disponibles post S5-01 y S5-02 |
| `apps/api/src/fiscal-compliance/facture/facture-client.service.ts` | HTTP client Facture.co |

## Definition of Ready

- [ ] S5-01 y S5-02 completos (builders disponibles).
- [ ] Schema de la tabla `fiscal_documents` verificado (campo `tipo` ya existe).
