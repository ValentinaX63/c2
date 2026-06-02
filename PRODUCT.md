# Product

## Register

brand

## Lector y acción esperada

Este archivo es para instructores, mentores, operadores del programa y agentes de IA que preparan material para Hardcore AI.

Después de leerlo, una persona o agente debe poder crear, revisar o actualizar una estación de una cohorte sin romper la promesa pedagógica, la estructura de trabajo ni la voz de marca del programa.

## Usuarios

### Participantes

Hardcore AI está diseñado para arquitectos, desarrolladores senior, product builders, fundadores, FDEs y líderes técnicos que ya saben construir software, pero necesitan operar con agentes de IA sin caer en "vibe coding", demos frágiles o prompts sueltos.

Llegan con contexto profesional real: productos propios, problemas internos de empresa, restricciones de arquitectura, dudas sobre herramientas y necesidad de evidencia. No necesitan una introducción genérica a IA. Necesitan criterio, método y práctica transferible.

### Instructores

Cada instructor prepara una estación con una misión clara, material ejecutable, ejemplos concretos, demos y tareas. El instructor no solo expone un tema, también deja al participante con un artefacto que alimenta la siguiente estación.

### Mentores y equipo operativo

El equipo de soporte usa este repositorio para revisar consistencia entre cohortes, detectar huecos antes de la clase, preparar tutorías y asegurar que las tareas tengan prerrequisitos, recursos y criterios de completitud claros.

### Agentes de IA

Los agentes leen este archivo junto con `DESIGN.md` para generar slides, READMEs, runbooks, prompts, templates, demos y guías de estudiante que mantengan continuidad entre sesiones y cohortes.

## Propósito del producto

Hardcore AI es un programa intensivo de enseñanza para pasar de usar IA como copiloto ocasional a operar un ciclo completo de construcción con agentes: descubrir producto, especificar con rigor, diseñar arquitectura, ejecutar con harnesses, validar con tests y documentar decisiones.

El producto real del programa no es una clase aislada. Es un sistema de aprendizaje por estaciones donde cada día produce insumos para el siguiente:

- Visión de producto o solución interna.
- Investigación de validación y crítica.
- Product Vision Board o Internal Solution Brief.
- PRD co-creado con IA.
- Artefactos de AI-DLC en Inception y Construction.
- Repositorios agent-ready con contexto, permisos, hooks, MCPs, skills y subagentes.
- Demos, diffs, planes y evidencia verificable.

El repositorio es la memoria operativa del programa. Debe permitir que una nueva cohorte herede lo aprendido por la anterior sin copiar accidentalmente ruido, inconsistencias o decisiones caducas.

## Modelo pedagógico

### Estaciones, no lecciones sueltas

Una estación es una unidad de avance. Tiene título, instructor, descripción, estructura modular, prerrequisitos, tareas, stack, assets y artefactos esperados. La estación debe responder: qué cambia en la capacidad del participante después de esta clase y qué debe entregar antes de la siguiente.

### Artefactos antes que teoría

La teoría aparece para desbloquear acción. Cada sesión debe aterrizar en un artefacto: un documento, un repo configurado, un prompt probado, un diagrama, un PRD, una arquitectura, un plan de ejecución, un conjunto de tests o una demo reproducible.

### Human-in-the-loop

El programa enseña a usar agentes con aprobaciones, revisiones y contratos explícitos. Los participantes deben aprender a responder preguntas del agente, aprobar artefactos por fase y detener ejecución cuando falta contexto.

### Context engineering

El diferencial del programa no está en prompts más largos. Está en preparar entornos, memoria, reglas, skills, MCPs, templates y documentos que reducen ambigüedad antes de pedirle trabajo a un agente.

### Evidencia verificable

Cada tarea debe pedir evidencia. No basta "lo hice". El entregable debe poder inspeccionarse: archivos Markdown, screenshots, logs, diffs, tests, plan aprobado, artefactos de AI-DLC o repo funcionando.

## Cohortes, repos y estaciones

La unidad superior es la cohorte. Las cohortes pueden vivir como directorios en este repositorio o como repositorios asociados cuando la operación del programa lo requiera. El convenio actual usa carpetas tipo `c1`, `c2`, `c3` para agrupar el material de cada cohorte.

Dentro de cada cohorte, las clases se organizan como `Estación N` o equivalente local. Cada estación debe poder leerse de forma independiente por un participante que llega frío, pero también debe declarar cómo se conecta con las estaciones anteriores y siguientes.

