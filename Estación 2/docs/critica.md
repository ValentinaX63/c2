# Documento de crítica — Riesgos técnicos de YARO
> Para ingenieros nuevos en el equipo.
> Este documento existe para que nadie llegue al equipo con ilusiones falsas.
> YARO tiene riesgos reales. Este documento los nombra, los mide y explica qué estamos haciendo al respecto.
> Si después de leer esto todavía quieres construir YARO, bienvenido al equipo.

---

## Principio de este documento

En el documento de referencia de AgentVault hay una pregunta que vale la pena replicar para YARO:

> *"Si tienes éxito a escala, ¿cuál es la primera forma en que se rompe la confianza?"*

Para YARO la pregunta equivalente es:

> *"¿Qué evento técnico haría que un restaurante desinstale YARO en las primeras dos semanas?"*

La respuesta a esa pregunta define la jerarquía de riesgos. Todo lo demás es secundario.

---

## Riesgo 1 — El POS falla en hora pico y el cajero no puede cobrar

**Probabilidad:** Alta si no se diseña correctamente desde el inicio.
**Impacto:** Catastrófico — es el escenario de muerte del producto.

### Por qué este es el riesgo más importante

Un restaurante tiene dos momentos críticos al día: el almuerzo (12–2pm) y la cena (7–9pm). En esos momentos, el cajero está atendiendo una fila de clientes. Si el POS no responde, el restaurante pierde ventas, el dueño llama furioso, y YARO se desinstala esa misma tarde.

No importa cuántas features tenga el sistema. No importa cuán bien esté integrada la DIAN. Si el POS falla en hora pico una sola vez en las primeras dos semanas, el cliente no le da una segunda oportunidad.

### Los tres vectores de fallo del POS

**Vector A — El endpoint `/cobros` tarda más de 3 segundos.**

Causa más probable: alguien metió una llamada sincrónica a Facture.co dentro del handler de la request. La llamada puede tardar 200–800ms en condiciones normales, y varios segundos si Facture.co tiene latencia. El cajero ve la pantalla congelada.

Solución implementada: patrón fire-and-forget con BullMQ. El endpoint `/cobros` responde en menos de 100ms sin esperar a Facture.co. La transmisión DIAN ocurre asíncronamente.

**Regla absoluta en el código:** ninguna llamada a Facture.co puede ocurrir dentro de un handler HTTP. Si alguien abre un PR que viola esta regla, el PR se rechaza sin discusión.

**Vector B — El internet del restaurante se cae en hora pico.**

Causa: conectividad inestable en centros comerciales y municipios intermedios de Colombia. No es un edge case — es una realidad frecuente en el mercado objetivo de YARO.

Solución implementada: modo offline con Workbox + IndexedDB. El POS sigue operando sin internet. Los cobros se almacenan localmente y se sincronizan con la DIAN al reconectarse. La normativa permite 48 horas de contingencia.

**Lo que puede fallar en la implementación del modo offline:**

- Si el Service Worker no está correctamente configurado, el navegador puede intentar la request de cobro contra el backend y fallar visiblemente en lugar de usar el caché.
- Si la SyncQueue no tiene el campo `expiresAt` correcto (createdAt + 48h), documentos pueden intentar transmitirse después del plazo de contingencia y ser rechazados por la DIAN.
- Si el menú no está en el caché al momento de perder internet, el cajero no puede agregar productos nuevos a la orden.

**Checklist obligatorio antes de hacer deploy a producción:**

```
[ ] Desconectar internet en el dispositivo de prueba
[ ] Abrir una mesa, agregar ítems y cobrar
[ ] Verificar que el cobro queda en la SyncQueue con expiresAt correcto
[ ] Reconectar internet
[ ] Verificar que el documento se transmite a la DIAN automáticamente
[ ] Verificar que el panel de transmisiones muestra ACCEPTED con CUFE
```

**Vector C — La base de datos tiene latencia alta o se cae.**

Causa: RDS PostgreSQL con alta carga de conexiones, o failover Multi-AZ tardando más de 60 segundos.

Solución implementada: RDS Multi-AZ con failover automático. El pool de conexiones de Prisma tiene un límite configurado para evitar saturar la base de datos. En Fase 1, con pocos tenants activos, esto no debería ser un problema — pero hay que monitorearlo desde el primer deploy.

