# Milestones — YARO S5/S6/S7 sprint

**Planning wave:** `yaro-s5-s6-sprint`  
**Repo:** `apps/api` + `apps/web` + `libs/xml-builder`  
**Backlog ref:** `backlog.md` sprints S5–S7

---

## S5 — DIAN Tipo 01/03 + Offline

Sprint S5 del backlog YARO: completar `libs/xml-builder` con FE Venta (Tipo 01) y Contingencia (Tipo 03), actualizar el `dian-transmit.worker`, y conectar el Service Worker con el builder offline.

**Exit criteria:**
- `buildFEVentaXml()` genera XML válido UBL 2.1 Tipo 01 con CUDE.
- `buildContingenciaXml()` genera XML Tipo 03 usable desde el Service Worker.
- Worker procesa los 3 tipos sin romper el flujo existente Tipo 04.
- Panel "Transmisiones DIAN" en configuracion.component muestra estado por tipo de documento.

---

## S6 — Motor de Reglas IA + QA

Sprint S6: endpoint `/ia/sugerir-puc` con motor determinístico + specs E2E Playwright (E8).

**Exit criteria:**
- `POST /ia/sugerir-puc` responde en < 200ms con sugerencia + confianza + fuente.
- Motor carga 12 reglas base del sector restaurantero desde archivo de configuración.
- Suite Playwright cubre flujo: login → abrir turno → crear orden → cobrar → verificar badge DIAN.
- Golden dataset Persona+Juez: 5 conversaciones de referencia con scorecard ≥ 4/5.

---

## S7 — IaC + Seguridad

E9 + E10: Terraform módulos para staging en AWS (LocalStack primero) + SAST + threat model.

**Exit criteria:**
- `terraform apply` contra LocalStack despliega API Fargate + worker task + RDS sin errores.
- Semgrep encuentra 0 hallazgos de alta severidad en apps/api y libs/.
- Threat model cubre las 8 superficies OWASP Agentic 2026 relevantes para YARO.
