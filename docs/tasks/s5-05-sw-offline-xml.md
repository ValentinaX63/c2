---
id: "S5-05"
title: "feat(service-worker): XML builder Tipo 03 en SW para cobro offline (US-OFF-009)"
milestone: S5
priority: P0
estimate: 5h
blockedBy: ["S5-02"]
blocks: []
---

## Summary

Integrar `buildContingenciaXml()` en el Service Worker (`apps/web/src/sw.js`) para que el cobro offline genere el XML de contingencia localmente, sin depender del servidor. Este es el story crítico ⭐ US-OFF-009.

## Scope

- `apps/web/src/sw.js` — importar builder (via importScripts o bundled), generar XML en el handler de cobro offline
- El XML se almacena en IndexedDB junto con el item de la SyncQueue para transmitirlo al reconectar
- `apps/web/src/core/offline/offline-sync.service.ts` — al sincronizar, enviar el XML pre-generado

## Acceptance Criteria

- [ ] SW genera XML Tipo 03 cuando `OfflineSyncService.enqueueOffline()` guarda un cobro.
- [ ] XML generado se almacena en el campo `xmlContingencia` del item de la SyncQueue.
- [ ] Al sincronizar, `/sync/cobros` recibe el XML pre-generado (no lo regenera en servidor).
- [ ] Test manual: DevTools → Network offline → cobrar → reconectar → verificar XML en SyncQueue item.
- [ ] `npx nx test web --testPathPattern="offline-sync"` pasa (tests existentes no rompen).

## Test Plan

```bash
npx nx test web --testPathPattern="offline-sync"
# Test manual: toggle offline en DevTools, cobrar, reconectar
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/web/src/sw.js` | Service Worker a modificar |
| `apps/web/src/core/offline/offline-sync.service.ts` | Orquestador offline (164 líneas) |
| `apps/web/src/core/offline/sync-queue.service.ts` | IndexedDB queue |
| `libs/xml-builder/src/builders/contingencia.builder.ts` | Builder (post S5-02) |
| `backlog.md US-OFF-009` | Story crítica ⭐ |
