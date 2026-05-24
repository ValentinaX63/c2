# Componentes: Moléculas

Las moléculas son agrupaciones de componentes de tipo Átomo que resuelven una necesidad de interfaz más compleja y específica. Todos los componentes consumen sus dependencias internas automáticamente (por ejemplo, `YaroKpi` importa y utiliza `YaroDelta` e `YaroBadge` por ti).

---

## 1. YaroCard (`yaro-card`)
Contenedor estructural con bordes definidos, sombra y soporte opcional para encabezados y botones de acción. Es la base de paneles de visualización.

### Ejemplo de Uso
```html
<yaro-card title="Métricas de Inventario" padding="md" shadow="md">
  <p>Cuerpo principal del panel...</p>

  <!-- Acciones superiores (Lado derecho del título) -->
  <ng-container card-actions>
    <yaro-badge intent="green">Activo</yaro-badge>
  </ng-container>

  <!-- Acciones inferiores (Footer con divisor superior) -->
  <ng-container card-footer>
    <yaro-button variant="secondary">Cancelar</yaro-button>
    <yaro-button variant="primary">Guardar</yaro-button>
  </ng-container>
</yaro-card>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `title` | `string` | `''` | Título del encabezado. Si se omite, no se renderiza la cabecera ni el slot `card-actions`. |
| `padding` | `'none' \| 'sm' \| 'md' \| 'lg'` | `'md'` | Espaciado interno del cuerpo (`sm` = 12px, `md` = 20px, `lg` = 24px). |
| `shadow` | `'none' \| 'sm' \| 'md' \| 'lg'` | `'md'` | Nivel de elevación visual por sombras. |

---

## 2. YaroKpi (`yaro-kpi`)
Tarjeta indicadora de métricas de rendimiento (KPI). Integra un badge de estado y un componente de variación `YaroDelta`.

### Ejemplo de Uso
```html
<yaro-kpi
  label="Ventas Hoy"
  value="$1.240.000"
  [delta]="8.3"
  compareLabel="vs ayer"
  badge="Meta ✓"
  badgeIntent="green">
</yaro-kpi>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `label` | `string` | `''` | Nombre de la métrica (e.g. `"Ventas Hoy"`). |
| `value` | `string \| number` | `'—'` | Valor numérico o texto principal formateado. |
| `delta` | `number \| undefined` | `undefined` | Variación numérica. Si se omite, no se dibuja la sección delta. |
| `deltaInverted`| `boolean` | `false` | Invierte el comportamiento del color de la variación (menos es mejor). |
| `compareLabel` | `string` | `'vs. ayer'` | Leyenda al lado del delta (e.g., `"vs. la semana pasada"`). |
| `badge` | `string` | `''` | Texto para la etiqueta de estado superior derecha. |
| `badgeIntent` | `BadgeIntent` | `'neutral'` | Color semántico de la etiqueta superior derecha (mismos valores que `YaroBadge`). |

---

## 3. YaroStatBar (`yaro-stat-bar`)
Barra de porcentaje visual para metas alcanzadas, niveles de ocupación o progreso de descargas.

### Ejemplo de Uso
```html
<yaro-stat-bar
  label="Ocupación de Salón"
  [value]="13"
  [max]="20"
  intent="green">
</yaro-stat-bar>

<!-- Formato de valor numérico en vez de porcentaje -->
<yaro-stat-bar
  label="Descargas"
  [value]="75"
  [max]="100"
  [showPercent]="false"
  intent="accent">
</yaro-stat-bar>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `label` | `string` | `''` | Título que describe la métrica. |
| `value` | `number` | `0` | Valor numérico actual de progreso (acepta signals). |
| `max` | `number` | `100` | Límite máximo para calcular el porcentaje. |
| `intent` | `'accent' \| 'green' \| 'amber' \| 'red' \| 'blue' \| 'purple'` | `'accent'` | Estilo de color de la barra de progreso. |
| `showPercent` | `boolean` | `true` | Si es `true`, muestra el porcentaje. Si es `false`, muestra la relación `valor / max`. |

---

## 4. YaroStockBar (`yaro-stock-bar`)
Barra de nivel de stock inteligente. Cambia automáticamente de color (Verde/Ámbar/Rojo) de acuerdo con los porcentajes de stock actuales y los límites de umbrales configurados.

### Ejemplo de Uso
```html
<!-- Nivel óptimo (>30%): Barra verde -->
<yaro-stock-bar
  label="Arroz Diana 500g"
  [current]="80"
  [max]="100">
