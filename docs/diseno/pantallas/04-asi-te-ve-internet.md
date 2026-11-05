# 04. Así te ve Internet

Pantalla «Así te ve internet»: la ficha del local con lo que se ha encontrado.

## Objetivo

Que el usuario revise y corrija lo que el análisis ha encontrado de su local.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «5 · S04 · Así te ve internet» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Contenido

Los datos del diseño son de ejemplo. Cada bloque lleva una etiqueta con su origen: Google, Tu web, IA o Tú.

- **Cabecera:** botón atrás y el título «Así te ve internet».
- **Texto:** «Si algo no está bien, toca el lápiz y cámbialo.»
- **Tu local:** nombre y descripción corta, editables con el lápiz.
- **Valoración:** estrellas y número de reseñas de Google Maps. Se piden en vivo y no se guardan.
- **Cómo hablas:** de 1 a 5 tonos, por ejemplo Cercano, Tradicional y Apetitoso, y una frase de ejemplo.
- **Tus colores y letras:** hasta seis colores y las tipografías detectadas, sacados del rótulo, la web y las fotos.
- **Dónde te encuentran:** web, teléfono e Instagram, con el botón «Conectar Instagram».
- **Botón «Todo correcto, seguir».** Exige el nombre y al menos un tono.

## Estados

Cargando, error, con datos, guardando un campo y confirmando. Un cambio guarda solo ese campo y lo marca con el origen «Tú». Si otro cambio llegó antes, avisa del conflicto.

Si el local se dio de alta a mano, la pantalla avisa de que sin ficha de Google no hay valoración ni reseñas.

## Navegación

Se llega desde [03. Análisis](03-analisis.md), o desde [13. Perfil](13-perfil.md) en modo edición. El botón «Todo correcto, seguir» lleva a [05. Home](05-home.md) y vacía la pila del alta.

## Implementación

- Android: pantalla provisional en `feature/asiteveinternet`, con la ruta `AsiTeVeInternetRoute(restauranteId, modoEdicion)`. Muestra el título y los botones que llevan a la siguiente pantalla. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
