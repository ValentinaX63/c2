# Ideal Customer Profile — YARO
> Perfil de cliente ideal para YARO. Para ingenieros nuevos en el equipo.
> Este documento responde: ¿con quién hablamos cuando necesitamos validar una decisión de producto?

---

## Segmento beachhead — Fase 1 (próximos 6 meses)

**Tipo de negocio:** Restaurantes independientes en Colombia con al menos 1 sede formal, operando bajo cualquier régimen tributario (Ordinario, SIMPLE o Persona Natural), con mínimo 1 año de operación activa y facturación mensual entre $30M y $500M COP.

**Por qué "mínimo 1 año de operación":** El análisis de mercado muestra que en 2024 cerraron más de 6.950 restaurantes en Colombia. Un cliente que lleva menos de un año tiene alta probabilidad de churn por quiebre. YARO prioriza restaurantes con historial demostrable de supervivencia.

**Tipo de cocina:** No discrimina. Hamburguesas, comida a la carta, dark kitchen, fast food casual — el problema de fragmentación de herramientas y el incumplimiento DIAN es transversal a todos los tipos de cocina.

**Geografía inicial:** Antioquia — Medellín y Oriente Antioqueño (Guarne, La Ceja, Rionegro, Envigado, Sabaneta). Expansión posterior a Bogotá y Cali.

---

## Los tres perfiles de usuario

YARO tiene tres tipos de usuarios que interactúan con el sistema de formas completamente distintas. Un ingeniero nuevo necesita entender los tres porque cada uno tiene pains diferentes, prioridades diferentes y puede vetar la adopción del producto de formas diferentes.

---

### Perfil 1 — El Dueño / Socio (decisor y pagador)

**Quién es:**

Dueño o socio de 1 a 4 restaurantes. Puede ser persona natural o representante legal de una SAS o LTDA. En restaurantes pequeños, opera directamente — está en el local, conoce a su equipo, entiende el producto. En negocios más grandes, ya tiene un administrador que opera el día a día pero él sigue tomando las decisiones de inversión.

Tiene entre 28 y 55 años. No es necesariamente técnico. Su relación con el software es pragmática: lo usa si le ahorra tiempo o dinero, y lo abandona si le complica la vida. Ha probado uno o dos sistemas antes — probablemente Siigo para la parte contable y un POS básico o incluso cuadernos para el resto. Sabe que tiene un problema pero no sabe exactamente cuál es la mejor solución.

**Qué evalúa antes de adoptar YARO:**

- ¿Cuánto me cuesta esto comparado con lo que pago hoy? El argumento de "reemplaza 3 herramientas" debe traducirse en un número de COP concreto, no en una propuesta de valor abstracta.
- ¿Mi equipo va a poder usarlo sin capacitación larga? El cajero y el mesero tienen que entenderlo el primer día.
- ¿Me resuelve el problema de la DIAN? Esta es su ansiedad principal — sabe que puede multarlo y no sabe si está cumpliendo correctamente.
- ¿Voy a poder ver mis números en tiempo real? La pregunta que más le quita el sueño es "¿estoy ganando o perdiendo dinero este mes?" — y con los sistemas actuales no lo sabe hasta que el contador le entrega el informe.

**Qué lo mata:**

- Precio que no se justifica en los primeros 30 días.
- Un sistema que su cajero no entienda sin ayuda.
- Que la migración desde su sistema actual sea un proceso traumático.
- Que el sistema falle en plena hora pico del almuerzo.

**Triggers de compra — qué evento detona la adopción:**

1. **Visita de la DIAN o amenaza de sanción.** Cuando recibe una notificación de incumplimiento o un colega le cuenta que lo multaron. El dolor regulatorio se vuelve concreto y urgente.
2. **El contador le dice que necesita facturación electrónica.** El contador tiene autoridad sobre él en temas fiscales. Si el contador recomienda YARO, el dueño lo adopta.
3. **Un mes con pérdidas inexplicables.** Cuando el food cost se dispara sin entender por qué. Ese es el momento en que busca control, no eficiencia.
4. **Apertura de una segunda sede.** La complejidad operativa se duplica y el sistema de cuadernos o Excel deja de funcionar.

**Objeciones probables:**

| Objeción | Respuesta |
|---|---|
| "Ya tengo Siigo y funciona bien" | Siigo hace contabilidad pero no tiene POS, no tiene KDS, no tiene inventario de restaurante. Sigues teniendo dos sistemas separados y doble trabajo de digitación. |
| "Mi cajero no sabe de tecnología" | El POS de YARO está diseñado para que un cajero sin experiencia lo use el primer día. El flujo de cobro es: seleccionar mesa → cobrar → confirmar. Nada más. |
| "Es muy caro" | ¿Cuánto pagas hoy entre Siigo + tu POS + el tiempo que tu admin dedica a consolidar la información? El precio de YARO es menor que la suma de lo que ya gastas. |
| "¿Qué pasa si se cae el internet?" | YARO funciona sin internet. El POS sigue operando y los tiquetes se sincronizan con la DIAN cuando vuelve la conexión. |

