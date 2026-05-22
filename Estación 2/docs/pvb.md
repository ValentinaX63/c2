# Product Vision Board — YARO
> **Producto:** Sistema operativo todo en uno para restaurantes (SaaS B2B)
> **Versión:** 1.0 — Mayo 2026
> **Audiencia de este documento:** Ingenieros nuevos en el equipo de YARO

---

## PRODUCTO

**Nombre:** YARO — Your All-In-One Restaurant Operations

**Descripción en una línea:** Sistema operativo que reemplaza las 4 o 5 herramientas que hoy usa un restaurante por separado — POS, contabilidad, inventario, nómina y centro de producción — en una sola plataforma integrada que además cumple automáticamente con la normativa fiscal colombiana (DIAN).

**Tagline interno:** *"El primer sistema que opera el restaurante y cumple con la DIAN al mismo tiempo."*

---

## 1. PROBLEMA

### El problema que resuelve YARO

Un restaurante colombiano mediano opera hoy con un ecosistema fragmentado de herramientas que no se hablan entre sí:

| Herramienta | Qué hace | Costo mensual aprox. |
|---|---|---|
| POS (Toteat, Square, etc.) | Cobros y mesas | $150–300 USD |
| Software contable (Siigo, Alegra) | Contabilidad + FE | $50–120 USD |
| Software de nómina | Liquidación de empleados | $30–60 USD |
| Excel / Google Sheets | Inventario y food cost | $0 (pero con horas de trabajo) |
| WhatsApp | Comunicación entre cocina y proveedores | $0 (pero caótico) |
| **Total** | | **$230–480 USD/mes + horas** |

**El resultado:** el dueño o administrador del restaurante consolida manualmente la información entre todas estas herramientas. El food cost real lo calcula en Excel a fin de mes. Las facturas de proveedores las clasifica manualmente en Siigo. La nómina la liquida en otra herramienta. La DIAN es un proceso separado que requiere coordinación con el contador.

**La consecuencia concreta:** decisiones financieras basadas en datos atrasados, imprecisos o incompletos. Un restaurante que vende $50M al mes puede no saber su food cost real hasta 30 días después.

### Cifras concretas del dolor

- **Obligatoriedad DIAN desde 2023:** el tiquete POS electrónico (Resolución 000151/2022) es obligatorio para todos los restaurantes. La mayoría lo incumple sin saberlo o lo maneja en un sistema separado del POS.
- **Sanción por no facturar electrónicamente:** cierre del establecimiento entre 1 y 3 días en la primera infracción (E.T. Art. 652).
- **Food cost fuera de control:** el promedio de la industria en Colombia estima que los restaurantes sin control de food cost tienen entre 5 y 12 puntos porcentuales de desperdicio invisible por errores de porcionado, merma no registrada y compras sin trazabilidad.
- **Tiempo de conciliación manual:** 4–5 horas por semana del administrador en consolidar información entre sistemas.

### ¿Por qué este problema persiste?

Porque las soluciones existentes resuelven un solo pedazo del problema:

- **POS especializados** (Toteat, GetJusto): excelente para operación pero sin contabilidad formal, sin DIAN completo, sin nómina.
- **Software contable** (Siigo, Alegra): excelente para contabilidad pero no entiende las particularidades operativas de un restaurante (lotes, CDP, mermas, food cost).
- **ERP genéricos** (Zoho, SAP): demasiado complejos y costosos para el tamaño del mercado objetivo.

**Nadie ha integrado los dos mundos — operación + compliance fiscal — en un solo sistema diseñado específicamente para el sector HORECA colombiano.**

---

## 2. SEGMENTO TARGET

### ¿Para quién es YARO?

YARO sirve **cualquier tamaño de operador del sector HORECA colombiano**, desde una persona natural con un solo local hasta una cadena con múltiples sedes y centro de producción propio. El sistema se adapta al régimen tributario, al tamaño y al nivel de formalización contable de cada negocio.

**Cliente Fase 1 — los próximos 6 meses:**

El cliente de Fase 1 no es un perfil único — es una combinación de tres perfiles que coexisten en el mercado y que YARO puede atender desde el primer día porque la plataforma escala entre ellos:

| Perfil | Descripción | Ejemplo |
|---|---|---|
| **Dueño operador** | 1 sede, sin estructura administrativa formal, el dueño hace todo | Restaurante de barrio en Medellín, 8–15 empleados |
| **Negocio en crecimiento** | 2–4 sedes, admin diferenciado, necesita control financiero real | Cadena de hamburguesas en Antioquia |
| **Operación con CDP** | 3+ sedes con centro de producción centralizado | Franquicia o marca con cocina central |

