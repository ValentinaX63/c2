# Análisis de mercado — YARO
> Documento de contexto para ingenieros nuevos en el equipo.
> Fuentes: ACODRES, ANDI, DANE, ACODRES Pacífico, La Barra, Portafolio — mayo 2026.

---

## 1. Por qué AHORA

El mercado de software para restaurantes en Colombia tiene una ventana de oportunidad específica que no existía hace 3 años y que no durará indefinidamente: la **obligatoriedad regulatoria de la DIAN** como acelerador de adopción digital.

Tres eventos convergentes crean el momento:

**Evento 1 — Obligatoriedad del tiquete POS electrónico (2022–2023)**
La Resolución 000151 de 2022 hizo obligatorio el tiquete POS electrónico para todos los restaurantes. La mayoría lo incumple o lo maneja en un sistema separado del POS. La sanción es cierre del establecimiento entre 1 y 3 días en la primera infracción. Esto convierte un pain de eficiencia en un pain regulatorio — y los pains regulatorios aceleran decisiones de compra que de otro modo tardarían años.

**Evento 2 — Costos operativos en máximos históricos (2024–2025)**
El sector de restaurantes en Colombia cerró el 2024 con resultados difíciles. El costo de operación se está triplicando y los reajustes de la reforma laboral están apretando más la rentabilidad. Un restaurante que opera con márgenes ajustados necesita visibilidad financiera en tiempo real — no un P&G que el contador entrega 30 días después.

**Evento 3 — El mercado sobrevive y se reorganiza (2025)**
Para el primer semestre de 2025 las ventas mostraron una ligera recuperación, creciendo un 7%. El sector no está en colapso — está en presión. Y cuando un sector está en presión busca eficiencia activamente. Ese es el momento en que el software de gestión se vende.

---

## 2. Tamaño del mercado en Colombia

### El universo total

Al cierre de 2024, en Colombia operaban más de 150.000 establecimientos gastronómicos, según datos de ACODRES. Ese es el TAM (Total Addressable Market) máximo — todos los restaurantes del país.

Los establecimientos independientes representan el 95% del mercado, según ACODRES. Eso significa que aproximadamente **142.500 restaurantes son independientes** — exactamente el perfil de cliente que YARO puede servir. Las cadenas grandes (Frisby, Crepes & Waffles, El Corral) ya tienen software propio o contratos enterprise con proveedores internacionales.

El sector de restaurantes, hoteles y cafeterías representó en Colombia un 3,9% del PIB en 2023, según la ANDI, con ventas de servicios de alimentación que alcanzaron US$14.000 millones ese año.

### TAM / SAM / SOM — estimación conservadora

**TAM (Total Addressable Market):** Todos los restaurantes independientes en Colombia con algún nivel de formalización.

```
150.000 restaurantes totales
× 95% independientes = 142.500
× 40% con algún nivel de facturación formal = ~57.000 establecimientos
× precio promedio YARO Starter ($219K COP/mes) = ~$12.5B COP/mes = ~$150B COP/año
```

**SAM (Serviceable Addressable Market):** Restaurantes con 1 o más sedes formales, con empleados registrados y que ya emiten o deben emitir facturas electrónicas. Estimación: ~25.000 establecimientos en Colombia.

```
25.000 establecimientos
× precio promedio YARO Pro ($289K COP/mes) = ~$7.2B COP/mes = ~$86.6B COP/año
```

**SOM (Serviceable Obtainable Market) — 3 años:**
Con un equipo pequeño y enfoque en Antioquia primero, seguido de Bogotá y Cali:

```
Año 1 (Fase 1): 5–10 clientes activos → ~$5-8M COP MRR
Año 2 (Fase 2): 20–40 clientes activos → ~$12-25M COP MRR
Año 3 (Fase 3): 50–100 clientes activos → ~$25-60M COP MRR
```

**Lectura importante para el ingeniero:** El SOM de YARO en 3 años es menos del **0.1% del SAM**. No necesitamos dominar el mercado para construir un negocio rentable — necesitamos capturar una porción muy pequeña de un mercado muy grande.

---

## 3. El dolor que hace que el mercado compre HOY

### Crisis de costos operativos — el pain real de 2025

Según el presidente de Acoga, "mejoraron ventas, pero se triplicaron los costos." Esa frase resume el pain principal del restaurantero colombiano hoy: vende más pero gana menos. Y no sabe exactamente por qué.

La respuesta suele estar en tres lugares que sin YARO son invisibles:

1. **Food cost descontrolado:** sin trazabilidad lote → plato, el dueño no sabe cuánto cuesta producir cada plato con los precios actuales de insumos. Calcula el food cost una vez al año y lo aplica todo el año mientras los insumos suben.

2. **Merma no registrada:** en un restaurante sin CDP formalizado, el ingrediente que se daña, el plato que sale mal, el porcionado incorrecto — todo eso se "pierde" sin registrarse. Esa merma invisible puede representar entre 5 y 12 puntos de food cost.

3. **Costos laborales sin control:** la reforma laboral de 2025 aumentó los recargos nocturnos y dominicales. Sin un sistema que calcule automáticamente las horas extras y los recargos por turno, la nómina es una fuente constante de errores y sorpresas al final del mes.

