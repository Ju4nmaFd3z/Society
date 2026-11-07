# 01. Inicio

Primera pantalla de la app: da la bienvenida y explica en pocas palabras qué hace Society.

## Objetivo

Que el usuario entienda qué le ofrece Society antes de buscar su local.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).

## Contenido

- **Título:** «Ideas que llenan tu local», con la mascota de Society.
- **Texto:** «Creamos contenido con IA, estrategias de marketing y campañas que atraen clientes y hacen crecer tu negocio.»
- **Tres ventajas:** Contenido con IA, Estrategias de marketing y Campañas que atraen clientes.
- **Botón «Descubre cómo funciona».**

## Estados

No muestra datos, así que no tiene estados de carga, vacío ni error.

## Navegación

Es la pantalla con la que se abre la app sin sesión. El botón «Descubre cómo funciona» lleva a [01A. Acceso](01a-acceso.md). Con una sesión válida, la app salta directamente a 02, 03, 04 o 05, según el estado del alta del local.

## Implementación

- Android: pantalla provisional en `feature/bienvenida`, con la ruta `BienvenidaRoute`. Muestra el título y los botones que llevan a la siguiente pantalla. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
