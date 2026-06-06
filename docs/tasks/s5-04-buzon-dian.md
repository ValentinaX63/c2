---
id: "S5-04"
title: "feat(config): buzón DIAN recepción básica (US-DIAN-008)"
milestone: S5
priority: P1
estimate: 4h
blockedBy: []
blocks: []
---

## Summary

Implementar el panel "Buzón DIAN Recepción" en la configuración (F2.9). Los documentos DIAN del tipo RADIAN (facturas electrónicas recibidas de proveedores) deben aparecer en una nueva tab del configuracion.component con estado de aceptación/rechazo.

## Scope

- `apps/api/src/config/config.controller.ts` — nuevo endpoint `GET /api/v1/config/dian/buzon`
- `apps/api/src/config/config.service.ts` — query `fiscalDocument` donde `tipo = 'RADIAN'`
- `apps/web/src/app/features/configuracion/configuracion.component.html` — nueva tab "Buzón"
- `apps/web/src/app/features/configuracion/config-api.service.ts` — método `getBuzonDian()`

## Acceptance Criteria

- [ ] `GET /api/v1/config/dian/buzon?page=0&limit=20` retorna documentos RADIAN paginados.
- [ ] Tab "Buzón" visible en configuracion.component con tabla de documentos recibidos.
- [ ] Cada fila muestra: NIT emisor, número, fecha, estado (PENDIENTE/ACEPTADO/RECHAZADO).
- [ ] Tests: spec del controller + spec del método en config-api.service.spec.ts.
- [ ] `npx nx test api --testPathPattern="config"` y `npx nx test web --testPathPattern="config"` pasan.

## Test Plan

```bash
npx nx test api --testPathPattern="config.controller|config.service"
npx nx test web --testPathPattern="config-api"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/api/src/config/config.controller.ts` | Controlador a extender |
| `apps/web/src/app/features/configuracion/configuracion.component.html` | Template a extender |
| `backlog.md F2.9, US-DIAN-008` | Story source |
