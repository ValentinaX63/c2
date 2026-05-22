# PRD — YARO (Your All-In-One Restaurant Operations)
### SaaS 360 para el sector HOREKA · v3.0

---

## 1) Resumen

**YARO** es una plataforma SaaS multi-tenant para la operación, gestión y administración del sector HOREKA (Hoteles, Restaurantes y Catering). Centraliza en un único sistema todo lo que hoy vive repartido entre múltiples herramientas: punto de venta (POS), cocina en tiempo real (KDS), inventario, centro de producción (CDP), contabilidad con integración DIAN, nómina electrónica, gestión de talento humano y conciliación bancaria.

YARO sirve cualquier tipo de operador del sector — desde una persona natural con un solo local hasta una cadena con múltiples sedes y centro de producción propio. El sistema se adapta al régimen tributario, al tamaño y al nivel de formalización contable de cada negocio, sin forzar una configuración única.

El mercado inicial es Colombia. La arquitectura está diseñada para escalar a cualquier país de Latinoamérica adaptando exclusivamente la capa normativa (plan de cuentas, documentos fiscales, tasas de impuesto, reglas de retención) sin modificar el núcleo del sistema.

---

## 2) Objetivos

- Operar un restaurante completo (POS, cocina, inventario, producción, contabilidad) desde una sola plataforma, adaptada al tamaño y régimen de cada negocio.
- Cumplir con la normativa fiscal colombiana desde el primer día: tiquete POS electrónico y factura electrónica en Fase 1, nómina electrónica y documentos complementarios en Fase 2.
- Funcionar con o sin internet — el POS y el KDS operan en modo offline y sincronizan con la DIAN al reconectarse dentro del plazo de contingencia de 48 horas.
- Garantizar la integridad de todos los documentos fiscales con un modelo de datos append-only: ningún documento transmitido puede editarse ni eliminarse.
- Transmitir documentos DIAN de forma asíncrona mediante el patrón fire-and-forget: el cajero recibe confirmación de cobro en menos de 100ms sin esperar la respuesta de Facture.co.
- Escalar a multi-sede y multi-país sin reescribir el núcleo del sistema.
- Reducir los procesos manuales de validación y consolidación entre herramientas, incluyendo la conciliación bancaria.

### No objetivos (fuera del alcance actual)

- Módulo de reservas de mesa — backlog Fase 3.
- Integración con plataformas de delivery (Rappi, iFood) — backlog Fase 3.
- Kiosco de autoservicio — backlog.
- Operador FE propio ante la DIAN — se usa intermediario (Facture.co) en todas las fases.
- Open Banking / conexión directa a APIs bancarias — fuera del alcance. La conciliación usa importación de extractos CSV.
- Hardware biométrico (lector de huella) — arquitectura preparada vía API, implementación de hardware futura.
- Gestión de alérgenos avanzada — backlog Fase 3.
- Declaración de renta — fuera del alcance permanente. YARO es el origen del dato; el contador declara con la información que exporta YARO.
- App móvil nativa — el frontend Angular es responsive y funciona en móvil desde el navegador.

---

## 3) Roadmap de fases

Cada fase tiene una propuesta de valor completa y vendible por sí sola. No se requiere la siguiente para generar valor real al cliente.

### Fase 1 — "Opera y cumple" (meses 1–8)

**Propuesta de valor:** El restaurante opera digitalmente y cumple con la DIAN desde el primer día, con o sin internet.

Módulos:
- POS completo: plano de mesas, pedidos, cobro con descuento/propina/método de pago
- KDS cocina en tiempo real (WebSocket + Valkey Pub/Sub)
- Turno con apertura/cierre de caja y arqueo por método de pago
- **Modo offline básico**: POS y cobro funcionan sin internet; tiquetes encolados en IndexedDB se sincronizan con la DIAN al reconectarse (Workbox + SyncQueue)
- **Tiquete POS electrónico DIAN** (Tipo 04) — prioridad máxima
- **Factura electrónica de venta DIAN** (Tipo 01) — para clientes con NIT
- Buzón DIAN básico: recepción de FE de proveedores
- Panel de transmisiones: estado de cada documento con CUFE de confirmación, en tiempo real via WebSocket
- Inventario básico por sede: stock, alertas, stock mínimo
- Marcación de horario con formulario de bienestar (entrada/salida)
- Gestión de usuarios y roles con módulos configurables por el Superadmin
- Dashboard básico: ventas del turno, stock crítico, estado DIAN
- **YARO Console**: portal interno para el equipo de YARO (crear tenants, soporte, monitoreo)

**Schema Prisma Fase 1 — 14 tablas:**
`tenants · country_config · sedes · users · sessions · mesas · ordenes · productos · turnos · arqueos · fiscal_documents · sync_queue · attendance_records · clientes`

### Fase 2 — "Gestiona y controla" (meses 9–18)

**Propuesta de valor:** El restaurante tiene visibilidad financiera real, controla su producción, gestiona su nómina y simplifica la conciliación bancaria.

Módulos añadidos:
- Centro de Producción (CDP) completo: recepción, producción, empaque, traslados
- Jefe de cocina por sede: stock, traslados, mermas, confirmación de recepción
- Contabilidad operativa: gastos fijos con alertas, facturas de compra, P&G, balance
- Nota crédito electrónica (Tipo 91)
- Documento soporte de compras — DSCE (Tipo 05): para proveedores informales
- Nómina básica: liquidación por empleado, comprobantes PDF, parafiscales colombianos
- **Nómina electrónica DIAN**: documento soporte con los 47 campos requeridos
- Recetas con costeo real, food cost automático y factor de corrección por merma
- **Retenciones en la fuente**: agente de retención y sujeto de retención
- **Régimen Simple**: anticipos mensuales, diferenciación de IVA implícito
- Módulo contable configurable: formal (PUC) o simplificado (libro de ingresos/gastos)
- **Conciliación bancaria asistida**: reporte de ventas por método de pago con neto esperado en banco, comisiones estimadas y fecha de depósito proyectada