---

### Perfil 2 — El Administrador / Gerente de sede (usuario operativo principal)

**Quién es:**

La persona que opera el restaurante día a día. Puede ser un familiar del dueño o un profesional contratado. Tiene entre 24 y 40 años. Es el puente entre el dueño (que quiere números) y el equipo operativo (cajero, meseros, cocina). Es quien usa YARO más horas al día — revisa el inventario, hace el cierre de turno, aprueba los descuentos, recibe los traslados del CDP.

Es el usuario que puede hundir la adopción del sistema si lo percibe como "más trabajo" en lugar de "menos trabajo". Y es el usuario que más puede convertirse en defensor de YARO si el sistema genuinamente le simplifica la vida.

**Qué evalúa:**

- ¿El cierre de turno y el arqueo son rápidos? Hoy ese proceso le puede tomar 30–45 minutos al final del día. Si YARO lo hace en 10, es un win inmediato.
- ¿Puede ver el inventario sin esperar al contador? La visibilidad en tiempo real del stock le evita el problema de quedarse sin un ingrediente en plena hora de servicio.
- ¿Las alertas de stock son inteligentes o son spam? Si el sistema le manda una alerta por cada producto, la ignora. Si le manda solo las alertas críticas, las atiende.
- ¿Puede ver qué está pasando en cocina sin ir a cocina? El KDS en tiempo real le da visibilidad del estado de las órdenes desde cualquier pantalla.

**Qué lo mata:**

- Un sistema que requiera pasos extra para tareas que antes hacía en 2 clics.
- Que los reportes sean difíciles de entender — él no es contador.
- Que no pueda usar el sistema desde su celular cuando no está en el local.
- Que la integración entre módulos no sea transparente — que tenga que "sincronizar" manualmente el POS con el inventario.

**Pains específicos que YARO resuelve para este perfil:**

- **El arqueo de caja manual.** Hoy suma en la calculadora o en Excel los billetes por denominación y los compara con el registro del POS. YARO automatiza ese cálculo — el admin solo ingresa los totales por método de pago y el sistema detecta el descuadre.
- **El control de mermas.** Sin YARO, la merma no se registra — simplemente "desaparece" del inventario. Con YARO, el jefe de cocina registra cada merma con causa y cantidad, y el admin ve el impacto en el food cost en tiempo real.
- **La comunicación con el CDP.** Hoy la solicitud de traslado de insumos va por WhatsApp. Con YARO, la solicitud queda registrada con estado — Solicitado → Aprobado → En camino → Recibido — y el inventario se actualiza automáticamente al confirmar la recepción.

---

### Perfil 3 — El Contador externo (veto técnico fiscal)

**Quién es:**

Un contador público independiente o de una firma pequeña que atiende entre 10 y 50 clientes, varios de ellos en el sector restaurantero. Tiene entre 30 y 55 años. Conoce la normativa DIAN en detalle. No es el que decide si el restaurante adopta YARO, pero sí puede vetar la adopción si el sistema no le genera confianza desde el punto de vista fiscal.

Este perfil es estratégico para YARO por una razón adicional: si YARO le facilita el trabajo al contador, el contador recomienda YARO a todos sus clientes del sector restaurantero. El contador puede convertirse en un canal de distribución indirecto.

**Qué evalúa antes de aprobar YARO:**

- ¿Los documentos DIAN que genera YARO son correctamente estructurados? El XML UBL 2.1, el CUFE, la firma XAdES-BES — todo tiene que estar correcto. Un documento mal generado puede generar contingencias tributarias para su cliente.
- ¿El impoconsumo del 8% está separado del IVA en las cuentas PUC? Es uno de los errores más frecuentes en el sector. Si YARO los mezcla, el contador rechaza el sistema.
- ¿Las propinas están correctamente excluidas de los reportes fiscales? Las propinas no son ingreso del establecimiento. Si aparecen en el P&G, afectan la declaración de renta.
- ¿El sistema soporta el régimen tributario de su cliente? Si el restaurante está en Régimen Simple, el módulo contable tiene que comportarse diferente al Régimen Ordinario.
- ¿Puede acceder a los reportes sin necesitar al dueño? Lo ideal para el contador es que YARO le dé acceso de solo lectura a los módulos contables del cliente, sin pasar por intermediarios.

**Qué lo mata:**

- Cualquier documento DIAN que no cumpla el anexo técnico vigente.
- Un sistema que mezcle impoconsumo con IVA en el PUC.
- Que no soporte el régimen tributario de su cliente específico.
- Que no pueda exportar los datos en un formato compatible con su software contable.

**Por qué este perfil es estratégico:**

Un contador que tiene 15 clientes en el sector restaurantero y recomienda YARO es equivalente a 15 demostraciones de ventas que YARO no tiene que hacer. El contador ya tiene la confianza del dueño del restaurante en temas fiscales — si él dice "usa YARO", el dueño lo usa.

