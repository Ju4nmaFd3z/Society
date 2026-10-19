# Guía de estilo

Convenciones para que la documentación sea clara y homogénea, y para trabajar de forma ordenada entre dos personas en todos los repositorios del proyecto.

## Redacción

- La documentación se escribe en español.
- Una nota, un propósito. Si una nota empieza a tratar dos temas, se divide.
- Se documenta el porqué de las cosas más que cada paso. Lo justo, bien explicado.
- Cada nota tiene un único título de primer nivel, una introducción de una o dos líneas y secciones de segundo nivel.

## Archivos y carpetas

- Nombres en minúsculas, con guiones, sin espacios, tildes ni guiones bajos: `vision.md`, `modelo-de-datos.md`. Si hay un orden, se prefija con el número: `01-inicio.md`.
- Cada carpeta tiene un `README.md` que funciona como índice.
- Las imágenes van en [`assets/`](assets/README.md).

## Enlaces y formato

- Enlaces relativos en Markdown estándar dentro de un mismo repositorio: `[texto](ruta/archivo.md)`.
- Entre repositorios, enlaces absolutos a la rama `main`.
- Sin cabeceras de metadatos ni sintaxis propia de herramientas concretas, para que todo se lea igual en GitHub y en cualquier editor.

## Qué no se sube

- Claves de API, credenciales ni datos personales. Aunque se borren después, quedan en el historial.
- Enlaces de edición o de invitación a herramientas externas. Si hay que enlazar un diagrama, se usa un enlace de solo lectura.
- Vídeos ni archivos pesados. Si hace falta, se enlazan desde la documentación.

## Repositorios

El proyecto tiene un repositorio general y uno por tecnología. La documentación común vive solo en el general y cada repositorio de tecnología documenta su código. Ver el [mapa de repositorios](arquitectura/repositorios.md).

## Commits

En inglés, cortos y en minúscula. Por ejemplo: `add product vision`, `update project status`.

## Flujo de trabajo

Se aplica en todos los repositorios. Resumen de la decisión [0002](decisiones/0002-flujo-de-trabajo-con-ramas.md):

1. Cada autor trabaja en su rama: `juanma` y `victor`.
2. `main` contiene siempre la versión presentable del proyecto.
3. Los cambios llegan a `main` mediante pull request, revisada por la otra persona.
4. Antes de empezar a trabajar, se trae `main` a la rama propia para evitar conflictos.
5. Repartir las áreas de trabajo, por ejemplo una persona en arquitectura y otra en diseño, reduce los conflictos.
