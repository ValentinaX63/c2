# Guía de Instalación y Configuración

`@yaro/ui` es una biblioteca privada de componentes de Angular standalone alojada en **GitHub Packages**. Dependiendo del entorno de desarrollo, existen dos formas principales de consumir la biblioteca.

---

## Requisitos Previos
* **Angular 17+**
* **Node.js 18+**
* Acceso al repositorio privado `ValentinaX63/yaro-ui` en GitHub.

---

## Método 1: Consumir en un Proyecto Externo (Instalación vía npm)

Sigue estos pasos para instalar y configurar la librería desde un repositorio independiente.

### Paso 1: Personal Access Token (PAT) de GitHub
Dado que el paquete es privado, necesitas un token de acceso clásico de GitHub con el scope `read:packages`.
1. Ve a **GitHub → Settings → Developer settings → Personal access tokens → Tokens (classic)**.
2. Genera un nuevo token con el permiso `read:packages`.
3. Guarda el token en tu configuración global de terminal como variable de entorno (por ejemplo, en `~/.zshrc` o `~/.bashrc`):

```bash
# ~/.zshrc o ~/.bashrc
export GITHUB_TOKEN=ghp_tuTokenAqui
```

> [!WARNING]
> **Nunca dejes el token de acceso expuesto directamente en el código o en archivos de configuración comiteados al repositorio público.** Referéncialo siempre a través de variables de entorno.

---

### Paso 2: Crear el archivo `.npmrc`
En la raíz de tu proyecto externo, crea un archivo `.npmrc` para indicar a npm que los paquetes bajo el ámbito (scope) `@yaro` deben descargarse del registro de paquetes de GitHub.

```ini
# .npmrc (raíz del proyecto)
@yaro:registry=https://npm.pkg.github.com
//npm.pkg.github.com/:_authToken=${GITHUB_TOKEN}
```

---

### Paso 3: Instalar la librería
Con el archivo `.npmrc` y el token listos, ejecuta la instalación como con cualquier otro paquete:

```bash
npm install @yaro/ui
```

---

### Paso 4: Cargar Estilos Globales y Fuentes

#### A. Registrar estilos globales en `angular.json`
Asegúrate de cargar la hoja de estilos de `@yaro/ui` **antes** que las hojas globales del proyecto consumidor para permitir que tus estilos locales sobrescriban las variables de ser necesario:

```json
// angular.json
"styles": [
  "node_modules/@yaro/ui/styles/yaro-ui.scss", // ← Debe ir primero
  "src/styles.scss"
]
```

#### B. Cargar tipografía en `index.html`
Para que el sistema tipográfico funcione correctamente, importa las fuentes **Outfit** y **JetBrains Mono** desde Google Fonts en el encabezado HTML:

```html
<!-- src/index.html -->
<link rel="preconnect" href="https://fonts.googleapis.com">
<link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
<link href="https://fonts.googleapis.com/css2?family=Outfit:wght@300;400;500;600;700&family=JetBrains+Mono:wght@400;500&display=swap" rel="stylesheet">
```

---

### Paso 5: Importar y Usar Componentes
Todos los componentes de `@yaro/ui` son **standalone**. No debes importarlos en un `NgModule`, sino declararlos directamente en el arreglo `imports` del componente que los requiera:

```typescript
// mi-feature.component.ts
import { Component } from '@angular/core';
import { YaroCardComponent, YaroButtonComponent } from '@yaro/ui';

@Component({
  selector: 'app-mi-feature',
  standalone: true,
  imports: [YaroCardComponent, YaroButtonComponent],
  template: `
    <yaro-card title="Bienvenido">
      <p>Contenido del proyecto externo.</p>
      <ng-container card-footer>
        <yaro-button variant="primary">Aceptar</yaro-button>
      </ng-container>
    </yaro-card>
  `
})
export class MiFeatureComponent {}
```

---

## Método 2: Consumir dentro del Monorepo Nx

Si estás construyendo una aplicación dentro del mismo monorepo Nx donde reside `libs/yaro-ui`, no es necesario realizar una instalación de npm. Puedes apuntar directamente a las fuentes a través de alias de TypeScript.

### Paso 1: Configurar Path Alias en tsconfig
Asegúrate de tener mapeado el alias `@yaro/ui` hacia el barrel export `index.ts` de la librería en la base de la configuración de TypeScript:

```json
// tsconfig.base.json
{
  "compilerOptions": {
    "paths": {
      "@yaro/ui": ["libs/yaro-ui/src/index.ts"]
    }
  }
}
```

### Paso 2: Registrar estilos en `angular.json`
Registra la ruta directa de los estilos locales de la librería en el arreglo de estilos del proyecto de tu app:

```json
// angular.json (bajo el proyecto de tu app)
"styles": [
  "libs/yaro-ui/src/styles/yaro-ui.scss",
  "src/styles.scss"
]
```

---

## Flujo de Publicación de Nuevas Versiones

Cuando realices modificaciones o agregues componentes a la librería en `libs/yaro-ui/src/`, sigue este flujo para empaquetarla y subir la nueva versión a **GitHub Packages**:

### Paso 1: Bump de Versión
Incrementa la versión siguiendo la convención de control de versiones semántico (**semver**):
* **Patch** (corrección de errores): `1.0.0` → `1.0.1`
* **Minor** (nuevas funcionalidades retrocompatibles): `1.0.0` → `1.1.0`
* **Major** (cambios incompatibles): `1.0.0` → `2.0.0`

Puedes incrementar la versión automáticamente desde la raíz utilizando:

```bash
npm version patch --prefix libs/yaro-ui
```

### Paso 2: Construir la librería con Nx
Usa el compilador de empaquetado para generar el output de distribución:

```bash
nx build yaro-ui
```
Esto creará los entregables optimizados dentro del directorio `dist/libs/yaro-ui/`.

### Paso 3: Publicar en el Registro
Navega a la carpeta generada y ejecuta la publicación de npm:

```bash
cd dist/libs/yaro-ui
npm publish
```

### Paso 4: Confirmar cambios en Git
Es una buena práctica confirmar el cambio de versión en el repositorio y subir la etiqueta (tag):

```bash
git add libs/yaro-ui/package.json
git commit -m "chore: publicar version v1.0.1"
git push origin main
```

Para actualizar los proyectos que consumen la librería de manera externa:

```bash
npm update @yaro/ui
```