**Schema Prisma Fase 2 — +13 tablas:**
`inventario_items · lotes · traslados · recepciones · produccion_runs · recetas · mermas · gastos_fijos · empleados · nomina_liquidaciones · retenciones · proveedores · ordenes_compra`

### Fase 3 — "Escala e intelige" (meses 19–30)

**Propuesta de valor:** El restaurante tiene inteligencia operativa, conciliación bancaria automatizada y puede escalar a nuevos mercados.

Módulos añadidos:
- Prevalidador de promociones con IA
- Predictor de stock con IA (promedio móvil ponderado + detección de tendencia)
- **Conciliación bancaria automatizada**: importación de extracto bancario CSV, matching automático contra ventas YARO con tolerancia de comisiones, panel de excepciones para el contador
- Multi-sede avanzado: benchmark entre sedes, traslados automáticos sugeridos
- Integración delivery (Rappi, iFood)
- Reservas de mesa
- Módulo de capacitación del equipo
- **Expansión normativa a nuevos países**: Perú (SUNAT), Ecuador (SRI), México (SAT)
- Modo offline avanzado: sincronización completa de todos los módulos

**Schema Prisma Fase 3 — +4 tablas:**
`ai_reglas_puc · predicciones_stock · promotion_alerts · bank_reconciliation_items`

---

## 4) Usuarios, roles y casos de uso

### Roles del sistema

Los roles no son rígidos. Cada usuario tiene exactamente los módulos que necesita, configurados por el Superadmin de forma granular.

- **Superadmin del tenant**: acceso total. Configura sedes, aprueba cambios de costo en recetas, gestiona usuarios y módulos, ve datos financieros consolidados. Único que puede activar/desactivar funcionalidades por rol.
- **Admin de sede**: gestión operativa y administrativa de una o varias sedes. No puede aprobar cambios de costo sin delegación explícita.
- **Jefe CDP**: opera el centro de producción. Recibe insumos, programa producción, empaca y despacha traslados. No ve datos financieros ni nómina.
- **Jefe de cocina (por sede)**: ve el stock de su sede, solicita traslados al CDP, reporta mermas, consulta recetas (solo lectura), confirma recepción de traslados con posibilidad de reportar discrepancias.
- **Cajero / PdV**: abre y cierra turno del restaurante, opera el plano de mesas, cobra, aplica descuentos (comentario obligatorio), agrega propina, genera tiquetes POS o FE, realiza el arqueo de caja.
- **Mesero**: accede al plano de mesas, abre mesas, toma pedidos y genera pre-cuentas. No cobra ni ve datos financieros.
- **Cocina KDS**: ve la pantalla de cocina en tiempo real. Sin acceso financiero ni a ningún otro módulo.

### Flujo principal — turno de un restaurante

1. El cajero abre el turno (activa la operación para todos los usuarios de la sede).
2. El cajero declara la base de caja (efectivo inicial).
3. Meseros y cajeros operan el plano de mesas: abren mesas, agregan ítems, generan pre-cuentas.
4. El KDS recibe las órdenes en tiempo real en un kanban (Pendiente → En proceso → Listo).
5. El cajero cobra: método de pago, descuento (comentario obligatorio), propina, cliente, documento (tiquete POS o FE). El cobro responde en < 100ms; la transmisión DIAN ocurre de forma asíncrona.
6. El cajero realiza el arqueo: ingresa el total por cada método de pago. El sistema compara el total reportado vs el registrado. Si hay descuadre, requiere comentario obligatorio.
7. El cajero cierra la caja y cierra el turno. Los módulos operativos quedan bloqueados.

El turno es de la sede, no del cajero. Sin turno activo, ninguna operación puede ejecutarse.

### Flujo del Centro de Producción (CDP)

1. Jefe CDP recibe insumos y califica calidad (Bueno / Regular / Rechazado).
2. Programa recetas del día. El sistema calcula insumos, rendimiento esperado y tiempo.
3. Ejecuta producción. El sistema descuenta insumos del inventario CDP.
4. Al finalizar, confirma el rendimiento real. Las diferencias se registran como merma.
5. Empaca con etiqueta imprimible: lote, producto, cantidad, fecha producción, fecha vencimiento.
6. Genera traslado a sede destino con precio de transferencia libre.
7. El Jefe de cocina de la sede confirma la recepción física. El inventario de la sede se actualiza al confirmar — no al despachar. Puede reportar discrepancias.

**Estados del traslado:** `Solicitado → Aprobado → En camino → Entregado ✓ / Con discrepancia ⚠`

---

## 5) Alcance técnico — stack

### Backend
- **NestJS** (Node.js 22) · API REST + WebSocket (Socket.io para KDS) · desplegado en **AWS ECS Fargate**
- **PostgreSQL** (AWS RDS Multi-AZ) · ORM **Prisma** · multi-tenancy row-level por `tenant_id`
- **BullMQ** sobre **ElastiCache Valkey** · cola de jobs para transmisiones DIAN (fire-and-forget), generación de XMLs, predicciones IA
- **ElastiCache Valkey** (fork Redis) · también usado como Pub/Sub adapter de Socket.io para KDS multi-instancia y como caché de sesiones JWT

### Frontend
- **Angular 17+** SPA · TypeScript · standalone components · desplegado en **AWS S3 + CloudFront**
- **Signals** para estado local de componentes · **RxJS** solo para streams complejos (WebSocket, polling, cadenas asíncronas)
- **Workbox** para Service Worker y estrategias de caché (CacheFirst para menú/config, NetworkFirst para mesas/turno, StaleWhileRevalidate para assets)
- **idb** (IndexedDB wrapper) para `SyncQueue` offline — almacena cobros, marcaciones y eventos KDS cuando no hay internet
- **`@yaro/ui`** — librería de componentes Angular propia construida sobre los prototipos HTML existentes (ver sección 6)

