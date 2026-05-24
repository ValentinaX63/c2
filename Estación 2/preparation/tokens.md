# Tokens de Diseño — Fuente Única de Verdad

Los tokens de diseño son las variables visuales fundamentales del sistema de diseño YARO. Todos los componentes de `@yaro/ui` se basan en estas variables CSS. **Nunca hardcodear valores hexadecimales, tamaños de fuente o espaciados** directamente en las hojas de estilo del componente.

---

## 1. Colores de Superficie y Texto (Tema Claro)

Representan la jerarquía de fondos de la interfaz. Se estructuran en capas: `--color-bg` es el más profundo, mientras que `--color-white` es el más elevado (cards y modales).

| Variable CSS | Valor por Defecto (Claro) | Uso Principal |
| :--- | :--- | :--- |
| `--color-bg` | `#f0ede8` | Fondo principal de la ventana/body. |
| `--color-bg2` | `#ece9e3` | Fondo de elementos estructurales (topbar, sidenav, toolbar). |
| `--color-surface` | `#faf9f7` | Fondo de contenedores secundarios y formularios. |
| `--color-white` | `#ffffff` | Fondo de cards, modales e inputs de texto. |
| `--color-border` | `rgba(0, 0, 0, 0.07)` | Divisores sutiles y bordes suaves. |
| `--color-border2` | `rgba(0, 0, 0, 0.11)` | Bordes con énfasis y estado hover. |
| `--color-text` | `#1a1916` | Texto principal y títulos destacados. |
| `--color-text2` | `#6b6760` | Texto secundario de apoyo y etiquetas. |
| `--color-text3` | `#a8a49e` | Placeholders y metadatos de baja prioridad. |

---

## 2. Acento YARO

El verde-limón cálido es el color de identidad de YARO. Proporciona distinción visual para estados seleccionados, activaciones y barras de progreso.

| Variable CSS | Valor por Defecto | Uso Principal |
| :--- | :--- | :--- |
| `--color-accent` | `#f7fd9c` | Fondo de elementos activos de navegación y celdas seleccionadas. |
| `--color-accent-dk` | `#d4e200` | Barra de progreso activa, bordes de enfoque principales. |
| `--color-accent-txt` | `#6b7200` | Texto de alto contraste sobre fondo `--color-accent`. |
| `--color-accent-bg` | `rgba(212, 226, 0, 0.10)` | Fondos sutiles y overlays para elementos acentuados. |
| `--color-accent-border` | `rgba(212, 226, 0, 0.28)` | Bordes decorativos sutiles para elementos destacados. |

---

## 3. Colores Semánticos (Estados)

Cada estado semántico posee tres variantes: **base** (iconos/texto), **fondo sutil** (badges, alertas) y **borde** (énfasis de estados).

| Estado | Variable CSS | Valor (Claro) | Uso Principal |
| :--- | :--- | :--- | :--- |
| **Verde** (OK / Activo) | `--color-green`<br>`--color-green-bg`<br>`--color-green-border` | `#3d9970`<br>`rgba(61,153,112,.09)`<br>`rgba(61,153,112,.20)` | Turno activo, transmisión exitosa de factura, stock correcto. |
| **Rojo** (Error / Crítico) | `--color-red`<br>`--color-red-bg`<br>`--color-red-border` | `#c0392b`<br>`rgba(192,57,43,.07)`<br>`rgba(192,57,43,.20)` | Error de facturación, stock crítico/agotado, descuadre de caja. |
| **Ámbar** (Advertencia) | `--color-amber`<br>`--color-amber-bg`<br>`--color-amber-border` | `#b7860b`<br>`rgba(183,134,11,.09)`<br>`rgba(183,134,11,.20)` | Stock bajo, producto próximo a vencer, terminal offline. |
| **Azul** (Información) | `--color-blue`<br>`--color-blue-bg`<br>`--color-blue-border` | `#2471a3`<br>`rgba(36,113,163,.07)`<br>`rgba(36,113,163,.18)` | Traslado de stock en camino, documento transmitiéndose. |
| **Púrpura** (IA / CDP) | `--color-purple`<br>`--color-purple-bg`<br>`--color-purple-border` | `#534ab7`<br>`rgba(83,74,183,.09)`<br>`rgba(83,74,183,.22)` | Lotes de CDP, recomendaciones automáticas, etiquetas especiales. |

---

## 4. Elevación y Sombras

Tres niveles de sombras para simular profundidad en el eje Z. Los niveles altos denotan elementos más cercanos al usuario.

* **Sombra Pequeña (`--shadow-sm`)**:
  `0 1px 3px rgba(0, 0, 0, 0.06), 0 1px 2px rgba(0, 0, 0, 0.04)`
  *Uso: Inputs de texto, topbar, sidenav y pequeños contenedores.*