**Geográfico:** Colombia. Mercado inicial Antioquia (Medellín, Guarne, La Ceja, municipios del oriente antioqueño). Expansión posterior a Bogotá, Cali, Barranquilla.

### ¿Quién controla la decisión de compra?

Dos perfiles, en este orden de poder:

1. **Dueño / Socio del restaurante** — Decisor final. Le importa el costo total vs lo que paga hoy, la facilidad de uso para su equipo operativo, y el cumplimiento DIAN (que le genera ansiedad porque sabe que lo puede sancionar). Adopta si ve que YARO le quita trabajo, no que le agrega.

2. **Contador externo del restaurante** — Veto técnico en todo lo fiscal. Si el contador dice que el sistema no genera los documentos correctamente para la DIAN, el dueño no lo adopta. **La estrategia: convertir al contador en aliado, no en obstáculo.** YARO debe ser la herramienta que el contador recomienda a sus clientes del sector restaurantero.

**Implicación para el producto:** la UX del cajero y del mesero debe ser tan simple que no requiera capacitación. La UX del módulo contable debe ser lo suficientemente rigurosa para que un contador la apruebe.

---

## 3. DIFERENCIADOR PRINCIPAL

### La unión de operación + administración + contabilidad en un solo sistema

El diferenciador de YARO no es una feature — es la **ausencia de fricción entre tres mundos que hoy están separados:**

```
OPERACIÓN          ADMINISTRACIÓN       CONTABILIDAD
(lo que pasa       (lo que se          (lo que se
 en el turno)       gestiona)           reporta)

  POS ──────────────── Inventario ──────── Food cost real
  KDS ──────────────── CDP / Lotes ─────── Trazabilidad fiscal
  Turno ─────────────── Nómina ───────────── DIAN automático
  Arqueo ─────────────── Reportes ──────────── Conciliación
```

En YARO, estos tres mundos comparten la misma base de datos en tiempo real. Cuando un cajero cobra una mesa, ese cobro genera automáticamente el tiquete POS electrónico para la DIAN, actualiza el inventario, registra el método de pago para la conciliación bancaria futura, y alimenta el P&G del día. **Sin intervención humana adicional.**

**Por qué esto es difícil de replicar:** no es un problema de código — es un problema de conocimiento del dominio. Integrar correctamente el impoconsumo del 8% con la normativa DIAN, la trazabilidad de lotes con el food cost real, y la nómina electrónica con el control de asistencia, requiere entender el sector desde adentro. **La fundadora de YARO opera su propio restaurante.** Eso no es un detalle menor — es la razón por la que cada decisión de producto tiene validación inmediata en producción real.

---

## 4. ARENA COMPETITIVA

**YARO compite en un mercado existente con una propuesta diferenciada, no en una categoría nueva.**

### Mapa de competidores

| Competidor | Fortaleza | Debilidad vs YARO |
|---|---|---|
| **Toteat** (Chile) | POS robusto, buena UX | Sin DIAN nativo, sin CDP, sin nómina |
| **GetJusto** (Chile/LatAm) | Delivery propio, ecommerce | Sin DIAN Colombia, sin contabilidad |
| **Siigo** (Colombia) | Contabilidad + DIAN completo | Sin POS, sin operación de restaurante |
| **Alegra** (Colombia) | Precio bajo, fácil de usar | Sin POS real, DIAN básico, sin CDP |
| **Zoho Creator** | Personalizable, ecosistema grande | Lo construyes tú — no viene hecho |

### Posicionamiento defensible

YARO no compite con ninguno de ellos directamente — ocupa el espacio que **ninguno de ellos puede llenar sin reescribir su producto**:

```
                    OPERACIÓN COMPLETA
                         ▲
                         │
                    [ YARO ]
                         │
CONTABILIDAD ────────────┼──────────── POS ESPECIALIZADO
    básica               │                  puro
                         │
                    Siigo/Alegra ─────── Toteat/GetJusto
                         │
                    COMPLIANCE
                     DIAN completo
```

**La amenaza real:** que GetJusto o Toteat decidan integrar la DIAN colombiana correctamente. Tienen la infraestructura técnica. Si lo hacen, el diferencial fiscal de YARO se reduce. Por eso el MOAT de datos (trazabilidad lote → plato → food cost histórico) es más estratégico a largo plazo que el diferencial fiscal — ese sí es imposible de replicar comprando un módulo.

