# 15. Calendario

Calendario de las piezas de contenido. Es P1.

## Objetivo

Ver cuándo sale cada pieza, prevista o ya publicada.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «15 · S15 · Calendario [P1]» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Contenido

Es de solo lectura.

- **Cabecera:** botón atrás y el título «Calendario».
- **Mes:** con los botones para cambiar al mes anterior o al siguiente.
- **Calendario:** cada día con una marca por cada pieza, según sea prevista o publicada.
- **Piezas del día:** lista por fecha con el formato, la idea y el estado de cada pieza.

## Estados

Cargando, error, vacío y con datos. Si no hay piezas este mes, muestra «Aún no hay piezas este mes» y el botón «Crear contenido». Ver [Modales y estados](modales-y-estados.md).

## Navegación

Se llega desde la barra inferior de [05. Home](05-home.md). Al tocar una pieza se pasa a [09. Contenido creado](09-contenido-creado.md).

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