* **Sombra Mediana (`--shadow-md`)**:
  `0 4px 16px rgba(0, 0, 0, 0.08), 0 1px 4px rgba(0, 0, 0, 0.04)`
  *Uso: El estándar del sistema. Cards de contenido, listados en reposo.*
* **Sombra Grande (`--shadow-lg`)**:
  `0 8px 32px rgba(0, 0, 0, 0.10), 0 2px 8px rgba(0, 0, 0, 0.05)`
  *Uso: Modales, dropdowns flotantes y cards con hover activo.*

---

## 5. Radios de Borde (Redondez)

El aspecto redondeado de YARO suaviza la UI y la hace sentir más moderna y amigable.

* **Grande (`--radius`)** = `14px`: Cards principales, ventanas modales, dropdowns complejos, plano de mesas.
* **Mediano (`--radius-sm`)** = `9px`: Botones, campos de entrada, badges grandes, items de navegación.
* **Pequeño (`--radius-xs`)** = `5px`: Chips compactos, píldoras indicadoras de estado dentro de tablas.

---

## 6. Tipografía

YARO utiliza tipografía de alta legibilidad optimizada para pantallas.

* **Fuente Principal (`--font-sans`)**: `'Outfit', sans-serif` (Para títulos, botones, inputs y cuerpo).
* **Fuente de Precisión (`--font-mono`)**: `'JetBrains Mono', monospace` (Para importes monetarios, cantidades numéricas, códigos de barras y la YARO Console).

### Escala de Tamaños
* `--text-xs` = `10px` (w600): Etiquetas uppercase en headers de sección, metadatos.
* `--text-sm` = `11px` (w400): Leyendas de apoyo, subtítulos y pie de cards.
* `--text-base` = `13px` (w400): Texto de contenido, filas de tabla, descripciones de productos.
* `--text-md` = `14px` (w500): Títulos de columna en tablas, etiquetas de formulario.
* `--text-lg` = `16px` (w700): Totales monetarios, valores de KPI.
* `--text-xl` = `20px` (w700): Números de KPI grandes en dashboard.
* `--text-2xl` = `24px` (w700): Títulos principales de páginas.
* `--text-3xl` = `32px` (w700): Importes financieros de resumen anual.

---

## 7. Espaciado

Escala estricta basada en múltiplos de 4px para asegurar la armonía geométrica. **Quedan prohibidos márgenes o paddings arbitrarios (ej. 15px, 7px)**.

* `--space-1` = `4px` (Gaps de iconos, padding interno de chips)
* `--space-2` = `8px` (Gap en listados de navegación, tags)
* `--space-3` = `12px` (Paddings en badges, gaps en formularios compactos)
* `--space-4` = `16px` (Padding en nav-items, gap estándar entre tarjetas)
* `--space-5` = `20px` (Padding interno estándar de cards)
* `--space-6` = `24px` (Paddings en modales, gap entre secciones de card)
* `--space-8` = `32px` (Padding de toolbar, separación de secciones del dashboard)
* `--space-10` = `40px` (Margen del contenedor principal del shell)
* `--space-12` = `48px` (Separación de secciones principales de pantalla)
* `--space-16` = `64px` (Altura del topbar, márgenes entre módulos macro)

---

## 8. Transiciones y Animaciones

* **Fast (`--transition-fast`)** = `0.12s ease`: Hover de botones, cambios sutiles de color de borde.
* **Base (`--transition-base`)** = `0.20s ease`: Animación de switches toggle, transiciones de tabs, indicadores de navegación activa.
* **Spring (`--transition-spring`)** = `0.40s cubic-bezier(0.22, 1, 0.36, 1)`: **La curva característica de YARO**. Genera un rebote elegante en la aparición de modales, paneles laterales y hover de cards.

---

## 9. Tema Oscuro — YARO Console

El tema oscuro del sistema se aplica de manera automática en cascada agregando el atributo `[data-theme="console"]` en el elemento raíz (`<html>` o `<body>`). Esto sobrescribe los valores de los tokens de color:

```css
[data-theme="console"] {
  --color-bg:       #0a0a0b;
  --color-bg2:      #111114;
  --color-surface:  #18181c;
  --color-white:    #222228;

  --color-border:   rgba(255, 255, 255, 0.06);
  --color-border2:  rgba(255, 255, 255, 0.10);

  --color-text:     #e8e8ec;
  --color-text2:    #9898a8;
  --color-text3:    #5a5a6a;

  --color-accent:         #7c6af7; /* Acento cambia a púrpura Console */
  --color-accent-dk:      #a08cf8;
  --color-accent-txt:     #e8e4ff;
  --color-accent-bg:      rgba(124, 106, 247, 0.12);
  --color-accent-border:  rgba(124, 106, 247, 0.30);

  --color-green:          #22c55e;
  --color-red:            #ef4444;
  --color-amber:          #f59e0b;
  --color-blue:           #38bdf8;
  --color-purple:         #a78bfa;
}
```
