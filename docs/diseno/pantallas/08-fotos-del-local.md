# 08. Fotos del local

Tercer paso de la creación de contenido: las fotos del local.

## Objetivo

Que el usuario suba fotos reales de su local o elija las ya subidas anteriormente, si existen.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «9 · S08 · Crear contenido · Fotos» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisión D-32.

## Contenido

- **Cabecera:** botón atrás, el título «Crear contenido» y el selector de pasos, con «Local» seleccionado.
- **Contador:** por ejemplo «3 / 5». Un carrusel admite de 2 a 5 fotos y una historia, de 1 a 3.
- **Miniaturas** de las fotos elegidas, con opción de quitarlas.
- **Origen de las fotos:** Galería, Cámara o Ya subidas.
- **Avisos:** las fotos HEIC o PNG se pasan a JPEG, el máximo es de 10 MB y, si el lado corto mide menos de 1080 px, avisa de que «Saldrá pixelada».
- **Casilla de derechos:** «Tengo derechos sobre estas fotos y permiso de las personas que salen en ellas». Es obligatoria.
- **Botón «Generar contenido».**

## Estados

Selección de fotos, subiendo (con el progreso de cada foto), error de una foto, creando la pieza y hecho. Errores posibles: foto de más de 10 MB, fallo de subida y límite diario de 40 imágenes (429).

## Navegación

Se llega desde [07. Elección de estilo](07-eleccion-de-estilo.md). Al generar, la pantalla crea la pieza y pasa a [09. Contenido creado](09-contenido-creado.md).

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
