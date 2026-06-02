---
version: alpha
name: Hardcore AI 30X
description: Sistema visual para el programa Hardcore AI, sus cohortes, estaciones, slides, guías, repos de clase y superficies web asociadas.
colors:
  primary: "#C8E600"
  on-primary: "#1A1A1A"
  background: "#0A0A0A"
  surface: "#1A1A1A"
  surface-raised: "#262626"
  separator: "#2A2A2A"
  text: "#FFFFFF"
  text-body: "#E6E6E6"
  text-muted: "#808080"
  text-faint: "#4D4D4D"
  keypoint-bg: "#1F2A05"
  negative: "#FF4444"
  negative-dim: "#CC3333"
  demo: "#FF8C00"
  demo-bg: "#3A1F00"
typography:
  display-xl:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 72px
    fontWeight: 700
    lineHeight: 0.98
    letterSpacing: 0em
  display-lg:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 60px
    fontWeight: 700
    lineHeight: 1.02
    letterSpacing: 0em
  title-lg:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 42px
    fontWeight: 700
    lineHeight: 1.12
    letterSpacing: 0em
  subtitle:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 24px
    fontWeight: 400
    lineHeight: 1.3
    letterSpacing: 0em
  body:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 21px
    fontWeight: 400
    lineHeight: 1.35
    letterSpacing: 0em
  body-sm:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 18px
    fontWeight: 400
    lineHeight: 1.4
    letterSpacing: 0em
  label-caps:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 15px
    fontWeight: 700
    lineHeight: 1.15
    letterSpacing: 0.06em
  footer:
    fontFamily: Calibri, Aptos, Arial, sans-serif
    fontSize: 12px
    fontWeight: 400
    lineHeight: 1.2
    letterSpacing: 0em
rounded:
  none: 0px
  sm: 4px
  md: 8px
  lg: 12px
spacing:
  xs: 4px
  sm: 8px
  md: 16px
  lg: 24px
  xl: 40px
  xxl: 64px
components:
  slide-cover:
    backgroundColor: "{colors.background}"
    textColor: "{colors.text}"
    typography: "{typography.display-xl}"
    rounded: "{rounded.none}"
    padding: 64px
  slide-content:
    backgroundColor: "{colors.background}"
    textColor: "{colors.text-body}"
    typography: "{typography.body}"
    rounded: "{rounded.none}"
    padding: 40px
  eyebrow-label:
    backgroundColor: "{colors.background}"
    textColor: "{colors.primary}"
    typography: "{typography.label-caps}"
    rounded: "{rounded.none}"
    padding: 0px
  key-point:
    backgroundColor: "{colors.keypoint-bg}"
    textColor: "{colors.text}"
    typography: "{typography.body-sm}"
    rounded: "{rounded.sm}"
    padding: 16px
  table-header:
    backgroundColor: "{colors.primary}"
    textColor: "{colors.on-primary}"
    typography: "{typography.body-sm}"
    rounded: "{rounded.none}"
    padding: 12px
  negative-panel:
    backgroundColor: "{colors.surface}"
    textColor: "{colors.negative}"
    typography: "{typography.title-lg}"
    rounded: "{rounded.sm}"
    padding: 24px
  demo-band:
    backgroundColor: "{colors.demo-bg}"
    textColor: "{colors.text}"
    typography: "{typography.body-sm}"
    rounded: "{rounded.none}"
    padding: 12px
  footer:
    backgroundColor: "{colors.background}"
    textColor: "{colors.text-faint}"
    typography: "{typography.footer}"
    rounded: "{rounded.none}"
    padding: 0px
---

# DESIGN.md

## Overview

Hardcore AI usa una identidad oscura, cinematográfica y técnica. La experiencia debe sentirse como entrenamiento de alto rendimiento para arquitectos y desarrolladores senior: poca decoración, mucha tensión visual, evidencia clara y una atmósfera que anuncia que el contenido es exigente.

El slide es un escenario. La fotografía carga la atmósfera, el lima eléctrico marca lo importante y el texto claro lleva el mensaje. En web, guías y repos, la misma lógica se traduce a superficies densas pero legibles: jerarquía fuerte, progreso visible, artefactos escaneables y cero ornamentación genérica.

Este archivo documenta la memoria visual para agentes y humanos. Los tokens del front matter son normativos para implementación. Las secciones Markdown explican cómo aplicarlos con criterio.

## Colors

La estrategia de color es oscura y comprometida. El fondo base es casi negro, las superficies se levantan en grises profundos y el lima `primary` funciona como señal de atención. El lima no es un color decorativo; es un instrumento de navegación y énfasis.

Roles principales:

- `background`: fondo base para slides, separadores y superficies inmersivas.
- `surface` y `surface-raised`: cajas, tablas, paneles y estados hover sutiles.
- `primary`: lima eléctrico para eyebrows, marcadores, números, headers de tabla, key points, citas y elementos de máximo énfasis.
- `on-primary`: texto oscuro sobre fondos lima.
- `text`, `text-body`, `text-muted`, `text-faint`: escala de lectura para título, cuerpo, metadatos y footer.
- `negative`: rojo reservado para errores, anti-patrones y comparativas donde algo falla.
- `demo` y `demo-bg`: naranja reservado para bandas de demo en vivo.

Regla de proporción: el lima no debe superar aproximadamente 15 por ciento del área visible en slides o pantallas densas. Si todo está en lima, nada está marcado.

No usar rojo para énfasis general. El rojo comunica contraste negativo: fallas, riesgos, anti-patrones o el lado "mueren" de una comparativa.

## Typography

Calibri es la fuente normativa para slides y documentos Office porque está disponible en el entorno de presentación del programa. Para web, usar la pila `Calibri, Aptos, Arial, sans-serif` para mantener una voz cercana cuando Calibri no exista.

