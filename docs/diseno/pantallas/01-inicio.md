# 01. Inicio

Primera pantalla de la app. Permite encontrar el local del usuario en Google Maps y seleccionarlo para proceder con el análisis.

## Objetivo

Que el usuario encuentre su local, o el que quiera gestionar.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pendiente.

## Contenido

- **Buscador:** campo de texto donde se escribe el nombre del local o la zona.
- **Lista de locales:** se carga de forma perezosa (lazy) a medida que el usuario hace scroll.
- **Tarjeta de local:** una por cada resultado, con:
    - Primeras imágenes del local.
    - Nombre.
    - Media de reseñas.
    - Número de reseñas.
    - Calle.
    - Botón Continuar.
- **Buscador de página web:** no devuelve resultados, porque la página web es única. El análisis empieza directamente desde aquí.

## Estados

Además de los cuatro estados obligatorios (cargando, vacío, error y con datos), la pantalla tiene un estado inicial, anterior a la primera búsqueda.

1. **Inicial:** aún no se ha buscado. Mensaje «No se han buscado locales todavía».
2. **Cargando:** indicador de carga mientras llegan los resultados.
3. **Vacío:** la búsqueda no ha devuelto locales.
4. **Error:** aviso de fallo de red, con opción de reintentar.
5. **Con datos:** búsqueda realizada, con los locales y la carga de más páginas al llegar al final de la lista.

## Navegación

Desde esta pantalla se llega a [02. Análisis](02-analisis.md): al pulsar Continuar en la tarjeta de un local o desde el buscador de página web.

## Implementación

- Android: [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).