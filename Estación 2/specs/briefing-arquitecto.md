# Briefing para el arquitecto · YARO

> **Documento de contexto** para validar [`arquitectura-diagrama.html`](./arquitectura-diagrama.html).
> Léelo antes de los diagramas — sin este contexto las decisiones parecen arbitrarias.

- **Autora:** Valentina Muñoz Tangarife · Fundadora 911 Hot Burger · CEO YARO
- **Fecha:** Mayo 2026
- **Versión:** v1.0
- **Tiempo de lectura:** ~10 minutos
- **Documentos anexos:**
  - `arquitectura-diagrama.html` — 15 vistas con preguntas específicas (entrada principal)
  - `arquitectura.md` — 1.925 líneas con 10 ADRs detallados (consulta profunda)
  - `prd.md` — Producto y casos de uso (si necesitás entender "el por qué" del negocio)
  - `backlog.md` — Roadmap ejecutable F1-F4 (solo si vas a estimar)

---

## 1. Qué es YARO en una frase

> **YARO es el sistema operativo SaaS B2B multi-tenant para restaurantes colombianos formalizados** — reemplaza el patchwork de POS + Excel de cierre de caja + contador externo + WhatsApp + carpeta de facturas físicas con **una sola plataforma** que integra: punto de venta, inventario, cumplimiento DIAN (tiquete + factura electrónica + nómina), conciliación bancaria, contabilidad PUC, CRM básico y **6 agentes de IA** que automatizan tareas operativas.

**Diferenciador clave:** no competimos con Toteat (POS) ni con Alegra (contabilidad) por separado. **Competimos con la suma de los dos + Excel + el contador.** La hipótesis: el dueño de restaurante prefiere pagar UNA mensualidad a un sistema que entiende su flujo completo, antes que mantener 4 herramientas desconectadas.

**Origen:** Yo (Valentina) fundé 911 Hot Burger en 2023. El dolor que estoy resolviendo es el mío propio antes que el de un mercado abstracto.

---

## 2. Tamaño objetivo y dimensionamiento

| Métrica | F1 (Mes 8) | F2 (Mes 14) | F3 (Mes 24) |
|---|---|---|---|
| **Tenants activos** | 100 | 500 | 2.000 |
| **Sedes promedio/tenant** | 1,2 | 1,5 | 2,0 |
| **Transacciones/min pico (por tenant)** | ~50 | ~80 | ~120 |
| **Transmisiones DIAN/día (sistema)** | ~120k | ~600k | ~2,4M |
| **Storage por tenant (12 meses)** | ~2 GB | ~3 GB | ~5 GB |
| **Latencia P95 POS (objetivo)** | < 200 ms | < 150 ms | < 100 ms |

**Implicación:** la arquitectura debe ser **suficiente para F1 sin sub-dimensionar F3**. No queremos un rewrite en F2. Sí aceptamos refactors quirúrgicos (ej: extraer 1-2 microservicios, mover de Fargate On-Demand a SPOT, agregar read replicas).

---

## 3. Equipo y presupuesto (lo que define qué es realista)

- **Equipo F1:**
  - 1 fundadora técnica full-stack (yo, Valentina) — full-time
  - 2-3 ingenieros senior contratados
  - 0 DevOps dedicado → infra como código obligatoria
  - 0 QA dedicado en F1 → tests automatizados desde día 1
- **Capacidad estimada F1:** ~1.440 story points en 8 meses (108% velocity → ajustado)
- **Presupuesto AWS F1:** target ~$1.500/mes (Fargate + RDS Multi-AZ + ElastiCache + S3)
- **Anthropic API:** ~$800/mes estimado (Sonnet capa 1 + Haiku para tareas baratas)

**Implicación arquitectónica clave:**
> Microservicios desde día 1 sería **suicida** con este equipo. El plan es **monolito modular bien diseñado** con extracción de servicios en F3+ si el load lo justifica. Por eso los Bounded Contexts están separados a nivel de módulo, no de despliegue.

---

## 4. Restricciones legales NO negociables (Colombia)

