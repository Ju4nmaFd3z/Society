# Estado del proyecto

Última actualización: 19 de noviembre de 2026.

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
- Panel web con Tailwind CSS: el tema de cristal del diseño definitivo, los colores y las fuentes de la marca, y el layout del panel con barra lateral, cabecera y páginas vacías. Código en [Society-TypeScript](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/web).
- Componentes reutilizables del panel web (botones, campos, casilla, chips, etiquetas, tarjetas de cristal y estados cargando, vacío y error), con una página de [catálogo](https://society-web-gamma.vercel.app/catalogo) (hito H3).
- Router del panel web con todas las rutas, los guards de sesión y de alta, y la pantalla [01A. Acceso](diseno/pantallas/01a-acceso.md) con validaciones (hito H3). De momento la sesión es simulada y, salvo el acceso, las vistas son páginas vacías.
- Contrato OpenAPI v0.2.0 con `/salud`, `/auth`, `/restaurantes`, `/analisis` y las rutas de Places, incluido el error de credenciales incorrectas. Código en [Society-TypeScript](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/contrato).
- Autenticación en la API con Supabase Auth: registro, acceso y renovación de la sesión (`/auth/registro`, `/auth/acceso` y `/auth/refresco`), y el *middleware* `requiereAuth` que protege las rutas privadas. Ver [API propia](arquitectura/tecnica/api-propia.md). Código en [Society-TypeScript](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/api).
- Búsqueda de locales en la API, `GET /places/search`, protegida por token: llama a Google Places y devuelve los campos de la tarjeta, paginados con `pageToken` y con hasta 5 fotos por local, cada una con sus autores. `GET /places/photo` devuelve la URL de una foto. Ver la decisión [0008](decisiones/0008-atribuciones-de-fotos-en-la-busqueda.md).
- Capa de red de Android con Retrofit, Gson y Hilt en [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin): una `SocietyApi`, un DataSource y un repositorio, con la URL de la API en `API_BASE`. Al arrancar, la app llama a `/salud` y escribe el resultado en el log.
- Fuentes de la marca en Android: DynaPuff y Nunito, aplicadas en `Type.kt`.
- Tema de cristal de Android: los colores de la interfaz, el esquema de Material, `TarjetaCristal` y `BotonCristal`, en [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin). Todavía no los usa ninguna pantalla.
- ERP Odoo del Proyecto Intermodular con los datos ficticios de Society y los cuatro bloques del curso cubiertos: operaciones (inventario, compras, contabilidad, empleados y proyectos), ventas (CRM, ventas, facturación, TPV, firma electrónica y alquiler), productividad (conversaciones, marketing social y por correo, partes de horas, documentos, planificación, ausencias y reclutamiento) y sitios web (sitio web, comercio electrónico, chat en vivo, blog, foro, eventos, eLearning y encuestas). Ver [Odoo](odoo.md).
- Prueba de Nano Banana Pro en la API de Higgsfield.
- Sprint 1 · Arranque (hitos H1 y H2), cerrado el 12 de noviembre, con la primera rotación de carriles preparada.

## En curso

- Sprint 2 · Acceso (hito H3), del 12 al 15 de noviembre: acceso y sesión en la app y en la web, router y componentes de Angular, autenticación en la API y búsqueda con Places. Carriles: Víctor en el backend, Pablo en Android y Juanma en la web. El carril web está terminado. En el backend queda `GET /restaurantes/mio`, y en Android, S01 y S01A con la API y el arranque con `AuthInterceptor`.
- Documentación de producto. Ver [Producto](producto/README.md).

## Pendiente

- Completar el modelo de datos con Prisma (el resto de tablas) y el seed con el local ficticio de la demo.
- Confirmar en equipo dos decisiones de datos: un restaurante por usuario (D-13) y que `perfil_usuario` lo cree la API al registrarse (el registro ya lo hace así).
- Cerrar en la API (`POST /auth/registro`) el registro de cuentas nuevas. En Supabase ya está cerrado desde el 16 de noviembre. Ver [Supabase Auth](arquitectura/tecnica/secretos-y-despliegue.md#supabase-auth).
- Elegir la licencia de los repositorios.
