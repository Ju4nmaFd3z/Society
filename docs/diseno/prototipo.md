# Prototipo

Recorrido navegable de la app en Figma, con las pantallas, los modales y los estados conectados entre sí.

## Cómo abrirlo

Está en el [archivo de Figma definitivo](https://www.figma.com/proto/G4bwEgiP3eRRlNoHK4ftVA/Society--Final-Version-?node-id=2027-50&starting-point-node-id=2027%3A50&scaling=scale-down&content-scaling=fixed), página «1 · Presentación». Se abre en la bienvenida (S01). Con el enlace de solo lectura basta para recorrerlo; no hace falta tener cuenta de edición.

Para verlo como en un móvil, en el modo presentación de Figma conviene elegir un teléfono de 390 × 844. Las pantallas más largas (S04, S05, S06, S07 y S11) se desplazan hacia abajo.

## Puntos de inicio

| Punto de inicio | Pantalla | Para qué sirve |
| --- | --- | --- |
| Recorrido completo (desde la bienvenida) | S01 | Todo el flujo del MVP |
| Home | S05 | Empezar con el local ya dado de alta |
| Crear contenido | S06 | Probar la creación y la publicación |
| Campaña propuesta | S11 | Ver el detalle de una oportunidad |
| Estado: sin resultados | S02 | Ver el buscador sin resultados |
| Estado: sin oportunidades | S05 | Home de un local sin oportunidades |
| Estado: sin piezas este mes | S15 | Calendario vacío |
| Error: sin conexión | Error | Pantalla de error con Reintentar |

## Conexiones

Siguen el [mapa general de navegación](flujo-de-navegacion/README.md) del propio archivo.

| Desde | Acción | Va a |
| --- | --- | --- |
| S01 | Descubre cómo funciona | S01A |
| S01A | Continuar | S02 |
| S02 | Es mi local | M02 y, al confirmar, S03 |
| S02 | ¿No aparece tu local? Añádelo a mano | S02M |
| S02M | Guardar y analizar | S03 |
| S03 | A los 4 segundos | S04 |
| S03 | Flecha atrás | M03 (seguir esperando vuelve a S03; cancelar, a S02) |
| S04 | Todo correcto, seguir | S05 |
| S05 | Campana | S10 |
| S05 | Tarjeta de una oportunidad | S11 |
| S05 | Contenido desde una idea, o el botón «+» | S06 |
| S05 | Campaña a tu medida | S12 |
| S05 | Una pieza reciente | S09 |
| S05 | Barra inferior: calendario, estadísticas y perfil | S15, S14 y S13 |
| S06, S07 y S08 | Siguiente, o el selector de pasos | El paso que toque |
| S08 | Generar contenido | S09 |
| S09 | Descripción y hashtags | S09P |
| S09P | Publicar en Instagram | S15 |
| S11 | Crear pieza | S06 |
| S11 | Guardar o Descartar | S05 |
| S12 | Crear plan | S11 |
| S13 | Cerrar sesión | M13 y, al confirmar, S01 |
| S15 | Una pieza | S09 |

La flecha de volver de cada pantalla lleva a la anterior del recorrido, o a S05 en las secciones de la Home.

## Qué hay que saber

- **Es un recorrido de diseño, no una app.** No hay datos reales ni teclado: los textos son los del ejemplo del local ficticio «Bar El Boquerón Dorado».
- **Las pantallas que no se alcanzan desde la bienvenida** son Sin oportunidades, Sin piezas este mes y Error: aparecen según los datos. Tienen su propio punto de inicio.
- **Supuestos del prototipo** que no están en el diagrama oficial:
  - Tocar el buscador de S02 lleva al estado sin resultados, para poder verlo.
  - «Desconectar» en S13 lleva a M09P, el aviso de que falta conectar Instagram.
  - «Publicar en Instagram» en S09P lleva al calendario, donde aparecería la pieza publicada, porque no hay pantalla de pieza publicada.
  - «Volver» en S04 lleva a S02, para cambiar de local.
- **S10 y S14 son P2:** solo tienen la flecha de volver.
- Las pestañas Crear cuenta e Iniciar sesión de S01A no cambian de formulario.

Cualquier cambio del diseño se hace en Figma y se refleja aquí. Ver la decisión [0007](../decisiones/0007-diseno-definitivo.md).
