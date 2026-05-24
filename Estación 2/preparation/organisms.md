# Componentes: Organismos

Los organismos son las entidades de mayor escala en el sistema de diseño YARO. Consisten en secciones complejas y autocontenidas de la UI que coordinan múltiples átomos y moléculas, manejando estados internos complejos o sirviendo como envoltorios de layout globales.

---

## 1. YaroModal (`yaro-modal`)
Diálogo flotante superpuesto (modal overlay) para acciones críticas o flujos secundarios. Controlado de forma reactiva en el DOM.

### Ejemplo de Uso
```html
<yaro-modal
  [open]="showModal"
  title="Confirmar Cierre de Caja"
  size="md"
  (closed)="showModal = false">
  
  <p>¿Estás segura de realizar el cierre de caja? Todos los turnos actuales se guardarán e imprimirán.</p>

  <ng-container modal-actions>
    <yaro-button variant="secondary" (click)="showModal = false">Cancelar</yaro-button>
    <yaro-button variant="primary" (click)="procesarCierre()">Cerrar Caja</yaro-button>
  </ng-container>
</yaro-modal>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `open` | `boolean` | `false` | Determina si el diálogo está abierto. Utiliza `@if` internamente, por lo que si es `false` no se renderiza en el DOM. |
| `@Input` | `title` | `string` | `''` | Título del encabezado del modal. |
| `@Input` | `size` | `'sm' \| 'md' \| 'lg' \| 'full'` | `'md'` | Ancho máximo (`sm`=400px, `md`=560px, `lg`=800px, `full`=100vw). |
| `@Input` | `closable` | `boolean` | `true` | Muestra el botón de cierre (✕) en la cabecera del modal. |
| `@Input` | `closeOnBackdrop`| `boolean` | `true` | Permite cerrar el modal haciendo clic sobre el overlay de fondo. |
| `@Output`| `closed` | `EventEmitter<void>`| — | Emite al cerrar el modal (por clic en ✕, overlay o presionar tecla `Escape`). |

---

## 2. YaroDataTable (`yaro-data-table`)
Tabla de datos responsiva de alto rendimiento. Soporta ordenamiento por columnas del lado del cliente, paginación, skeletons dinámicos de carga y celdas semánticas (badges y deltas).

### Ejemplo de Uso
```typescript
// En el controlador TS
import { TableColumn } from '@yaro/ui';

columnas: TableColumn[] = [
  { key: 'name', header: 'Producto', type: 'text' },
  { key: 'stock', header: 'Stock', type: 'number', sortable: true, align: 'right' },
  { key: 'variacion', header: 'Variación semanal', type: 'delta' },
  { 
    key: 'status', 
    header: 'Estado', 
    type: 'badge',
    intentFn: (row) => row['stock'] > 10 ? 'green' : 'red'
  }
];
```

```html
<!-- En el template HTML -->
<yaro-data-table
  [columns]="columnas"
  [rows]="listaProductos"
  [loading]="cargando"
  [pageSize]="10"
  [page]="paginaActual"
  [rowClickable]="true"
  (rowClick)="abrirDetalle($event)"
  (pageChange)="paginaActual = $event">
</yaro-data-table>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `columns` | `TableColumn[]` | `[]` | Definición del esquema de columnas de la tabla. |
| `@Input` | `rows` | `Record<string, unknown>[]` | `[]` | Arreglo de objetos JSON a renderizar en las filas. |
| `@Input` | `loading` | `boolean` | `false` | Muestra una animación de carga tipo skeleton. |
| `@Input` | `pageSize` | `number` | `0` | Filas por página. Si es `0`, deshabilita la paginación de la tabla. |
| `@Input` | `page` | `number` | `1` | Página actual del selector (1-indexed). |
| `@Input` | `rowClickable`| `boolean` | `false` | Habilita hover interactivo y cursor tipo pointer sobre las filas. |
| `@Input` | `skeletonCount`| `number` | `5` | Número de renglones skeleton mostrados si `loading` es `true`. |
| `@Input` | `emptyMessage`| `string` | `'Sin datos…'`| Texto mostrado si el arreglo de filas está vacío. |
| `@Output`| `rowClick` | `EventEmitter<Record>` | — | Emite la información del renglón al hacer clic sobre él. |
| `@Output`| `pageChange` | `EventEmitter<number>` | — | Emite el número de la nueva página seleccionada en el footer. |