Estas son las restricciones que **explican muchas decisiones que parecen raras** si venís de un contexto que no es Colombia/LATAM:

### 4.1 DIAN — Facturación electrónica

- **Tiquete POS (04):** transmisión obligatoria de cada venta retail al consumidor final en < 24h. Validación de CUFE (Código Único de Factura Electrónica) firmado con XAdES-BES.
- **Factura Electrónica (01):** para clientes B2B con NIT. Firma + validación DIAN antes de entregar al cliente.
- **Contingencia (03):** si DIAN está caído, el restaurante debe **seguir vendiendo**. Almacenamos local + transmitimos cuando vuelva el servicio.
- **Nota Crédito (91):** anulación o devolución parcial. Inmutable.
- **DSCE (05):** documento soporte de adquisiciones a no obligados (proveedor sin RUT formal).
- **Nómina electrónica:** transmisión mensual.
- **Operador autorizado DIAN:** decidimos **Facture.co** (lo cubre todo). Habilitarnos como operador propio = 3-6 meses + certificación carísima. Postergado a F3+.

### 4.2 Apéndable-only obligatorio

- **Ley 1314 (contabilidad NIIF):** los libros contables **no se pueden editar ni borrar** durante 10 años fiscales. Solo se corrigen con notas crédito/débito.
- **Implicación:** triggers SQL `BEFORE DELETE/UPDATE` en tablas fiscales bloquean cualquier modificación. Soft-delete está **prohibido** en estas tablas (P1).

### 4.3 Ley 1581 (Habeas Data)

- Registro obligatorio en SIC (Superintendencia de Industria y Comercio).
- Derechos ARCO (Acceso, Rectificación, Cancelación, Oposición) accionables por el titular.
- **Implicación:** PII de clientes (CRM) requiere consent log + export + deletion endpoints desde F1.

### 4.4 Decreto 358 / Resolución 165 (POS específico)

- Identificación del cajero en cada transacción.
- Numeración consecutiva sin saltos.
- Arqueo de caja documentado.

---

## 5. Los 10 principios no negociables (P1–P10)

Son los axiomas de la arquitectura. Si los cuestionás, todo el resto colapsa. **Léelos antes de los diagramas.**

| # | Principio | Implementación |
|---|---|---|
| **P1** | **Append-only fiscal** | Triggers SQL bloquean UPDATE/DELETE en tablas fiscales. Notas crédito para corrección. |
| **P2** | **Multi-tenancy paranoico (4 capas defensa)** | JWT (capa 1) → Guards NestJS (capa 2) → CLS namespace (capa 3) → Prisma middleware (capa 4) inyectando `tenant_id` en todas las queries. Tests de aislamiento obligatorios. |
| **P3** | **Fire-and-forget para DIAN** | UI nunca espera respuesta DIAN. Cobro se registra local, transmisión va a BullMQ + Valkey. Si falla, retry con backoff. Status visible en cola. |
| **P4** | **Offline-first POS** | Service Worker + Workbox + IndexedDB. `@yaro/xml-builder` compartido cliente/servidor. Reconciliación al volver online. |
| **P5** | **IA verificable, nunca prescriptiva** | Todo output de agente IA muestra fuentes + confianza. Acción crítica requiere confirmación humana. Eval continua con judge LLM. |
| **P6** | **Plug-in country adapters** | Lógica fiscal aislada en `@yaro/fiscal-co` (Colombia). F4 multi-país = nuevo módulo, no rewrite. |
| **P7** | **Eventos antes que llamadas directas** | Outbox pattern + EventBridge (F3+). En F1: in-process events + transactional outbox para fiscal. |
| **P8** | **Observabilidad first-class** | OpenTelemetry desde día 1. Logs estructurados + traces + métricas. Sentry para errores. |
| **P9** | **Cero secrets en código** | AWS Secrets Manager + IAM roles. Variables no sensibles en SSM Parameter Store. |
| **P10** | **CI/CD con rollback < 5 min** | GitHub Actions + Fargate blue/green. Migraciones SQL backward-compatible 2 versiones. |