La estrategia concreta: en el módulo de buzón DIAN, cuando YARO clasifica automáticamente una factura de proveedor con la cuenta PUC correcta, el contador ve ese trabajo hecho. Si la clasificación es correcta el 80% de las veces, el contador deja de hacer ese trabajo manual y empieza a recomendar YARO a sus demás clientes.

---

## Mapa de interacción entre los tres perfiles

```
                    DECISIÓN DE COMPRA
                          │
              ┌───────────▼───────────┐
              │   DUEÑO / SOCIO       │  ← Paga la suscripción
              │   (decisor final)     │     Necesita ver ROI
              └───────────┬───────────┘     en < 30 días
                          │
              ┌───────────▼───────────┐
              │  ADMINISTRADOR        │  ← Usa YARO 8h/día
              │  (usuario principal)  │     Puede hundir o
              └───────────┬───────────┘     defender la adopción
                          │
              ┌───────────▼───────────┐
              │  CONTADOR EXTERNO     │  ← Veto técnico fiscal
              │  (validador fiscal)   │     Potencial canal
              └───────────────────────┘     de distribución
```

**Regla de adopción:** Los tres deben estar alineados. Si el dueño quiere pero el admin no entiende el sistema, la adopción falla en la primera semana. Si el dueño y el admin están alineados pero el contador desconfía del módulo DIAN, el dueño lo desinstala al primer informe contable problemático.

---

## Pains compartidos entre los tres perfiles

Aunque cada perfil tiene sus pains específicos, hay tres dolores que los tres sienten de formas diferentes:

**Pain 1 — La DIAN como fuente de ansiedad**
- El dueño le teme a la sanción.
- El administrador no sabe si está facturando correctamente.
- El contador dedica horas a verificar que los documentos sean correctos.

YARO los resuelve a los tres con el mismo mecanismo: transmisión automática, CUFE de confirmación visible, panel de estado en tiempo real.

**Pain 2 — La información financiera llegando tarde**
- El dueño no sabe si ganó o perdió hasta que el contador le entrega el informe.
- El administrador no puede tomar decisiones de compra sin datos de inventario actualizados.
- El contador tiene que esperar a que el dueño le mande los "papeles" del mes.

YARO los resuelve con datos en tiempo real compartidos: el dueño ve el P&G del día, el administrador ve el inventario en tiempo real, el contador accede al módulo contable directamente.

**Pain 3 — La comunicación entre sistemas como fuente de errores**
- El dueño no entiende por qué Siigo y el POS dan números diferentes.
- El administrador tiene que digitar la misma información en dos sistemas.
- El contador reconcilia manualmente los datos del POS con los del sistema contable.

YARO los resuelve eliminando la duplicación: una sola base de datos, un solo flujo de datos, sin doble digitación.

---

## Triggers de compra por perfil

| Evento | Dueño | Administrador | Contador |
|---|---|---|---|
| Notificación de incumplimiento DIAN | ✅ Urgencia máxima | — | ✅ Urgencia máxima |
| Mes con food cost inexplicablemente alto | ✅ Urgencia alta | ✅ Urgencia alta | — |
| Apertura de segunda sede | ✅ Urgencia alta | ✅ Urgencia media | — |
| Primer descuadre de caja grave | ✅ Urgencia media | ✅ Urgencia alta | — |
| Reforma laboral y cambio en recargos | ✅ Urgencia media | — | ✅ Urgencia alta |
| Recomendación del contador | ✅ Urgencia alta | — | — |

---

## Implicaciones para el desarrollo de producto

Esta sección existe específicamente para ingenieros. Cada decisión de perfil tiene una consecuencia directa en el código.

**Del Perfil 1 (Dueño):** La pantalla de dashboard debe mostrar el P&G del día en lenguaje no contable. Nada de "Cuenta 4135 — Ingresos por ventas". El dueño necesita ver "Ventas de hoy: $2.4M" y "Food cost estimado: 31%". La complejidad contable vive en la capa de datos — la capa de presentación para este perfil es deliberadamente simple.

**Del Perfil 2 (Administrador):** El arqueo de caja no puede tener más de 3 pasos. El cierre de turno no puede tomar más de 5 minutos. Las alertas de inventario deben ser accionables — no solo decir "stock bajo" sino mostrar el botón "Solicitar al CDP" en la misma pantalla. Si el administrador tiene que navegar a otra sección para tomar acción, la alerta pierde el 70% de su valor.

**Del Perfil 3 (Contador):** El impoconsumo del 8% nunca puede aparecer mezclado con el IVA en ningún reporte. Los XMLs DIAN deben estar disponibles para descarga directa desde el módulo contable. El sistema debe soportar Régimen Ordinario, SIMPLE y Persona Natural no obligada — sin excepción. Si falta alguno de estos regímenes, el contador no lo aprueba para sus clientes en ese régimen.

---

*YARO ICP v1.0 · Mayo 2026*
*Para el equipo de ingeniería: cuando tengas duda sobre si una feature es prioritaria, pregúntate cuál de los tres perfiles la necesita y qué consecuencia tiene si no está. Eso ordena el backlog mejor que cualquier framework de priorización.*
