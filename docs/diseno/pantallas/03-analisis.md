# 03. Análisis

Pantalla «Estudiando tu local». Aquí empieza el análisis del local que el usuario ha elegido en Empezamos.

## Objetivo

Estudiar lo que internet sabe del local seleccionado en [02. Empezamos](02-empezamos.md) y mantener al usuario informado mientras tanto.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «4 · S03 · Estudiando tu local» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Contenido

Los datos del diseño son de ejemplo.

- **Cabecera:** botón atrás y el título «Estudiando tu local».
- **Texto:** «Buscamos lo que internet sabe de [nombre del local]. Tarda unos minutos.»
- **Progreso:** barra con «3 de 5», que es el número de pasos terminados entre cinco, y el tiempo restante, por ejemplo «Quedan unos 2 min». Con tiempo cero, «Casi listo…».
- **Cinco pasos:** Google Maps, Tu web, Instagram, Colores y letras, y Ficha con IA. Cada uno muestra su estado (pendiente, en curso, hecho, omitido o error) y un detalle, por ejemplo «695 reseñas leídas». Instagram es P1 y se omite si no está conectado.
- **Aviso «No salgas de la aplicación».** Es la decisión D-19: no hay botón «Avisarme cuando termine».

## Estados

Cargando, en curso, terminado (completo o parcial), error con opción de reintentar y cancelado.

La pantalla consulta el estado cada 3 segundos. Si falla un paso que no es crítico, el análisis sigue y termina como parcial.

## Navegación

Se llega desde [02. Empezamos](02-empezamos.md), [02M. Alta manual](02m-alta-manual.md) o al abrir la app con un análisis en curso. Al terminar pasa a [04. Así te ve Internet](04-asi-te-ve-internet.md), con un aviso de «Faltan datos» si fue parcial. «Atrás» abre el modal «¿Cancelar el análisis?» y, al confirmar, vuelve a 02. Ver [Modales y estados](modales-y-estados.md).

## Implementación

- Android: pantalla de cartón piedra que recibe el ID del local. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