**Métrica de alerta:** si el p95 del endpoint `/cobros` supera 500ms, hay que investigar antes de que llegue a 1 segundo.

---

## Riesgo 2 — Un documento fiscal se genera incorrectamente y la DIAN lo rechaza

**Probabilidad:** Media — los XMLs DIAN tienen reglas de validación muy específicas.
**Impacto:** Alto — un documento rechazado por la DIAN puede generar contingencias tributarias para el cliente.

### Por qué el XML DIAN es frágil

El XML UBL 2.1 que YARO genera para la DIAN tiene estructura estricta con schema XSD. Un campo mal formateado, una fecha en formato incorrecto, o un código de actividad económica inválido produce un rechazo. La DIAN no rechaza parcialmente — rechaza el documento completo.

Los rechazos más frecuentes en producción según la experiencia del sector:

| Causa de rechazo | Dónde falla en YARO |
|---|---|
| NIT del emisor sin dígito de verificación | Configuración del tenant en el onboarding |
| Código de actividad económica inválido | Configuración de la sede |
| Impoconsumo calculado sobre base incorrecta | Lógica de cálculo en el módulo de cobros |
| Fecha de emisión fuera del rango permitido | Jobs de BullMQ con delay excesivo |
| Total de la factura no cuadra con la suma de ítems | Redondeo de decimales en COP |

### El problema del redondeo de decimales en COP

Este es uno de los bugs más comunes en sistemas de facturación colombianos y uno de los más difíciles de detectar en desarrollo.

El impoconsumo del 8% sobre $38.000 COP es $3.040 exactos. Pero el 8% sobre $37.500 COP es $3.000 exactos. El problema aparece cuando el total de la orden tiene precios que generan decimales:

```
Plato 1: $18.500 × 8% = $1.480
Plato 2: $22.300 × 8% = $1.784
Total impoconsumo: $3.264

Pero si el sistema calcula:
Subtotal: $40.800 × 8% = $3.264

Hasta ahí bien. Pero si hay un descuento del 10%:
Subtotal con descuento: $36.720 × 8% = $2.937.6

¿Se redondea a $2.937 o $2.938?
La DIAN espera consistencia. Si el total no cuadra con la suma de líneas, rechaza.
```

**Regla en el código:** todos los cálculos fiscales se hacen en enteros de COP (pesos enteros, sin centavos). El redondeo siempre es hacia abajo (`Math.floor`). Esta regla debe estar en el `CONTRIBUTING.md` y en los tests unitarios del módulo de cobros.

### La cadena de validaciones antes de transmitir

Antes de que un documento llegue a Facture.co, YARO debe validar:

```typescript
// Checklist interno de validación pre-transmisión
function validarDocumentoFiscal(doc: FiscalDocumentPayload): ValidationResult {
  const errores: string[] = [];

  // 1. NIT del emisor con dígito de verificación
  if (!nitValido(doc.emisor.nit)) errores.push('NIT del emisor inválido');

  // 2. Código de actividad económica CIIU válido
  if (!codigoCIIUValido(doc.emisor.actividadEconomica)) errores.push('CIIU inválido');

  // 3. Impoconsumo solo sobre ítems GRAVADO_IMPOCONSUMO
  const baseImpo = doc.items
    .filter(i => i.categoriaFiscal === 'GRAVADO_IMPOCONSUMO')
    .reduce((sum, i) => sum + i.subtotal, 0);
  const impoCalculado = Math.floor(baseImpo * 0.08);
  if (impoCalculado !== doc.totales.impoconsumo) errores.push('Impoconsumo no cuadra');

  // 4. Total de la factura cuadra con la suma de líneas
  const totalEsperado = doc.items.reduce((sum, i) => sum + i.subtotal, 0) + impoCalculado;
  if (totalEsperado !== doc.totales.total) errores.push('Total no cuadra con suma de líneas');

  // 5. Fecha de emisión no es futura
  if (doc.fechaEmision > new Date()) errores.push('Fecha de emisión en el futuro');

  return { valido: errores.length === 0, errores };
}
```

Si la validación falla, el documento queda en estado `REJECTED_PRE_VALIDATION` y aparece en el panel de transmisiones con el error específico. El cajero ve un indicador de error y el Admin puede ver el detalle.

---