### Infraestructura
- **AWS ECS Fargate**: servicios separados para API, WebSocket y Worker (BullMQ). Fargate Spot para workers de background (hasta 70% más barato)
- **AWS RDS PostgreSQL Multi-AZ**: failover automático < 60s
- **AWS ElastiCache Valkey**: Pub/Sub + BullMQ + sesiones
- **AWS S3**: XMLs DIAN (versionado activado, retención 5 años), PDFs, backups
- **AWS CloudFront**: CDN + WAF + SSL · región `us-east-1`
- **AWS SES**: email transaccional (facturas, alertas, onboarding)
- **Terraform** (IaC) · **GitHub Actions** (CI/CD: test → build Docker → push ECR → rolling deploy ECS)
- **Ambientes**: `development` (Docker Compose), `staging` (AWS reducido), `production` (AWS completo)

### Facturación electrónica e IA
- **Facture.co** (operador DIAN habilitado) · YARO genera XML UBL 2.1, Facture.co firma XAdES-BES y transmite a la DIAN
- **Claude Sonnet** via API Anthropic · con fallback de tres capas (IA → Motor de reglas → Default)

### Normativa por país
- Todos los valores fiscales (tasas, tipos de documento, cuentas PUC) viven en la tabla `country_config`, no en el código
- Un nuevo país = nueva fila de configuración, no un nuevo módulo
- Ruta de expansión: Perú (Fase 3) → Ecuador → México

---

## 6) Librería de componentes — `@yaro/ui`

YARO tiene múltiples pantallas prototipadas en HTML/CSS con el sistema de diseño completamente definido. Estas pantallas son la referencia visual de la librería — cada componente Angular debe verse idéntico al HTML original.

### Sistema de diseño (tokens)

```scss
// Superficies
--color-bg: #f0ede8       --color-surface: #faf9f7
--color-bg2: #ece9e3      --color-white: #ffffff

// Texto
--color-text: #1a1916     --color-text2: #6b6760     --color-text3: #a8a49e

// Acento
--color-accent: #f7fd9c   --color-accent-dk: #d4e200  --color-accent-txt: #6b7200

// Semáforos
--color-green: #3d9970    --color-red: #c0392b
--color-amber: #b7860b    --color-blue: #2471a3    --color-purple: #534AB7

// Sombras
--shadow-sm / --shadow-md / --shadow-lg

// Radios: --radius: 14px · --radius-sm: 9px
// Tipografía: --font-sans: 'Outfit', sans-serif
```

### Inventario de componentes (extraídos de los prototipos HTML)

**Átomos** (sin dependencias):
`YaroButton · YaroBadge · YaroInput · YaroToggle · YaroAvatar · YaroDelta`

**Moléculas** (componen átomos):
`YaroCard · YaroKpi · YaroStatBar · YaroStockBar · YaroNavItem · YaroFormRow · YaroInfoBox`

**Organismos** (componen moléculas):
`YaroModal · YaroDataTable · YaroSidenav · YaroTopbar · YaroBienestarPicker · YaroConnectionBadge · YaroPilloSede`

### Estructura del proyecto de la librería

```
libs/
└── yaro-ui/
    ├── src/
    │   ├── index.ts               ← barrel export de todo
    │   ├── styles/
    │   │   ├── tokens.scss        ← única fuente de verdad de variables CSS
    │   │   ├── reset.scss
    │   │   ├── typography.scss    ← Outfit + escalas
    │   │   └── animations.scss    ← floatUp, pulse, blink
    │   └── lib/
    │       ├── atoms/
    │       ├── molecules/
    │       └── organisms/
    ├── package.json
    └── ng-package.json
```

### Orden de construcción

1. **Semana 1** — `tokens.scss` + átomos (`YaroButton`, `YaroBadge`, `YaroInput`)
2. **Semana 2** — Moléculas (`YaroCard`, `YaroKpi`, `YaroInfoBox`)
3. **Semana 3** — Organismos (`YaroModal`, `YaroTopbar`, `YaroSidenav`)
4. **Semana 4+** — Módulos de negocio ensamblados con componentes de la librería

### Convenciones Angular 17

- Estado local de componentes: **Signals** (`signal()`, `computed()`)
- Streams complejos (WebSocket, polling, cadenas async): **RxJS**
- Todos los componentes son **standalone** (sin NgModules)
- `ChangeDetectionStrategy.OnPush` por defecto en todos los componentes

---

## 7) Requerimientos funcionales (FR)

### FR-01 Gestión de tenants y YARO Console

- El sistema soporta múltiples tenants con aislamiento absoluto por `tenant_id`. Todos los queries de Prisma incluyen este filtro forzado por middleware — ningún usuario puede acceder a datos de otro tenant.
- **YARO Console** (`console.yaro.app`) es el portal interno del equipo de YARO. Roles: `PLATFORM_ADMIN`, `SUPPORT_ENGINEER` (lectura + soporte), `SUPPORT_BASIC` (solo lectura).
- Al crear un tenant se configura: datos del negocio, tipo de entidad, régimen tributario, modo contabilidad, impoconsumo, agente de retención, plan, operador FE y ambiente DIAN.
- YARO Console incluye: gestión de tenants, monitor de transmisiones DIAN, logs del sistema, tickets de soporte, facturación de YARO y gestión del equipo interno.

### FR-02 Onboarding del tenant

Wizard de 3 pasos al activar un tenant nuevo:

**Paso 1 — Configuración fiscal:**
- ¿Persona jurídica o natural?
- ¿Régimen tributario? (Ordinario / SIMPLE / No sé)
- ¿Obligado a llevar contabilidad formal? → activa PUC o modo simplificado

**Paso 2 — Impoconsumo:**
- El sistema explica el tope (3.500 UVT ~$165M en 2025)
- Opciones: Activo / Inactivo / Monitor (alertar al acercarse)
- Nunca se activa automáticamente

**Paso 3 — Sedes, menú y usuarios iniciales**

### FR-03 Monitor de topes del impoconsumo

- Alerta amarilla al 85% del tope (~$140M acumulado en el año)
- Alerta naranja al superar el tope (3.500 UVT)
- Aviso al abrir una segunda sede
- Activación siempre manual con confirmación explícita y registro en auditoría
- El impoconsumo aplica al NIT completo — todas las sedes simultáneamente
- Los ítems del menú tienen clasificación fiscal: `GRAVADO_IMPOCONSUMO`, `EXCLUIDO`, `EXENTO`