---

## 5. MODELO ECONÓMICO

### Pricing — por sede activa / mes en COP

El modelo de cobro es **plan base (primera sede incluida) + cargo por cada sede adicional**. La lógica: el costo de infraestructura AWS no escala linealmente con las sedes (el RDS y el cluster Fargate son compartidos), pero sí escala el volumen de transacciones y almacenamiento de XMLs DIAN. El cargo por sede adicional refleja ese costo real.

| Plan | Base/mes (1 sede) | Sede adicional/mes | Incluye |
|---|---|---|---|
| **Starter** | $219.000 COP | $150.000 COP | POS + KDS + Inventario básico + Tiquete POS DIAN + Marcación horario |
| **Pro** | $289.000 COP | $200.000 COP | Todo Starter + FE completa DIAN + Buzón DIAN + Recetas + Food cost |
| **Multi** | $429.000 COP | $300.000 COP | Todo Pro + CDP completo + Nómina electrónica + Conciliación bancaria |
| **Enterprise** | A convenir | A convenir | Todo Multi + API acceso + SLA dedicado + onboarding personalizado |

### Ejemplo concreto — 911 Hot Burger (cliente piloto)

```
Plan Multi, 2 sedes:
  Base:              $429.000 COP
  Sede adicional:    $300.000 COP
  Total/mes:         $729.000 COP (~$178 USD)

Lo que reemplaza:
  Toteat (POS):      ~$500 USD
  Siigo (contab.):   ~$120 USD
  Software nómina:   ~$50 USD
  Total actual:      ~$670 USD

Ahorro del cliente: ~$492 USD/mes (~$2.4M COP/mes)
YARO es 73% más barato y más completo.
```

### Economía por cliente activo

| Escenario | Ingreso/mes | Costo AWS/mes | Margen bruto |
|---|---|---|---|
| Starter, 1 sede | $219.000 COP | ~$57.000 COP | **74%** |
| Pro, 1 sede | $289.000 COP | ~$57.000 COP | **80%** |
| Multi, 2 sedes | $729.000 COP | ~$73.000 COP | **90%** |
| Multi, 4 sedes | $1.329.000 COP | ~$106.000 COP | **92%** |

**Lectura:** El margen mejora con las sedes adicionales porque el costo de AWS crece más lento que el ingreso. A partir de 20 clientes activos en plan Pro o Multi, YARO es rentable en infraestructura.

### MRR proyectado por fase

| Fase | Clientes | MRR estimado |
|---|---|---|
| Fase 1 (mes 8) | 5 activos, mix Starter/Pro | ~$1.4M COP/mes |
| Fase 2 (mes 18) | 20 activos, mix Pro/Multi | ~$7.2M COP/mes |
| Fase 3 (mes 30) | 50 activos, mix Pro/Multi | ~$18M COP/mes |

---

## 6. ROADMAP DE PRODUCTO

El roadmap está dividido en tres fases con propuesta de valor independiente. Cada fase es vendible sin la siguiente.

### Fase 1 — "Opera y cumple" (meses 1–8)

**Lo que se construye:** POS, KDS, Inventario básico, Tiquete POS DIAN (prioridad máxima), Factura electrónica DIAN, Modo offline (POS sin internet), Marcación de horario + bienestar, YARO Console (portal interno).

**Por qué este orden:** el tiquete POS electrónico es la urgencia regulatoria que acelera la adopción. Un restaurante que incumple con la DIAN está buscando solución activamente. YARO les da eso y la operación completa al mismo tiempo.

**Schema Prisma:** 14 tablas. No más.

### Fase 2 — "Gestiona y controla" (meses 9–18)

**Lo que se añade:** CDP completo, Contabilidad formal, Nómina electrónica DIAN, Recetas con food cost, Retenciones en la fuente, Conciliación bancaria asistida.

### Fase 3 — "Escala e intelige" (meses 19–30)

**Lo que se añade:** IA predictiva (stock + promociones), Conciliación bancaria automatizada (matching CSV), Expansión normativa (Perú, Ecuador, México).

### Fase 4 — Backlog estratégico

Ecommerce propio + delivery personalizado (canal directo sin comisión de terceros como Rappi). Decisión tomada en mayo 2026 tras análisis competitivo con GetJusto.