## Riesgo 3 — Acceso cruzado entre tenants

**Probabilidad:** Baja si el middleware de Prisma funciona correctamente. Alta si alguien lo desactiva o lo bypasea.
**Impacto:** Catastrófico — exposición de datos de un restaurante a otro. Violación de la Ley 1581 (Habeas Data). Fin de YARO como producto.

### Cómo puede pasar

El middleware de Prisma que fuerza el filtro `tenantId` en todos los queries es la primera línea de defensa. Pero hay tres formas en que puede fallar:

**Forma 1 — Un developer usa `this.prisma` directamente sin pasar por el servicio que tiene el middleware.**

```typescript
// ❌ PELIGROSO — acceso directo a prisma sin contexto de tenant
const ordenes = await this.prisma.orden.findMany(); // devuelve órdenes de TODOS los tenants

// ✅ CORRECTO — el middleware inyecta el tenantId automáticamente
const ordenes = await this.prisma.orden.findMany(); // con middleware activo, solo devuelve las del tenant en contexto
```

La diferencia es que el middleware debe estar activo en el `PrismaService`. Si alguien instancia Prisma directamente sin pasar por `PrismaService`, el middleware no aplica.

**Forma 2 — Un endpoint de la Console (`/console/*`) no tiene el guard correcto.**

Los endpoints de la Console tienen acceso multi-tenant por diseño — pero solo el rol `PLATFORM_ADMIN`. Si un endpoint de la Console no tiene el `PlatformAdminGuard`, cualquier usuario autenticado podría acceder a datos de cualquier tenant.

**Forma 3 — Una migración de base de datos agrega una tabla sin el campo `tenantId`.**

Si alguien agrega una tabla nueva sin incluir `tenantId`, esa tabla no estará protegida por el middleware. Cualquier query a esa tabla devolverá datos de todos los tenants.

### Cómo lo mitigamos

**Mitigación 1 — Tests de aislamiento en CI:**

```typescript
describe('Aislamiento multi-tenant', () => {
  it('un query sin tenantId en contexto debe lanzar excepción', async () => {
    // Remover el tenantId del contexto CLS
    cls.set('tenantId', undefined);
    await expect(ordenService.findAll()).rejects.toThrow('Operación sin contexto de tenant');
  });

  it('tenant A no puede ver órdenes del tenant B', async () => {
    cls.set('tenantId', TENANT_A_ID);
    const ordenes = await ordenService.findAll();
    expect(ordenes.every(o => o.tenantId === TENANT_A_ID)).toBe(true);
  });
});
```

Estos tests deben pasar en cada PR. Si alguno falla, el PR no puede hacer merge.

**Mitigación 2 — Checklist de PR:**

Toda tabla nueva en el schema de Prisma debe tener `tenantId` como campo obligatorio. Está en el checklist de PR del `CONTRIBUTING.md`. El reviewer debe verificarlo explícitamente.

**Mitigación 3 — Auditoría de endpoints:**

Cada endpoint nuevo debe tener documentado cuál guard lo protege. Los endpoints de la Console tienen `PlatformAdminGuard`. Los endpoints operativos tienen `AuthGuard` + `TenantGuard`. Si un endpoint no tiene guard documentado, no puede hacer merge.

---

## Riesgo 4 — BullMQ pierde jobs y documentos DIAN quedan sin transmitir

**Probabilidad:** Media — depende de la disponibilidad de ElastiCache Valkey.
**Impacto:** Alto — documentos no transmitidos generan incumplimiento DIAN para el cliente.

### Cómo puede pasar

BullMQ usa Redis/Valkey como backend. Si ElastiCache Valkey se cae o tiene una partición de red:

- Los jobs en la cola se pierden si no estaban en un estado persistido.
- Los jobs nuevos no pueden encolarse.
- El endpoint `/cobros` puede fallar porque no puede encolar el job de transmisión.

### La estrategia de mitigación en capas

**Capa 1 — ElastiCache Multi-AZ:** en producción, ElastiCache corre con replicación. Si el nodo primario falla, hay failover automático a la réplica.

**Capa 2 — El documento fiscal existe en PostgreSQL antes del job:**