### FR-04 Soporte multi-régimen tributario

- **Régimen Ordinario**: PUC completo, IVA independiente, agente de retención, nómina electrónica DIAN
- **Régimen SIMPLE**: anticipos mensuales sobre ingresos brutos (sin liquidación de IVA independiente), impoconsumo sí aplica por separado si supera topes, no son agentes de retención en renta (Art. 911 E.T.)
- **Persona natural no obligada**: modo contabilidad simplificada (categorías en lenguaje natural, sin PUC), mismas obligaciones de FE y tiquete POS si supera topes DIAN

### FR-05 Turno del restaurante (operación bloqueante)

- El turno es de la sede. Sin turno activo, ninguna operación puede ejecutarse.
- Flujo secuencial: `APERTURA DE TURNO → APERTURA DE CAJA → operación → ARQUEO → CIERRE DE CAJA → CIERRE DE TURNO`
- El arqueo compara el **total reportado** (suma de todos los métodos) vs el **total del sistema**. No valida método por método. Si hay descuadre, requiere comentario obligatorio.
- Las propinas se registran por separado. **Nunca aparecen en reportes financieros ni en documentos DIAN.**

### FR-06 Modo offline (POS sin internet — Fase 1)

- El POS funciona sin conexión. Es un requisito de Fase 1, no un backlog.
- Indicador siempre visible: banda ámbar con conteo de transacciones en cola y advertencia si alguna lleva > 36h (vence en 48h según normativa).
- **Funciona offline**: abrir mesas, agregar ítems (menú cacheado via Workbox CacheFirst), cobrar, KDS cocina, marcación de horario.
- **Se bloquea offline**: transmisión DIAN, buzón DIAN, sincronización entre sedes.
- Al reconectarse: `SyncQueue` transmite en orden de prioridad — tiquetes POS primero, luego FE, luego otros. Los tiquetes de contingencia llevan Tipo 03 según Resolución 000042/2020.
- Items expirados (> 48h) se purgan y se registran en auditoría con advertencia.

**Estrategias Workbox por recurso:**

| Recurso | Estrategia | TTL |
|---|---|---|
| Menú / productos | CacheFirst | 24h |
| Configuración del tenant | CacheFirst | 7 días |
| Plano de mesas / turno activo | NetworkFirst (timeout 3s) | 1h |
| Assets (logos, iconos) | StaleWhileRevalidate | — |
| Órdenes / cobros offline | Solo IndexedDB | 48h |

### FR-07 Patrón fire-and-forget para transmisiones DIAN

Todos los documentos DIAN se transmiten de forma asíncrona. El endpoint de cobro responde en < 100ms sin esperar la respuesta de Facture.co.

```
POST /cobros
  → Registra cobro en PostgreSQL (< 50ms)
  → Crea fiscal_document con status='DRAFT'
  → Encola job en BullMQ → cola 'dian-transmit'
  → Responde al cajero: { cobroId, status: 'ok' }   ← < 100ms total

Worker DianTransmitWorker (asíncrono):
  → Genera XML UBL 2.1
  → Llama a Facture.co (firma + transmisión)
  → Recibe CUFE de la DIAN
  → Actualiza fiscal_document → status='ACCEPTED'
  → Emite evento WebSocket: { type: 'TIQUETE_TRANSMITIDO', cufe, cobroId }
```

**Configuración de la cola BullMQ:**
- 5 reintentos con backoff exponencial (2s, 4s, 8s, 16s, 32s)
- Jobs completados: retención 24h
- Jobs fallidos: nunca se eliminan — van a dead letter queue, generan alerta en YARO Console
- Prioridad: FE (1) > Tiquete POS (2) > Nómina (3) > Otros (4)

**Panel de transmisiones — estados visibles al Admin:**

| Estado | Significado |
|---|---|
| ⏳ Generando XML | Job en progreso |
| 📡 Transmitiendo | Llamada a Facture.co en curso |
| ✅ Transmitido | CUFE recibido, documento aceptado |
| 🔄 Reintentando | Falló, BullMQ reintentará en Ns |
| ❌ Error | Agotó los 5 intentos — requiere atención |
| 📴 Offline | En SyncQueue local — enviará al reconectar |

### FR-08 Documentos fiscales DIAN — modelo append-only

Ningún documento fiscal puede editarse ni eliminarse después de generado. Garantizado a nivel de base de datos y de repositorio NestJS.

```sql
CREATE TABLE fiscal_documents (
  id              UUID PRIMARY KEY DEFAULT gen_random_uuid(),
  tenant_id       UUID NOT NULL,
  type            TEXT NOT NULL,    -- TIQUETE_POS | FE_VENTA | NOTA_CREDITO | DSCE | NOMINA | CONTINGENCIA
  status          TEXT NOT NULL,    -- DRAFT | TRANSMITTED | ACCEPTED | REJECTED
  cufe            TEXT,
  xml_s3_key      TEXT NOT NULL,    -- inmutable desde el primer write
  payload         JSONB NOT NULL,   -- snapshot completo del documento
  related_doc_id  UUID,             -- FK a doc original (nota crédito → FE original)
  created_at      TIMESTAMPTZ NOT NULL DEFAULT now(),
  transmitted_at  TIMESTAMPTZ,
  created_by      UUID NOT NULL
  -- No existe updated_at. No existe deleted_at. Nunca.
);
```

El repositorio en NestJS solo expone `create()` y `findBy()`. El método `update()` no existe en la capa de documentos fiscales. Una regla SQL impide modificar filas con `status = 'TRANSMITTED'`.

**Documentos por fase:**

| Documento | Tipo DIAN | Fase |
|---|---|---|
| Tiquete POS electrónico | 04 | Fase 1 — prioridad máxima |
| Factura electrónica de venta | 01 | Fase 1 |
| Tiquete de contingencia (offline) | 03 | Fase 1 |
| Nota crédito electrónica | 91 | Fase 2 |
| Documento soporte de compras (DSCE) | 05 | Fase 2 |
| Nómina electrónica (doc. soporte) | — | Fase 2 |

