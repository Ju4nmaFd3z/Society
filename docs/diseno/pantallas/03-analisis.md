# 03. Análisis

Pantalla «Estudiando tu local». Aquí empieza el análisis del local que el usuario ha elegido en Empezamos.

## Objetivo

Estudiar lo que internet sabe del local seleccionado en [02. Empezamos](02-empezamos.md) y mantener al usuario informado mientras tanto.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).

## Contenido

Es el paso 3 de 6 del proceso. Los datos del diseño son de ejemplo.

- **Cabecera:** botón atrás, «Paso 3 de 6» y el título «Estudiando tu local».
- **Texto:** «Buscamos lo que internet sabe de Torre de Vega. Tarda unos minutos.»
- **Progreso:** porcentaje y tiempo restante, por ejemplo «46 %» y «Quedan unos 4 minutos».
- **Fuentes:** Google Maps, Instagram, Tu web, TikTok y Colores y letras, cada una con su estado: hecho, ahora o pendiente.
- **Botón «Avisarme cuando termine».**
- **Aviso:** «Puedes cerrar la app. Seguimos trabajando.»

## Estados

Por definir. Muestra el progreso del análisis y el estado de cada fuente. Si la pantalla muestra datos, incluirá los cuatro estados obligatorios: cargando, vacío, error y con datos.

## Navegación

Se llega desde [02. Empezamos](02-empezamos.md). El botón «Avisarme cuando termine» lleva a [04. Así te ve Internet](04-asi-te-ve-internet.md).

## Implementación

- Android: pantalla de cartón piedra que recibe el ID del local. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