</yaro-stock-bar>

<!-- Stock crítico (<=15%): Barra roja -->
<yaro-stock-bar
  label="Aceite Girasol"
  [current]="8"
  [max]="100"
  [criticalThreshold]="15">
</yaro-stock-bar>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `label` | `string` | `''` | Nombre del producto. |
| `current` | `number` | `0` | Cantidad física en inventario (acepta signals). |
| `max` | `number` | `100` | Límite máximo del stock recomendado. |
| `lowThreshold`| `number` | `30` | Límite porcentual para considerar el stock como 'bajo' (ámbar). |
| `criticalThreshold`| `number` | `10` | Límite porcentual para considerar el stock como 'crítico' (rojo). |

---

## 5. YaroNavItem (`yaro-nav-item`)
Elemento individual de navegación estructurado con icono, etiqueta de texto y badge indicador de notificaciones.

### Ejemplo de Uso
```html
<yaro-nav-item
  label="Bandeja de Entrada"
  icon="✉️"
  [active]="true"
  badge="4"
  badgeIntent="red"
  (itemClick)="irBandeja()">
</yaro-nav-item>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `label` | `string` | `''` | Texto del botón de navegación. |
| `@Input` | `icon` | `string` | `''` | Emoji o carácter de icono a mostrar. |
| `@Input` | `active` | `boolean` | `false` | Aplica estilos de item seleccionado. |
| `@Input` | `badge` | `string \| number` | `''` | Leyenda de notificación. Si está vacío no se dibuja. |
| `@Input` | `badgeIntent`| `BadgeIntent` | `'accent'` | Estilo de color del badge de notificación. |
| `@Output`| `itemClick` | `EventEmitter<void>`| — | Emite al hacer clic sobre el item. |

---

## 6. YaroFormRow (`yaro-form-row`)
Fila estructurada para formularios. Permite crear layouts en rejilla (grid) de 1, 2 o 3 columnas que colapsan automáticamente a 1 columna en pantallas móviles.

### Ejemplo de Uso
```html
<yaro-form-row label="Información Personal" [cols]="2" [required]="true" hint="Campos obligatorios">
  <yaro-input label="Nombre" placeholder="Ana"></yaro-input>
  <yaro-input label="Apellido" placeholder="López"></yaro-input>
</yaro-form-row>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `label` | `string` | `''` | Encabezado superior de la fila de campos. |
| `hint` | `string` | `''` | Leyenda descriptiva bajo el label. |
| `cols` | `1 \| 2 \| 3` | `1` | Cantidad de columnas responsivas de ancho idéntico. |
| `required` | `boolean` | `false` | Pinta un asterisco rojo (`*`) al lado del label principal. |

---

## 7. YaroInfoBox (`yaro-info-box`)
Caja de advertencia, error o información estructurada con iconos semánticos automáticos y fondo de color correspondiente.

### Ejemplo de Uso
```html
<yaro-info-box intent="amber" title="Conexión Inestable">
  Tu señal de internet está fluctuando. Los datos se sincronizarán al recuperar la conexión.
</yaro-info-box>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `intent` | `'green' \| 'red' \| 'amber' \| 'blue' \| 'purple' \| 'neutral'` | `'neutral'` | Intención de alerta. Asigna el icono (`✓`, `✕`, `⚠`, `ℹ`, `◈`, `·`) automáticamente si no se pasa uno personalizado. |
| `title` | `string` | `''` | Título destacado en negrita. |
| `icon` | `string` | `''` | Reemplaza el icono semántico por defecto con un carácter/emoji personalizado. |

---

## 8. YaroTableCard (`yaro-table-card`)
Card dedicada a representar una mesa en el plano de salón del restaurante. Cambia de color por estados.

### Ejemplo de Uso
```html
<yaro-table-card
  tableNumber="12"
  status="occupied"
  [capacity]="4"
  [guestCount]="3"
  [elapsedMinutes]="45"
  (select)="onSelectMesa()">
