# 05. Home

Pantalla principal de la app, a la que se llega una vez terminado el análisis del local.

## Objetivo

Dar acceso a las oportunidades detectadas, a la creación de contenido y al resto de secciones de la app.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: archivo definitivo, página «1 · Presentación». Ver [Diseño](../README.md).
- Detalle: página «6 · S05 · Home» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisión D-11: ninguna cifra sin fuente. Se quita el aviso «Mes bajo detectado · 10 % menos de clientes».

## Contenido

- **Cabecera:** el nombre del local y la valoración de Google, con estrellas y número de reseñas, que se piden en vivo.
- **Campana de notificaciones:** P2. En el MVP aparece como «Próximamente», con el acceso desactivado.
- **Oportunidades para tu local:** una tarjeta por oportunidad, con su categoría, su confianza (observado, estimado o hipótesis), el motivo y el número de piezas sugeridas.
- **Crea algo nuevo:** «Contenido desde una idea» y «Campaña a tu medida». La segunda es P1.
- **Estilos que funcionan:** referencias del catálogo para inspirar el próximo post.
- **Tus piezas recientes:** hasta cinco, con su formato, estado, miniatura y fecha.
- **Barra inferior:** Inicio, calendario (P1), botón «+» para crear, estadísticas (P2, «Próximamente») y perfil.

## Estados

Cargando, con esqueletos, error con opción de reintentar, con datos y sin oportunidades, que muestra la tarjeta «Crea tu primera pieza». Ver [Modales y estados](modales-y-estados.md).

La Home solo lee datos: no guarda nada.

## Navegación

Se llega desde [04. Así te ve Internet](04-asi-te-ve-internet.md) o al abrir la app con el alta completada. Desde aquí:

- Una oportunidad lleva a [11. Campaña propuesta](11-campana-propuesta.md).
- «Contenido desde una idea», un estilo y el botón «+» llevan a [06. Crear contenido](06-crear-contenido.md).
- «Campaña a tu medida» lleva a [12. Campaña a medida](12-campana-a-medida.md).
- Una pieza reciente lleva a [09. Contenido creado](09-contenido-creado.md).
- En la barra inferior, el calendario lleva a [15. Calendario](15-calendario.md) y el perfil a [13. Perfil](13-perfil.md).
- La campana y las estadísticas, a [10. Notificaciones](10-notificaciones.md) y [14. Estadísticas](14-estadisticas.md), que son P2.

## Implementación

- Android: pantalla de cartón piedra, ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
