# Diagramas

Diagramas de arquitectura y de pantallas de Society, en formato draw.io.

## Contenido

- [`diagrama-flujo-app.drawio`](diagrama-flujo-app.drawio): visión general en 7 páginas. Mapa de pantallas, alta, funcionalidades, arquitectura por funcionalidad, datos, estados, y capas de los dos clientes con el despliegue.
- [`diagrama-pantallas-detalle.drawio`](diagrama-pantallas-detalle.drawio): una página por pantalla, con los datos que entran, se muestran y se guardan, y qué hace cada función en el backend y en las API.

## Cómo abrirlos

En [app.diagrams.net](https://app.diagrams.net), con **File › Open From › Device**. Cada página es una pestaña de la parte de abajo.

Se explican en la [arquitectura técnica](../tecnica/README.md).

## Decisiones citadas en los diagramas

Los diagramas citan decisiones con un identificador D-xx. Estas son las que aparecen:

| ID | Decisión |
|---|---|
| D-04 | Acceso con email y contraseña, sin confirmación de email en la demo |
| D-05 | Google Places solo desde el backend |
| D-06 | Se guardan los datos básicos del local confirmados por el dueño; valoración, reseñas y fotos, en vivo |
| D-07 | Imágenes con Nano Banana Pro en Higgsfield (comprobar que está en la API) |
| D-08 | Texto y visión con Gemini (`gemini-3.8-flash`), capa gratuita |
| D-09 | Instagram conectado con Instagram Login y publicación automática |
| D-11 | Sin cifras sin fuente en la Home (fuera «10 % menos de clientes») |
| D-13 | Un local por cuenta; `google_place_id` único |
| D-14 | Se muestran ★ y número de reseñas en la búsqueda |
| D-15 | Paginación manual con `nextPageToken` (máx. 60 resultados) |
| D-16 | Formatos: carrusel (3–5 imágenes 4:5) e historia (1–3 pantallas 9:16) |
| D-17 | Máximo 3 cambios por diapositiva |
| D-18 | Campañas: S11 en P0; S12 y calendario si sobra tiempo |
| D-19 | Sin «Avisarme cuando termine»: «No salgas de la aplicación» con tiempo restante estimado |
| D-23 | El panel web tiene las mismas funciones que la app |
| D-24 | Backend, panel web y contrato en el repositorio `Society-TypeScript` |
| D-26 | Trabajos largos en Vercel (ahora como rutas internas de la API) |
| D-27 | Panel web alojado en Vercel |
| D-28 | Cola de trabajos en una tabla de Postgres |
| D-30 | Tokens de Instagram cifrados con AES-256-GCM |
| D-31 | Instagram se conecta desde S04, S09P y Perfil |
| D-32 | Máximo 40 imágenes generadas al día por local |
| D-33 | Historias publicadas por la API (a comprobar con la cuenta de pruebas) |
| D-35 | Plantilla sin IA como respaldo de las imágenes |
| D-36 | Supabase activa el worker de Vercel (webhook, `pg_cron` y *webhook* de Higgsfield) |
| D-37 | Demo con un local ficticio e imágenes del equipo |