### FR-09 Retenciones en la fuente

**Como agente de retención (el restaurante retiene a sus proveedores):**
- Aplica a personas jurídicas y personas naturales con ingresos > 92.000 UVT
- Al registrar una factura de compra, YARO evalúa: ¿es proveedor SIMPLE? (sin retefuente) → ¿cuál es el concepto? (compras 2.5%, servicios 4%, arrendamiento 3.5%, honorarios 11%) → ¿supera la base mínima?
- El valor retenido va a cuenta por pagar a la DIAN (cuenta 2365)
- Tenants en Régimen SIMPLE no son agentes de retención en renta (Art. 911 E.T.)

**Como sujeto de retención (le retienen al restaurante):**
- Al emitir FE a gran contribuyente o agente retenedor, YARO calcula la retención esperada
- Se registra como anticipo de impuesto (cuenta 1355)

**ReteICA:** configurable por municipio en cada sede. El Admin confirma la tarifa — YARO no la asume.

### FR-10 Nómina electrónica DIAN (47 campos, Fase 2)

La Resolución 000013/2021 exige 5 grupos de información por empleado:
- **Devengados**: salario base, horas extra (diurnas, nocturnas, dominicales, festivas), recargos, auxilio de transporte, vacaciones, primas, viáticos, bonificaciones
- **Deducciones**: EPS empleado (4%), pensión empleado (4%), fondo de solidaridad (> 4 SMMLV), retención sobre salarios, libranzas
- **Aportes patronales**: salud (8.5%), pensión (12%), ARL (0.522% Clase I para restaurantes), SENA (2%), ICBF (3%), caja de compensación (4%)
- **Información de pago**: período, forma, banco si aplica
- **Validación XML**: contra el schema XSD de la DIAN antes de transmitir

La nómina electrónica es un entregable de Fase 2 dedicado. En Fase 1, la nómina se liquida internamente y genera comprobantes PDF sin transmisión DIAN.

### FR-11 Conciliación bancaria

**Fase 2 — Reporte para conciliación (asistida):**

YARO genera automáticamente al cierre de cada turno un reporte estructurado por método de pago:
- Valor bruto cobrado por método
- Comisión estimada configurable por terminal/red (Redeban, Credibanco, Nequi, etc.)
- Neto esperado en banco (bruto - comisión)
- Fecha estimada de depósito según días hábiles por método
- Número de lote del datáfono (ingresado por el cajero al cerrar turno)

Esto reduce el trabajo del contador a comparar una sola cifra por método contra el extracto bancario, en lugar de revisarlo transacción por transacción.

**Datos que se capturan desde Fase 1 para habilitar la conciliación en Fase 3:**
```typescript
metodoPago: {
  tipo:                  'TARJETA_CREDITO' | 'TARJETA_DEBITO' | 'NEQUI' | 'QR' | 'EFECTIVO',
  red:                   'REDEBAN' | 'CREDIBANCO' | 'NEQUI' | 'DAVIPLATA' | null,
  terminal:              'TID-001',           // ID del datáfono
  valorBruto:            186000,
  comisionPct:           3.2,                 // configurable por terminal/red
  valorNeto:             179952,
  fechaEsperadaDeposito: '2026-05-16',        // calculado según día hábil
}
```

**Fase 3 — Conciliación automatizada (matching por CSV):**

```
Admin importa extracto bancario CSV
        │
        ▼
YARO parsea cada movimiento bancario
  fecha · descripción · valor · referencia
        │
        ▼
Matching automático contra ventas YARO:
  ¿Coincide valor (bruto - comisión) con lote de ventas?
  ¿La fecha corresponde al día hábil siguiente?
  ¿El tipo de transacción (REDEBAN/NEQUI) coincide con el método?
        │
        ▼
Resultado:
  ✓ Conciliado automáticamente   → el movimiento tiene su venta
  ? Diferencia tolerable (< 0.1%) → marcado como conciliado con nota
  ✗ Sin match / requiere revisión → aparece en panel de excepciones

Admin solo revisa las excepciones (~5-10% del total)
Conciliación pasa de 4-5 horas a 20-30 minutos
```

**Tabla `bank_reconciliation_items` (Fase 3):**
```sql
-- Lado YARO (ventas): metodo, bruto, comision, neto, fecha_deposito_esperada, lote_datafono
-- Lado Banco (movimiento real): fecha, valor, referencia, descripcion
-- Resultado: status (PENDIENTE/CONCILIADO/DIFERENCIA/SIN_MATCH), diferencia, diferencia_pct, comentario
```

### FR-12 Marcación de horario y bienestar

- Botón de marcación siempre visible en el topbar de todos los roles (verde = entrada, ámbar = salida)
- Al marcar: mini formulario de bienestar (😔/😐/😊) + campo libre de novedades (opcional)
- El Superadmin activa/desactiva la marcación por rol: operativos (obligatorio), administrativos (opcional)
- Genera `AttendanceRecord` con: `userId`, `tenantId`, `sedeId`, `timestamp`, `type`, `bienestar`, `novedad`
- Arquitectura preparada para hardware biométrico futuro vía API sin modificar la lógica de nómina
- El historial alimenta el cálculo de horas trabajadas en la liquidación de nómina

### FR-13 IA con fallback de tres capas

```
Capa 1 — Claude Sonnet (API Anthropic)
  Sugiere cuenta PUC con explicación y nivel de confianza
  Si falla o timeout > 3s → Capa 2

Capa 2 — Motor de reglas (NestJS, sin API externa, latencia < 50ms)
  NIT proveedor → cuenta PUC mapeada (aprende del historial del tenant)
  Keywords en descripción → cuenta sugerida
  Si sin match → Capa 3

Capa 3 — Default (latencia 0ms)
  Cuenta genérica + alerta "Clasificar manualmente"
```

