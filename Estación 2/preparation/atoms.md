# Componentes: Átomos

Los átomos son las piezas fundamentales de la interfaz. No contienen dependencias de otros componentes de la biblioteca `@yaro/ui`, son altamente reutilizables, independientes del contexto y están optimizados para rendimiento mediante `ChangeDetectionStrategy.OnPush` y `:host { display: contents }`.

---

## 1. YaroButton (`yaro-button`)
Botón básico del sistema. Soporta tres tamaños y tres variantes estéticas. Incluye soporte nativo para estados deshabilitados y de carga con spinner integrado.

### Ejemplo de Uso
```html
<!-- Variantes -->
<yaro-button variant="primary">Guardar</yaro-button>
<yaro-button variant="secondary">Cancelar</yaro-button>
<yaro-button variant="ghost">Eliminar</yaro-button>

<!-- Tamaños -->
<yaro-button size="sm">Pequeño</yaro-button>
<yaro-button size="md">Normal</yaro-button>
<yaro-button size="lg">Grande</yaro-button>

<!-- Estados -->
<yaro-button [disabled]="true">Deshabilitado</yaro-button>
<yaro-button [loading]="cargando">Procesar...</yaro-button>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `variant` | `'primary' \| 'secondary' \| 'ghost'` | `'primary'` | Estilo visual del botón. |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Dimensiones del botón. |
| `type` | `'button' \| 'submit' \| 'reset'` | `'button'` | Tipo HTML nativo del botón. |
| `disabled` | `boolean` | `false` | Deshabilita la interacción visual y de teclado. |
| `loading` | `boolean` | `false` | Muestra un indicador de carga y deshabilita la interacción automáticamente. |

---

## 2. YaroBadge (`yaro-badge`)
Etiqueta semántica de estado o clasificación. Cuenta con siete intenciones de color que se corresponden con los estados semánticos del sistema y un indicador de punto opcional.

### Ejemplo de Uso
```html
<yaro-badge intent="green">Activo</yaro-badge>
<yaro-badge intent="red" [dot]="true">Crítico</yaro-badge>
<yaro-badge intent="neutral" size="sm">Borrador</yaro-badge>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `intent` | `'accent' \| 'green' \| 'red' \| 'amber' \| 'blue' \| 'purple' \| 'neutral'` | `'neutral'` | Color semántico o intención de la etiqueta. |
| `size` | `'sm' \| 'md'` | `'md'` | Altura de la etiqueta. |
| `dot` | `boolean` | `false` | Pinta un círculo de color antes del texto para denotar estado dinámico. |

---

## 3. YaroInput (`yaro-input`)
Campo de entrada de texto estructurado con etiqueta de formulario, texto de ayuda (hint) y alertas de error integradas.

> [!NOTE]
> Este componente implementa `ControlValueAccessor`, por lo que funciona nativamente con `[(ngModel)]` o `[formControl]/formControlName` en Angular.

### Ejemplo de Uso
```html
<!-- Básico con NgModel -->
<yaro-input
  label="Usuario"
  placeholder="Ej: yaro_val"
  [(ngModel)]="usuario">
</yaro-input>

<!-- Con Validaciones (Reactivo) -->
<yaro-input
  label="Precio de Venta"
  type="number"
  hint="Precio antes de IVA"
  [error]="form.get('precio')?.hasError('required') ? 'El campo es obligatorio' : ''"
  formControlName="precio">
</yaro-input>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `label` | `string` | `''` | Etiqueta de texto descriptivo ubicada arriba del campo. |
| `placeholder`| `string` | `''` | Texto de marcador de posición nativo. |
| `type` | `string` | `'text'` | Tipo de campo nativo (e.g. `'text'`, `'number'`, `'password'`). |
| `error` | `string` | `''` | Mensaje de validación errónea. Si tiene valor, el borde cambia a rojo y se oculta el `hint`. |
| `hint` | `string` | `''` | Texto explicativo secundario abajo del campo. |
| `inputId` | `string` | *auto* | ID del input HTML. Se genera un ID aleatorio único por defecto. |

---

## 4. YaroToggle (`yaro-toggle`)
Interruptor binario (On/Off) estructurado y accesible para configuraciones y activaciones de estados.

> [!NOTE]
> Este componente implementa `ControlValueAccessor` y es compatible con directivas de formularios de Angular.

### Ejemplo de Uso
```html
<yaro-toggle
  label="Habilitar notificaciones"
  [(ngModel)]="alertasActivas">
