---
id: "S6-03"
title: "test(e2e): specs Playwright flujo POS cobro + DIAN badge (E8)"
milestone: S6
priority: P1
estimate: 4h
blockedBy: ["S5-03"]
blocks: []
---

## Summary

Configurar Playwright y crear las primeras specs E2E para YARO siguiendo el patrón de la Estación 8. Cubre el flujo crítico: login → turno → crear orden → cobrar → verificar badge DIAN. Usa Page Object Model.

## Scope

- `apps/web/playwright.config.ts` — configuración inicial
- `apps/web/tests/e2e/pages/` — POMs: LoginPage, TurnoPage, MesasPage, CobroPage
- `apps/web/tests/e2e/pos-cobro.spec.ts` — flujo completo POS cobro
- `apps/web/tests/e2e/dian-badge.spec.ts` — verificación badge DIAN en UI post-cobro
- `.claude/skills/testing.md` — steering file para el agente de testing

## Acceptance Criteria

- [ ] `npx playwright test` corre los specs en modo headless.
- [ ] `pos-cobro.spec.ts`: login con usuario cajero → abrir turno → seleccionar mesa → agregar ítem → cobrar → verificar mensaje de éxito.
- [ ] `dian-badge.spec.ts`: post-cobro, el badge fiscal muestra estado (PENDING/ACCEPTED).
- [ ] POMs encapsulan todos los selectores (no hay selectores inline en los specs).
- [ ] Reporte HTML generado en `playwright-report/`.
- [ ] CI: `npx playwright test --reporter=html` exitoso.

## Test Plan

```bash
cd apps/web && npx playwright install --with-deps
npx playwright test --reporter=html
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `apps/web/src/app/features/pos/` | Componentes POS a testear |
| `apps/web/src/app/features/pos/cobro/cobro.component.ts` | Flujo cobro |
| `Estación 8/clase-8-estudiante.md` | Patrones POM, BDD, Playwright MCP |
| `apps/api/src/operations/cobros/cobros.service.ts` | Backend cobro fire-and-forget |