Una estación nueva debe incluir, como mínimo:

- `README.md` o guía principal para estudiantes.
- Título de la estación.
- Instructor principal y colaboradores si aplica.
- Descripción de 2 a 4 líneas con relevancia y enfoque.
- Estructura de módulos con duración, instructor y takeaways accionables.
- Prerrequisitos que puedan completarse antes de la clase.
- Tarea escrita en lenguaje de participante, no en lenguaje de objetivo académico.
- Stack usado, cuando aplique.
- Assets utilizados: slides, PDFs, prompts, templates, demos, repos base, codelabs.
- Criterios de completitud o checklist para llegar listo a la siguiente estación.

Cuando una estación tenga práctica técnica, el directorio debe separar inputs, outputs y ejemplos. Por ejemplo: `docs` para insumos, `specs` para salidas, `demo` para proyectos ejecutables, `templates` para archivos copiables y `runbook` para ejecución paso a paso.

## Personalidad de marca

Tres palabras: intenso, cinematográfico, operativo.

Hardcore AI debe sentirse como una sala de guerra bien iluminada por pantallas: técnica, exigente, directa y con energía de alto rendimiento. No es academia pasiva ni contenido motivacional. Es entrenamiento para equipos que quieren construir mejor con IA en condiciones reales.

La voz debe ser:

- Clara, sin solemnidad innecesaria.
- Senior, sin explicar lo obvio.
- Provocadora cuando una tensión ayuda a pensar.
- Práctica cuando hay una tarea que ejecutar.
- Exigente con la evidencia.
- Humana, con suficiente cercanía para sostener sesiones largas.

## Anti-referencias

Evitar estos patrones:

- Páginas y slides genéricos de "AI startup": gradientes morados, paneles de vidrio, tarjetas repetidas, hero vacío, palabras grandes sin evidencia.
- E-learning corporativo blando: fondos blancos, iconos decorativos, objetivos académicos abstractos, mucho texto y poca acción.
- Prompt zoo: listas de prompts sin contexto, sin artefactos, sin criterio de validación.
- Vibe coding: construir antes de definir contrato, saltar de demo a producción, confiar en el chat como fuente de verdad.
- Material de clase que solo enumera temas. La descripción debe explicar para qué sirve la estación y qué podrá hacer el participante.
- Exceso de lima. El acento pierde fuerza si todo grita.
- Slides sin imagen en portadas o separadores. La marca depende de atmósfera, profundidad y tensión visual.

## Principios de diseño del producto

### 1. Cada estación mueve el sistema

Una estación no existe para cubrir temario. Existe para cambiar el estado del participante y producir un insumo que desbloquea el siguiente paso.

### 2. El participante trabaja sobre su propio caso

Los ejemplos son andamios. El aprendizaje ocurre cuando la persona aplica el método a su producto, solución interna o problema real.

### 3. Las reglas viven en archivos, no en memoria oral

Si una decisión debe repetirse entre cohortes, debe quedar documentada en Markdown, templates, skills, runbooks o checks. El programa debe mejorar cada vez que se ejecuta.

### 4. La IA es colaborador, no espectáculo

Las demos deben mostrar control, contexto y verificación. La magia sin método no es el objetivo.

### 5. La marca sube la exigencia

El look oscuro cinematográfico y el lima eléctrico no son decoración. Crean una expectativa de foco, profundidad técnica y energía. El material debe estar a la altura de esa promesa.

## Accesibilidad e inclusión

El idioma principal del programa es español. Los términos técnicos pueden mantenerse en inglés cuando son nombres propios o estándares de la industria, pero deben explicarse cuando afecten una tarea.

Las experiencias web y documentos generados para participantes deben aspirar a WCAG AA. Los slides y materiales deben mantener contraste alto, texto escaneable, diagramas con alternativas textuales cuando sea razonable y tareas que no dependan solo del color para comunicar estado.

Para sesiones largas, priorizar claridad visual: un concepto por slide, máximo 5 bullets, tamaños de texto legibles, pausas visuales y progreso visible. Evitar motion innecesario, parpadeos o efectos que cansen.

## Fuentes de contexto

Este documento sintetiza la estructura de clases de C1, los materiales de C2 y los estándares y skills de diseño según el artículo "Fixing Visual AI Slop": skills dan mejores hábitos al agente, mientras `PRODUCT.md` y `DESIGN.md` le dan memoria estable y revisable.