---

## 6. Lo que YA está decidido (no abrir esta caja)

Esto NO es para validar. Está decidido y la organización está alineada. Si querés debatirlo, abrimos otra reunión, pero **no en esta validación de arquitectura**.

| Capa | Decisión | Por qué |
|---|---|---|
| **Lenguaje backend** | TypeScript + NestJS | Reutilización con Next.js frontend, ecosistema, tipado fuerte |
| **Frontend** | Next.js 14 App Router + React Server Components | SSR para SEO landing, Server Actions para POS |
| **ORM** | Prisma | Type-safety end-to-end, middleware para multi-tenancy |
| **DB principal** | PostgreSQL 16 (RDS Multi-AZ) | Triggers, JSON, FTS, ACID |
| **Cache + Queue** | Valkey (fork OSS de Redis) en ElastiCache | BullMQ requirement, fork open-source post-licencia Redis |
| **Workers** | BullMQ | Maduro, dashboard, retries, priorities |
| **Compute** | AWS Fargate (ECS) — On-Demand para API, SPOT para workers | No queremos administrar EC2/K8s |
| **Operador DIAN** | Facture.co | TCO inferior a habilitarnos propios. Plan B Carvajal en F3+ |
| **LLM** | Anthropic Claude (Sonnet 4 capa 1, Haiku capa 2) | Opt-out training, latencia aceptable, UI clara |
| **Auth colaboradores** | Google OAuth + sessions | ~80% gmail en Colombia |
| **Auth dueños** | Email + password + 2FA TOTP | Persistente, control total |
| **IaC** | Terraform | Estándar, módulos reutilizables |
| **CI/CD** | GitHub Actions | Mismo provider del repo |
| **Observabilidad** | OpenTelemetry → Grafana Cloud + Sentry | Costo razonable, no vendor lock-in |

---

## 7. Las 5 preguntas que SÍ querés que respondas

Sin esto, el arquitecto devuelve un essay genérico de 30 páginas que no nos ayuda a decidir. Estas son las **metapreguntas estratégicas** (distintas a las preguntas técnicas por vista del HTML):

### Pregunta 1 — Escalabilidad sin rewrite
> **¿La arquitectura sirve para 100 tenants F1 Y para 5.000 F3 sin rewrite estructural?**
> Si NO, ¿dónde está el rewrite inevitable y cuándo dispararlo?
> Si SÍ, ¿qué refactor incremental necesitamos en F2 y F3?

### Pregunta 2 — Riesgo operacional #1
> **¿Cuál es el cuello de botella real que nos va a romper primero?**
> No el bonito ("podría escalar mejor"). El feo ("esto te va a quemar en producción a los X tenants").
> Candidatos sospechosos:
> - Cola DIAN saturada con backoff exponencial
> - Conexiones Postgres agotadas por workers + API
> - Latencia Anthropic API en agentes IA capa 1
> - Multi-tenancy Prisma middleware si un dev olvida `tenant_id`

### Pregunta 3 — Sobre-ingeniería en F1
> **¿Qué eliminarías de F1 para reducir 20% de complejidad sin perder valor?**
> Sospechas internas:
> - ¿Outbox pattern desde F1 o esperar F3?
> - ¿RLS Postgres + Prisma middleware o solo middleware?
> - ¿OpenTelemetry distributed tracing desde F1 o solo logs estructurados?
> - ¿6 agentes IA en F1 o priorizar 3 y diferir 3?

### Pregunta 4 — Sub-ingeniería en F1
> **¿Qué agregarías a F1 que es barato ahora y carísimo después?**
> Ej clásico: si en F1 no separamos `audit_log` en su propia DB, en F3 cuando tenga 200M rows el migrate va a ser sangriento.

### Pregunta 5 — Realismo del plan
> **¿El alcance F1 (1.440 SP · 3 ingenieros · 8 meses) es realista o estamos delirando?**
> Mi sensación: tight pero alcanzable. ¿Coincidís?
> Si NO, ¿qué cortamos del scope F1 sin matar la propuesta de valor?

