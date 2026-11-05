# 09P. Publicar contenido

Última pantalla de la creación de contenido: el pie, los hashtags y la publicación en Instagram. No tiene número en Figma.

## Objetivo

Que el usuario revise cómo se publicará su contenido y lo publique en Instagram, o lo haga él.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Detalle: página «11 · S09P · Publicar» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio). Decisiones D-09, D-16 y D-33.

## Contenido

- **Miniaturas** de las diapositivas.
- **Cuenta de destino:** el usuario de Instagram conectado y su estado.
- **Pie de página:** de 1 a 2.200 caracteres. No se usa en las historias.
- **Hashtags:** de 0 a 5, cada uno con `#` y de 1 a 30 letras, números o guiones bajos. Tampoco se usan en las historias.
- **Texto alternativo:** hasta 1.000 caracteres.
- **Etiqueta «Contenido creado con IA».**
- **Botón «Publicar en Instagram»:** en las historias se llama «Publicar».
- **Enlace «Prefiero publicarlo yo»:** respaldo manual. Descarga la pieza, copia el texto y abre Instagram. Después se pulsa «Ya lo he publicado».

## Estados

Editando, guardando, necesita Instagram, publicando, publicada, error de publicación y compartiendo.

Si no hay Instagram conectado aparece el modal «Conecta tu Instagram para publicar». Ver [Modales y estados](modales-y-estados.md).

Errores posibles: Instagram no conectado o caducado, publicación fallida y validación del pie o los hashtags.

## Navegación

Se llega desde [09. Contenido creado](09-contenido-creado.md). Al publicar, la pantalla muestra «¡Publicada!» con un enlace «Ver en Instagram» y se vuelve a [05. Home](05-home.md). Conectar Instagram sale a Instagram y vuelve a esta pantalla.

## Implementación

- Android: pendiente. Issue SOCIETY-95.
- Web: pendiente. Issue SOCIETY-97.