---

## 3. YaroSidenav (`yaro-sidenav`)
Barra de navegación lateral estructurada y responsiva. En computadoras de escritorio, soporta expandir y colapsar a modo icono. En dispositivos móviles (<768px), se renderiza automáticamente como un cajón deslizante con fondo oscuro.

### Ejemplo de Uso
```typescript
import { SidenavSection } from '@yaro/ui';

secciones: SidenavSection[] = [
  {
    label: 'Principal',
    items: [
      { id: 'home', label: 'Dashboard', icon: '🏠' },
      { id: 'ventas', label: 'Ventas', icon: '💰', badge: '12' }
    ]
  }
];
```

```html
<yaro-sidenav
  [sections]="secciones"
  [collapsed]="isCollapsed"
  [mobileOpen]="isMobileOpen"
  activeId="home"
  (itemSelect)="onNavigate($event)"
  (toggleCollapse)="isCollapsed = !isCollapsed"
  (mobileClose)="isMobileOpen = false">
</yaro-sidenav>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `sections` | `SidenavSection[]` | `[]` | Listado jerárquico de secciones e ítems de navegación. |
| `@Input` | `collapsed` | `boolean` | `false` | Modo compacto (56px) para desktop. No afecta visualización móvil. |
| `@Input` | `mobileOpen`| `boolean` | `false` | En móvil, comanda la aparición tipo Drawer. |
| `@Input` | `activeId` | `string` | `''` | Identificador único (`id`) del ítem actualmente activo. |
| `@Output`| `itemSelect` | `EventEmitter<string>`| — | Emite el `id` al hacer clic sobre un enlace de navegación. |
| `@Output`| `mobileClose`| `EventEmitter<void>` | — | Emite cuando el usuario toca fuera del Drawer móvil para cerrarlo. |
| `@Output`| `toggleCollapse`| `EventEmitter<void>`| — | Emite cuando se presiona el gatillo de contraer/expandir en escritorio. |

---

## 4. YaroTopbar (`yaro-topbar`)
Barra de navegación superior fija (`z-index: 200`). Detecta automáticamente la relación de pantalla y dibuja un botón hamburguesa en dispositivos móviles para sincronizar con la apertura del menú lateral.

### Ejemplo de Uso
```html
<yaro-topbar
  title="Inventario"
  subtitle="Bodega Principal"
  (menuToggle)="isMobileOpen = true">

  <!-- Slot Central (Se oculta en móvil) -->
  <ng-container topbar-center>
    <input class="search-bar" placeholder="Buscar producto...">
  </ng-container>

  <!-- Acciones del lado derecho (Siempre visibles) -->
  <ng-container topbar-right>
    <yaro-connection-badge status="online"></yaro-connection-badge>
    <yaro-avatar name="Yaro Dev" size="sm"></yaro-avatar>
  </ng-container>
</yaro-topbar>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `title` | `string` | `''` | Título de la sección actual de la app. |
| `@Input` | `subtitle` | `string` | `''` | Contexto de texto secundario bajo el título. |
| `@Output`| `menuToggle` | `EventEmitter<void>` | — | Emite en móvil al hacer tap sobre el botón hamburguesa (☰). |

---

## 5. YaroBienestarPicker (`yaro-bienestar-picker`)
Selector visual de opción única para registrar estados de ánimo, niveles de fatiga o conformidad del personal en los cierres de turno.

> [!NOTE]
> Este componente implementa `ControlValueAccessor` y es compatible con directivas de formularios de Angular.

