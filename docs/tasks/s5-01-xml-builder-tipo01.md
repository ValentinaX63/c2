---
id: "S5-01"
title: "feat(xml-builder): FE Venta Tipo 01 — builder UBL 2.1"
milestone: S5
priority: P0
estimate: 5h
blockedBy: []
blocks: ["S5-03"]
---

## Summary

Implementar `buildFEVentaXml()` en `libs/xml-builder/src/builders/` siguiendo el mismo patrón de `tiquete-pos.builder.ts`. La FE Venta (Tipo 01) es el documento de factura electrónica completa requerido cuando el cliente es empresa (requiere NIT) o cuando el valor supera el umbral del tiquete POS.

## Scope

**In scope:**
- `libs/xml-builder/src/builders/fe-venta.builder.ts` — `buildFEVentaXml(input: FEVentaInput): string`
- `libs/xml-builder/src/builders/fe-venta.builder.spec.ts` — suite de tests
- `libs/xml-builder/src/index.ts` — exportar el nuevo builder
- CUDE calculator (equivalente al CUFE para Tipo 01)

**Out of scope:** integración con el worker (tarea S5-03).

## Acceptance Criteria

- [ ] `buildFEVentaXml()` genera XML válido conforme al esquema UBL 2.1 Tipo 01.
- [ ] CUDE se calcula correctamente con SHA-384 sobre los campos obligatorios DIAN.
- [ ] Maneja campos obligatorios: NIT comprador, dirección entrega, líneas de ítem con impuestos.
- [ ] Tests AAA: XML con cliente empresa, XML con múltiples ítems, XML con descuento global.
- [ ] `npx nx test xml-builder` pasa.

## Test Plan

```bash
npx nx test xml-builder --testPathPattern="fe-venta"
```

## Context

| Archivo | Relevancia |
|---------|-----------|
| `libs/xml-builder/src/builders/tiquete-pos.builder.ts` | Patrón de referencia |
| `libs/xml-builder/src/builders/cufe.calculator.ts` | CUFE/CUDE calculator base |
| `apps/api/src/fiscal-compliance/dian-queue/dian-transmit.worker.ts` | Consumidor futuro |
| `backlog.md F2.2` | Story source |

## Definition of Ready

- [ ] Esquema XSD Tipo 01 disponible (Facture.co docs o DIAN portal técnico).
- [ ] `tiquete-pos.builder.ts` leído y entendido como referencia.
