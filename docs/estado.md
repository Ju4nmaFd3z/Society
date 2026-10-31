# Estado del proyecto

Última actualización: 31 de octubre de 2026.

## Hecho

- Equipo de tres personas: Juanma Fernández, Víctor Pagola y Pablo Jiménez, con rotación semanal de carriles (backend, web y Android).
- Arquitectura definida: dos clientes, la app Android y el panel web Angular, sobre un único backend Node.js con Express y Prisma. Ver [Arquitectura](arquitectura/README.md) y la decisión [0003](decisiones/0003-backend-unico-node.md).
- Diagramas de flujo general y de detalle de cada pantalla. Ver [Diagramas](arquitectura/diagramas/README.md).
- Planificación en Jira: épicas, historias de usuario, tareas y diez sprints semanales, uno por hito. La documentación del proyecto, centralizada en Notion. Ver la decisión [0004](decisiones/0004-jira-y-notion.md).
- Proyecto organizado en un repositorio por tecnología, más este repositorio general. Ver [Repositorios](arquitectura/repositorios.md).
- Identidad visual definida: logotipo, mascota y vídeo de identidad de 20 s. Se adopta la estética de cristal de Figma v3. Ver la decisión [0005](decisiones/0005-estetica-de-cristal.md).
- Perfil de Instagram creado: [@society.app](https://www.instagram.com/society.app/).
- Proyecto Android creado, con la estructura de carpetas y el esqueleto de la pantalla de búsqueda de locales, en [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin).
- Flujo de navegación y especificación de la pantalla [02. Empezamos](diseno/pantallas/02-empezamos.md).
- Navegación montada en Society-Kotlin, con una ruta por pantalla del diseño (15 en total) y los enlaces del [flujo](diseno/flujo-de-navegacion/README.md). Salvo [02. Empezamos](diseno/pantallas/02-empezamos.md), todas las pantallas son de cartón piedra.
- Notas de las 15 pantallas en [Pantallas](diseno/pantallas/README.md).
- Parte visual de la pantalla [02. Empezamos](diseno/pantallas/02-empezamos.md) en Society-Kotlin, sin funcionalidad real todavía.

## En curso

- Sprint 1 · Arranque (hitos H1 y H2), del 1 al 8 de noviembre: Supabase, API Express con Prisma, proyecto Angular con Tailwind y capa de red Android.
- Diseño de las pantallas de la app móvil en Figma: diseñadas de la 01 a la 07; las 08 a 15 tienen solo la cabecera. Ver [Diseño](diseno/README.md).
- Documentación de producto. Ver [Producto](producto/README.md).
- Funcionalidad de la pantalla 02. Empezamos: búsqueda de locales y lectura de los datos.
- Definición del contenido y los estados de las pantallas de cartón piedra.

## Pendiente

- Actualizar la arquitectura técnica y los diagramas a la API Express con Prisma (SOCIETY-128 en Jira).
- Implementar el modelo de datos con Prisma, sus migraciones y el seed.
- Elegir la licencia de los repositorios.
