# 12. Campaña a medida

Diseño de una campaña a medida. Es P1: se construye si sobra tiempo y, si no, se oculta el acceso en la Home. En el diseño, el título es «Diseña tu campaña».

## Objetivo

Ayudar al usuario a diseñar una campaña según lo que quiera conseguir.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «14 · S12 · Campaña a medida [P1]» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisión D-18.

## Contenido

- **Cabecera:** botón atrás y el título «Diseña tu campaña».
- **Objetivo:** obligatorio. Atraer clientes, Promocionar un producto, Fidelizar, Evento, Notoriedad o Participación.
- **Producto o servicio:** obligatorio, de 3 a 80 caracteres.
- **Mensaje:** opcional, hasta 200 caracteres.
- **Fechas:** inicio y fin. El fin no puede ser anterior al inicio ni el inicio anterior a hoy.
- **Formatos:** carrusel, historia o ambos.
- **Botón «Crear plan».**

## Estados

Formulario, enviando, error y hecho. Al crear el plan, Society propone de 2 a 4 piezas con su idea, formato y fecha.

## Navegación

Se llega desde «Campaña a tu medida» de [05. Home](05-home.md). Al crear el plan se pasa a [11. Campaña propuesta](11-campana-propuesta.md).

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
