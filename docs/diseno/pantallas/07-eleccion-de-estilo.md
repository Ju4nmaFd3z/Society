# 07. Elección de estilo

Segundo paso de la creación de contenido: el estilo visual del post.

## Objetivo

Que el usuario elija la estética que quiere para su contenido.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «8 · S07 · Crear contenido · Estética» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Contenido

- **Cabecera:** botón atrás, el título «Crear contenido» y el selector de pasos, con «Estética» seleccionado.
- **Sube tus referencias:** botón «+» para subir una imagen. JPEG o PNG, de 10 MB como máximo. Es solo inspiración: nunca se publica.
- **Estética de tu local:** colores y tonos de la ficha. Es de solo lectura.
- **Elige las mejores:** rejilla de referencias del catálogo. Hay que elegir exactamente una, del catálogo o subida.
- **Botón «Siguiente: local».**

## Estados

Cargando, error, con datos y subiendo. Si no hay una referencia elegida, avisa con «Elige o sube una referencia». Una referencia subida solo se guarda si el usuario la sube.

## Navegación

Se llega desde [06. Crear contenido](06-crear-contenido.md). El siguiente paso es [08. Fotos del local](08-fotos-del-local.md).

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
