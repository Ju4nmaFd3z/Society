# 06. Crear contenido

Primer paso de la creación de contenido: la idea del post.

## Objetivo

Que el usuario diga qué quiere publicar.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «7 · S06 · Crear contenido · Idea» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisiones D-16 y D-18.

## Contenido

Es el primer paso de tres: Idea, Estética y Local.

- **Cabecera:** botón atrás y el título «Crear contenido», con el selector de pasos.
- **Formato:** Carrusel (de 3 a 5 imágenes 4:5) o Historia (de 1 a 3 pantallas 9:16).
- **Tu idea:** campo de texto de 10 a 300 caracteres, con contador.
- **Tipo de publicación:** Plato estrella, Novedad, Oferta o Evento.
- **Condiciones:** precio, fechas y límites. Son obligatorias si el tipo es Oferta o Evento, con al menos un campo y 120 caracteres como máximo cada uno. La IA no puede inventar precios ni fechas: solo usa los que escribe el dueño.
- **Botón «Siguiente: estética».**

Si se llega desde una campaña propuesta, la idea y el tipo vienen precargados.

## Estados

Borrador, con los errores por campo, y cargando la campaña cuando viene de [11. Campaña propuesta](11-campana-propuesta.md). El error típico es «Indica precio o fechas».

## Navegación

Se llega desde el botón «+» de [05. Home](05-home.md) o desde 11. El siguiente paso es [07. Elección de estilo](07-eleccion-de-estilo.md). El borrador es el mismo hasta 08: no se guarda nada antes.

## Implementación

- Android: pantalla provisional en `feature/crearcontenidograph`, con la ruta `CrearContenidoRoute`. Muestra el título y los botones que llevan a la siguiente pantalla. Va dentro del grafo `CrearContenidoGraph(campanaId)`, que agrupa 06, 07 y 08 para que compartan el borrador. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