La jerarquía debe ser evidente:

- Portadas: `display-xl`, títulos de 1 a 3 líneas, ubicados en zona segura sobre fotografía oscura.
- Separadores: `display-lg`, composición centrada o monumental, con pregunta o tagline breve.
- Slides de contenido: `title-lg` para el concepto, `subtitle` para contexto breve, `body` para bullets.
- Eyebrows: `label-caps`, siempre en lima, mayúsculas y con tracking controlado.
- Footer: `footer`, bajo contraste, siempre discreto.

No usar subrayado. Usar negrita solo para el concepto inicial de un bullet o para etiquetas de tabla. Evitar párrafos largos en slides; si el texto necesita respirar, dividir en dos slides.

## Layout

La regla base es un concepto por slide o pantalla. La audiencia es senior y escanea rápido. El diseño debe ayudarle a captar la estructura en tres segundos y luego profundizar si lo necesita.

Patrones principales:

- `cover`: fotografía inmersiva full bleed, overlay oscuro, título grande abajo a la izquierda, eyebrow de estación y cohorte.
- `section`: fotografía full bleed con composición monumental, título centrado y pregunta provocadora.
- `content`: fondo oscuro plano, barra lima superior, eyebrow, título, cuerpo y footer. Puede tener imagen lateral si refuerza el concepto.
- `diagram`: el diagrama es protagonista. No mezclar diagramas con bullets en el mismo slide.
- `demo-band`: banda inferior naranja solo cuando el siguiente paso es una demo en vivo.
- `task`: subsecciones claras, checklist de entregables y caja de recursos.

Los repos de estación deben reflejar la misma estructura mental: guía primero, inputs separados de outputs, ejemplos marcados como ejemplos y tareas con checklist.

## Elevation & Depth

La profundidad viene de tres recursos: fotografía inmersiva, contraste entre fondo y superficie, y jerarquía tipográfica. Evitar sombras decorativas en slides oscuros. Si una caja necesita separarse, usar `surface`, `surface-raised`, bordes sutiles con `separator` o un fondo de key point.

En web, usar elevación mínima. Las superficies deben sentirse técnicas y sobrias, no como tarjetas flotantes de SaaS genérico. Preferir separación por layout, tabla, ritmo y contraste antes que stacks de cards.

## Shapes

Los radios son pequeños. `sm` y `md` cubren la mayoría de cajas, tablas, botones y key points. `lg` solo se usa cuando una superficie grande necesita suavizarse. No usar pills redondeadas como decoración.

La barra lima superior es una forma de identidad. En slides `content` y `diagram`, debe aparecer como marcador horizontal pequeño en la esquina superior izquierda, antes del eyebrow. No convertirla en borde lateral grueso.

## Components

### Slides

Las portadas y separadores siempre usan imagen. Una portada sin imagen rompe la identidad. La imagen debe tener zonas oscuras para texto, profundidad espacial y, cuando sea posible, dialogar con el lima mediante luces, glow, vegetación o reflejos.

Los slides de contenido llevan footer con `Hardcore AI by 30X` y el número de estación. El número de página va en la esquina inferior derecha. En portadas y separadores, el footer puede reducirse al número si la composición lo pide.

### Tablas

Las tablas usan header lima con texto oscuro. La primera columna puede destacarse en lima cuando es la dimensión conceptual. Las celdas deben tener padding generoso y bordes sutiles.

### Key points

Un key point cierra el slide. Debe ubicarse hacia el final, no competir con el título. Su fondo es `keypoint-bg`, con texto claro y acento lima discreto.

### Comparativas

Para contrastes fuertes, usar dos paneles: negativo en rojo y positivo en lima. El rojo solo tiene sentido cuando el slide comunica un trade-off explícito.

### Diagramas

Los diagramas Mermaid deben usar fondo oscuro o transparente, nodos con fills oscuros, texto claro y strokes semánticos. El lima se reserva para actor principal o concepto de mayor jerarquía.

### Repos y guías

Una guía de estación debe abrir con el nombre de la estación, para qué sirve, qué hay en la carpeta, cómo usarla y cuál es la tarea. Las tareas deben escribirse en lenguaje de participante: "configura", "genera", "adjunta", "guarda", "verifica".

## Do's and Don'ts

Do:

- Usar el lima para orientar, no para decorar.
- Mantener un concepto principal por slide.
- Escribir tareas accionables y verificables.
- Separar teoría, demo y práctica con ritmo claro.
- Usar fotografía en portadas y separadores.
- Hacer que cada estación produzca un artefacto que alimente la siguiente.
- Mantener alto contraste y texto legible durante sesiones largas.
- Reutilizar tokens y patrones antes de inventar nuevos estilos.

Don't:

- No usar gradientes morados, glassmorphism, tarjetas repetidas o hero vacíos.
- No crear portadas ni separadores sin imagen.
- No usar lima en bloques largos de texto.
- No usar rojo salvo para fallas, riesgos o anti-patrones.
- No llenar slides con más de cinco bullets.
- No mezclar diagrama, bullets e imagen lateral si compiten por atención.
- No escribir objetivos académicos abstractos cuando se necesita una tarea.
- No entregar material que dependa de la conversación del instructor para entenderse.

## Fuentes de contexto

Este sistema visual es el artefacto canónico de diseño del programa. Consolida los materiales de C1 y C2, el estándar Google DESIGN.md y los estándares y skills de diseño según el artículo "Fixing Visual AI Slop". La regla práctica es la misma: skills dan mejores hábitos al agente, `PRODUCT.md` da criterio y `DESIGN.md` da memoria visual revisable.