El Admin ve qué capa clasificó cada documento: `✦ IA` · `⚙ Regla automática` · `✋ Manual requerido`. El motor de reglas aprende del historial del tenant y persiste las reglas aprobadas.

### FR-14 Predictor de stock e IA de promociones (Fase 3)

**Predictor de stock:** promedio móvil ponderado (datos recientes pesan más). Si detecta tendencia al alza sostenida y el stock mínimo no cubre la proyección a 7/14/30 días → alerta con acciones directas: "Actualizar stock mínimo" y "Crear orden de compra sugerida".

**Prevalidador de promociones:** cruza stock actual + fecha de vencimiento de lotes + promedio histórico de ventas. Si `stock / promedio_diario > días_para_vencimiento` → alerta con botón para crear promoción o notificar meseros.

---

## 8) Requerimientos no funcionales (NFR)

### NFR-01 Performance y escalabilidad

- `p95` de API (POS, inventario, contabilidad): < 500ms bajo carga normal
- Endpoint `/cobros`: < 100ms (gracias al patrón fire-and-forget)
- KDS WebSocket: latencia < 200ms en condiciones normales
- Auto-scaling ECS Fargate: mínimo 1 task, máximo 10 tasks por servicio
- Fargate Spot para workers de background (hasta 70% más barato que On-Demand)

### NFR-02 Disponibilidad y resiliencia

- SLA objetivo para módulos fiscales (tiquetes POS, FE): **99.5%** mensual
- RDS Multi-AZ: failover automático < 60 segundos
- Rolling deployments sin downtime en ECS Fargate
- Fallo de Facture.co: BullMQ reintenta con backoff exponencial. Normativa permite 48h de contingencia. Dead letter queue con alerta en YARO Console
- Fallo de internet: modo offline cubre POS, cobro y KDS. SyncQueue en IndexedDB

### NFR-03 Seguridad

- HTTPS obligatorio. Certificado SSL via AWS ACM
- CORS restringido al dominio de CloudFront
- JWT: access token 15min, refresh token 7 días rotativo
- **Aislamiento de tenants**: middleware Prisma fuerza `WHERE tenant_id = :tenantId` en todos los queries
- VPC privada: ECS, RDS y ElastiCache en subnets privadas
- AWS Secrets Manager: todas las credenciales. Ninguna en variables de entorno hardcodeadas
- Rate limiting por IP y por tenant en endpoints críticos
- Validación de inputs con `class-validator` antes de cualquier operación
- Auditoría en log estructurado: cambios de permisos, aprobaciones, cierres de caja, transmisiones DIAN, cambios de impoconsumo
- Cifrado en reposo: RDS con KMS, S3 con SSE-S3

### NFR-04 Modelo fiscal append-only

Los documentos fiscales nunca se editan ni eliminan. Garantizado a nivel SQL y a nivel del repositorio NestJS. Para anular una FE se crea una nota crédito que referencia la original — la FE original permanece intacta. Trazabilidad completa para cualquier auditoría de la DIAN.

### NFR-05 Schema de Prisma por fases y política de migraciones

El schema crece con las fases (14 → +13 → +4 tablas). Nunca se crean tablas de fases futuras antes de tiempo.

**Regla obligatoria para migraciones en producción (expand-contract):**
```
Paso 1 (Expand):   Agregar la columna como nullable
Paso 2 (Migrate):  Poblar los datos existentes
Paso 3 (Contract): Agregar el constraint NOT NULL

NUNCA: ALTER TABLE con NOT NULL en tabla con datos en un solo paso.
```

Esta regla está en el `CONTRIBUTING.md` del repositorio.

### NFR-06 Observabilidad

- CloudWatch Metrics: CPU/memoria Fargate, latencia RDS, hit rate ElastiCache, request count y error rate
- CloudWatch Logs estructurados en JSON: `timestamp · level · traceId · tenantId · userId · endpoint · duration · statusCode`
- Trazas distribuidas: AWS X-Ray
- Alertas para: error rate > 1%, p95 latency > 1s, RDS CPU > 80%, ElastiCache memory > 85%, fallo DIAN repetido, dead letter queue con items

### NFR-07 Calidad y pruebas

- **Unit tests (Jest):** cálculo de food cost, lógica de arqueo, validación XML DIAN, aislamiento multi-tenant, idempotencia de la SyncQueue offline
- **Integration tests:** flujo completo de turno, emisión de tiquete con mock de Facture.co, traslado CDP → sede, modo offline y sincronización
- **E2E (Playwright):** abrir mesa → cobrar → verificar tiquete; modo offline → reconectar → verificar transmisión; marcar entrada → salida → verificar AttendanceRecord
- **Smoke test post-deploy:** `GET /health` → ok, abrir turno → cerrar turno, emitir tiquete de prueba → verificar CUFE

### NFR-08 Normativa y cumplimiento

- XMLs DIAN en S3 con versionado activado, retención mínima 5 años (Art. 632 E.T.)
- Impoconsumo del 8% siempre en cuenta separada del IVA (cuenta 2408 separada)
- Propinas excluidas de todos los reportes financieros y documentos DIAN
- Aviso visible: "Los datos ingresados están protegidos bajo la Ley 1581 de 2012. No ingrese información de terceros sin su consentimiento."
- `AttendanceRecord` y registros de bienestar retenidos por mínimo 1 año (cumplimiento laboral colombiano)

### NFR-09 Variables de configuración

```bash
# Nunca commitear valores reales
# AWS Secrets Manager en producción · .env.local (gitignored) en desarrollo

DATABASE_URL=REPLACE_WITH_RDS_POSTGRESQL_URL
REDIS_URL=REPLACE_WITH_ELASTICACHE_VALKEY_URL
FACTURECO_API_KEY=REPLACE_WITH_FACTURECO_API_KEY
FACTURECO_API_URL=https://api.facture.co/v1
ANTHROPIC_API_KEY=REPLACE_WITH_ANTHROPIC_API_KEY
JWT_SECRET=REPLACE_WITH_STRONG_JWT_SECRET
JWT_EXPIRY=15m
JWT_REFRESH_EXPIRY=7d
AWS_REGION=us-east-1
S3_BUCKET_NAME=REPLACE_WITH_S3_BUCKET_NAME
FRONTEND_URL=REPLACE_WITH_CLOUDFRONT_URL
CONSOLE_URL=REPLACE_WITH_CONSOLE_CLOUDFRONT_URL
AI_FALLBACK_MODE=enabled
OFFLINE_QUEUE_TTL=48h
BULLMQ_CONCURRENCY=5
```