### Ejemplo de Uso
```html
<yaro-bienestar-picker
  label="¿Cómo te sentiste en tu turno hoy?"
  [(ngModel)]="bienestarTurno">
</yaro-bienestar-picker>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `label` | `string` | `''` | Texto de pregunta o instrucción. |
| `@Input` | `options` | `BienestarOption[]` | *Ver por defecto*| Opciones del picker. Por defecto carga: Excelente (😊), Bien (🙂), Regular (😐), Cansado (😴) y Mal (😣). |
| `@Output`| `selectedChange`| `EventEmitter<string>`| — | Emite la clave identificadora (`id`) al pulsar sobre una opción. |

---

## 6. YaroConnectionBadge (`yaro-connection-badge`)
Indicador del estado de conexión de la aplicación o servicios externos (e.g. sincronización de facturación ante la DIAN) con animaciones CSS integradas en el punto indicador.

### Ejemplo de Uso
```html
<!-- Badge en línea estándar -->
<yaro-connection-badge status="online"></yaro-connection-badge>

<!-- Mapeo con servicio específico -->
<yaro-connection-badge
  service="DIAN"
  status="connecting">
</yaro-connection-badge>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `status` | `'online' \| 'offline' \| 'connecting' \| 'error'` | `'online'` | Estado de conexión (controla color del punto e icono de animación). |
| `service` | `string` | `''` | Prefijo de etiqueta del servicio (e.g., `"Servidor"`, `"DIAN"`). |
| `showLabel` | `boolean` | `true` | Muestra el texto explicativo al lado del punto indicador. |

---

## 7. YaroCategoryNav (`yaro-category-nav`)
Selector horizontal deslizable en el eje X, utilizado para cambiar rápidamente entre categorías del menú en el POS.

### Ejemplo de Uso
```html
<yaro-category-nav
  [categories]="categoriasPlatos"
  activeId="bebidas"
  (select)="onCategoriaChange($event)">
</yaro-category-nav>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `categories` | `MenuCategory[]` | `[]` | Arreglo de categorías con `id`, `label`, `icon` (opcional) y `count` (contador opcional). |
| `@Input` | `activeId` | `string` | `''` | ID de la categoría activa actualmente. |
| `@Output`| `select` | `EventEmitter<string>`| — | Emite el `id` de la categoría pulsada. |

---

## 8. YaroFloorMap (`yaro-floor-map`)
Rejilla (grid) de distribución del plano del restaurante para visualizar y administrar la ocupación de mesas.

### Ejemplo de Uso
```html
<yaro-floor-map
  [tables]="mesasSalon"
  [columns]="5"
  (tableSelect)="onMesaClick($event)">
</yaro-floor-map>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `tables` | `FloorTable[]` | `[]` | Arreglo de mesas con `id`, `number`, `status`, `capacity`, `guestCount` y `elapsedMinutes`. |
| `@Input` | `columns` | `number` | `4` | Cantidad de columnas que se configurarán en la cuadrícula CSS. |
| `@Output`| `tableSelect` | `EventEmitter<FloorTable>`| — | Emite los detalles de la mesa cuando es seleccionada. |

---

## 9. YaroOrderPanel (`yaro-order-panel`)
Barra lateral deslizable derecha (slide-over drawer) que actúa como el carro de compras o comanda activa en la terminal POS.

### Ejemplo de Uso
```html
<yaro-order-panel
  [open]="isPanelOpen"
  tableLabel="5"
  [items]="itemsComanda"
  (close)="isPanelOpen = false"
  (itemChange)="onQtyChange($event)"
  (itemRemove)="onRemoveItem($event)"
  (sendToKitchen)="procesarComanda($event)">
</yaro-order-panel>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `open` | `boolean` | `false` | Abre o cierra el slide-over lateral con animación de entrada y salida. |
| `@Input` | `tableLabel` | `string` | `''` | Nombre o número de la mesa relacionada al pedido. |
| `@Input` | `items` | `OrderItem[]` | `[]` | Listado de productos agregados a la comanda activa. |
| `@Input` | `currency` | `string` | `'$'` | Símbolo de moneda de facturación. |
| `@Output`| `close` | `EventEmitter<void>`| — | Emite al presionar la X de cierre o el backdrop opaco. |
| `@Output`| `itemChange` | `EventEmitter<{id, qty}>` | — | Emite cambios de cantidad de algún ítem. |
| `@Output`| `itemRemove` | `EventEmitter<string>`| — | Emite el ID de un ítem para retirarlo del pedido. |
| `@Output`| `sendToKitchen`| `EventEmitter<OrderItem[]>`| — | Emite el arreglo de ítems del pedido al presionar el botón "Enviar a cocina". |
