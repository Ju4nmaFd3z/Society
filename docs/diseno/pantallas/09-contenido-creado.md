# 09. Contenido creado

Resultado de la creación de contenido.

## Objetivo

Mostrar el contenido creado mientras se genera, recoger los cambios que quiera el usuario y pasar a publicarlo.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «10 · S09 · Contenido creado» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisiones D-17 y D-19.

## Contenido

- **Cabecera:** botón atrás y el título «Crear contenido».
- **Progreso:** «Generando 2 de 4» y el tiempo restante, por ejemplo «Quedan unos 40 s».
- **Diapositiva actual:** carrusel 4:5 o historia 9:16, con su posición y su estado.
- **Tira de diapositivas:** cada una con su estado (pendiente, generando, generada o error).
- **¿Cambiar algo?:** campo de 3 a 300 caracteres para pedir un cambio. Cada diapositiva admite 3 cambios («1/3 cambios») y, al agotarlos, avisa de que «ya no admite más cambios».
- **Botón «Regenerar esta diapositiva».**
- **Botón «Descripción y hashtags»:** desactivado hasta que todas las diapositivas estén generadas.

## Estados

Generando, lista, error de una diapositiva, moderación y error. Errores posibles: la imagen no cumple las normas del proveedor, límite alcanzado (3 cambios o 40 imágenes al día) y proveedor no disponible, con la opción de reintentar.

## Navegación

Se llega desde [08. Fotos del local](08-fotos-del-local.md), desde [05. Home](05-home.md) con una pieza reciente o desde [15. Calendario](15-calendario.md). El botón «Descripción y hashtags» lleva a [09P. Publicar contenido](09p-publicar-contenido.md).

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