**YARO hace visible lo invisible.** Eso no es una metáfora — es la propuesta de valor concreta: el dueño del restaurante pasa de calcular su food cost en Excel una vez al mes a verlo en tiempo real por plato.

### La obligatoriedad DIAN — el pain que crea urgencia

La DIAN no es solo una obligación — es un acelerador de adopción tecnológica para YARO.

**Por qué:** Antes de la obligatoriedad del tiquete POS electrónico, un restaurante podía operar indefinidamente con un cuaderno o con una caja registradora sin software. La Resolución 000151/2022 cambió eso. Ahora cada venta debe generar un documento electrónico transmitido a la DIAN. Los restaurantes que no lo hacen están en incumplimiento diario.

**La oportunidad de YARO:** La mayoría de los restaurantes están resolviendo este problema de forma fragmentada — tienen un software de facturación electrónica (Siigo, Alegra, o el portal gratuito de la DIAN) separado de su POS. Eso genera doble trabajo: el cajero cobra en el POS, y alguien más tiene que registrar esa venta en el sistema de facturación. YARO elimina ese doble trabajo porque el POS y la DIAN son el mismo sistema.

---

## 4. El contexto regulatorio completo

Un ingeniero que trabaja en YARO necesita entender el marco normativo porque impacta directamente las decisiones técnicas del sistema.

### Documentos DIAN que YARO debe generar

| Documento | Norma | Obligatoriedad | Fase YARO |
|---|---|---|---|
| Tiquete POS electrónico | Resolución 000151/2022 | Todos los restaurantes | Fase 1 — prioridad máxima |
| Factura electrónica de venta | Estatuto Tributario | Obligatoria a solicitud del cliente | Fase 1 |
| Nota crédito | Art. E.T. | Para anular FE | Fase 2 |
| Documento soporte de compras (DSCE) | Resolución 000167/2021 | Para compras a no obligados a facturar | Fase 2 |
| Nómina electrónica | Resolución 000013/2021 | Empleadores con costos de nómina deducibles | Fase 2 |

### Impuesto al consumo — el más mal entendido del sector

El impoconsumo del 8% (Art. 512-1 del E.T.) es uno de los temas más confusos del sector HORECA colombiano. Puntos clave que el equipo debe entender:

- **No depende del régimen tributario.** Un restaurante en Régimen Simple puede estar obligado al impoconsumo si supera los topes (3.500 UVT ≈ $165M de ingresos anuales en 2025).
- **No es IVA.** Se registra en cuentas PUC separadas. Mezclarlo es un error contable grave.
- **Las propinas no son del restaurante.** Nunca deben aparecer en reportes fiscales ni en el impoconsumo. Son del trabajador.

ACODRES ha propuesto reducir el impuesto al consumo del 8% al 5%, con la posibilidad de convertirlo en un IVA más estable para el sector. Si esto ocurre, YARO debe poder actualizarlo desde la configuración sin cambiar código — por eso todos los valores fiscales viven en la tabla `country_config`, no hardcodeados.

---

## 5. Mapa competitivo detallado

### ¿Quién está compitiendo hoy en Colombia?

El mercado colombiano de software para restaurantes tiene tres capas:

**Capa 1 — Software contable con algo de POS**

| Software | Fortaleza | Debilidad crítica para restaurantes |
|---|---|---|
| **Siigo** | DIAN completo, contabilidad robusta, marca fuerte en Colombia | Sin POS operativo real. Sin inventario de restaurante. Sin CDP. Sin KDS. |
| **Alegra** | Precio bajo, fácil de usar, buena FE | Sin POS, inventario básico, sin food cost, sin CDP |
| **World Office** | Muy usado por contadores en Colombia | Interfaz anticuada, sin integración operativa |

**Capa 2 — POS especializado sin contabilidad**

| Software | Fortaleza | Debilidad crítica para Colombia |
|---|---|---|
| **Toteat** (Chile) | POS robusto, buena UX, experiencia en LATAM | Sin DIAN nativo, sin contabilidad colombiana, sin nómina |
| **GetJusto** (Chile/LatAm) | Delivery propio, ecommerce, IA de comandas | Sin integración DIAN Colombia, sin contabilidad |
| **iFood POS** | Integración con delivery iFood | Enfocado en delivery, sin gestión completa |

**Capa 3 — Software genérico adaptado**

| Software | Fortaleza | Debilidad |
|---|---|---|
| **Zoho Creator** | Personalizable, ecosistema enorme | Lo construyes tú — no viene hecho para restaurantes |
| **QuickBooks** | Contabilidad internacional | Sin adaptación a normativa colombiana DIAN |

### El espacio que nadie ocupa

```
                    OPERACIÓN COMPLETA
                   (POS + KDS + CDP + RRHH)
                           ▲
                           │
                      [ YARO ]
                           │
CONTABILIDAD  ─────────────┼─────────────  POS ESPECIALIZADO
DIAN completa              │               puro
                           │
              Siigo ───────┤─────── Toteat / GetJusto
              Alegra       │
                           │
                    COMPLIANCE FISCAL
                    (DIAN + Nómina)
```

