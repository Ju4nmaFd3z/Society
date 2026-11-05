# 11. Campaña propuesta

Detalle de una oportunidad detectada por Society y de la campaña que propone.

## Objetivo

Que el usuario entienda por qué se le propone una campaña y pueda crear contenido a partir de ella.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «12 · S11 · Campaña propuesta» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisiones D-11 y D-18.

## Contenido

Los datos del diseño son de ejemplo.

- **Cabecera:** botón atrás y el título «Campaña propuesta».
- **Oportunidad:** título, categoría (producto, experiencia, novedad, participación, evento o fidelización) y confianza (observado, estimado o hipótesis).
- **Por qué te lo proponemos:** al menos una señal, cada una con su fuente y su periodo. Ninguna cifra va sin fuente.
- **Falta confirmar:** lo que el usuario debe comprobar antes de publicar, por ejemplo qué platos de invierno hay en carta.
- **Objetivo sugerido y periodo.**
- **Piezas sugeridas:** cada una con su formato (carrusel o historia), su idea y su tipo.
- **Botón «Crear pieza».** Crea la campaña y abre [06. Crear contenido](06-crear-contenido.md) con la idea precargada.
- **Botones «Guardar» y «Descartar».** Guardar deja la oportunidad en la Home. Descartar hace que no vuelva a aparecer.

## Estados

Cargando, error con opción de reintentar, con datos y guardando.

## Navegación

Se llega desde una oportunidad de [05. Home](05-home.md) o al crear una campaña en [12. Campaña a medida](12-campana-a-medida.md). Desde aquí se vuelve a 05 o se pasa a 06.

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
