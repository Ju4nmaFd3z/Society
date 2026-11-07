# 02. Empezamos

Pantalla «Busca tu local». Permite encontrar el local del usuario en Google Maps y confirmarlo para empezar el análisis.

## Objetivo

Que el usuario encuentre su local, o el que quiera gestionar.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «2 · S02 · Busca tu local» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisión D-05: la clave de Google solo vive en el servidor.

## Contenido

Disposición: la cabecera con el botón atrás y el título «Busca tu local», el buscador y, debajo, los resultados.

- **Buscador:** campo de texto donde se escribe el nombre del local y la zona, de 3 a 100 caracteres. Con menos de 3, el botón de buscar está desactivado.
- **Resumen de resultados:** por ejemplo, «2 resultados en Google Maps».
- **Lista de locales:** 20 resultados por página y 60 como máximo. Se carga a medida que el usuario hace scroll.
- **Tarjeta de local:** una por cada resultado, con:
    - Foto del local.
    - Nombre.
    - Calle y zona.
    - Estrellas y número de reseñas.
    - Autor de la foto y el texto «Google Maps», que exigen las políticas de Places.
- **Botón «Es mi local»:** abre el modal «¿Es tu local?», con la casilla obligatoria «Soy propietario o gestor». En Android, de momento, es el botón «Analizar» de la tarjeta.
- **Enlace «¿No aparece tu local? Añádelo a mano»:** lleva a [02M. Alta manual](02m-alta-manual.md).

El buscador de página web de la especificación anterior se descarta: la web se pide en 02M y se edita en [04. Así te ve Internet](04-asi-te-ve-internet.md).

## Estados

Además de los cuatro estados obligatorios (cargando, vacío, error y con datos), la pantalla tiene un estado inicial, anterior a la primera búsqueda.

1. **Inicial:** aún no se ha buscado.
2. **Cargando:** indicador de carga mientras llegan los resultados.
3. **Vacío:** «No encontramos tu local», con la salida «Añádelo a mano».
4. **Error:** «Revisa tu conexión» y el botón «Reintentar».
5. **Con datos:** locales y carga de más páginas al llegar al final de la lista.

Después de confirmar hay dos estados más: confirmando y guardando. Errores posibles: local ya dado de alta con otra cuenta (409), falta de conexión y proveedor no disponible. Los textos de Android todavía son provisionales. Ver [Modales y estados](modales-y-estados.md).

## Navegación

Se llega desde [01A. Acceso](01a-acceso.md) o al abrir la app sin local dado de alta. Al confirmar el local se pasa a [03. Análisis](03-analisis.md). «Añádelo a mano» lleva a 02M.

## Implementación

- Android: [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md). El código está en la carpeta `feature/empezamos`.
