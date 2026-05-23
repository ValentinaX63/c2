# Backlog Ejecutable — YARO

### Sistema operativo de los restaurantes colombianos
**Versión 1.0 (consolidado co-creación iterativa) · Mayo 2026**

> Documento de backlog generado en sesión de co-creación entre la fundadora (Mariana, 911 Hot Burger) y un Product Engineering Lead + Agile Coach. Construido segmento por segmento sobre `specs/prd.md`, `specs/arquitectura.md` y los documentos base.

---

## Tabla de contenidos

0. [Paso 0 — Conflictos de priorización resueltos](#paso-0)
1. [B1 — Estrategia de Backlog + Frameworks](#b1)
2. [B2 — Mapa de 15 Épicas](#b2)
3. [B3 — Definition of Ready + Definition of Done](#b3)
4. [B4 — Épicas E1-E3 (Operación Core + DIAN F1 + Offline)](#b4)
5. [B5 — Épicas E4-E5 (Agentes IA + Inventario)](#b5)
6. [B6 — Épicas E6-E8 (Multi-tenant + Cobranza + Console)](#b6)
7. [B7 — Épicas E9-E12 (Hardening + Observabilidad + Personas + Auditoría)](#b7)
8. [B8 — Backlog Técnico + DevOps](#b8)
9. [B9 — Plan Detallado Sprints F1 (S1-S8)](#b9)
10. [B10 — Backlog Fase 2](#b10)
11. [B11 — Backlog Fase 3 + Agente A6 Arqueo Inteligente](#b11)
12. [B12 — Procesos del Equipo + Ceremonias + Métricas](#b12)
13. [Adendum — Arqueo Extendido (basado en Excel real 911)](#adendum-arqueo)
14. [Apéndices](#apendices)

---

<a name="paso-0"></a>

## Paso 0 — Conflictos de priorización resueltos

Antes de empezar B1 se identificaron 8 conflictos materiales entre PRD + Arquitectura que afectan la priorización del backlog. Las 4 decisiones clave:

| # | Conflicto | Decisión |
|---|---|---|
| **CB1** | Orden de implementación Must Have | **Orden técnico de A13**: cimientos → core → DIAN → piloto |
| **CB2** | Granularidad de historias | **Historias atómicas 1-8 SP** (cada flujo UX = 1 historia) |
| **CB3** | Tratamiento principios P1-P10 | **Criterios de aceptación transversales** (no tickets propios) |
| **CB7** | Foco piloto 911 vs ICP-A genérico | **70% ICP-A genérico + 30% específico 911** balanceado |

Conflictos pendientes resueltos en segmentos respectivos:
- **CB4** (hardening) → épica E9 dedicada
- **CB5** (multi-formato) → AC por formato en cada historia
- **CB6** (5 agentes en F1) → solo A1 motor reglas; A2-A6 son F2-F3
- **CB8** (métrica de Done) → KPI asociado en DoD

---

<a name="b1"></a>

## B1 — Estrategia de Backlog + Frameworks de Priorización

### Jerarquía de elementos

| Nivel | Cantidad F1 | Ejemplo |
|---|---|---|
| **Épica** | 15 | E4 "Cumplimiento DIAN F1" |
| **Feature** | 60-80 | F4.2 "Transmisión Tiquete POS Tipo 04" |
| **User Story** | ~280 | US-DIAN-013 "Cajero recibe CUFE en pantalla" |
| **Task técnica** | ~51 | T-INFRA-007 "AWS WAF + OWASP" |

### Frameworks de priorización

| Framework | Cuándo se usa |
|---|---|
| **MoSCoW** (heredado PRD §8) | Filtro inicial — qué entra a Fase 1 |
| **RICE** (Reach × Impact × Confidence / Effort) | Priorización dentro del Must Have para user stories |
| **WSJF** (Weighted Shortest Job First) | Priorización de tasks técnicas (hardening, observabilidad) |

### Velocidad estimada

| Variable | Valor |
|---|---|
| Equipo F1 | 5 ingenieros |
| SP útiles por ingeniero / sprint (2 sem) | 12-15 SP |
| Velocity por sprint | **50-65 SP** |
| Velocity F1 trimestre 1 (6 sprints) | ~360 SP |
| Velocity F1 total (16 sprints) | 800-1.040 SP |

### Story points (Fibonacci modificado)

| SP | T-shirt | Tiempo | Características |
|---|---|---|---|
| 1 | XS | < 2 horas | CRUD trivial |
| 2 | XS | 2-4 horas | Endpoint simple |
| 3 | S | 4-8 horas | UI con interactividad básica |
| 5 | M | 1-2 días | Feature BE+FE+tests |
| 8 | L | 2-4 días | Múltiples módulos |
| 13 | XL | 1 semana+ | **Romper antes de meter al sprint** |

### Ceremonias

| Ceremonia | Frecuencia | Duración |
|---|---|---|
| Daily Standup | Diaria 9am | 15 min |
| Sprint Planning | Quincenal lunes | 2h |
| Sprint Review + Demo | Quincenal viernes | 1h |
| Sprint Retro | Post-review | 1h |
| Backlog Grooming | Semanal miércoles | 1h |
| Tech Sync | Semanal martes | 30 min |

### Tooling

- **Linear** ($8/user/mes × 5 = $40/mes) — UX moderna, integración GitHub nativa
- Cada PR linkea a ticket Linear (gate CI)
- Labels obligatorios: tipo · categoría · módulo · persona · principio · fase · riesgo

### Trazabilidad obligatoria (5 preguntas por historia)

1. ¿A qué FR/NFR/Principio del PRD responde?
2. ¿En qué segmento/ADR de arquitectura está diseñado?
3. ¿Qué KPI del PRD §10 mueve?
4. ¿Qué riesgo del PRD §12 mitiga (si aplica)?
5. ¿Qué persona/perfil es el beneficiario?

### Anti-patterns prohibidos

❌ Historias > 13 SP · Spike sin output · Bug sin reproducción · Task técnica sin métrica · Historia sin persona · Historia que viola principio · Refactor sin justificación · Historia sin AC medibles · Dependencias de 5+ historias previas · Carry-over > 30%

### Métricas del proceso

| Métrica | Target |
|---|---|
| Velocity por sprint | 50-65 SP |
| Sprint completion rate | ≥ 85% |
| Lead time (idea → producción) | < 4 semanas |
| Cycle time (in-progress → done) | < 5 días |
| Bug escape rate | < 5% |
| Carry-over % | < 15% |
| DoR rejection rate | 10-20% (saludable) |

---

<a name="b2"></a>

## B2 — Mapa de 15 Épicas

```mermaid
flowchart TB
    subgraph CORE_F1["🎯 NÚCLEO F1 — Must Have"]
        E1["E1 · Operación de Piso"]
        E2["E2 · Cumplimiento DIAN F1"]
        E3["E3 · Modo Offline"]
        E4["E4 · Plataforma Agentes IA + Reglas"]
        E5["E5 · Inventario y Mermas"]
    end

    subgraph PLAT_F1["🏛️ PLATAFORMA F1 — Must Have"]
        E6["E6 · Multi-tenant + Onboarding"]
        E7["E7 · Suscripción + Cobranza FR-15"]
        E8["E8 · YARO Console"]
        E9["E9 · Hardening + Seguridad"]
        E10["E10 · Observabilidad + SLOs"]
        E11["E11 · Personas (Marcación + Dashboards)"]
        E12["E12 · Auditoría + Trazabilidad"]
    end

    subgraph FUTURE["🔮 VISIÓN F2-F3"]
        E13["E13 · Operación Extendida F2 (CDP + Contab + Nómina)"]
        E14["E14 · Agentes IA Ampliados (incluye A6 Arqueo)"]
        E15["E15 · Escala F3+ (Multi-país + Microservicios)"]
    end

    style CORE_F1 fill:#c0392b,color:#ffffff
    style PLAT_F1 fill:#534AB7,color:#ffffff
    style FUTURE fill:#b7860b,color:#ffffff
```

### Tabla resumen

| # | Épica | Módulos PRD | Fase | # Stories | SP | Personas |
|---|---|---|:-:|:-:|:-:|---|
| **E1** | Operación de Piso | M1, M2, M3 | F1 | ~50 | ~180 | Cajero · Andrés · Cocina |
| **E2** | Cumplimiento DIAN F1 | M5, M6 | F1 | ~30 | ~120 | Carolina · Andrés |
| **E3** | Modo Offline | A6 transversal | F1 | ~22 | ~95 | Cajero · Andrés |
| **E4** | Plataforma Agentes IA + Reglas | M11 F1 | F1 | ~15 | ~60 | Andrés · Carolina (futuro) |
| **E5** | Inventario y Mermas | M4 | F1 | ~18 | ~55 | Andrés · Jefe Cocina |
| **E6** | Multi-tenant + Onboarding | M14 | F1 | ~25 | ~110 | Equipo YARO · Mariana |
| **E7** | Suscripción + Cobranza FR-15 | M14+M15 | F1 | ~15 | ~60 | Equipo YARO · Mariana |
| **E8** | YARO Console | M15 | F1 | ~20 | ~80 | Platform Admin · Support |
| **E9** | Hardening + Seguridad | A8 transversal | F1 | ~20 | ~85 | Equipo YARO |
| **E10** | Observabilidad + SLOs | A9 transversal | F1 | ~15 | ~65 | Equipo YARO |
| **E11** | Personas (Marcación + Dashboards) | M12+M13 | F1 | ~25 | ~90 | Todos |
| **E12** | Auditoría + Trazabilidad | M16 | F1 | ~12 | ~50 | Transversal |
| **E13** | Operación Extendida F2 | M7+M8+M9 | F2 | ~33 | ~280 | Jefe CDP · Carolina · Mariana |
| **E14** | Agentes IA Ampliados | M11 F2+, M10, A6 | F2-F3 | ~50 | ~280 | Carolina · Mariana · Andrés |
| **E15** | Escala F3+ | Multi-país, microservicios | F3 | ~40 | ~250 | Todos |
| **Total F1 Must** | | | | **~267** | **~1.050 SP** | |
| **Total F2-F3** | | | | **~123** | **~810 SP** | |

### Mapa de dependencias críticas F1

```mermaid
flowchart TB
    E6["E6 · Multi-tenant + Onboarding<br/>(cimientos)"]
    E9["E9 · Hardening + Seguridad"]
    E10["E10 · Observabilidad + SLOs"]
    E12["E12 · Auditoría + Trazabilidad"]
    E1["E1 · Operación de Piso"]
    E2["E2 · Cumplimiento DIAN F1"]
    E3["E3 · Modo Offline"]
    E4["E4 · Plataforma Agentes IA"]
    E5["E5 · Inventario"]
    E7["E7 · Suscripción + Cobranza"]
    E8["E8 · YARO Console"]
    E11["E11 · Personas + Dashboards"]

    E6 --> E1
    E6 --> E5
    E6 --> E8
    E6 --> E9
    E9 --> E1
    E9 --> E8
    E1 --> E2
    E1 --> E5
    E1 --> E3
    E2 --> E3
    E2 --> E4
    E6 --> E7
    E8 --> E7
    E1 --> E11
    E2 --> E11

    style E6 fill:#c0392b,color:#ffffff
    style E1 fill:#3d9970,color:#ffffff
    style E2 fill:#3d9970,color:#ffffff
```

---

<a name="b3"></a>

## B3 — Definition of Ready + Definition of Done

### DoR — User Story estándar (checklist obligatorio antes de sprint)

**Contenido mínimo:**
- [ ] Formato "Como [persona] quiero [capacidad] para [beneficio]"
- [ ] Persona del PRD §3 identificada
- [ ] 2-7 criterios de aceptación binarios y medibles
- [ ] No hay AC tipo "X funciona bien" sin métrica

**Trazabilidad (5 preguntas):**
- [ ] FR/NFR/Principio del PRD
- [ ] Segmento/ADR de arquitectura
- [ ] KPI del PRD §10
- [ ] Riesgo del PRD §12 (si aplica)
- [ ] Beneficiario claro

**Principios aplicables (CB3):**
- [ ] Si toca módulo crítico → AC para P1, P2, P3
- [ ] Si toca UI → AC para P7, P9
- [ ] Si toca IA → AC para P5
- [ ] Si genera operaciones críticas → AC para P10

**Multi-formato:**
- [ ] Comportamiento por `tipoOperacion` (RESTAURANTE/BAR/FOOD_TRUCK/DARK_KITCHEN/CADENA_CDP)

**Estimación y testabilidad:**
- [ ] SP entre 1 y 8 (si > 8 → romper)
- [ ] Equipo entiende cómo construir
- [ ] Dependencias resueltas
- [ ] Testabilidad confirmada (unit / integration / E2E)
- [ ] Mockups/wireframes disponibles (si UI)

### DoD — User Story estándar (checklist obligatorio para Done)

**Funcionalidad:**
- [ ] Todos los AC cumplidos
- [ ] AC transversales de principios cumplidos
- [ ] Multi-formato validado en formatos aplicables

**Calidad de código:**
- [ ] Code review aprobado por 1 (o 2 para módulos críticos M5/M6/M11/M14)
- [ ] CI 100% verde
- [ ] Cobertura ≥ 70% F1 (≥ 80% F2)
- [ ] Sin warnings nuevos en lint

**Tests:**
- [ ] Unit tests pasando
- [ ] Integration tests si toca BD o externos
- [ ] E2E si tiene flujo de usuario completo
- [ ] Tests aislamiento multi-tenant si toca tabla con tenant_id

**Despliegue:**
- [ ] Mergeada a main (auto deploy staging)
- [ ] Smoke test post-deploy pasa
- [ ] PR linkeado a ticket Linear actualizado
- [ ] Migración DB (si aplica) sigue expand-contract
- [ ] No deploy en hora pico si tiene migración

### DoD ESPECIAL — historias críticas

**Fiscal Documents (P1):**
- Trigger SQL append-only validado
- Test que `UPDATE` post-ACCEPTED falla
- Test E2E sandbox Facture
- Validación pre-transmisión completa
- `tax_snapshot_id` referenciado correctamente
- Cálculos en enteros COP con `Math.floor`
- **Approval explícito CTO en PR**

**Multi-tenancy (P3):**
- Test de aislamiento específico escrito y pasando
- Tabla nueva con `tenant_id NOT NULL` + índice
- ESLint custom rules NO bloquean
- $queryRaw incluye `WHERE tenant_id` explícito
- **Code review por 2 ingenieros**
- Verificado en staging con 2 tenants

**Agentes IA (P5):**
- Logueo en `agent_interactions` obligatorio
- Guardrails P5: verificable + no prescriptiva
- Dataset eval actualizado si nuevo caso edge
- Prompt versionado en S3
- Canary 5% antes de 100%

**Schema Migration:**
- Sigue patrón expand-contract
- CI gate `schema-migration-safety` pasa
- Probada en RDS copy de staging
- Tiempo ejecución estimado < 5 seg
- Rollback documentado

**Seguridad:**
- Pen-test interno: pasa escenario adversarial relevante
- Logs de seguridad emitidos
- Si toca secrets: rotación documentada
- **Code review por Security Lead o CTO**

### Quality gates automatizados en CI

- Lint + Format
- Custom ESLint rules YARO (multi-tenancy, P5)
- Unit tests + coverage ≥ 70%
- **Integration tests multi-tenancy (gate crítico P3)**
- Security audit (Snyk + npm audit)
- **xml-builder version check (CA2)**
- Schema migration safety
- E2E smoke tests (Playwright)
- Build OK

---

<a name="b4"></a>

## B4 — Épicas E1-E3 (Operación Core + DIAN F1 + Offline)

> **103 user stories · ~395 SP** — corazón funcional de Fase 1.

### E1 · Operación de Piso (50 stories · ~180 SP)

| Feature | # Stories | SP |
|---|:-:|:-:|
| F1.1 Apertura y cierre de turno | 5 | 18 |
| F1.2 Caja con base y arqueo | 4 | 17 |
| F1.3 POS plano de mesas (RESTAURANTE) | 8 | 32 |
| F1.4 POS vista barra (BAR) | 3 | 9 |
| F1.5 POS lista pedidos (FOOD_TRUCK + DARK_KITCHEN) | 3 | 8 |
| F1.6 Operación de orden | 5 | 17 |
| F1.7 Pre-cuenta | 2 | 6 |
| F1.8 Cobro multi-método | 6 | 30 |
| F1.9 KDS WebSocket | 6 | 22 |
| F1.10 Anulación con Nota Crédito | 3 | 13 |
| F1.11 Configuración soporte | 5 | 14 |

**Historia clave ⭐ US-COBRO-001** — Cajero cobra mesa con tiquete DIAN en < 100ms (8 SP)
- Cobertura: P1, P2, P3, P4, P10
- Multi-formato: 4 tipos (RESTAURANTE/BAR/FOOD_TRUCK/DARK_KITCHEN)
- Riesgo mitigado: R1 (POS falla hora pico)

### E2 · Cumplimiento DIAN F1 (31 stories · ~120 SP)

| Feature | # Stories | SP |
|---|:-:|:-:|
| F2.1 Tiquete POS Tipo 04 | 4 | 21 |
| F2.2 FE Venta Tipo 01 | 3 | 12 |
| F2.3 Contingencia Tipo 03 | 2 | 13 |
| F2.4 Integración Facture.co | 2 | 8 |
| F2.5 State machine fiscal_documents | 3 | 11 |
| F2.6 Panel de transmisiones | 3 | 11 |
| F2.7 Cron reconciliación 15min | 2 | 8 |
| F2.8 Validación pre-transmisión | 2 | 6 |
| F2.9 Buzón DIAN recepción RADIAN | 3 | 13 |
| F2.10 UI Buzón clasificación manual | 3 | 11 |
| F2.11 failed_fiscal_documents | 2 | 6 |
| F2.12 Tax snapshot | 2 | 7 |

**Historia clave ⭐ US-DIAN-001** — Sistema genera XML UBL 2.1 Tiquete Tipo 04 (8 SP, approval CTO obligatorio)

### E3 · Modo Offline y Sincronización (22 stories · ~95 SP)

| Feature | # Stories | SP |
|---|:-:|:-:|
| F3.1 Service Worker setup | 2 | 8 |
| F3.2 Workbox estrategias | 4 | 13 |
| F3.3 IndexedDB SyncQueue | 2 | 8 |
| F3.4 @yaro/xml-builder compartido | 2 | 13 |
| F3.5 Cobro offline | 3 | 13 |
| F3.6 Detección híbrida conectividad | 1 | 5 |
| F3.7 Background Sync | 1 | 3 |
| F3.8 Endpoint /sync/tiquetes | 2 | 8 |
| F3.9 Tax snapshot cache | 1 | 3 |
| F3.10 UI offline | 2 | 6 |
| F3.11 Cleanup automático | 1 | 5 |
| F3.12 Tests offline | 1 | 5 |

**Historia clave ⭐ US-OFF-009** — Builder genera XML contingencia en Service Worker (8 SP, requiere spike previo T-SPIKE-002)

### Cobertura de principios E1+E2+E3

| Principio | # stories | % |
|---|:-:|---|
| P1 Integridad fiscal | 18 | 17.5% |
| P2 Cajero nunca espera | 12 | 11.7% |
| P3 Aislamiento multi-tenant | 6 | 5.8% |
| P4 Offline-first | 24 | 23.3% |
| P7 Lenguaje del usuario | 17 | 16.5% |
| P9 Alertas accionables | 12 | 11.7% |
| P10 Trazabilidad | 41 | 39.8% |

---

<a name="b5"></a>

## B5 — Épicas E4-E5 (Agentes IA + Inventario)

> **33 stories · ~115 SP**

### E4 · Plataforma de Agentes IA + Motor de Reglas (15 stories · ~60 SP)

> **F1 sin Claude** — solo Capa 2 (motor de reglas) + Capa 3 (default). Claude Sonnet activa en F2 mes 9.

| Feature | Stories clave | SP |
|---|---|:-:|
| F4.1 Orquestador 3 capas (placeholder F1) | US-AI-001 a 003 | 13 |
| F4.2 Motor de reglas determinístico | US-AI-004 carga 12+ reglas base sector restaurantero · US-AI-005/006/007 | 17 |
| F4.3 Persistencia (`agent_interactions` etc.) | US-AI-008/009/010 | 13 |
| F4.4 Endpoint /ia/sugerir-puc | US-AI-011 | 5 |
| F4.5 Eval continua infraestructura | US-AI-012/013 | 7 |
| F4.6 Dataset Carolina | US-AI-014 (100 facturas etiquetadas) · US-AI-015 (importación) | 5 |

### E5 · Inventario y Mermas (18 stories · ~55 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F5.1 CRUD inventario | US-INV-001/002/003 | 9 |
| F5.2 Stock mínimo y alertas | US-INV-004/005 (botón "Solicitar al CDP") /006 | 11 |
| F5.3 Descuento automático | US-INV-007 (suscriptor CobroRegistrado) / 008 | 8 |
| F5.4 Mermas | US-MERMA-001 (causa obligatoria) /002/003/004 | 14 |
| F5.5 Histórico y vistas | US-INV-009/010/011 | 8 |
| F5.6 Preparación lotes F2 | US-INV-012 (código de barras) /013 | 4 |
| F5.7 Reportes | US-INV-014 (export CSV) | 3 |

---

<a name="b6"></a>

## B6 — Épicas E6-E8 (Multi-tenant + Cobranza + Console)

> **60 stories · ~250 SP** — cimientos de la plataforma F1.

### E6 · Multi-tenant + Configurabilidad + Onboarding (25 stories · ~110 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F6.1 Schema Prisma F1 | US-TENANT-001 (37 tablas con adendums) | 8 |
| F6.2 Multi-tenant middleware | **US-TENANT-002 ⭐** · 003 · 004 | 16 |
| F6.3 platformQuery + audit log | US-TENANT-005 (CA3) | 5 |
| F6.4 Wizard onboarding | US-ONBOARD-001 a 004 (Paso 0: tipoOperacion + "decido después" DIAN) | 19 |
| F6.5 Configuración fiscal | US-TENANT-006/007 (tax_snapshot) /008 (activación DIAN) | 11 |
| F6.6 Configurabilidad módulos | US-TENANT-009 (CA4) /010 | 8 |
| F6.7 Roles + permisos | US-USER-001/002/003 | 11 |
| F6.8 Invitaciones (Google OAuth + email/password) | US-INVT-001/002/003 | 13 |
| F6.9 Migración manual asistida | US-MIGR-001/002/003 (importación CSV) | 11 |
| F6.10 Country config | US-COUNTRY-001/002 | 8 |

### E7 · Suscripción + Cobranza Escalonada FR-15 (15 stories · ~60 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F7.1 Estados L0-L4 | US-SUB-001 (estados) /002 (auditoría) | 7 |
| F7.2 Cron evaluador adaptativo | US-SUB-003/004 (regla horaria) /005 | 16 |
| F7.3 Bloqueo escalonado | US-SUB-006 (L1 reportes) /007 (L2 contabilidad) /**008 ⭐** (L3 operación horario protegido) | 14 |
| F7.4 Notificaciones escalonadas | US-NOTIF-001/002/003 | 11 |
| F7.5 Pantalla regularizar | US-PAY-001 | 5 |
| F7.6 Pasarela de pago | US-PAY-002 | 5 |
| F7.7 Override manual | US-SUB-009/010 | 8 |

### E8 · YARO Console (20 stories · ~80 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F8.1 Frontend Console separado | US-CON-001/002 | 8 |
| F8.2 Auth con MFA obligatorio | **US-CON-003 ⭐** /004 | 13 |
| F8.3 Gestión tenants | US-CON-005/006/007 | 13 |
| F8.4 Monitor DIAN global | US-CON-008/009 | 8 |
| F8.5 Health checks | US-CON-010/011 | 6 |
| F8.6 Panel logs | US-CON-012/013 | 8 |
| F8.7 Dashboard agentes IA | US-CON-014 | 5 |
| F8.8 Panel cobranza | US-CON-015 | 3 |
| F8.9 Gestión equipo interno | US-CON-016/017 | 6 |
| F8.10 Dead letter queue | US-CON-018 | 3 |
| F8.11 platform_access_log auditoría | US-CON-019/020 | 7 |

---

<a name="b7"></a>

## B7 — Épicas E9-E12 (Hardening + Observabilidad + Personas + Auditoría)

> **72 stories · ~290 SP** — cierre de épicas F1 transversales.

### E9 · Hardening + Seguridad (20 stories · ~85 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F9.1 AWS WAF + Rate Limiting | US-SEC-001/002/003 | 11 |
| F9.2 Auth reforzada | US-SEC-004 (Argon2) /005 (Lockout) /**006 ⭐ Google OAuth** | 13 |
| F9.3 JWT + Sesiones | US-SEC-007/008 | 8 |
| F9.4 Secrets Manager | US-SEC-009/010 | 6 |
| F9.5 Encryption | US-SEC-011 | 5 |
| F9.6 Dependencias seguras | US-SEC-012/013 | 5 |
| F9.7 Backups + restore-test | US-SEC-014/015 | 8 |
| F9.8 Compliance Ley 1581 | US-SEC-016 (SIC) /017 (ARCO) /**018 ⭐ runbook + simulacro** | 13 |
| F9.9 Headers + CSP | US-SEC-019/020 | 6 |

### E10 · Observabilidad + SLOs (15 stories · ~65 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F10.1 CloudWatch metrics + dashboards | US-OBS-001/002/003 | 16 |
| F10.2 Logs estructurados | US-OBS-004/005 (anti-PII sanitizer) /006 | 13 |
| F10.3 X-Ray Traces | US-OBS-007/008 | 8 |
| F10.4 Outbox + eventos | US-OBS-009/010 | 8 |
| F10.5 Health checks | US-OBS-011/012 | 6 |
| F10.6 Error budgets + feature freeze | US-OBS-013/014 | 10 |
| F10.7 Particionado + retención | US-OBS-015 | 4 |

### E11 · Personas: Marcación + Bienestar + Dashboards (25 stories · ~90 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F11.1 Marcación + Bienestar | US-MARC-001 (botón topbar) /002 (😔/😐/😊) /003/004 | 13 |
| F11.2 Dashboard Mariana | US-DASH-001/002/003/004 | 17 |
| F11.3 Dashboard Andrés | US-DASH-005/006/007/008 | 16 |
| F11.4 Dashboard Carolina | US-DASH-009/010/011 | 11 |
| F11.5 Dashboard Cajero (minimal) | **US-DASH-012 ⭐ ** /013 | 6 |
| F11.6 Dashboard Cocina | US-DASH-014/015 | 6 |
| F11.7 Notificaciones push PWA | US-NOTIF-PWA-001/002/003 | 11 |
| F11.8 Reportes básicos exportables | US-REP-001/002/003 | 10 |

### E12 · Auditoría + Trazabilidad (12 stories · ~50 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| F12.1 Tabla audit_events + particionado | US-AUD-001 | 5 |
| F12.2 Interceptor automático | US-AUD-002/003 | 8 |
| F12.3 Auditoría dominios críticos | US-AUD-004 (config fiscal) /005 (descuentos) /006 (cierres) /007 (FR-15) | 16 |
| F12.4 Accesos plataforma | US-AUD-008 (CA3) | 5 |
| F12.5 Logueo IA | US-AUD-009 | 3 |
| F12.6 UI auditoría | US-AUD-010 | 5 |
| F12.7 Política retención | US-AUD-011 | 3 |
| F12.8 Exportación | US-AUD-012 | 5 |

---

<a name="b8"></a>

## B8 — Backlog Técnico + DevOps

> **51 tickets · ~210 SP** — trabajo no-UI que habilita todo.

### T-INFRA (12 tickets · 58 SP)
Terraform: VPC + ECS Fargate + RDS Multi-AZ + ElastiCache Valkey + S3 (tier transition) + CloudFront/WAF/Shield + Secrets Manager + IAM + SES + ambientes dev/staging/prod + Bastion SSM

### T-DEVOPS (10 tickets · 42 SP)
GitHub Actions PR checks + Deploy staging auto + Promote production manual + Rollback workflow + Docker multi-stage + ECR + Smoke tests + Sync prod→staging anonimizado + Feature flags + Linear integration

### T-TEST (8 tickets · 36 SP)
Jest + testcontainers + Playwright + Mock Facture sandbox + Mock Anthropic + **Suite tests aislamiento multi-tenant ⭐ (8 SP — gate CI crítico)** + k6 load testing + Chaos testing

### T-LIB (5 tickets · 26 SP)
`@yaro/shared` + `@yaro/ui` + **`@yaro/xml-builder` ⭐ (8 SP — compartido FE/BE)** + `@yaro/eslint-rules` + `@yaro/openapi-client`

### T-DEBT (5 tickets · 15 SP)
Audit performance + Audit security headers + Audit cobertura + Audit deps obsoletas + Refactor DRY (sprint dedicado mes 6)

### T-DOCS (4 tickets · 14 SP)
OpenAPI spec generado + Storybook @yaro/ui + 10 ADRs versionados + Onboarding < 1 día

### T-SPIKE (4 tickets · 10 SP)
Latencia Facture sandbox + **XSD validation en SW (3 SP — bloqueante US-OFF-009)** + Estructura prompts IA + Estrategia multi-país Perú

### T-PERF (3 tickets · 9 SP)
APM X-Ray detallado + Synthetic monitoring + Profiling Node.js

### Top 5 WSJF prioridades

| # | Ticket | WSJF |
|---|---|:-:|
| 1 | T-TEST-006 Suite tests aislamiento multi-tenant | 6.8 |
| 2 | T-INFRA-008 Secrets Manager + MFA | 6.5 |
| 3 | T-DEVOPS-001 GitHub Actions PR checks completo | 6.0 |
| 4 | T-LIB-003 @yaro/xml-builder | 5.8 |
| 5 | T-INFRA-004 RDS Multi-AZ | 5.5 |

---

<a name="b9"></a>

## B9 — Plan Detallado Sprints F1 (S1-S8)

```mermaid
gantt
    title Sprint Planning F1 — Trimestre 1 + 2 inicial
    dateFormat YYYY-MM-DD
    axisFormat S%U

    section Cimientos
    S1 Plataforma + WAF + Tests aislamiento     :crit, s1, 2026-06-01, 14d
    S2 Auth + MFA + Console + xml-builder spike :crit, s2, 2026-06-15, 14d

    section Core
    S3 POS + Onboarding + Roles                 :crit, s3, 2026-06-29, 14d
    S4 Cobro fire-and-forget + DIAN Tipo 04     :crit, s4, 2026-07-13, 14d

    section DIAN + Offline
    S5 Offline + DIAN Tipo 01/03 + Worker       :crit, s5, 2026-07-27, 14d
    S6 KDS + Buzón básico + Motor reglas        :s6, 2026-08-10, 14d

    section Piloto
    S7 Dashboards + Marcación + Notif + 911     :s7, 2026-08-24, 14d
    S8 Piloto producción + FR-15 + Iteración    :s8, 2026-09-07, 14d
```

### Goals por sprint

| Sprint | Goal | Demo objetivo |
|---|---|---|
| **S1** | Plataforma multi-tenant aislada + AWS infra base | 2 tenants ficticios, validar aislamiento, WAF bloquea SQLi |
| **S2** | Auth completo (MFA + Google OAuth) + Console básico | Login Console con MFA, Google OAuth, spike XSD concluido |
| **S3** | POS plano mesas + Onboarding wizard + Roles | Tenant via wizard, usuarios con roles, mesa con ítems |
| **S4** | **Cobro fire-and-forget < 100ms + Tiquete DIAN sandbox** ⭐ | Cobro real con CUFE en alpha interno |
| **S5** | **Modo offline funcional + DIAN Tipo 01/03 + Contingencia** ⭐ | DevTools offline + cobro + reconectar = sync |
| **S6** | KDS + Buzón DIAN + Motor reglas base | Cocina ve órdenes tiempo real, motor clasifica facturas |
| **S7** | Dashboards por rol + Marcación + Migración 911 | Mariana ve dashboard, equipo 911 capacitado |
| **S8** | **911 Hot Burger en producción + FR-15 + Iteración** ⭐ | 911 sede Guarne 15 días reales sin caídas |

### SP por sprint vs velocity

| Sprint | SP total | Stories ⭐ críticas |
|---|:-:|---|
| S1 | 88 | US-TENANT-002, US-TENANT-003 |
| S2 | 90 | US-SEC-006, US-CON-003 |
| S3 | 85 | (cimientos sin críticas explícitas) |
| **S4** | **92** ⭐ | **US-COBRO-001 + US-DIAN-001 + T-LIB-003** |
| S5 | 75 | US-OFF-009, US-DIAN-008 |
| S6 | 74 | — |
| S7 | 77 | — |
| S8 | 70 | US-SUB-004, US-SUB-008, US-SEC-018 |

**Total: ~651 SP combinado (stories + tasks)** — primer trimestre cubre ~44% del F1 Must Have. Restante ~530 SP en S9-S16.

### Checkpoints Go/No-Go

| Checkpoint | Criterio GO | Acción NO-GO |
|---|---|---|
| Día 30 | Tests aislamiento 100% + MFA + WAF | Refuerzo 2-4 sem, postergar POS |
| Día 60 | Alpha cobra DIAN ACCEPTED + KDS < 200ms + offline funciona + Q7=0 | Extender 4 sem, cortar nice-to-have |
| Día 90 | 911 Guarne 15d sin caída + 0 docs rechazados + arqueo < 12 min | Iterar piloto 4-8 sem antes de cliente 2 |

---

<a name="b10"></a>

## B10 — Backlog Fase 2 (Vista alta-nivel)

> **~56 stories · ~480 SP** distribuidos en S17-S36 (meses 9-18).

### E13 · Operación Extendida F2 (33 stories · ~280 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| **F13.1 CDP Centro de Producción** | US-CDP-001 a 012 (incluye **US-CDP-009 ⭐ trazabilidad lote→plato — MOAT**) | 100 |
| F13.2 Contabilidad (PUC, P&G, Balance) | US-CONTAB-001 a 010 (impoconsumo separado, retenciones, Régimen Simple) | 80 |
| **F13.3 Nómina Electrónica DIAN (47 campos)** | US-NOM-001 a 008 (**US-NOM-006 ⭐ nómina electrónica**) | 75 |
| F13.4 Recetas con costeo real | US-REC-001 | 13 |
| F13.5 Documentos DIAN adicionales | US-DIAN-NC (Tipo 91) + DSCE (Tipo 05) | 22 |

### E14 · Agentes IA Ampliados F2-F3 (23 stories · ~200 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| **F14.1 Buzón con Claude Sonnet** | **US-AI-CLAUDE-001 ⭐ ** (activación F2) + guardrails P5 + cache | 52 |
| F14.2 A2 Food Cost & Márgenes | US-A2-001 a 005 (alerta deriva) | 40 |
| **F14.3 A4 Copiloto Dueño descriptivo** | **US-A4-002 ⭐ click-through fuente** + **US-A4-003 ⭐ detección prescriptiva** | 50 |
| F14.4 Conciliación asistida F2 | US-CONC-001 a 004 (reporte por método + comisiones) | 28 |
| F14.5 Eval continua activa | US-EVAL-001 (Haiku judge batch) /002 (alertas F2/S1) /003 (red-team Q1) | 30 |

### Criterios éxito F2 mes 18

✅ B1: 20 tenants activos · B2: $7.2M MRR · F4: > 80% aceptación IA · U4: Carolina < 1h/día · Food cost real vs estimado < 2pp · 100% nóminas DIAN · S1 < 0.5% · F2 < 2% · Costo IA < $80/tenant/mes

### Hiring F2 (preparación desde F1)

- Mes 7-8 F1: **AI Engineer / ML Engineer**
- Mes 8-9 F1: **DevOps Senior** (segundo)
- Mes 7 F1: **Customer Success Engineer**
- Mes 9 F2: Frontend Senior · Mes 10 F2: Backend Senior · Mes 11 F2: QA Senior · Mes 12 F2: Sales / Account Manager

---

<a name="b11"></a>

## B11 — Backlog Fase 3 + Agente A6 Arqueo Inteligente

> **~50 stories · ~330 SP** distribuidos en S37-S56 (meses 19-30).

### Nuevo agente: A6 — Arqueo Inteligente (F2-F3)

> **Justificación:** el arqueo es el momento donde se pierde más plata sin que nadie lo note. Un agente especializado detecta patrones de descuadre, fraude y predice base óptima.

| Stories A6 (F2 + F3) | Persona | SP | Fase |
|---|---|:-:|:-:|
| US-A6-001 A6 analiza histórico descuadres por cajero + turno + método | Sistema | 8 | F2 |
| US-A6-002 Andrés recibe sugerencia causa probable al cerrar con descuadre | Andrés | 8 | F2 |
| US-A6-003 Detecta patrón sospechoso (mismo cajero, mismo monto, recurrente) | Sistema | 8 | F2 |
| US-A6-004 Predice base óptima por turno según histórico | Cajero/Admin | 5 | F2-F3 |
| US-A6-005 Detecta "cuadre perfecto sospechoso" (fraude inverso) | Sistema | 5 | F3 |
| US-A6-006 Carolina ve panel patrones agregados multi-sede | Carolina | 3 | F2 |
| US-A6-007 Aprende reglas tenant via tenant_rules específicas A6 | Sistema | 3 | F2 |
| US-A6-CASH-001 Detecta deriva sutil de cajero (descuadres pequeños recurrentes) | Sistema | 5 | F2 |
| US-A6-CASH-002 Sugiere base óptima día semana + estacionalidad | Sistema | 5 | F2 |
| US-A6-CASH-003 Detecta consignaciones tardías o monto que no coincide | Sistema | 5 | F2 |

### E14 F3 — Agentes IA avanzados (15 stories · ~135 SP)

| Feature | Stories clave | SP |
|---|---|:-:|
| **F14.6 A3 Conciliación Bancaria automatizada** | **US-A3-002 ⭐ matching automático ventas ↔ extracto** | 38 |
| F14.7 A4 Copiloto conversacional (intent classifier) | **US-A4-CONV-002 ⭐ clasificador prescriptivo** | 40 |
| F14.8 A5 Predicción de Stock | Promedio móvil + tendencia + estacionalidad | 32 |
| F14.9 A6 Arqueo Inteligente F3 | Predicción base + detección fraude inverso | 13 |
| F14.10 Eval continua intensificada | Red-team monthly primer trimestre A4 | 12 |

### E15 · Escala F3+ (35 stories · ~195 SP)

| Feature | SP |
|---|:-:|
| F15.1 Multi-sede avanzado (benchmark, traslados auto) | 30 |
| F15.2 Multi-país: **Perú (SUNAT)** + Ecuador (SRI) + México (SAT) | 85 |
| F15.3 OpenSearch + búsqueda histórica | 28 |
| **F15.4 Microservicios** (AI Service + Fiscal Worker primero) | 30 |
| F15.5 Cross-region + alta escalabilidad | 12 |
| F15.6 Hardware + Periferia (térmicas, biométrico) | 10 |

### Criterios éxito F3 mes 30

✅ B1: 50 tenants · B2: $18M MRR · Multi-país: ≥ 1 país adicional producción · A3 conciliación ≥ 90% auto · A4 conversacional < 0.5% prescriptivas · S5 costo IA < $25/tenant/mes · A6 detección fraude ≥ 1 caso validado · MOAT 18+ meses datos lote→plato

---

<a name="b12"></a>

## B12 — Procesos del Equipo + Ceremonias + Métricas

### Cadencia semanal

| Día | Ceremonia | Duración |
|---|---|---|
| Lunes | Sprint Planning (quincenal) | 2h |
| Martes | Daily 9am + Tech Sync 30min | 45 min |
| Miércoles | Daily 9am + Backlog Grooming 1h | 1h 15 min |
| Jueves | Daily 9am + Customer Success Sync (quincenal) | 45 min |
| Viernes | Daily 9am + Sprint Review + Demo + Retro (quincenal) | 3h cada 2 semanas |

### Métricas operativas (Sprint Review)

| Métrica | Target |
|---|---|
| Velocity por sprint | 50-65 SP F1 / 80-120 SP F2 |
| Sprint completion rate | ≥ 85% |
| Cobertura tests | ≥ 70% F1, ≥ 80% F2 |
| Bug escape rate | < 5% |
| DoR rejection rate | 10-20% saludable |
| Carry-over % | < 15% |
| Code review p50 | < 24h |
| CI pass first try | ≥ 80% |

### Comunicación con stakeholders

| Stakeholder | Update | Frecuencia |
|---|---|---|
| Mariana (911) | Sprint Demo + email weekly | Quincenal + semanal |
| CFO | Burn-rate + métricas | Mensual |
| Inversionistas | Métricas + roadmap + lecciones | Trimestral |
| Cohorte Hardcore AI | Avance + bloqueos | Mensual |
| Carolina (Contadora) | Cambios DIAN + buzón | Mensual |
| Equipo interno YARO | Estado completo | Semanal (All Hands) |

### Hot-fix process

```mermaid
flowchart LR
    DETECT["🚨 Incidente"] --> SEV{Severidad}
    SEV -->|CRITICAL| WAR["War room + runbook"]
    SEV -->|HIGH| HOT["Hot-fix branch + senior"]
    SEV -->|MEDIUM/LOW| BACKLOG["Sprint próximo"]
    WAR --> FIX["Fix + test + review"]
    HOT --> FIX
    FIX --> DEPLOY["Deploy con MFA"]
    DEPLOY --> POSTMORTEM["Post-mortem < 48h"]
    POSTMORTEM --> COMP["Ticket compensación obligatorio"]

    style WAR fill:#c0392b,color:#ffffff
```

### Onboarding nuevos ingenieros (< 1 día setup local + 2 semanas hasta primer PR mergeado)

---

<a name="adendum-arqueo"></a>

## Adendum — Arqueo Extendido (basado en Excel real 911)

> Capturado del Excel "CIERRE DE CAJA 911 HOT BURGERS.xlsx" + workflow verbal de Mariana.

### Análisis del Excel actual

**Hoja 1 — DATOS (catálogos):**
- 15 PERSONAL · 2 SEDES (Guarne, La Ceja) · 2 TURNOS (1, 2) · 5 DESCUENTOS (Cortesías, Influencers, Empleados, Errores Cocina, Promociones) · 3 ADMIN (autorizadores) · 8 PERSONAL ACTIVO (para ventas a colaboradores)

**Hoja 2 — CIERRE DE CAJA (5 bloques):**
1. **Cabecera:** FECHA · SEDE · TURNO · RESPONSABLE · HORA APERTURA · HORA CIERRE
2. **BILLETES:** 7 denominaciones (100K, 50K, 20K, 10K, 5K, 2K, 1K) × cantidad × total auto
3. **MONEDAS:** 5 denominaciones ($1K, $500, $200, $100, $50) × cantidad × total auto
4. **DETALLE:** 30 transferencias · 30 datáfono · 30 ventas a colaboradores · 17 descuentos con autorizador
5. **COMPARACIÓN:** Total YARO calculado AUTO vs Total Toteat MANUAL → diferencia

### Nuevas features capturadas (que no estaban en mi modelo original)

#### F1.17 — Ventas a colaboradores (8 stories · ~30 SP)

> Comida que se da a empleados. **Descuento de nómina obligatorio en F2 (M9).**

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-COL-001 | Cajero registra venta a colaborador con dropdown personal activo | Cajero | 5 |
| US-COL-002 | Cajero ingresa valor + comentario opcional | Cajero | 3 |
| US-COL-003 | Sistema acumula ventas a colaborador por empleado/mes | Sistema | 5 |
| US-COL-004 | Andrés ve histórico de ventas a colaboradores con filtros | Andrés | 3 |
| **US-COL-005 ⭐** | Sistema descuenta automáticamente de nómina (F2 — integración M9) | Sistema | 8 |
| US-COL-006 | Mariana ve top colaboradores con más consumo | Mariana | 3 |
| US-COL-007 | Admin configura empleados habilitados para ventas internas | Admin | 3 |

#### F1.18 — Control de descuentos estructurado (6 stories · ~25 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-DESC-001 | Cajero selecciona motivo descuento de catálogo (5 tipos) | Cajero | 3 |
| US-DESC-002 | Cajero selecciona autorizador del descuento (dropdown ADMIN) | Cajero | 3 |
| US-DESC-003 | Sistema captura valor + motivo + autorizador en `audit_events` | Sistema | 5 |
| US-DESC-004 | Admin configura catálogo de motivos por tenant | Admin | 5 |
| US-DESC-005 | Andrés ve reporte de descuentos por motivo / autorizador / cajero | Andrés | 5 |
| US-DESC-006 | Sistema alerta a Mariana si descuentos > umbral % ventas mensual | Mariana | 4 |

#### F1.19 — Modo "transición desde Toteat" (3 stories · ~13 SP) — solo migración 1-2 meses

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-TRANS-001 | Andrés ingresa manualmente totales Toteat al cerrar turno | Andrés | 5 |
| US-TRANS-002 | Sistema calcula diferencia YARO vs Toteat con alerta visual | Sistema | 3 |
| US-TRANS-003 | Admin habilita/deshabilita "modo comparación Toteat" por sede | Admin | 5 |

### Gestión de efectivo cross-day (lo que NO estaba en el Excel principal pero Mariana describió)

> El efectivo disponible para consignar se guarda en caja fuerte mientras se consigna. Otra hoja del Excel acumula día a día. Cuando se consigna, se resta del acumulado.

#### F1.14 — Base y efectivo disponible (3 stories · ~13 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-ARQ-BASE-001 | Cajero define base que queda para el día siguiente | Cajero | 3 |
| US-ARQ-BASE-002 | Sistema calcula efectivo disponible = contado - base | Sistema | 5 |
| US-ARQ-BASE-003 | Sistema sugiere base según promedio histórico (≥ 7 días) | Cajero/Admin | 5 |

#### F1.15 — Acumulación + visibilidad efectivo en caja fuerte (3 stories · ~13 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| **US-CASH-001 ⭐** | Sistema mantiene saldo running de efectivo disponible por sede | Sistema | 5 |
| US-CASH-002 | Andrés ve dashboard de efectivo acumulado por sede con histórico | Andrés | 5 |
| US-CASH-003 | Andrés ve alerta si efectivo acumulado supera umbral configurable (ej. $5M) | Andrés | 3 |

#### F1.16 — Consignaciones bancarias (5 stories · ~23 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| **US-CONS-001 ⭐** | Admin/Jefe registra consignación con monto + banco + responsable | Andrés | 5 |
| US-CONS-002 | Admin sube foto del comprobante de consignación a S3 | Andrés | 5 |
| US-CONS-003 | Sistema resta consignación del efectivo disponible y actualiza saldo | Sistema | 5 |
| US-CONS-004 | Andrés ve histórico de consignaciones por sede + filtros | Andrés | 5 |
| US-CONS-005 | Mariana ve reporte cross-sede de efectivo + consignaciones | Mariana | 3 |

### Conteo por denominación (3 stories · 13 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-ARQ-DENOM-001 | Cajero ingresa # billetes por denominación (teclado numérico touch) | Cajero | 5 |
| US-ARQ-DENOM-002 | Cajero ingresa # monedas por denominación | Cajero | 3 |
| US-ARQ-DENOM-003 | Sistema calcula y muestra total efectivo contado tiempo real | Cajero | 5 |

### Comparación detallada por método (2 stories · 8 SP)

| Código | Story | Persona | SP |
|---|---|---|:-:|
| US-ARQ-COMP-001 | Sistema compara total contado vs total YARO por método | Cajero | 5 |
| US-ARQ-COMP-002 | Sistema muestra diferencia por método con comentario obligatorio | Cajero | 3 |

### Nuevas tablas en el schema (F1 sube de 28 a 37 tablas)

```sql
-- 1. Catálogo de denominaciones por país
CREATE TABLE denominaciones_efectivo (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  country_code    CHAR(2) NOT NULL REFERENCES country_config(code),
  tipo            denominacion_tipo_enum NOT NULL,  -- BILLETE | MONEDA
  valor           BIGINT NOT NULL,
  nombre          TEXT NOT NULL,
  activo          BOOLEAN NOT NULL DEFAULT TRUE,
  orden_display   INT NOT NULL
);

-- 2. Detalle del arqueo por denominación
CREATE TABLE arqueo_denominaciones (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  arqueo_id       UUID NOT NULL REFERENCES arqueos(id),
  denominacion_id UUID NOT NULL REFERENCES denominaciones_efectivo(id),
  cantidad        INT NOT NULL CHECK (cantidad >= 0),
  subtotal        BIGINT NOT NULL,
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 3. Saldo de efectivo disponible por sede (running balance)
CREATE TABLE efectivo_disponible (
  id                UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id         UUID NOT NULL REFERENCES tenants(id),
  sede_id           UUID NOT NULL REFERENCES sedes(id),
  saldo_actual      BIGINT NOT NULL DEFAULT 0,
  ultima_actualizacion TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 4. Movimientos del saldo (auditoría append-only)
CREATE TABLE efectivo_movimientos (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL REFERENCES tenants(id),
  sede_id         UUID NOT NULL REFERENCES sedes(id),
  tipo            mov_efectivo_enum NOT NULL,   -- DEPOSITO_DIA | CONSIGNACION | AJUSTE
  monto           BIGINT NOT NULL,
  saldo_anterior  BIGINT NOT NULL,
  saldo_nuevo     BIGINT NOT NULL,
  referencia_id   UUID,                          -- FK a arqueo_id o consignacion_id
  responsable_id  UUID NOT NULL REFERENCES users(id),
  comentario      TEXT,
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 5. Consignaciones bancarias
CREATE TABLE consignaciones (
  id                  UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id           UUID NOT NULL REFERENCES tenants(id),
  sede_id             UUID NOT NULL REFERENCES sedes(id),
  monto               BIGINT NOT NULL,
  banco               TEXT NOT NULL,
  cuenta_destino      TEXT NOT NULL,
  fecha_consignacion  DATE NOT NULL,
  responsable_id      UUID NOT NULL REFERENCES users(id),
  registrado_por      UUID NOT NULL REFERENCES users(id),
  comprobante_s3_key  TEXT,
  numero_consignacion TEXT,
  estado              estado_consignacion_enum NOT NULL DEFAULT 'REGISTRADA',
  comentario          TEXT,
  created_at          TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 6. Ventas a colaboradores
CREATE TABLE ventas_colaboradores (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL REFERENCES tenants(id),
  sede_id         UUID NOT NULL REFERENCES sedes(id),
  turno_id        UUID NOT NULL REFERENCES turnos(id),
  cajero_id       UUID NOT NULL REFERENCES users(id),
  colaborador_id  UUID NOT NULL REFERENCES users(id),
  valor           BIGINT NOT NULL,
  comentario      TEXT,
  descontado_en_nomina BOOLEAN NOT NULL DEFAULT FALSE,
  liquidacion_id  UUID,  -- FK a liquidacion cuando se descuente (F2)
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 7. Catálogo motivos descuento
CREATE TABLE motivos_descuento (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL REFERENCES tenants(id),
  nombre          TEXT NOT NULL,  -- CORTESIAS, INFLUENCERS, etc.
  activo          BOOLEAN NOT NULL DEFAULT TRUE,
  requiere_autorizacion BOOLEAN NOT NULL DEFAULT TRUE,
  orden_display   INT NOT NULL
);

-- 8. Descuentos aplicados con autorizador
CREATE TABLE descuentos_aplicados (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL REFERENCES tenants(id),
  cobro_id        UUID REFERENCES cobros(id),
  motivo_id       UUID NOT NULL REFERENCES motivos_descuento(id),
  valor           BIGINT NOT NULL,
  autorizado_por  UUID NOT NULL REFERENCES users(id),
  cajero_id       UUID NOT NULL REFERENCES users(id),
  comentario      TEXT,
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now()
);

-- 9. Comparación con sistema externo (modo transición)
CREATE TABLE comparacion_externa (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL REFERENCES tenants(id),
  arqueo_id       UUID NOT NULL REFERENCES arqueos(id),
  sistema_externo TEXT NOT NULL,
  total_efectivo  BIGINT,
  total_datafono  BIGINT,
  total_transferencia BIGINT,
  total_colaboradores BIGINT,
  total_descuentos BIGINT,
  total_general   BIGINT NOT NULL,
  diferencia      BIGINT,
  comentario      TEXT
);

-- Extender tabla arqueos
ALTER TABLE arqueos ADD COLUMN total_efectivo_contado BIGINT;
ALTER TABLE arqueos ADD COLUMN base_dia_siguiente BIGINT DEFAULT 0;
ALTER TABLE arqueos ADD COLUMN efectivo_para_consignar BIGINT;
```

### Resumen impacto adendum Arqueo

| Aspecto | Antes | Después |
|---|---|---|
| **Tablas F1** | 28 | **37** (+9) |
| **Stories F1** | ~265 | **~298** (+33 stories nuevas) |
| **SP F1** | ~1.050 | **~1.180** (+130 SP) |
| **Épica E1** | 50 stories / 180 SP | **83 stories / 250 SP** |
| **Agentes IA** | 5 | **6** (+ A6 Arqueo Inteligente) |

---

<a name="apendices"></a>

## Apéndices

### Trazabilidad cross-documentos

| Documento | Cómo se conecta con el backlog |
|---|---|
| `specs/prd.md` | Cada historia linkea a FR/NFR/Principio/KPI |
| `specs/arquitectura.md` | Cada historia linkea a segmento + ADR |
| `docs/overview.md` | Contexto HORECA Colombia |
| `docs/critica.md` | Riesgos técnicos que las stories mitigan |
| `docs/pvb.md` | Visión + MOAT (US-CDP-009 implementa MOAT principal) |
| `docs/icp.md` | 3 perfiles (Mariana, Andrés, Carolina) en cada historia |
| `CIERRE DE CAJA 911.xlsx` | Workflow real del piloto → adendum Arqueo |

### Glosario de códigos

- **US-XXX-NNN** — User Story
- **T-CAT-NNN** — Task técnica
- **E[N]** — Épica
- **F[N].M** — Feature
- **A[N]** — Agente IA
- **P[N]** — Principio no negociable (1-10)
- **R[N]** — Riesgo (1-10)
- **C[N]** — Conflicto Paso 0 PRD (1-9)
- **CA[N]** — Conflicto Arquitectónico Paso 0 (1-10)
- **CB[N]** — Conflicto Backlog Paso 0 (1-8)
- **YR-NNN** — Ticket A13 plan técnico
- **⭐** — Story crítica con DoD especial

### Resumen ejecutivo del backlog completo

| Categoría | Cantidad |
|---|---|
| Épicas | **15** (12 F1 + 3 F2-F3) |
| User stories F1 | **~298** (con adendum Arqueo) |
| User stories F2 | **~56** |
| User stories F3 | **~50** |
| Tasks técnicas | **~51** |
| **Total ítems** | **~455** |
| SP F1 | **~1.180** |
| SP F2 | **~480** |
| SP F3 | **~330** |
| **Total SP** | **~1.990** |

### Cambios vs PRD/Arquitectura original

1. **6 agentes IA** (no 5) — A6 Arqueo Inteligente agregado tras análisis Excel real
2. **37 tablas F1** (no 28) — adendum Arqueo agrega 9 tablas
3. **~298 stories F1** (no ~265) — +33 stories de Arqueo Extendido
4. **Modo "transición Toteat"** como feature temporal (1-2 meses sunset)
5. **Ventas a colaboradores** integradas con M9 Nómina (F2)
6. **Control de descuentos estructurado** con autorizador explícito (5 motivos catálogo)
7. **Gestión de efectivo cross-day** con consignaciones formalizadas (US-CONS-*)

---

*YARO Backlog · v1.0 consolidado · Mayo 2026*
*Co-creado entre la fundadora-operadora (911 Hot Burger) y un Product Engineering Lead + Agile Coach.*
*Documento base: `specs/prd.md`, `specs/arquitectura.md`, `docs/`, y el Excel "CIERRE DE CAJA 911 HOT BURGERS.xlsx".*
*Output guardado en: `specs/backlog.md`.*
