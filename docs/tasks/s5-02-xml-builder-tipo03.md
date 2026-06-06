---
id: "S5-02"
title: "feat(xml-builder): Contingencia Tipo 03 — builder para modo offline"
milestone: S5
priority: P0
estimate: 4h
blockedBy: []
blocks: ["S5-03", "S5-05"]
---

## Summary

Implementar `buildContingenciaXml()` en `libs/xml-builder`. El Tipo 03 es el documento de contingencia que se genera cuando no hay conectividad con Facture.co. Debe ser isomórfico (funcionar en Node.js y en el Service Worker del navegador) ya que US-OFF-009 requiere que el SW pueda generar este XML.

## Scope

- `libs/xml-builder/src/builders/contingencia.builder.ts`
- `libs/xml-builder/src/builders/contingencia.builder.spec.ts`
- Exportar desde `libs/xml-builder/src/index.ts`
- El builder NO puede usar APIs exclusivas de Node.js — debe correr en browser/SW context

## Acceptance Criteria

- [ ] `buildContingenciaXml()` genera XML Tipo 03 válido según estructura DIAN.
- [ ] No usa `Buffer`, `fs`, ni ninguna API exclusiva Node.js (compatible con Service Worker).
- [ ] Incluye `NroContingencia`, `FechaContingencia`, y datos del cobro offline.
- [ ] Tests cubren: builder básico, builder con múltiples ítems.
- [ ] `npx nx test xml-builder` pasa.

## Test Plan

```bash
npx nx test xml-builder --testPathPattern="contingencia"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `libs/xml-builder/src/builders/tiquete-pos.builder.ts` | Referencia isomórfica |
| `apps/web/src/sw.js` | Service Worker que lo consumirá |
| `apps/web/src/core/offline/offline-sync.service.ts` | Orquestador offline |
| `backlog.md F2.3, US-OFF-009` | Story source |

## Definition of Ready

- [ ] Validado que el `tiquete-pos.builder.ts` actual ya es isomórfico (no usa Buffer nativo).
