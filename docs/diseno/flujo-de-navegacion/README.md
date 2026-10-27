# Flujo de navegación

Reglas para dibujar el diagrama de navegación de la aplicación.

## Diagrama

El diagrama se mantiene en Lucidchart. Enlace de solo lectura: pendiente.

## Flujo actual

El flujo de las pantallas también está en el archivo de Figma, en la pestaña v3 · Cristal líquido. Ver [Diseño](../README.md). Las pantallas son las de [Pantallas](../pantallas/README.md).

- 01 Inicio → 02 Empezamos → 03 Análisis → 04 Así te ve Internet → 05 Home.
- Desde 05 Home: 10 Notificaciones, 11 Campaña propuesta, 12 Campaña a medida, 15 Calendario, 06 Crear contenido, 14 Estadísticas y 13 Perfil.
- Crear contenido sigue los pasos del selector: 06 Idea, 07 Estética y 08 Local, y termina en 09 Contenido creado. Figma no dibuja esas flechas: el orden sale del selector de pasos.


## Reglas obligatorias

- Cada pantalla es un marco grande con su nombre arriba, por ejemplo «INICIO». Todo lo que pasa en esa pantalla va dentro del marco.
- Cada cosa que el usuario ve (buscador, lista, botón, tarjeta) es un rectángulo dentro del marco.
- Cuando hay que decidir algo («¿hay resultados?»), se usa un rombo con dos salidas: sí y no.
- Los servicios externos, como Google Maps, se dibujan fuera del marco, con otra forma y otro color.
- Cada flecha lleva escrito lo que la provoca: «escribe un texto», «pulsa Continuar», «llega al final de la lista».
- Una flecha que sale del marco lleva a otra pantalla. Esa pantalla se dibuja con otro marco, aunque esté vacío o con línea punteada si aún no está diseñada.
- Toda pantalla que muestra datos incluye sus cuatro estados: cargando, vacío, error y con datos.
- Los post-its amarillos son solo para aclaraciones. Nunca forman parte del flujo.
- Los nombres de las pantallas son los mismos en Lucidchart, en Figma y en el código.