Ningún competidor existente puede entrar al cuadrante de YARO sin reescribir su producto. Siigo tendría que construir un POS desde cero. Toteat tendría que construir integración DIAN colombiana desde cero. Ambas son apuestas grandes para mercados que ya tienen clientes en otro segmento.

---

## 6. Riesgos de mercado que el equipo debe conocer

### Riesgo 1 — Alta informalidad del sector

La informalidad está en aumento en el sector restaurantero colombiano, con una tendencia hacia modelos que enfrentan menores cargas tributarias. Un restaurante informal no es cliente de YARO — no quiere visibilidad fiscal, quiere lo contrario. El mercado objetivo de YARO es el **restaurante formal o en proceso de formalización**. Eso reduce el universo real pero también define un cliente con una necesidad clara y urgente.

### Riesgo 2 — Alta mortalidad del sector

En 2024 cerraron más de 6.950 restaurantes en Colombia, el doble de los 3.500 cierres de 2023. Esto significa que el churn por quiebre del cliente es una realidad del mercado. La estrategia de YARO debe incluir clientes con mayor probabilidad de sobrevivencia — restaurantes con más de 12 meses de operación, con flujo de caja demostrable, no startups gastronómicas de primer año.

### Riesgo 3 — Competidor grande que integra DIAN

Si GetJusto o Toteat deciden construir la integración DIAN colombiana correctamente, el diferencial fiscal de YARO se reduce. Este riesgo es real pero manejable por dos razones:
- La integración DIAN es compleja (47 campos de nómina electrónica, múltiples tipos de documentos, validación XSD) — toma meses hacerla bien.
- El MOAT de datos de trazabilidad lote → plato → food cost histórico es imposible de replicar comprando un módulo.

### Riesgo 4 — Cambios normativos DIAN

La DIAN actualiza sus resoluciones periódicamente. Un cambio en el anexo técnico del tiquete POS electrónico puede requerir actualizaciones urgentes al sistema. Por eso la arquitectura de YARO separa la lógica de generación de XMLs de la lógica de negocio — el worker de BullMQ que genera el XML puede actualizarse de forma independiente sin afectar el POS.

---

## 7. Go-to-market — estrategia geográfica recomendada

### Fase 1 — Antioquia (meses 1–12)

**Por qué empezar aquí:**
- El restaurante piloto (911 Hot Burger) opera en Guarne y La Ceja — hay acceso directo al cliente para validar en tiempo real.
- El ecosistema gastronómico de Medellín y el Oriente Antioqueño es activo y creciente.
- Los restaurantes de municipios intermedios (Guarne, La Ceja, Rionegro, Envigado, Sabaneta) tienen un problema adicional: la conectividad es inestable. Eso hace que el **modo offline de YARO sea un diferenciador concreto**, no solo una feature en el PRD.

**Target inicial:** restaurantes de 1–4 sedes en el eje Medellín – Oriente Antioqueño, con facturación entre $50M y $500M COP al mes, con al menos 1 año de operación formal.

### Fase 2 — Bogotá (meses 12–24)

Mayor concentración de restaurantes formales con estructura administrativa. Ciclo de ventas más largo pero ticket más alto (más sedes, más usuarios, más volumen DIAN).

### Fase 3 — Cali, Barranquilla, ciudades intermedias

Expansión horizontal dentro de Colombia antes de salir del país.

### Expansión LATAM — Fase 3+

La arquitectura de `country_config` permite agregar un nuevo país sin modificar el núcleo del sistema. La ruta recomendada: Perú (SUNAT, la normativa más similar a la DIAN), Ecuador (SRI), México (SAT, mercado enorme pero normativa más compleja).

---

## 8. Hipótesis comercial que guía las decisiones de producto

**Hipótesis 1:** El restaurante colombiano formal con 1–4 sedes está dispuesto a pagar entre $219K y $730K COP/mes por un sistema que consolide operación + DIAN en un solo lugar, si ese sistema le ahorra el equivalente en tiempo y en errores.

**Validación:** 911 Hot Burger hoy paga ~$670 USD/mes entre Toteat + Siigo + software de nómina. YARO Multi para 2 sedes cuesta $729K COP (~$178 USD). El ahorro es de ~$492 USD/mes. La propuesta se vende sola cuando se muestra el número.

**Hipótesis 2:** El contador externo del restaurante puede convertirse en canal de distribución. Si YARO le facilita el trabajo al contador (buzón DIAN integrado, clasificación automática de facturas, nómina electrónica en un clic), el contador recomienda YARO a sus clientes del sector.

**Hipótesis 3:** El modo offline no es un feature opcional — es el requisito que desbloquea la adopción en ciudades intermedias y centros comerciales con conectividad inestable.

---

*YARO — Análisis de mercado v1.0 · Mayo 2026*
*Fuentes: ACODRES, ANDI, DANE, Portafolio, La Barra, ACODRES Pacífico.*
*Para el equipo de ingeniería: este documento explica el contexto de mercado que justifica cada decisión de producto.*