---

## 8. Glosario express (5 minutos)

Términos que aparecen en los diagramas y que un arquitecto sin contexto Colombia/LATAM no necesariamente conoce:

| Término | Significado |
|---|---|
| **DIAN** | Dirección de Impuestos y Aduanas Nacionales · ente fiscal de Colombia (equivalente a SAT México / IRS USA) |
| **CUFE** | Código Único de Factura Electrónica · hash firmado XAdES-BES que valida cada transmisión |
| **CUDE** | Código Único de Documento Electrónico (variante para tiquete POS) |
| **RADIAN** | Registro de la Factura Electrónica como Título Valor · buzón de FE recibidas |
| **MUISCA** | Plataforma DIAN para servicios fiscales |
| **Tiquete POS (04)** | Tipo de doc electrónico para venta al consumidor final sin NIT |
| **Factura Electrónica (01)** | Tipo de doc electrónico para venta B2B con NIT |
| **NC (91)** | Nota Crédito · anulación o devolución parcial |
| **ND (92)** | Nota Débito · ajuste al alza |
| **DSCE (05)** | Documento Soporte de Adquisición a no obligado a facturar electrónicamente |
| **PUC** | Plan Único de Cuentas · catálogo contable colombiano estandarizado |
| **NIIF** | Normas Internacionales de Información Financiera · marco contable obligatorio en Colombia desde 2015 |
| **Habeas Data** | Ley 1581/2012 · protección de datos personales (equivalente a LGPD Brasil / GDPR EU) |
| **SIC** | Superintendencia de Industria y Comercio · regulador de habeas data |
| **POS (Punto de Venta)** | Aquí significa el módulo de venta de YARO, no el hardware |
| **KDS** | Kitchen Display System · pantalla en cocina con comandas |
| **CDP** | Centro de Producción · cocina central que produce para varias sedes |
| **Arqueo de caja** | Cierre diario · cuadre de efectivo · denominaciones billete/moneda · transferencias · datáfono |
| **Tenant** | En YARO: un restaurante (puede tener N sedes) |
| **BC** | Bounded Context (DDD) · 10 en YARO |
| **Fire-and-forget** | Patrón donde UI no espera respuesta del worker — devuelve OK al usuario y el worker procesa async |
| **Append-only** | Tablas donde solo se INSERTA · UPDATE/DELETE bloqueado por triggers |

---

## 9. Cómo trabajamos esta validación

**Formato preferido de tu feedback** (en orden de utilidad):

1. **Punch list accionable** · "Cambia X por Y porque Z" — feedback que podemos implementar mañana
2. **Riesgos con probabilidad estimada** · "Esto tiene 60% probabilidad de romperse a los 500 tenants"
3. **Trade-offs alternativos** · "Considerá A en lugar de B porque [...]"
4. **Validación silenciosa** · "Esto está bien" (silencio implícito sobre lo que no comentás)

**Lo que NO necesitamos:**
- Essay genérico sobre microservicios
- Recomendaciones de stack distinto (ver §6)
- "Depende" sin condiciones específicas

**Tiempo estimado de validación:** 4-6 horas (lectura HTML + briefing + decisiones priorizadas).

**Entregable esperado:** documento o sesión 1h donde respondas las 5 preguntas + decisiones por vista marcadas como ✅ / ⚠️ / ❌.

---

## 10. Cómo contactarme

- **Email:** yara63valentina@gmail.com
- **GitHub:** ValentinaX63
- **Slack/Discord:** [a definir]
- **Disponibilidad para sync:** lunes a viernes 9am-6pm Colombia (UTC-5)

---

> **Cierre:** este briefing es la versión "tldr" del contexto. Si en cualquier momento sentís que necesitás profundizar en un punto específico, el archivo `arquitectura.md` tiene los 10 ADRs completos con trade-offs detallados, alternativas consideradas y rationale.
>
> Gracias por el tiempo. La idea no es que aceptes todo — es que cuestiones lo que tenga que ser cuestionado **antes** de que escribamos código.