---

## 9) API — endpoints principales

### Autenticación
- `POST /auth/login` → `{ accessToken, refreshToken, user }`
- `POST /auth/refresh` → `{ accessToken }`
- `POST /auth/logout` → `{ ok: true }`

### Turno y caja
- `POST /turno/abrir` → `{ turnoId, sede, aperturaAt }`
- `POST /turno/abrir-caja` body `{ baseCaja }` → `{ cajaId, base }`
- `POST /turno/arqueo` body `{ metodoPago: { efectivo, tarjetaCredito, tarjetaDebito, nequi, qr } }` → `{ totalSistema, totalCajero, descuadre }`
- `POST /turno/cerrar-caja` body `{ comentario, loteDatáfono? }` → `{ ok: true }`
- `POST /turno/cerrar` → `{ ok: true, resumen }`
- `GET /turno/activo?sedeId=` → `{ turnoId, estado, base, ventasAcumuladas }`

### Mesas y POS
- `GET /mesas?sedeId=` → `{ mesas[] }`
- `POST /mesas/:mesaId/abrir` → `{ mesaId, estado }`
- `POST /mesas/:mesaId/items` body `{ productoId, qty, modificadores[] }` → `{ orden }`
- `GET /mesas/:mesaId/orden` → `{ items[], subtotal, impoconsumo, total }`
- `POST /mesas/:mesaId/precuenta` → `{ pdf: url }`
- `POST /cobros` body `{ mesaId, metodoPago, descuento?, motivoDescuento?, propina?, clienteId?, tipoDocumento }` → `{ cobroId, status: 'ok', fiscalDocId }`

### KDS (WebSocket)
- `WS /kds?sedeId=&token=` → eventos: `nueva_orden`, `orden_actualizada`, `orden_lista`
- `PATCH /kds/ordenes/:ordenId` body `{ estado }` → `{ ok: true }`

### Offline / Sync Queue
- `POST /sync/tiquetes` body `{ tiquetes[], contingencyCode }` → `{ transmitidos[], errores[] }`
- `GET /sync/pendientes?sedeId=` → `{ count, oldest }`

### Inventario
- `GET /inventario?sedeId=&tipo=insumo|cdp` → `{ items[] }`
- `POST /inventario/solicitar-traslado` body `{ productos[], sedeDestino, urgencia }` → `{ solicitudId }`
- `POST /inventario/confirmar-recepcion/:trasladoId` body `{ items[], discrepancias[] }` → `{ ok: true }`
- `POST /inventario/merma` body `{ productoId, cantidad, causa, descripcion }` → `{ mermaId }`

### CDP
- `POST /cdp/recepciones` body `{ ordenCompraId, items[{ productoId, cantidadRecibida, calidad }] }` → `{ recepcionId }`
- `POST /cdp/produccion` body `{ recetaId, lotes, loteCode }` → `{ produccionId, insumosDescontados[] }`
- `PATCH /cdp/produccion/:produccionId/rendimiento` body `{ rendimientoReal, comentario }` → `{ merma }`
- `POST /cdp/traslados` body `{ sedeDestino, productos[], precioTransferencia }` → `{ trasladoId, remision: url }`
- `GET /cdp/etiqueta/:produccionId` → `{ etiquetaPDF: url }`

### Documentos DIAN
- `POST /dian/tiquete` body `{ cobroId }` → `{ fiscalDocId, status: 'queued' }` ← fire-and-forget
- `POST /dian/factura-venta` body `{ cobroId, clienteNit }` → `{ fiscalDocId, status: 'queued' }`
- `POST /dian/nota-credito` body `{ facturaOriginalId, motivo, valor }` → `{ fiscalDocId, status: 'queued' }`
- `GET /dian/buzon?tenantId=` → `{ facturas[] }`
- `POST /dian/clasificar-factura` body `{ facturaId, cuentaPUC }` → `{ ok: true }`
- `GET /dian/transmisiones?tenantId=&periodo=` → `{ documentos[], errores[], pendientes[] }`

### Contabilidad
- `GET /contabilidad/pyg?sedeId=&periodo=` → `{ ingresos, costos, gastos, ebitda }`
- `GET /contabilidad/balance?periodo=` → `{ cuentas[] }`
- `POST /contabilidad/gastos-fijos` body `{ concepto, cuentaPUC, valor, diaVencimiento, anticipacion, recurrencia }` → `{ gastoId }`
- `POST /retenciones/calcular` body `{ facturaId, concepto }` → `{ retencion, cuentaPUC, baseMinima }`

### Conciliación bancaria
- `GET /conciliacion/reporte?sedeId=&periodo=` → `{ metodos[], totalEsperadoBanco, comisionesEstimadas }`
- `POST /conciliacion/importar` body `{ extractoCSV, periodo, sedeId }` → `{ conciliados, conDiferencia, sinMatch }`
- `GET /conciliacion/excepciones?periodo=&sedeId=` → `{ items[] }`
- `PATCH /conciliacion/items/:id` body `{ status, comentario }` → `{ ok: true }`

### Impoconsumo
- `GET /config/impoconsumo` → `{ activo, tope, ingresoAcumulado, alertaActiva }`
- `POST /config/impoconsumo/activar` body `{ motivo? }` → `{ ok: true, activoDesde }`
- `POST /config/impoconsumo/desactivar` body `{ motivo }` → `{ ok: true }`

