# 02. Empezamos

Pantalla «Busca tu local». Permite encontrar el local del usuario en Google Maps y seleccionarlo para proceder con el análisis.

## Objetivo

Que el usuario encuentre su local, o el que quiera gestionar.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).

## Contenido

Disposición: la cabecera con el botón atrás y el título «Busca tu local», el buscador y, debajo, los resultados.

- **Buscador:** campo de texto donde se escribe el nombre del local o la zona.
- **Resumen de resultados:** por ejemplo, «2 resultados en Google Maps».
- **Lista de locales:** se carga de forma perezosa (lazy) a medida que el usuario hace scroll.
- **Tarjeta de local:** una por cada resultado, con:
    - Primeras imágenes del local.
    - Nombre.
    - Calle y zona.
    - Media de reseñas.
    - Número de reseñas.
- **Botón «Es mi local»:** confirma el local elegido. En Android, de momento, es el botón «Analizar» de la tarjeta.
- **Enlace «¿No aparece tu local? Añádelo a mano»:** destino por definir.

Pendiente de confirmar: la especificación anterior incluía un buscador de página web, que el diseño actual no muestra.

## Estados

Además de los cuatro estados obligatorios (cargando, vacío, error y con datos), la pantalla tiene un estado inicial, anterior a la primera búsqueda.

1. **Inicial:** aún no se ha buscado. Mensaje «No se han buscado locales todavía».
2. **Cargando:** indicador de carga mientras llegan los resultados.
3. **Vacío:** la búsqueda no ha devuelto locales. Mensaje «No se han encontrado locales».
4. **Error:** aviso de fallo de red, con opción de reintentar. Mensaje «Error de red. Revisa tu conexión e inténtalo de nuevo.» y botón «Reintentar».
5. **Con datos:** búsqueda realizada, con los locales y la carga de más páginas al llegar al final de la lista.

## Navegación

Se llega desde [01. Inicio](01-inicio.md). Al pulsar «Es mi local» se pasa a [03. Análisis](03-analisis.md).

## Implementación

- Android: [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md). La carpeta del código se llama `feature/inicio`, por historia, y está pendiente de renombrar.