```typescript
// El orden correcto es crítico
// 1. Primero: crear el fiscal_document en PostgreSQL (DRAFT)
const fiscalDoc = await this.prisma.fiscalDocument.create({ data: { status: 'DRAFT', ... } });

// 2. Después: encolar el job en BullMQ
await this.dianQueue.add('transmitir', { fiscalDocId: fiscalDoc.id });

// Si BullMQ falla en el paso 2, el fiscal_document existe en status='DRAFT'
// Un proceso de reconciliación puede detectar documentos DRAFT viejos y reencolarlos
```

**Capa 3 — Proceso de reconciliación:**

Un job cron que corre cada 15 minutos busca documentos en estado `DRAFT` o `QUEUED` con más de 10 minutos de antigüedad y los reencola. Esto captura cualquier documento que haya quedado huérfano por un fallo de BullMQ.

```typescript
@Cron('*/15 * * * *')
async reconciliarDocumentosPendientes() {
  const huerfanos = await this.prisma.fiscalDocument.findMany({
    where: {
      status: { in: ['DRAFT', 'QUEUED'] },
      createdAt: { lt: new Date(Date.now() - 10 * 60 * 1000) }, // más de 10 min
    },
  });

  for (const doc of huerfanos) {
    await this.dianQueue.add('transmitir', { fiscalDocId: doc.id }, { priority: 1 });
    this.logger.warn(`[RECONCILIACION] Documento ${doc.id} reencolado`);
  }
}
```

**Capa 4 — Dead letter queue con alerta:**

Si un job falla los 5 reintentos, va a la dead letter queue y genera una alerta en YARO Console. El equipo de soporte puede revisarlo y decidir si reencolarlo manualmente o contactar al cliente.

---

## Riesgo 5 — El modo offline genera documentos de contingencia mal formados

**Probabilidad:** Media — el XML de contingencia tiene reglas diferentes al XML normal.
**Impacto:** Alto — la DIAN puede rechazar el lote de contingencia cuando se sincroniza, generando incumplimiento retroactivo.

### La diferencia entre un tiquete POS normal y uno de contingencia

Un tiquete POS electrónico normal (Tipo 04) se transmite en tiempo real y recibe CUFE inmediatamente. Un tiquete de contingencia (Tipo 03) se genera offline y se transmite después, dentro del plazo de 48 horas.

Los documentos de contingencia tienen diferencias específicas en el XML:

- El campo `tipoDocumentoElectronico` va como `03` en lugar de `04`
- El campo `indicadorContigencia` debe ser `true`
- El número de la contingencia debe seguir la secuencia del prefijo de contingencia habilitado ante la DIAN, no la secuencia normal de tiquetes
- El timestamp del documento debe ser el momento real del cobro offline, no el momento de la sincronización

Si cualquiera de estas reglas falla, la DIAN rechaza el lote completo de contingencia — incluyendo todos los tiquetes válidos que estaban en el mismo lote.

### Qué hacer al respecto

**Punto crítico:** el XML de contingencia debe generarse en el momento del cobro offline, no en el momento de la sincronización. El Service Worker debe tener la lógica de construcción del XML — no puede depender de una llamada al backend para generarlo.

Esto tiene una implicación arquitectónica importante: la lógica de construcción del XML UBL 2.1 base debe estar disponible tanto en el backend (NestJS) como en el frontend (Service Worker en JavaScript). La solución es extraer esa lógica a una librería compartida (`@yaro/xml-builder`) que pueda importarse en ambos contextos.

**Checklist de prueba para modo offline y contingencia:**

```
[ ] Cobrar con internet activo → verificar tiquete Tipo 04 con CUFE
[ ] Activar modo avión en el dispositivo
[ ] Cobrar sin internet → verificar que queda en SyncQueue con tipo CONTINGENCIA
[ ] Verificar que el XML de contingencia tiene tipoDocumento=03 y indicadorContigencia=true
[ ] Verificar que el timestamp del cobro en el XML coincide con el momento del cobro, no con la sincronización
[ ] Reactivar internet
[ ] Verificar que la transmisión lote de contingencia llega a la DIAN y recibe CUFE
[ ] Verificar que el panel de transmisiones muestra ACCEPTED para todos los documentos del lote
```

---

## Riesgo 6 — La librería `@yaro/ui` diverge de los prototipos HTML

**Probabilidad:** Alta sin un proceso explícito de validación visual.
**Impacto:** Medio — inconsistencia visual que reduce la percepción de calidad del producto.