### Nómina
- `GET /nomina/empleados?sedeId=` → `{ empleados[] }`
- `POST /nomina/procesar` body `{ periodo, sedeId }` → `{ liquidaciones[] }`
- `GET /nomina/comprobante/:empleadoId/:periodo` → `{ pdf: url }`
- `POST /nomina/dian/generar` body `{ liquidacionId }` → `{ fiscalDocId, status: 'queued' }`

### Horario y bienestar
- `POST /horario/marcar` body `{ tipo: 'entrada'|'salida', bienestar, novedad? }` → `{ attendanceRecordId, timestamp }`
- `GET /horario/historial?userId=&periodo=` → `{ registros[] }`

### IA
- `POST /ia/sugerir-puc` body `{ facturaId, descripcion, proveedor }` → `{ cuentaPUC, confianza, capa: 'ia'|'regla'|'default' }`
- `GET /ia/prediccion-stock?sedeId=` → `{ alertas[] }`
- `GET /ia/prevalidador-promociones?sedeId=` → `{ alertas[] }`

### Salud
- `GET /health` → `{ status: 'ok', db: 'ok', cache: 'ok', dian: 'ok', bullmq: 'ok', offline_queue: number, timestamp }`

### YARO Console (solo `PLATFORM_ADMIN`)
- `POST /console/tenants` → `{ tenantId, createdAt, onboardingUrl }`
- `GET /console/tenants` → `{ tenants[] }`
- `GET /console/tenants/:tenantId` → `{ tenant, metricas, alertas }`
- `PATCH /console/tenants/:tenantId/estado` body `{ estado }` → `{ ok: true }`
- `GET /console/monitor/dian` → `{ transmisiones[], errores[], pendientes[], deadLetterQueue[] }`
- `GET /console/logs?tenantId=&nivel=&desde=` → `{ logs[] }`
- `GET /console/health` → `{ servicios[], alertas[] }`

---

## 10) Métricas de éxito

### Fase 1 (primeros 6 meses)
- ≥ 5 tenants activos con operación diaria completa
- Tasa de transmisión exitosa de tiquetes POS a la DIAN ≥ 99%
- Tasa de éxito de sincronización offline → DIAN ≥ 98%
- Latencia endpoint `/cobros` < 100ms (p95)
- p95 general de API < 500ms
- Cero incidentes de acceso cruzado entre tenants
- Reducción de tiempo en procesos manuales ≥ 3h/semana por restaurante

### Fase 2 (meses 9–18)
- Food cost real vs estimado: diferencia < 2pp en primer mes
- Tasa de confirmación de sugerencias IA (clasificación PUC) > 80% en mes 3
- 100% de nóminas procesadas con documento soporte DIAN correcto
- ≥ 20 tenants activos
- Tiempo de conciliación bancaria asistida < 45 minutos por período

### Fase 3 (meses 19–30)
- Conciliación bancaria automatizada: ≥ 90% de movimientos conciliados sin intervención manual
- ≥ 50 tenants activos
- Al menos 1 país adicional en producción con normativa configurada

---

## 11) Riesgos y mitigaciones

- **Caída de Facture.co**: BullMQ reintenta con backoff exponencial. Dead letter queue con alerta en YARO Console. Normativa permite 48h de contingencia. Tiquetes de contingencia con Tipo 03.
- **Fallo de RDS**: Multi-AZ con failover automático < 60s.
- **Fallo de internet**: modo offline con Workbox + IndexedDB SyncQueue. Indicador visual siempre visible con tiempo restante antes del vencimiento de 48h.
- **Latencia WebSocket KDS**: ElastiCache Valkey Pub/Sub garantiza entrega multi-instancia. Auto-scaling activo.
- **Acceso cruzado de tenants**: middleware Prisma fuerza `tenant_id`. Tests de aislamiento en CI como requisito de merge.
- **Downtime de Claude Sonnet**: fallback de tres capas. La contabilidad nunca se bloquea.
- **Integridad fiscal**: modelo append-only con regla SQL. No existe UPDATE/DELETE en documentos fiscales.
- **Activación incorrecta del impoconsumo**: nunca automático. Siempre confirmación explícita con consecuencias. Registro en auditoría.
- **Complejidad de la nómina electrónica**: Fase 2 dedicada. Validación contra XSD de la DIAN antes de transmitir.
- **Migraciones de BD en producción**: política expand-contract obligatoria documentada en `CONTRIBUTING.md`.
- **Inconsistencia en la SyncQueue offline**: cada item tiene `expiresAt` (createdAt + 48h). Los items expirados se purgan y registran en auditoría con advertencia al Admin.
- **Errores en el matching de conciliación bancaria**: el sistema solo marca como "conciliado automáticamente" cuando la diferencia es < 0.1%. Cualquier diferencia mayor va al panel de excepciones para revisión manual del contador.
- **Adopción del KDS en cocina**: UX de un solo toque. Sin capacitación formal requerida.
- **Error en configuración de ReteICA**: configurable por el Admin, con aviso claro de que la tarifa debe ser confirmada con el contador. YARO no asume tarifas municipales.

---

*YARO PRD v3.0 · Mayo 2026*

*Stack: Angular 17+ · NestJS · PostgreSQL (Prisma) · ECS Fargate · ElastiCache Valkey · BullMQ · Workbox · @yaro/ui · Facture.co (DIAN) · Claude Sonnet (IA)*

*Cambios v3.0 vs v2.0:*
- *Schema Prisma por fases (14 → +13 → +4 tablas) con política expand-contract*
- *Patrón fire-and-forget con BullMQ para todas las transmisiones DIAN*
- *Service Worker con Workbox — estrategias de caché por tipo de recurso*
- *SyncQueue en IndexedDB con expiración de 48h y purga automática*
- *Librería de componentes `@yaro/ui` con estructura atoms/molecules/organisms*
- *Convenciones Angular 17: Signals para estado local, RxJS para streams*
- *Conciliación bancaria: reporte asistido en Fase 2, matching automático CSV en Fase 3*
- *Open Banking descartado del scope — fuera del alcance*
- *Endpoint `/cobros` responde < 100ms (nueva métrica)*
- *Conciliación bancaria como métrica de éxito en Fase 3 (≥ 90% automático)*
