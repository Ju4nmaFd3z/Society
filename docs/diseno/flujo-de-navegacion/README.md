# Flujo de navegación

Reglas para dibujar el diagrama de navegación de la aplicación y las decisiones de diseño que lo cierran.

## Diagrama

El diagrama de navegación es la página 1 («Mapa general del MVP») de [`diagrama-flujo-app.drawio`](../../arquitectura/diagramas/diagrama-flujo-app.drawio). El detalle de cada pantalla está en [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

Los diagramas usan los mismos números con el prefijo S (S01 … S15). Tres pantallas no tienen número en Figma: [01A. Acceso](../pantallas/01a-acceso.md), [02M. Alta manual](../pantallas/02m-alta-manual.md) y [09P. Publicar contenido](../pantallas/09p-publicar-contenido.md). Los modales y los estados vacíos o de error están en [Modales y estados](../pantallas/modales-y-estados.md).

## Flujo actual

Es el del mapa general del diagrama oficial. Las pantallas son las de [Pantallas](../pantallas/README.md).

- **Arranque:** sin sesión, 01 Inicio → 01A Acceso. Con sesión, la app salta a 02, 03, 04 o 05 según el estado del alta del local.
- **Alta del local:** 02 Empezamos → 03 Análisis → 04 Así te ve Internet → 05 Home. Si el local no aparece en Google Maps, 02 → 02M Alta manual → 03.
- **Desde 05 Home:** 10 Notificaciones, 11 Campaña propuesta, 12 Campaña a medida, 15 Calendario, 06 Crear contenido, 14 Estadísticas y 13 Perfil. Todas las pantallas vuelven a 05 con «atrás» o con la barra inferior.
- **Crear contenido:** 06 Idea → 07 Estética → 08 Local → 09 Contenido creado → 09P Publicar. Las flechas están dibujadas en la galería «Crear y publicar contenido» de la presentación de Figma.
- **Campañas:** 12 Campaña a medida → 11 Campaña propuesta → 06 («Crear pieza», con la idea precargada). 11 también se alcanza desde una oportunidad de 05.
- **Calendario:** 15 → 09 al tocar una pieza.
- **Perfil:** 13 → conectar Instagram y 13 → 01 al cerrar sesión.
- **Prioridades:** 12 y 15 son P1. 10 y 14 son P2: en el MVP aparecen como «Próximamente», con el acceso desactivado.

## Dudas de diseño cerradas

Respuestas a las dudas que quedaron abiertas en Figma. Cierran la issue SOCIETY-45.

| Duda | Respuesta |
| --- | --- |
| «Paso X de 6» en 03 y 04 | El análisis tiene 5 pasos y 03 muestra «3 de 5». El contador del alta se quita (C-08) |
| Selector: «2 · Platos» (06) o «2 · Estética» (07) | El del diseño: Idea · Estética · Local (C-11) |
| Qué elemento de Home lleva a 08 | Ninguno. El asistente es 06 → 07 → 08, sin salto directo |
| A dónde lleva «Contenido desde una idea» | A 06 |
| A dónde lleva «Añádelo a mano» (02) | A 02M, alta manual |
| A dónde lleva un estilo de «Estilos que funcionan» | A 06, con esa referencia ya elegida en 07 |
| ¿El icono a la izquierda del «+» es el calendario? | Sí, 15 Calendario [P1] |
| Nombre de la pantalla 12 | «Campaña a medida» |
| Buscador de página web | Se descarta. La web se pide en 02M y se edita en 04 (C-10) |

## Reglas obligatorias

- Cada pantalla es un marco grande con su nombre arriba, por ejemplo «INICIO». Todo lo que pasa en esa pantalla va dentro del marco.
- Cada cosa que el usuario ve (buscador, lista, botón, tarjeta) es un rectángulo dentro del marco.
- Cuando hay que decidir algo («¿hay resultados?»), se usa un rombo con dos salidas: sí y no.
- Los servicios externos, como Google Maps, se dibujan fuera del marco, con otra forma y otro color.
- Cada flecha lleva escrito lo que la provoca: «escribe un texto», «pulsa Continuar», «llega al final de la lista».
- Una flecha que sale del marco lleva a otra pantalla. Esa pantalla se dibuja con otro marco, aunque esté vacío o con línea punteada si aún no está diseñada.
- Toda pantalla que muestra datos incluye sus cuatro estados: cargando, vacío, error y con datos.
- Los post-its amarillos son solo para aclaraciones. Nunca forman parte del flujo.
- Los nombres de las pantallas son los mismos en los diagramas, en Figma y en el código.