</yaro-table-card>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `tableNumber` | `string \| number` | `''` | Identificador visual de la mesa. |
| `@Input` | `status` | `'free' \| 'occupied' \| 'reserved' \| 'billing'` | `'free'` | Estado actual de la mesa (determina colores semánticos). |
| `@Input` | `capacity` | `number` | `4` | Límite máximo de comensales. |
| `@Input` | `guestCount` | `number` | — | Cantidad actual de personas sentadas (opcional). |
| `@Input` | `elapsedMinutes`| `number` | — | Minutos transcurridos desde la apertura (opcional, muestra e.g. "45min" o "1h 15m"). |
| `@Output`| `select` | `EventEmitter<void>` | — | Emite al presionar la tarjeta de la mesa. |

---

## 9. YaroMenuItemCard (`yaro-menu-item-card`)
Tarjeta visual para platos del menú. Muestra la imagen (con fallback automático si falla), descripción, precio y botón de agregar.

### Ejemplo de Uso
```html
<yaro-menu-item-card
  name="Hamburguesa Yaro"
  [price]="28000"
  description="Carne angus, queso cheddar y papas fritas."
  imageUrl="assets/hamburguesa.jpg"
  [available]="true"
  (add)="agregarAlPedido()">
</yaro-menu-item-card>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `name` | `string` | `''` | Nombre del platillo o producto. |
| `@Input` | `price` | `number` | `0` | Precio base numérico. |
| `@Input` | `description`| `string` | `''` | Descripción de ingredientes o notas del menú. |
| `@Input` | `imageUrl` | `string` | `''` | Dirección de la imagen. Oculta el contenedor de imagen si falla su carga. |
| `@Input` | `available` | `boolean` | `true` | Si es `false`, deshabilita el botón de agregar y pinta la card opaca con etiqueta "Agotado". |
| `@Input` | `currency` | `string` | `'$'` | Símbolo de moneda. |
| `@Output`| `add` | `EventEmitter<void>` | — | Emite al presionar el botón `+`. |

---

## 10. YaroOrderLine (`yaro-order-line`)
Línea de pedido con control de cantidad integrado (`YaroQuantityControl`), útil para la barra lateral de comandas o pre-factura.

### Ejemplo de Uso
```html
<yaro-order-line
  name="Hamburguesa Yaro"
  [unitPrice]="28000"
  [quantity]="2"
  notes="Sin cebolla"
  (quantityChange)="actualizarCantidad($event)"
  (remove)="eliminarItem()">
</yaro-order-line>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `name` | `string` | `''` | Nombre del producto. |
| `@Input` | `unitPrice` | `number` | `0` | Precio unitario. |
| `@Input` | `quantity` | `number` | `1` | Cantidad seleccionada actualmente. |
| `@Input` | `notes` | `string` | `''` | Modificaciones o notas especiales del cliente. |
| `@Input` | `currency` | `string` | `'$'` | Símbolo de moneda. |
| `@Output`| `quantityChange`| `EventEmitter<number>`| — | Emite la nueva cantidad numéricas mayor a 0. |
| `@Output`| `remove` | `EventEmitter<void>`| — | Emite cuando la cantidad se reduce a 0, solicitando la remoción del item. |

---

## 11. YaroOrderCard (`yaro-order-card`)
Card compacta de visualización de pedidos en cocina, categorizada por estados mediante etiquetas y con un botón de transición de estado.

### Ejemplo de Uso
```html
<yaro-order-card
  orderId="458"
  tableLabel="3"
  status="preparing"
  [itemCount]="5"
  [total]="124000"
  createdAt="19:35"
  (statusChange)="avanzarPedido($event)">
</yaro-order-card>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `orderId` | `string` | `''` | Identificador único del pedido. |
| `@Input` | `tableLabel` | `string` | `''` | Número o etiqueta de la mesa del pedido. |
| `@Input` | `status` | `'pending' \| 'preparing' \| 'ready' \| 'delivered' \| 'cancelled'` | `'pending'` | Estado del pedido. |
| `@Input` | `itemCount` | `number` | `0` | Total de ítems contenidos en el pedido. |
| `@Input` | `total` | `number` | `0` | Valor monetario total acumulado del pedido. |
| `@Input` | `createdAt` | `string` | `''` | Hora o tiempo de creación del pedido. |
| `@Output`| `select` | `EventEmitter<void>`| — | Emite al hacer clic sobre el cuerpo de la tarjeta. |
| `@Output`| `statusChange`| `EventEmitter<OrderStatus>`| — | Emite el siguiente estado recomendado cuando se presiona el botón de acción (e.g. de "pending" emite "preparing"). |
