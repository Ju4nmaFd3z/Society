# Estado del proyecto

Última actualización: 7 de noviembre de 2026.

## Hecho

- Equipo de tres personas: Juanma Fernández, Víctor Pagola y Pablo Jiménez, con rotación semanal de carriles (backend, web y Android).
- Arquitectura definida: dos clientes, la app Android y el panel web Angular, sobre un único backend Node.js con Express y Prisma. Ver [Arquitectura](arquitectura/README.md) y la decisión [0003](decisiones/0003-backend-unico-node.md).
- Diagramas de flujo general y de detalle de cada pantalla. Ver [Diagramas](arquitectura/diagramas/README.md).
- Planificación en Jira: épicas, historias de usuario, tareas y diez sprints semanales, uno por hito. La documentación del proyecto, centralizada en Notion. Ver la decisión [0004](decisiones/0004-jira-y-notion.md).
- Proyecto organizado en un repositorio por tecnología, más este repositorio general. Ver [Repositorios](arquitectura/repositorios.md).
- Identidad visual definida: logotipo, mascota y vídeo de identidad de 20 s. Se adopta la estética de cristal. Ver la decisión [0005](decisiones/0005-estetica-de-cristal.md).
- Perfil de Instagram creado: [@society.app](https://www.instagram.com/society.app/).
- Proyecto Android creado, con la estructura de carpetas y el esqueleto de la pantalla de búsqueda de locales, en [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin).
- Flujo de navegación y especificación de la pantalla [02. Empezamos](diseno/pantallas/02-empezamos.md).
- Navegación montada en Society-Kotlin, con una ruta por pantalla del diseño (15 en total) y los enlaces del [flujo](diseno/flujo-de-navegacion/README.md). Salvo [02. Empezamos](diseno/pantallas/02-empezamos.md), todas las pantallas son de cartón piedra.
- Notas de las 15 pantallas en [Pantallas](diseno/pantallas/README.md).
- Pantalla [02. Empezamos](diseno/pantallas/02-empezamos.md) en Society-Kotlin, funcional con datos de ejemplo: busca en los locales de prueba y muestra sus cinco estados.
- Proyecto de Supabase creado en Frankfurt, con todo el equipo invitado.
- API Express con TypeScript desplegada en Vercel, organizada en rutas, controladores y servicios, con un formato de error común. Responde en [`/api/v1/salud`](https://society-api-xi.vercel.app/api/v1/salud) (hito H1). Código en [Society-TypeScript](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/api).
- Prisma conectado a Supabase, con la primera migración (`perfil_usuario` y `restaurante`) y RLS en todas las tablas.
- Proyecto Angular creado en Society-TypeScript (`web/`) y primera versión del contrato OpenAPI, con las rutas de Places.
- [Arquitectura técnica](arquitectura/tecnica/README.md) y [diagramas](arquitectura/diagramas/README.md) actualizados a la API Express con Prisma.
- Panel web desplegado en [society-web-gamma.vercel.app](https://society-web-gamma.vercel.app), y API con sus variables de entorno cargadas en Vercel (hito H1).
- Regla de una rama por issue de Jira en todos los repositorios. Ver la decisión [0006](decisiones/0006-una-rama-por-issue-de-jira.md).
- Esqueleto de Android con una carpeta por pantalla y la navegación de las pantallas 02 y 03 en [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin).
- Diseño visual de la app terminado en el [archivo de Figma definitivo](diseno/README.md), con su [sistema de diseño](diseno/sistema-de-diseno.md) y la decisión [0007](decisiones/0007-diseno-definitivo.md). El contenido y los estados de todas las pantallas están en [Pantallas](diseno/pantallas/README.md).
- [Prototipo](diseno/prototipo.md) navegable en Figma para iPhone 16 Pro, con scroll real, modales como pantallas, secuencias animadas (análisis, generación y publicación) y todas las pantallas y estados conectados. Revisión final del archivo de diseño: sin restos del diseño anterior y con los datos de ejemplo coherentes en todo el recorrido.
- Prueba de Nano Banana Pro en la API de Higgsfield.

## En curso

- Sprint 1 · Arranque (hitos H1 y H2), del 1 al 8 de noviembre. Quedan: Tailwind en el panel web, capa de red Android y contrato OpenAPI de `/auth` y `/restaurantes`.
- Documentación de producto. Ver [Producto](producto/README.md).

## Pendiente

- Completar el modelo de datos con Prisma (el resto de tablas) y el seed con el local ficticio de la demo.
- Confirmar en equipo dos decisiones de datos: un restaurante por usuario (D-13) y que `perfil_usuario` lo cree la API al registrarse.
- Elegir la licencia de los repositorios.
