# Documentación Técnica — Sistema de Diseño YARO

Bienvenida a la documentación del sistema de diseño **YARO**. Esta carpeta contiene la guía completa sobre los principios visuales y la biblioteca de componentes `@yaro/ui`.

El sistema de diseño está organizado bajo la metodología de **Atomic Design** (Diseño Atómico) para asegurar la reutilización, modularidad y mantenibilidad del código:

```
[Tokens de Diseño] ──> [Átomos] ──> [Moléculas] ──> [Organismos]
```

## Índice de Contenidos

### 1. [Fundamentos y Tokens de Diseño](tokens.md)
* Guía de colores: paleta de superficies, color de identidad (acento) y estados (verde, rojo, ámbar, azul, púrpura).
* Elevación visual a través de sombras (`--shadow-sm`, `--shadow-md`, `--shadow-lg`).
* Redondez y radios de borde (`--radius`, `--radius-sm`, `--radius-xs`).
* Escala tipográfica utilizando **Outfit** (principal) y **JetBrains Mono** (numérico y código).
* Escala de espaciado estricto basada en múltiplos de 4px (`--space-1` a `--space-16`).
* Transiciones y animaciones, incluyendo la curva *spring* característica de YARO.
* **YARO Console**: Configuración del tema oscuro reemplazando variables CSS.

### 2. [Instalación e Integración](instalacion.md)
* Generación del Personal Access Token de GitHub.
* Configuración del archivo `.npmrc` del proyecto consumidor.
* Configuración de estilos globales en `angular.json` y fuentes en `index.html`.
* Configuración específica para el Monorepo Nx (resolución por path alias).
* Flujo de publicación e integración continua para subir nuevas versiones al registro npm de GitHub.

### 3. [Componentes: Átomos](atoms.md)
* Elementos básicos e indivisibles.
* Incluye: `YaroButton`, `YaroBadge`, `YaroInput`, `YaroToggle`, `YaroAvatar`, `YaroDelta` y `YaroQuantityControl`.

### 4. [Componentes: Moléculas](molecules.md)
* Combinaciones de átomos con responsabilidades específicas.
* Incluye: `YaroCard`, `YaroKpi`, `YaroStatBar`, `YaroStockBar`, `YaroNavItem`, `YaroFormRow`, `YaroInfoBox`, `YaroTableCard`, `YaroMenuItemCard`, `YaroOrderLine` y `YaroOrderCard`.

### 5. [Componentes: Organismos](organisms.md)
* Secciones completas de la interfaz compuestas por múltiples moléculas y átomos.
* Incluye: `YaroModal`, `YaroDataTable`, `YaroSidenav`, `YaroTopbar`, `YaroBienestarPicker`, `YaroConnectionBadge`, `YaroCategoryNav`, `YaroFloorMap` y `YaroOrderPanel`.

---

## Principios Técnicos de @yaro/ui

Para asegurar el rendimiento y consistencia, la base de código de `@yaro/ui` se adhiere a las siguientes directrices:

1. **Standalone Components**: Todos los componentes se declaran autónomos para evitar la sobrecarga de módulos pesados.
2. **OnPush Change Detection**: Optimizado para un rendimiento excepcional. Cualquier actualización de la vista depende de cambios en referencias de entradas (`@Input`) o signals locales.
3. **Display Contents**: Los componentes utilizan `:host { display: contents }` para no interferir en layouts Grid o Flex del contenedor padre.
4. **Variables CSS Nativas**: La librería no depende de frameworks utilitarios como TailwindCSS en su núcleo. Todo el estilado hace uso directo de variables CSS (`var(--*)`) para soportar temas y cambios dinámicos en caliente.