### Por qué pasa

Los prototipos HTML del sistema de diseño son la referencia visual del equipo. Pero a medida que la librería de componentes Angular se construye, los componentes pueden ir derivando visualmente — un padding diferente aquí, un border-radius distinto allá — hasta que la app Angular se ve diferente al prototipo HTML.

Esto no es un problema de código — es un problema de disciplina. Cada componente Angular debe verse idéntico al HTML de referencia. Si hay diferencia visual, el componente no está terminado.

### Cómo lo evitamos

**Regla de PR para componentes de `@yaro/ui`:**

Antes de hacer merge de cualquier componente de la librería, el developer debe tomar un screenshot del componente Angular y del prototipo HTML de referencia y subirlos al PR. El reviewer debe comparar visualmente. Si hay diferencia, el PR se rechaza.

**Checklist específico de componentes:**

```
[ ] Colores: usar var(--color-*) de tokens.scss — nunca hex directo
[ ] Sombras: usar var(--shadow-sm/md/lg) — nunca box-shadow hardcodeado
[ ] Radios: usar var(--radius/radius-sm/radius-xs) — nunca border-radius hardcodeado
[ ] Tipografía: usar var(--font-sans) — nunca font-family hardcodeado
[ ] Espaciado: usar var(--space-*) — nunca px sueltos
[ ] El componente es standalone: true
[ ] El componente usa ChangeDetectionStrategy.OnPush
[ ] El componente tiene al menos un test en su .spec.ts
```

---

## Riesgo 7 — Las migraciones de Prisma en producción generan downtime

**Probabilidad:** Media si no se sigue el patrón expand-contract.
**Impacto:** Alto — downtime del POS en hora de servicio es inaceptable.

### Cómo puede pasar

Una migración que agrega una columna `NOT NULL` sin valor por defecto en una tabla grande bloquea la tabla durante segundos o minutos. Si `ordenes` tiene 500.000 filas y alguien hace:

```sql
-- ❌ ESTO BLOQUEA LA TABLA
ALTER TABLE "ordenes" ADD COLUMN "loteDatafono" TEXT NOT NULL;
```

PostgreSQL necesita actualizar cada fila para agregar el valor `NOT NULL`. Durante ese tiempo, cualquier insert o update en la tabla falla. El POS no puede cobrar.

### La regla que evita esto

El patrón expand-contract es obligatorio para toda migración en producción. Está en el `CONTRIBUTING.md`. Está en el checklist de PR. Y se repite aquí porque es el error que más cuesta en producción:

```sql
-- Paso 1: agregar como nullable (no bloquea)
ALTER TABLE "ordenes" ADD COLUMN "loteDatafono" TEXT;

-- Paso 2: poblar los datos existentes (en background)
UPDATE "ordenes" SET "loteDatafono" = '' WHERE "loteDatafono" IS NULL;

-- Paso 3: agregar el constraint (rápido si ya no hay nulls)
ALTER TABLE "ordenes" ALTER COLUMN "loteDatafono" SET NOT NULL;
```

**Regla adicional:** ninguna migración se aplica en producción durante las horas pico del restaurante (11am–3pm y 6pm–10pm). Se aplican en horario de madrugada o con coordinación explícita con el cliente.

---

## Riesgo 8 — El IA fallback no tiene suficientes reglas iniciales y siempre cae en Default

**Probabilidad:** Alta en los primeros meses cuando la base de datos de reglas está vacía.
**Impacto:** Medio — el contador tiene que clasificar manualmente cada factura, lo que reduce el valor del módulo contable.

### El problema del cold start del motor de reglas

El motor de reglas de Capa 2 (fallback de IA) aprende del historial del tenant: si el Admin siempre aprueba "Carnes Antioqueñas → cuenta 1430", esa regla se persiste. Pero en los primeros días de un tenant nuevo, no hay historial. Claude Sonnet cae, no hay reglas del tenant, y el sistema siempre responde con el Default de Capa 3 ("Clasificar manualmente").

Si el 100% de las facturas requieren clasificación manual en las primeras semanas, el contador dice que el sistema no sirve y el dueño lo desinstala.

### La solución de las reglas base pre-cargadas

El sistema debe venir con un conjunto de reglas base pre-cargadas para el sector restaurantero colombiano:

```typescript
const REGLAS_BASE_RESTAURANTES: ReglaClasificacion[] = [
  // Proveedores comunes
  { keywords: ['carne', 'carnes', 'proteína'],     cuenta: '1430', desc: 'Inventario — cárnicos' },
  { keywords: ['verdura', 'vegetal', 'fruta'],      cuenta: '1430', desc: 'Inventario — perecederos' },
  { keywords: ['aceite', 'condimento', 'especia'],  cuenta: '1430', desc: 'Inventario — insumos cocina' },
  { keywords: ['gas', 'propano', 'energía'],        cuenta: '5220', desc: 'Servicios — gas' },
  { keywords: ['epm', 'energía eléctrica'],         cuenta: '5220', desc: 'Servicios — electricidad' },
  { keywords: ['arriendo', 'arrendamiento'],        cuenta: '5205', desc: 'Arrendamientos' },
  { keywords: ['aseo', 'limpieza', 'jabón'],        cuenta: '5240', desc: 'Mantenimiento y aseo' },
  { keywords: ['publicidad', 'marketing', 'redes'], cuenta: '5280', desc: 'Publicidad' },
  { keywords: ['contador', 'honorarios', 'asesor'], cuenta: '5110', desc: 'Honorarios profesionales' },
  { keywords: ['seguro', 'póliza'],                 cuenta: '5250', desc: 'Seguros' },
  { keywords: ['empaque', 'caja', 'bolsa'],         cuenta: '1430', desc: 'Inventario — empaques' },
];
```

Con estas reglas base, el cold start mejora significativamente. Y con el tiempo, las reglas del tenant específico van reemplazando las reglas base cuando son más precisas.

---

## Resumen de riesgos por probabilidad e impacto

| # | Riesgo | Probabilidad | Impacto | Estado |
|---|---|---|---|---|
| 1a | POS lento por llamada síncrona a Facture.co | Alta (sin control) | Catastrófico | ✅ Resuelto con BullMQ |
| 1b | POS caído sin internet | Alta (sin control) | Catastrófico | ✅ Resuelto con Workbox offline |
| 1c | Base de datos con latencia alta | Media | Alto | ✅ Resuelto con RDS Multi-AZ |
| 2 | Documento DIAN mal formado rechazado | Media | Alto | ⚠️ Validación pre-transmisión implementada — requiere tests exhaustivos |
| 3 | Acceso cruzado entre tenants | Baja (con middleware) | Catastrófico | ✅ Resuelto con middleware Prisma + tests de aislamiento |
| 4 | BullMQ pierde jobs DIAN | Media | Alto | ✅ Resuelto con proceso de reconciliación cron |
| 5 | Documentos de contingencia mal formados | Media | Alto | ⚠️ Requiere librería `@yaro/xml-builder` compartida frontend/backend |
| 6 | Librería `@yaro/ui` diverge de prototipos | Alta (sin disciplina) | Medio | ⚠️ Mitigado con checklist de PR — requiere disciplina constante |
| 7 | Migraciones generan downtime | Media (sin expand-contract) | Alto | ✅ Resuelto con política expand-contract en CONTRIBUTING.md |
| 8 | Motor de reglas IA sin reglas iniciales | Alta en cold start | Medio | ⚠️ Requiere reglas base pre-cargadas por sector |

**Leyenda:**
- ✅ Resuelto: hay una solución implementada o definida que mitiga el riesgo de forma robusta.
- ⚠️ Mitigado: hay controles pero el riesgo no está completamente eliminado — requiere atención continua.

---

## La verdad incómoda

Los riesgos marcados con ⚠️ son los que más probabilidad tienen de causar problemas en producción. No porque sean difíciles técnicamente, sino porque requieren disciplina sostenida: que cada PR sea revisado con el checklist, que cada migración siga el patrón expand-contract, que cada componente nuevo se compare visualmente con el prototipo.

La disciplina de equipo es más difícil de mantener que el código. Ese es el riesgo real.

---

*YARO — Documento de crítica v1.0 · Mayo 2026*
*Este documento debe actualizarse cada vez que se descubra un nuevo riesgo técnico en producción. Si encontraste un bug grave que no está acá, agrégalo. El objetivo es que este documento sea siempre más honesto que optimista.*
