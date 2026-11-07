# Prototipo

Recorrido navegable de la app en Figma, pensado para verse en un iPhone 16 Pro y con las pantallas, los modales y los estados conectados entre sí.

## Cómo abrirlo

Está en la página «3 · Prototipo» del [archivo de Figma definitivo](https://www.figma.com/proto/G4bwEgiP3eRRlNoHK4ftVA/Society--Final-Version-?page-id=4109%3A775&node-id=4109-776&starting-point-node-id=4109%3A776&scaling=scale-down&content-scaling=fixed). Se abre en la bienvenida. Con el enlace de solo lectura basta para recorrerlo.

En el modo presentación hay que elegir el dispositivo iPhone 16 Pro. Todos los marcos miden 402 × 874, igual que su pantalla, así que encajan sin márgenes.

## Cómo está hecho

- **Una página aparte.** La página «1 · Presentación» sigue siendo la galería de pantallas del diseño. El prototipo tiene sus propios 32 marcos, copias de esas pantallas ajustadas al iPhone 16 Pro, y no cambia el diseño.
- **Scroll real.** S04, S05, S06, S07, S09, S09P y S11 se desplazan hacia abajo. La barra de estado, el botón de volver, el indicador de inicio y la barra inferior de Home se quedan fijos, como en una app.
- **Modales como pantallas reales.** La hoja de cristal se superpone a la pantalla que la abre, con su velo, así que desenfoca lo que hay detrás.
- **Secuencias animadas.** Las que tardan en la app real avanzan solas:
  - **Estudiando tu local:** pasa de 3 a 5 pasos, con la barra y las filas cambiando, y a los pocos segundos lleva a «Así te ve internet».
  - **Contenido creado:** a los 3 segundos terminan las 4 diapositivas y se activa «Descripción y hashtags».
  - **Publicar:** «Publicar en Instagram» muestra un indicador de carga, luego «¡Publicada!» y, al cabo de un momento, el calendario.
- **Transiciones.** Avanzar y volver usan un desplazamiento lateral con muelle; las pestañas y los modales, un fundido.

## Puntos de inicio

| Punto de inicio | Pantalla | Para qué sirve |
| --- | --- | --- |
| Recorrido completo (desde la bienvenida) | 01 · Bienvenida | Todo el flujo del MVP |
| Estudiando tu local | 05 · Estudiando tu local | Ver el análisis animado |
| Home | 07 · Home | Empezar con el local ya dado de alta |
| Crear contenido | 08 · Crear contenido · Idea | Probar la creación y la publicación |
| Publicar contenido | 12 · Publicar contenido | Ver la publicación en Instagram |
| Campaña propuesta | 14 · Campaña propuesta | Ver el detalle de una oportunidad |
| Estado: sin resultados | 19 · Sin resultados | Buscador sin resultados |
| Estado: sin oportunidades | 21 · Sin oportunidades | Home de un local sin oportunidades |
| Estado: sin piezas este mes | 22 · Sin piezas este mes | Calendario vacío |
| Error: sin conexión | 20 · Sin conexión | Pantalla de error con Reintentar |

## Conexiones

Siguen el [mapa general de navegación](flujo-de-navegacion/README.md) del archivo.

| Desde | Acción | Va a |
| --- | --- | --- |
| Bienvenida | Descubre cómo funciona | Tu cuenta |
| Tu cuenta | Continuar | Busca tu local |
| Busca tu local | Es mi local | Modal ¿Es tu local? y, al confirmar, el análisis |
| Busca tu local | ¿No aparece tu local? Añádelo a mano | Añade tu local |
| Añade tu local | Guardar y analizar | Estudiando tu local |
| Estudiando tu local | Flecha atrás | Modal ¿Cancelar el análisis? |
| Así te ve internet | Todo correcto, seguir | Home |
| Home | Campana | Notificaciones |
| Home | Tarjeta de una oportunidad | Campaña propuesta |
| Home | Contenido desde una idea, o el botón «+» | Crear contenido |
| Home | Campaña a tu medida | Campaña a medida |
| Home | Una pieza reciente | Contenido creado |
| Home | Barra inferior: calendario, estadísticas y perfil | Calendario, Estadísticas y Perfil |
| Crear contenido | Siguiente, o el selector de pasos | El paso que toque |
| Fotos del local | Generar contenido | Contenido creado |
| Campaña propuesta | Crear pieza | Crear contenido |
| Campaña propuesta | Guardar o Descartar | Home |
| Campaña a medida | Crear plan | Campaña propuesta |
| Perfil | Cerrar sesión | Modal ¿Cerrar sesión? y, al confirmar, la bienvenida |
| Calendario | Una pieza | Contenido creado |

La flecha de volver de cada pantalla lleva a la anterior del recorrido, o a Home en las secciones.

## Qué hay que saber

- **Es un recorrido de diseño, no una app.** No hay datos reales ni teclado: los textos son los del ejemplo del local ficticio «Bar El Boquerón Dorado».
- **Las pantallas que no se alcanzan desde la bienvenida** son Sin oportunidades, Sin piezas este mes y Sin conexión: aparecen según los datos. Tienen su propio punto de inicio.
- **Supuestos del prototipo** que no están en el diagrama oficial:
  - Tocar el buscador de Busca tu local lleva al estado sin resultados, para poder verlo.
  - «Desconectar» en Perfil abre el aviso de que falta conectar Instagram.
  - Al publicar se llega al calendario, donde aparecería la pieza publicada, porque no hay pantalla de pieza publicada.
  - «Volver» en Así te ve internet lleva a Busca tu local, para cambiar de local.
- **Notificaciones y Estadísticas son P2:** solo tienen la flecha de volver.
- Las pestañas Crear cuenta e Iniciar sesión no cambian de formulario.

Cualquier cambio del diseño se hace en la página 1 de Figma y se rehace en el prototipo. Ver la decisión [0007](../decisiones/0007-diseno-definitivo.md).
