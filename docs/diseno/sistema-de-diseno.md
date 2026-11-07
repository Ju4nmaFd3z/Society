# Sistema de diseño

Guía de estilos de la app Android y del panel web. Los valores son los de la página «2 · Sistema de diseño» del [archivo de Figma definitivo](https://www.figma.com/design/G4bwEgiP3eRRlNoHK4ftVA/Society--Final-Version-?node-id=2027-1844). Si algo de este documento no coincide con Figma, se corrige este documento.

## Principios

1. **El cristal vive sobre el grafiti.** Las superficies son translúcidas y dejan ver la pared de grafitis, que es la identidad de la marca. No se tapa con filtros de color.
2. **El texto no depende del fondo.** Todo texto va sobre una superficie de cristal o lleva un halo claro.
3. **Las etiquetas son editoriales.** La información se expresa con forma y tipografía, no con píldoras de colores. Un estado nunca se distingue solo por su color.

## Color

### Marca

| Nombre | Valor | Uso |
| --- | --- | --- |
| Azul Society | `#1033EF` | Color principal de la marca: logotipo, vídeo y grafiti |
| Amarillo Society | `#F5C518` | Acento y acciones |
| Tinta | `#151515` | Texto y contornos de la marca |
| Papel | `#F2F1EC` | Fondo principal |
| Papel cálido | `#F4F2EC` | Fondo alternativo |
| Piedra | `#E3E0D8` | Superficies y bordes |
| Gris texto | `#6F6B62` | Texto secundario de marca |

### Interfaz

| Nombre | Valor | Uso |
| --- | --- | --- |
| Azul | `#2440E0` | Controles activos |
| Amarillo | `#F5C518` | Acción principal |
| Tinta | `#16161A` | Texto |
| Texto secundario | `#2E2C36` | Apoyo y descripciones |
| Texto terciario | `#46434F` | Etiquetas y notas |
| Papel | `#F1ECE3` | Base de pantalla |
| Blanco | `#FFFFFF` | Superficies |
| Positivo | `#1F9D6B` | Conectado y mejoras |
| Alerta | `#D64545` | Acciones destructivas |
| Tinta violeta | `#221D3B` | Texto sobre azul |

El Azul Society es el de la marca. El azul de interfaz es el de los controles, calibrado para llevar texto blanco encima.

## Tipografía

DynaPuff Bold para títulos y cifras. Nunito para todo el texto de interfaz. El interlineado es automático.

| Estilo | Fuente | Tamaño |
| --- | --- | --- |
| Título/Grande 31 | DynaPuff Bold | 31 px |
| Título/Pantalla 24 | DynaPuff Bold | 24 px |
| Título/Cifra 34 | DynaPuff Bold | 34 px |
| Título/Cifra 22 | DynaPuff Bold | 22 px |
| Cuerpo/Intro 15 | Nunito Medium | 15 px |
| Cuerpo/Destacado 15 | Nunito ExtraBold | 15 px |
| Cuerpo/Destacado 13 | Nunito ExtraBold | 13 px |
| Cuerpo/Secundario 12.5 | Nunito SemiBold | 12,5 px |
| Cuerpo/Pequeño 11.5 | Nunito SemiBold | 11,5 px |
| Etiqueta/Sobretítulo 11.5 | Nunito ExtraBold, mayúsculas, espaciado del 6 % | 11,5 px |
| Chip/12.5 | Nunito Bold | 12,5 px |
| Botón/16 | Nunito ExtraBold | 16 px |

## Cristal

Tres niveles de translucidez. Cuanto más texto lleva una superficie, más densa es.

| Nivel | Uso | Relleno (blanco, de arriba abajo) | Desenfoque en Figma | Desenfoque en CSS (orientativo) |
| --- | --- | --- | --- | --- |
| Claro | Hojas y controles flotantes | 46 % a 28 % | 36 | `blur(18px)` |
| Estándar | Tarjetas sueltas sobre el fondo | 60 % a 42 % | 40 | `blur(20px)` |
| Denso | Modales y superficies con mucho texto | 78 % a 66 % | 44 | `blur(22px)` |

La equivalencia habitual es que el desenfoque de Figma sea el doble que el de CSS. Al implementarlo conviene compararlo a ojo con el diseño, porque no se ha medido.

Todos los niveles llevan lo mismo, además del relleno:

- **Filo especular:** trazo interior de 1,25 px con degradado vertical de blanco, del 95 % arriba al 28 % hacia la mitad y el 60 % abajo.
- **Brillo interior:** sombra interior blanca del 90 % desplazada 1,5 px hacia abajo, otra blanca del 35 % de 1 px de extensión y otra de tinta (`#0C1028`) del 6 %, a −1,5 px con 3 de desenfoque.
- **Sombras exteriores**, siempre con el color `#0C1028`. Las alfas son las del nivel Estándar y Denso. En el Claro son el 80 % de estas.

| Capa | Desplazamiento | Desenfoque | Extensión | Alfa |
| --- | --- | --- | --- | --- |
| Contacto | 0, 1 | 2 | −1 | 10 % |
| Ambiente | 0, 14 | 30 | −12 | 20 % |
| Profundidad | 0, 34 | 70 | −30 | 13 % |

## Elevación

| Estilo | Capas |
| --- | --- |
| Tarjeta (superficie blanca) | `0 1 2` al 6 %, `0 12 26 −10` al 14 % y `0 28 60 −26` al 10 %, en `#0C1028` |
| Botón amarillo | `0 3 6 −2` al 20 %, `0 14 30 −10` al 30 % y `0 32 70 −28` al 16 %, en ámbar `#BF8500`. Más una sombra interior ámbar oscura del 14 % y un brillo interior blanco del 65 % |

Ninguna sombra tiene borde duro: todas se difuminan por completo. En el código, un contenedor que recorte su contenido corta las sombras de sus hijos, así que solo se recorta cuando hay imágenes con esquinas redondeadas.

## Texto sobre el fondo

- **Texto sobre cristal:** va en tinta o en los textos secundario y terciario de la tabla anterior. Son más oscuros que los de la versión anterior (el terciario era `#5C5966`), porque sobre una mancha azul de grafiti translúcida el gris claro apenas se leía.
- **Texto directamente sobre el fondo:** si mide menos de 20 px, lleva un halo del color Papel de marca (`#F2F1EC`), de 3,2 px y por fuera de la letra.
- **Velo superior:** degradado del Papel de marca, de 200 px de alto, del 90 % arriba al 0 % abajo. Garantiza que el título se lea sobre la pared.

## Radios

| Elemento | Radio |
| --- | --- |
| Chip | 15 px |
| Tarjeta y campo | 20 px |
| Tarjeta destacada | 22 px |
| Botón | 27 px |
| Modal | 30 px |
| Barra de pestañas | 33 px |
| Hoja | 45 px |

Dentro de un contenedor, el radio interior es siempre menor que el exterior.

## Espacios

No hay una rejilla fija. Los valores que más se repiten en las pantallas son, para el relleno, 14, 16 y 12 px, y para la separación entre elementos, 12, 6, 8 y 10 px.

## Etiquetas

No se usan píldoras de colores. Hay cuatro familias.

| Familia | Qué expresa | Cómo se muestra |
| --- | --- | --- |
| Categoría | Producto, Experiencia, Novedad, Participación, Evento, Fidelización | Icono propio y rótulo en mayúsculas de 10,5 px, sin contenedor |
| Confianza | Observado, Estimado, Hipótesis | Medidor de tres barras (tres, dos o una rellenas) y la palabra |
| Estado | Prevista, Publicada, Generada, Borrador | Icono con forma distinta (reloj, verificación o lápiz) y la palabra |
| Origen | Google, Tu web, IA, Tú | Marca pequeña de la fuente y la palabra, en el texto terciario |

Los avisos llevan un rótulo en mayúsculas con un trazo amarillo de rotulador detrás de la mitad inferior de las letras, y debajo el texto. Sustituyen a los recuadros tintados con borde.

Las etiquetas sobre fotos son una insignia: oscura (tinta al 78 %, texto en blanco) para los avisos, y blanca al 90 % para el estado.

## Iconos

Trazo de 1,8 px, extremos redondos y rejilla de 24. Son propios los de contenido (carrusel e historia) y los de las categorías. El catálogo completo, de 35 iconos, está en la página «2 · Sistema de diseño» de Figma.

## Componentes

| Componente | Descripción |
| --- | --- |
| Botón primario | 54 px de alto, radio 27, amarillo de interfaz y texto Botón/16 en tinta, con la elevación del botón amarillo |
| Botón secundario | Cristal claro con filo especular |
| Chip activo | Azul de interfaz con texto blanco, radio 15 |
| Chip neutro | Cristal con filo especular, radio 15 |
| Interruptor | Dos estados: activado y desactivado |
| Campo de texto | Blanco al 84 %, radio 20, con la elevación de tarjeta |
| Barra de pestañas | Cristal, radio 33, con el botón central amarillo |

## Cambios respecto a la hoja de estilos de la versión anterior

Para quien implemente el tema (SOCIETY-33 en la web y SOCIETY-129 en Android).

| Elemento | Antes (versión 3) | Ahora |
| --- | --- | --- |
| Texto secundario | `#5C5966` | `#2E2C36` y, para etiquetas y notas, `#46434F` |
| Papel claro | `#FBF8F2` | No es un color del sistema de diseño |
| Colores | Papel, tinta, azul y amarillo | Además, la paleta completa de marca, el blanco, el positivo, la alerta y la tinta violeta |
| Cristal | Dos niveles: hoja (70 % a 62 %) y tarjeta (de gris al 35 % a blanco al 51 %) | Tres niveles de blanco: claro, estándar y denso |
| Filo del cristal | Brillo interior fijo | Trazo de 1,25 px con degradado vertical |
| Sombra del cristal | `0 18 36` al 42 % | Tres capas muy difuminadas |
| Sombra del botón amarillo | `0 10 9` al 70 %, un borde casi duro | Tres capas ámbar muy difuminadas |
| Radios | Hoja 45, tarjeta 20, botón 27 y chip 15 | Los mismos, más modal 30, barra de pestañas 33 y tarjeta destacada 22 |
| Texto sobre el fondo | Sin tratamiento | Halo de 3,2 px en el Papel de marca |
| Etiquetas | Píldoras de colores | Las cuatro familias editoriales de este documento |