---

## 7. MOAT — VENTAJA COMPETITIVA DURADERA

### El MOAT de YARO no es tecnológico — es de datos y conocimiento del dominio

**MOAT 1 — Trazabilidad lote → plato (el más difícil de replicar)**
YARO registra el ciclo completo desde el primer día: insumo → lote CDP → producción → traslado → venta → food cost real. Después de 12 meses de operación, un restaurante tiene un historial que no existe en ningún otro sistema y que no puede migrar fácilmente. Ese historial se vuelve más valioso cada mes.

**MOAT 2 — Integración DIAN nativa (el más urgente hoy)**
La obligatoriedad crea urgencia de mercado. La configuración fiscal validada por el contador, los XMLs almacenados 5 años, el historial de transmisiones — todo eso genera fricción de salida real.

**MOAT 3 — Founder-market fit (el más valioso en etapa temprana)**
La fundadora opera 911 Hot Burger en Guarne y La Ceja. Cada decisión de producto tiene validación inmediata en producción real. Esa velocidad de iteración con feedback real es imposible de replicar para un equipo que no opera un restaurante.

**MOAT 4 — Switching cost acumulado**
Cada mes que un restaurante usa YARO acumula: historial fiscal, configuración contable, historial de nómina, recetas calibradas, reglas de IA aprendidas. Después de 24 meses, migrar no es difícil técnicamente — es doloroso operativamente.

---

## 8. MÉTRICAS DE ÉXITO

### Métricas de Fase 1 (primeros 6 meses)

1. **Tenants activos con operación diaria completa** — target: 5. Proxy de adopción real, no solo registro.
2. **Tasa de transmisión exitosa DIAN** — target: ≥ 99%. Métrica de confianza del sistema.
3. **Latencia del endpoint `/cobros`** — target: p95 < 100ms. Si el cajero espera, la adopción muere.
4. **Tasa de éxito offline → sincronización DIAN** — target: ≥ 98%.

### Métricas de Fase 2 (meses 9–18)

5. **Food cost real vs estimado** — target: diferencia < 2pp en primer mes de operación. Valida que el módulo de recetas y CDP funciona correctamente.
6. **Tasa de confirmación de sugerencias IA (clasificación PUC)** — target: > 80% en mes 3. Si el motor de reglas sugiere bien, el contador confía.
7. **Tiempo de conciliación bancaria asistida** — target: < 45 min por período.

### Métrica de salud del negocio

8. **Gross margin de infraestructura** — target: > 80% en promedio del portafolio activo. Si baja de 70%, hay un problema de arquitectura o pricing.

---

## 9. DECISIONES TÉCNICAS CRÍTICAS

> Esta sección existe para que un ingeniero nuevo entienda el *por qué* detrás de las decisiones de arquitectura, no solo el *qué*. Cada una de estas decisiones tiene un documento más detallado en el `CONTRIBUTING.md`.

**Decisión 1 — Fire-and-forget con BullMQ para DIAN**
El endpoint de cobro del POS responde en < 100ms sin esperar a Facture.co. La transmisión ocurre asíncronamente en un worker BullMQ. Esto no es una optimización — es un requisito de UX del cajero en operación real.

**Decisión 2 — Modelo append-only para documentos fiscales**
Ningún documento fiscal puede editarse ni eliminarse después de generado. Garantizado a nivel SQL y a nivel de repositorio NestJS. Para anular una FE se emite una nota crédito — nunca se modifica la fila original. Esto es un requisito legal, no una preferencia técnica.

**Decisión 3 — Schema de Prisma por fases**
14 tablas en Fase 1, +13 en Fase 2, +4 en Fase 3. No se crean tablas de fases futuras antes de tiempo. Las migraciones en producción siguen el patrón expand-contract obligatoriamente.

**Decisión 4 — Modo offline con Workbox + IndexedDB**
El POS debe funcionar sin internet. Los cobros se almacenan en IndexedDB con expiración de 48 horas (el límite de contingencia de la DIAN) y se sincronizan al reconectarse. Esto no es backlog — es Fase 1.

**Decisión 5 — Fallback de IA en tres capas**
Claude Sonnet → Motor de reglas → Default. La contabilidad nunca se bloquea por downtime de la API de Anthropic.

---

*YARO PVB v1.0 · Mayo 2026*
*Para ingenieros nuevos: este documento responde el "por qué". El "cómo" está en el PRD y el CONTRIBUTING.md.*