</yaro-toggle>

<yaro-toggle
  label="Turno Activo"
  [value]="isTurnoActive"
  (valueChange)="onTurnoChange($event)">
</yaro-toggle>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `label` | `string` | `''` | Etiqueta de texto descriptivo a la derecha del switch. |
| `@Input` | `value` | `boolean` | `false` | Asigna el estado activo/inactivo de forma manual. |
| `@Output`| `valueChange` | `EventEmitter<boolean>`| — | Emite el nuevo valor booleano cuando cambia el estado del switch. |

---

## 5. YaroAvatar (`yaro-avatar`)
Representación en miniatura de un usuario. Muestra la imagen si `src` carga correctamente; de lo contrario, extrae e inicializa las letras iniciales a partir de `name`.

### Ejemplo de Uso
```html
<!-- Caso con imagen existente -->
<yaro-avatar
  src="https://api.dicebear.com/7.x/thumbs/svg?seed=yaro"
  name="Yaro UI"
  size="md">
</yaro-avatar>

<!-- Caso con fallback a iniciales (YV) -->
<yaro-avatar
  name="Valentina Tangarife"
  size="lg">
</yaro-avatar>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `src` | `string` | `''` | Dirección URL del archivo de imagen. |
| `name` | `string` | `''` | Nombre del usuario. Se genera la inicial de las dos primeras palabras. |
| `size` | `'sm' \| 'md' \| 'lg'` | `'md'` | Dimensiones del círculo del avatar (`sm` = 28px, `md` = 40px, `lg` = 52px). |

---

## 6. YaroDelta (`yaro-delta`)
Muestra la variación porcentual o numérica con flechas indicadoras (`↑` o `↓`) y un color semántico. Útil para tendencias.

### Ejemplo de Uso
```html
<!-- Variación estándar positiva (+15.4%) en verde -->
<yaro-delta [value]="15.4"></yaro-delta>

<!-- Variación negativa (-2.5%) en rojo -->
<yaro-delta [value]="-2.5"></yaro-delta>

<!-- Delta invertido (donde los valores negativos son positivos, ej: reducción de quejas) -->
<yaro-delta [value]="-10" unit=" quejas" [inverted]="true"></yaro-delta>
```

### API — `@Inputs`
| Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- |
| `value` | `number` | `0` | Valor numérico de la diferencia. Si es $\ge 0$ renderiza `↑`, si es $< 0$ renderiza `↓`. |
| `unit` | `string` | `'%'` | Sufijo de unidad a mostrar (e.g. `'%'`, `' uds'`, `' COP'`). |
| `decimals` | `number` | `1` | Cantidad de decimales a formatear. |
| `inverted` | `boolean` | `false` | Invierte el comportamiento del color. Si es `true`, un valor menor o igual a 0 se tiñe de verde (OK) y uno positivo de rojo (Error). |

---

## 7. YaroQuantityControl (`yaro-quantity-control`)
Selector numérico compacto con botones más (`+`) y menos (`−`). Comúnmente usado en comandas de ventas e inventario.

### Ejemplo de Uso
```html
<yaro-quantity-control
  [value]="cantidad"
  [min]="1"
  [max]="10"
  (valueChange)="onCantidadChange($event)">
</yaro-quantity-control>
```

### API — `@Inputs` y `@Outputs`
| Flujo | Propiedad | Tipo | Default | Descripción |
| :--- | :--- | :--- | :--- | :--- |
| `@Input` | `value` | `number` | `1` | Valor numérico seleccionado actualmente. |
| `@Input` | `min` | `number` | `0` | Límite mínimo seleccionable. Deshabilita el botón `-` al alcanzarlo. |
| `@Input` | `max` | `number` | `99` | Límite máximo seleccionable. Deshabilita el botón `+` al alcanzarlo. |
| `@Input` | `disabled`| `boolean` | `false` | Deshabilita por completo la interacción del control. |
| `@Output`| `valueChange` | `EventEmitter<number>`| — | Emite la nueva cantidad al presionar `+` o `-`. |
