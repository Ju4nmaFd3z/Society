# Arquitectura de SOCIETY

Este documento y los dos diagramas de la carpeta [`diagramas/`](diagramas/README.md) fijan la arquitectura del TFG. Actualizado el 02/11/2026: el backend es una API Express con Prisma en Vercel, según la decisión [0003](../decisiones/0003-backend-unico-node.md).

> **Diagramas pendientes de actualizar.** Este documento ya describe la API Express con Prisma, pero los diagramas todavía muestran la API como una Supabase Edge Function. Su actualización sigue en la tarea SOCIETY-128 de Jira.

| Archivo | Qué contiene | Cuándo mirarlo |
|---|---|---|
| `arquitectura-tecnica.md` (este) | Servicios, tecnologías, lista de APIs, API propia, datos y despliegue | Primero, para tener el mapa completo |
| [`diagramas/diagrama-flujo-app.drawio`](diagramas/diagrama-flujo-app.drawio) | Visión general en 7 páginas: mapa de pantallas, alta, funcionalidades, arquitectura por funcionalidad, datos, estados, y capas de los dos clientes con el despliegue | Para entender cómo encaja todo (páginas 1, 4 y 7 como mínimo) |
| [`diagramas/diagrama-pantallas-detalle.drawio`](diagramas/diagrama-pantallas-detalle.drawio) | Una página por pantalla: datos que entran, se muestran y se guardan; flujo con sus funciones F1, F2…; qué hace cada función en el backend y en las APIs | Al programar o revisar una pantalla concreta |

Los diagramas se abren en [app.diagrams.net](https://app.diagrams.net) con **File › Open From › Device**. Cada página es una pestaña de la parte de abajo.

---

## 1. Qué se construye

SOCIETY tiene **dos clientes con las mismas funciones** que comparten **un único backend**:

- **App Android** (Kotlin): parte Android del TFG.
- **Panel web** (Angular): parte TypeScript del TFG, con composición de escritorio (barra lateral).
- **API REST** (Express + Prisma): backend compartido, también parte TypeScript del TFG.

Los dos clientes permiten dar de alta el local, analizarlo, crear carruseles e historias con fotos reales, publicarlos en Instagram y gestionar oportunidades. Lo que se hace en uno aparece en el otro.

```
   App Android (Kotlin)                     Panel web (Angular + Tailwind, en Vercel)
   Retrofit + Gson · OkHttp                 HttpClient + interceptores
          │                                        │
          └──────── HTTPS + JWT · /api/v1 ─────────┘
                             ▼
 ┌──────────────── API Express + Prisma (TypeScript, Vercel) ─────────────────┐
 │ rutas → controladores → servicios · middlewares (auth, errores)             │
 │ worker: rutas internas /trabajos (las llama Supabase) y webhook Higgsfield  │
 └───────┬──────────────────────────────┬─────────────────────────────┬───────┘
         │ Prisma                       │ supabase-js (servidor)      │
         ▼                              ▼                             ▼
 ┌──────────── Supabase ─────────────────────────────┐   Google Places · web del local
 │ Postgres (tablas de negocio, cola «trabajo»)      │   Gemini (texto y visión)
 │ Auth (email + contraseña) · Storage (fotos)       │   Higgsfield · Nano Banana Pro
 │ Database Webhook + pg_cron → /trabajos            │   Instagram (OAuth y publicar)
 └───────────────────────────────────────────────────┘
   Fotos: el cliente sube con PUT a una URL firmada que le da la API
```

---

## 2. Servicios y tecnologías

| Pieza | Tecnología | Para qué | Plan |
|---|---|---|---|
| **API propia** | **Express 5** + TypeScript en **Vercel** (plan Hobby). Carpetas `routes/` → `controllers/` → `services/`, más `middlewares/` | Todo lo que piden los clientes: auth, CRUD, búsqueda en Places, crear trabajos, OAuth de Instagram | Gratuito |
| **ORM** | **Prisma 7** (`schema.prisma`, migraciones y `seed`) sobre el Postgres de Supabase | Modelo de datos, relaciones, migraciones, consultas y transacciones | — |
| **Base de datos** | Supabase · Postgres (región Frankfurt) | Todos los datos de negocio de los dos clientes y la cola de trabajos | Gratuito |
| **Cuentas** | Supabase Auth (email + contraseña), **usado solo desde la API** | Registro, acceso, JWT de acceso y de refresco | Gratuito |
| **Archivos** | Supabase Storage (buckets privados `recursos`, `referencias`, `generadas`) | Fotos del local, referencias, imágenes generadas | Gratuito |
| **Trabajos largos (worker)** | Rutas internas `/trabajos` de la **misma API** en Vercel | Análisis, plan con IA, imágenes, publicar en Instagram, renovar tokens | Gratuito |
| **Activación de trabajos** | Supabase **Database Webhooks**, `pg_cron` y `pg_net` | Avisar al worker al crear un trabajo y hacer un barrido cada minuto | Gratuito |
| **App Android** | Kotlin, Jetpack Compose, Material 3, Navigation Compose, Hilt, **Retrofit + Gson** + OkHttp, Coil, DataStore, Custom Tabs | Cliente móvil | — |
| **Panel web** | Angular (componentes *standalone* y *signals*), RxJS, `HttpClient` con interceptores, **Tailwind CSS**, Angular CDK; alojado en **Vercel** | Cliente de escritorio | Gratuito |
| **Contrato de la API** | OpenAPI 3.1 (`contrato/openapi.yaml`) | Una sola definición para Android y Angular | — |

**Presupuesto:** 0 € en servicios, salvo el saldo de Higgsfield, que paga el equipo. Todo es para pruebas del equipo. La demo usa un **local ficticio** con imágenes propias.

### Repositorios

| Repositorio | Contenido |
|---|---|
| `Society-Kotlin` | App Android |
| `Society-TypeScript` | `contrato/` (OpenAPI) · `api/` (Express + Prisma + worker, un proyecto de Vercel) · `web/` (panel Angular, otro proyecto de Vercel) |
| `Society` | Documentación (`docs/arquitectura`) y registro de decisiones |

---

## 3. Reglas de comunicación

1. Los clientes **solo hablan con la API propia**, también para registrarse y entrar. La única excepción es **subir fotos**: la API devuelve una URL de subida firmada y el cliente hace `PUT` directo a Storage, así el archivo no pasa por Vercel (que limita el cuerpo a 4,5 MB).
2. Cada llamada a la API lleva `Authorization: Bearer <JWT>`. Las excepciones son `/salud`, `/auth/registro`, `/auth/acceso`, `/auth/refresco` y `GET /instagram/callback` (protegida con un `state` de un solo uso).
3. **Ningún secreto en los clientes.** La app y el panel solo llevan la URL de la API. Android guarda los tokens en DataStore y la web en `localStorage`.
4. Las operaciones largas responden **202** con un ID. El cliente consulta el estado cada 3 s y muestra «No salgas de la aplicación» con el tiempo restante estimado.
5. Errores: código HTTP + `{"error": {"code", "message", "retryable"}}`. Los GET se reintentan hasta 3 veces; los POST nunca de forma automática. Los códigos están en el [apartado 5](#5-api-propia).
6. Ningún cambio de la API sin cambiar antes `openapi.yaml`, en una *pull request* que revisen la parte Android y la parte Angular.
7. El contrato (`openapi.yaml`) documenta **nuestras** rutas (`/api/v1/...`), no las de Google, Gemini, Higgsfield o Instagram. Por ejemplo, la búsqueda se documenta como `GET /places/search` con nuestra respuesta; la llamada a Google Places va solo en la descripción de esa ruta.

### Cómo se ejecutan los trabajos largos en Vercel

Vercel (plan Hobby) no mantiene procesos encendidos: cada función dura **como máximo 300 s**, y sus tareas programadas solo pueden ejecutarse una vez al día. Por eso es Supabase quien avisa al worker, que son rutas internas de la misma API. Estas rutas comprueban la cabecera `X-Trabajos-Secreto`.

| Disparador | Cuándo | Llama a |
|---|---|---|
| *Database Webhook* de Supabase | Cada vez que se inserta un trabajo | `POST /api/v1/trabajos/ejecutar` |
| `pg_cron` + `pg_net` | Cada minuto | `POST /api/v1/trabajos/barrido`: reintentos, esperas vencidas y trabajos colgados |
| `pg_cron` | Una vez al día | Encola la renovación de tokens de Instagram |
| *Webhook* de Higgsfield | Cuando termina una imagen | `POST /api/v1/webhooks/higgsfield` (firma HMAC) |

Ninguna función espera dentro de su ejecución: los trabajos largos se dividen en **fases**, cada una se guarda en la tabla `trabajo` y se retoma en la siguiente llamada. La función SQL que reclama el siguiente trabajo (con `FOR UPDATE SKIP LOCKED`) y la programación de `pg_cron` van en una migración de Prisma escrita a mano.

| Trabajo | Qué hace | Fases |
|---|---|---|
| `ANALISIS` | Google Places · web del local · Instagram (P1) · colores y letras · ficha y oportunidades con Gemini | Una por paso (5) |
| `PLAN_PIEZA` | Gemini crea el plan: diapositivas, textos, prompt por imagen, pie, hashtags, texto alternativo | 1 |
| `IMAGEN_DIAPOSITIVA` | Imagen de cada diapositiva con Higgsfield a partir de la foto real; JPEG 1080×1350 (carrusel) o 1080×1920 (historia) | Enviar → esperar el *webhook* → guardar |
| `PUBLICAR_INSTAGRAM` | Carrusel o historia en la cuenta conectada | Crear contenedores → esperar procesado → publicar |
| `RENOVAR_TOKENS_IG` | Renueva los tokens que caducan en menos de 10 días | 1 |

---

## 4. APIs externas

| API | Para qué | Quién la llama | Llamadas principales | Autenticación | Coste y plan | Estado |
|---|---|---|---|---|---|---|
| **Google Places API (New)** | Buscar el local, leer sus datos públicos y su foto | API (búsqueda, fotos) y worker (detalles) | `POST places.googleapis.com/v1/places:searchText` · `GET /v1/places/{id}` · `GET /v1/{photo}/media` | Clave de API en el servidor (`X-Goog-Api-Key`) y `X-Goog-FieldMask` obligatoria | Gratis hasta 1.000 llamadas al mes por tipo (Enterprise); **exige cuenta de facturación con tarjeta** | Aprobada |
| **Gemini API** (`gemini-3.8-flash`) | Análisis, ficha de marca, oportunidades, plan de cada pieza, pie y hashtags (texto y visión) | Worker | SDK `@google/genai`, salida JSON con esquema | Clave de API en el servidor | Capa gratuita, solo para pruebas del equipo | Aprobada |
| **Higgsfield API** (Nano Banana Pro) | Imagen de cada diapositiva a partir de la foto real y una referencia | Worker | `POST https://api.higgsfield.ai/<modelo>` · `GET /requests/{id}/status` · `DELETE /requests/{id}` · *webhook* · `POST /estimate/<modelo>` | `Authorization: Key ID:SECRET` en el servidor | Prepago (lo paga el equipo). No cobra los fallos ni las imágenes bloqueadas | Aprobada con condición: comprobar que la cuenta tiene Nano Banana Pro por API |
| **Instagram API with Instagram Login** (Meta) | Conectar la cuenta profesional y publicar carruseles e historias | API (OAuth) y worker (publicar, renovar) | `instagram.com/oauth/authorize` · `POST api.instagram.com/oauth/access_token` · `GET graph.instagram.com/access_token` · `POST /{ig-id}/media` · `POST /{ig-id}/media_publish` · `GET refresh_access_token` | OAuth del propietario; token de 60 días cifrado en el servidor | Gratis. Con acceso estándar, solo cuentas con rol en la app (cuenta de pruebas del equipo) | Aprobada |
| **Web del local** (HTTP GET) | Texto, enlaces a redes, colores y letras de la web del local | Worker | `GET` a la URL (10 s, 2 MB, respetando `robots.txt`) | — | Gratis | Aprobada |
| **Supabase Auth y Storage** | Cuentas y URL firmadas de subida y descarga | Solo la API (supabase-js en el servidor) | `signUp` · `signInWithPassword` · `refreshSession` · `createSignedUploadUrl` · `createSignedUrl` | Clave secreta (`sb_secret_…`) en el servidor | Gratuito | Aprobada |

**Respaldo sin IA de imagen:** si Higgsfield falla o no hay saldo, el worker compone la diapositiva con una **plantilla**: la foto real recortada, el texto y los colores de la ficha, con `sharp`.

**Descartadas:**
- Google Business Profile: el foco es Instagram.
- TikTok.
- *Scraping* de Google, Instagram o TikTok.
- Claude: se eligió Gemini por su capa gratuita.
- Imagen con Gemini (`gemini-3-pro-image`): no tiene capa gratuita.
- Firebase Cloud Messaging: no hay notificaciones en el MVP.
- Supabase Edge Functions y SDK de Supabase en los clientes: sustituidos por la API Express (decisión [0003](../decisiones/0003-backend-unico-node.md)).

---

## 5. API propia

**Base:** `https://society-api-xi.vercel.app/api/v1` · JSON en camelCase · fechas ISO 8601 en UTC. El código está en [`Society-TypeScript/api`](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/api).

| Método y ruta | Pantallas | Devuelve |
|---|---|---|
| `GET /salud` | — | `{ok: true}`. Comprueba que la API está en marcha. **Hecha** |
| `POST /auth/registro` · `POST /auth/acceso` · `POST /auth/refresco` · `POST /auth/salir` · `POST /auth/recuperar` [P1] | S01A, S13 | `{accessToken, refreshToken, expiraEn}` · 401 · 409 (email ya registrado) · 429 |
| `GET /perfil` | S13 | email y términos aceptados |
| `GET /restaurantes/mio` | Arranque, S01A | `{restaurante \| null, altaEstado, analisisEnCursoId?}` |
| `GET /places/search?q=&pageToken=` | S02 | `{resultados[], nextPageToken?}` (no se guarda) |
| `GET /places/photo?name=&maxWidthPx=` | S02 | `{photoUri, atribuciones[]}` |
| `POST /restaurantes` | S02, S02M | 201 · 409 `DUPLICADO` |
| `POST /restaurantes/{id}/analisis` | S02, S02M, S03 | 202 `{analisisId}` |
| `GET /analisis/{id}` · `POST /analisis/{id}/cancelar` | S03 | estado, pasos y `tiempoRestanteSeg` |
| `GET /restaurantes/{id}/ficha` · `PATCH /restaurantes/{id}/ficha` | S04, S07 | ficha con origen por campo y `version` |
| `GET /restaurantes/{id}/resumen-google` | S04, S05 | valoración y reseñas en vivo |
| `GET /restaurantes/{id}/oportunidades` · `GET /oportunidades/{id}` · `PATCH /oportunidades/{id}` | S05, S11 | oportunidades |
| `POST /campanas` · `GET /campanas` · `GET /campanas/{id}` | S11, S06, S12 | campaña + idea sugerida |
| `GET /referencias-esteticas` · `POST /referencias-esteticas` | S05, S07 | catálogo y referencias propias |
| `POST /recursos/subida` | S07, S08 | `{urlSubida, storagePath}` (URL firmada de 10 min) |
| `GET /recursos` · `POST /recursos` | S08 | fotos del local |
| `POST /piezas` · `GET /piezas` · `GET /piezas/{id}` · `PATCH /piezas/{id}` | S05, S08, S09, S09P, S15 | pieza con diapositivas y `tiempoRestanteSeg` |
| `POST /piezas/{id}/generar` | S08 | 202 |
| `POST /piezas/{id}/diapositivas/{n}/regenerar` | S09 | 202 · 429 (3 cambios por diapositiva o 40 imágenes al día) |
| `POST /piezas/{id}/publicar-instagram` · `POST /piezas/{id}/marcar-publicada` | S09P | 202 · 409 si Instagram no está conectado |
| `POST /instagram/conexiones` · `GET /instagram/callback` · `GET /instagram/cuenta` · `DELETE /instagram/cuenta` | S04, S09P, S13 | URL de autorización · redirección · estado de la cuenta |

**Listados** (`GET /piezas`, `GET /recursos`, `GET /campanas`): búsqueda, filtros, orden y paginación en el backend con Prisma. Ejemplo: `GET /piezas?q=&estado=&formato=&campanaId=&orden=creada_desc&pagina=1&tamano=20` → `{datos[], total, pagina, tamano}`.

**Transacciones** (`prisma.$transaction`): crear una pieza con sus fotos y su trabajo, y pasar una pieza a PUBLICANDO con su trabajo.

Rutas internas (las llaman Supabase o Higgsfield, nunca los clientes): `POST /trabajos/ejecutar`, `POST /trabajos/barrido`, `POST /webhooks/higgsfield`.

### Errores

Todas las respuestas de error tienen el formato `{"error": {"code", "message", "retryable"}}`. Los códigos son los mismos que traducen Android y la web a `ErrorSociety`:

| `code` | HTTP | Cuándo |
|---|---|---|
| `VALIDACION` | 400 | Datos de entrada no válidos o JSON mal formado |
| `SESION_CADUCADA` | 401 | JWT ausente, caducado o no válido |
| `NO_ENCONTRADO` | 404 | No existe el recurso o la ruta |
| `DUPLICADO` | 409 | El recurso ya existe (por ejemplo, el local ya está dado de alta) |
| `LIMITE_ALCANZADO` | 429 | Demasiadas peticiones o límite de cambios o imágenes |
| `MODERACION` | 422 | La IA ha bloqueado el contenido |
| `PROVEEDOR_NO_DISPONIBLE` | 503 | Google, Gemini, Higgsfield o Instagram no responden (`retryable: true`) |
| `INTERNO` | 500 | Error inesperado de la API; no se dan detalles al cliente |

En el código, cualquier capa lanza `new ErrorApi(status, code, mensaje)` y un único *middleware* de errores construye la respuesta.

---

## 6. Datos (Postgres con Prisma)

| Tabla | Qué guarda |
|---|---|
| `perfil_usuario` | Cuenta: email, versión y fecha de aceptación de los términos |
| `restaurante` | El local (uno por cuenta): origen GOOGLE o MANUAL, `google_place_id` único, datos básicos confirmados por el dueño, estado del alta |
| `canal` | Web, Instagram y teléfono del local, con su fuente |
| `analisis` · `paso_analisis` | Estado del análisis y de cada uno de sus 5 pasos (con duraciones para el tiempo restante) |
| `ficha_marca` | Identidad del local: descripción, tonos, colores, letras, rasgos únicos; origen por campo y versión |
| `oportunidad` · `campana` | Propuestas de campaña con señales, fuente y confianza; campañas creadas |
| `pieza` · `diapositiva` · `pieza_recurso` | Carruseles e historias, sus diapositivas, cambios usados y estado de publicación |
| `recurso` · `referencia_estetica` | Fotos (reales y generadas) y referencias de estilo |
| `cuenta_social` · `oauth_estado` | Cuenta de Instagram conectada (token cifrado) y estado de la conexión OAuth |
| `trabajo` · `trabajo_generacion` | Cola de trabajos (tipo, estado, fase, intentos) y registro de cada llamada a la IA |

- Nombres en castellano y `snake_case`. Relaciones, `@unique`, índices y enumeraciones en [`schema.prisma`](https://github.com/Ju4nmaFd3z/Society-TypeScript/blob/main/api/prisma/schema.prisma).
- **Ya creadas en Supabase** con la primera migración: `perfil_usuario` y `restaurante`. El resto se añade con nuevas migraciones a medida que se programa cada parte.
- El `seed` creará el local ficticio de la demo (usuario, restaurante, ficha, análisis, oportunidades) y el catálogo de referencias del equipo. De momento está vacío.
- `perfil_usuario` lo crea la API en `/auth/registro`, con el mismo `id` que el usuario de Supabase Auth. Pendiente de confirmar en equipo: sustituye al *trigger* que se planteó cuando la API era una Edge Function.
- Valoración, reseñas y fotos de Google **nunca se guardan**: se piden en vivo.
- **Quién ve qué:** la API filtra cada consulta por el usuario del JWT (`owner_id = userId`). RLS queda activado en todas las tablas, también en `_prisma_migrations`, y sin políticas. Así nadie puede leer ni modificar Postgres con la clave pública de Supabase. Prisma se conecta como propietario de las tablas y no le afecta.

**Dos conexiones a la base de datos:**

| Variable | Conexión de Supabase | La usa |
|---|---|---|
| `DATABASE_URL` | *Transaction pooler*, puerto 6543, con `?pgbouncer=true` | La API en marcha (en Vercel cada petición puede abrir una conexión nueva) |
| `DIRECT_URL` | *Session pooler*, puerto 5432 | El CLI de Prisma para las migraciones |

---

## 7. Secretos

| Secreto | Dónde vive |
|---|---|
| `DATABASE_URL` y `DIRECT_URL` (ver el [apartado 6](#6-datos-postgres-con-prisma)) | Variables de Vercel de la API y `.env` local |
| `SUPABASE_URL` (solo `https://<proyecto>.supabase.co`) y `SUPABASE_SECRET_KEY` | API |
| `GOOGLE_MAPS_API_KEY` (restringida a Places y con cuota diaria) | API |
| `GEMINI_API_KEY` | API (worker) |
| `HF_API_KEY_ID`, `HF_API_KEY_SECRET` y secreto del *webhook* de Higgsfield | API (worker) |
| `IG_APP_SECRET` e `IG_TOKEN_KEY` (cifrado AES-256-GCM de los tokens) | API |
| `TRABAJOS_SECRETO` (Supabase → `/trabajos`) | Vault de Supabase y variables de Vercel |
| URL de la API | App (`BuildConfig`) y panel (`environment.ts`): no es secreta |

Ningún secreto se sube a los repositorios: cada parte tiene su `.env.example` con los nombres de las variables y sin valores. Casi todos los valores de Supabase se copian del panel del proyecto, al que tiene acceso todo el equipo. La contraseña de la base de datos se comparte una sola vez con un enlace que caduca.

---

## 8. Despliegue

| Pieza | Dónde |
|---|---|
| Base de datos, Auth, Storage, webhooks y `pg_cron` | Un proyecto de Supabase en Frankfurt (plan gratuito; se pausa tras una semana sin uso) |
| API Express + worker | Proyecto de Vercel con raíz en `api/`: <https://society-api-xi.vercel.app/api/v1/salud> |
| Panel web | Proyecto de Vercel con raíz en `web/` |
| App Android | APK en el móvil de la demo |
| Desarrollo | `npm run dev` en `api/` contra el proyecto de Supabase · `npm run db:migrar` para aplicar migraciones · `ng serve` en `web/` |

Vercel detecta Express sin configuración: despliega `api/src/app.ts`, que exporta la app. Cada fusión en `main` despliega de nuevo.

---

## 9. Decisiones citadas en los diagramas

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

---

## 10. Antes de la primera línea de código

1. ~~Crear el proyecto de Supabase y el proyecto de Vercel de la API.~~ Hecho: API en Vercel y Supabase con el equipo invitado.
2. ~~Configurar Prisma con `DATABASE_URL` y `DIRECT_URL`, y hacer la primera migración.~~ Hecho.
3. Comprobar en [console.higgsfield.ai](https://console.higgsfield.ai) que la cuenta tiene Nano Banana Pro por API. Si no, elegir otro modelo con imágenes de referencia.
4. Google Cloud: asociar una tarjeta, crear la clave de Places restringida y poner una cuota diaria por debajo de lo gratuito. Sin tarjeta, la búsqueda usa datos de prueba y el alta se hace a mano.
5. Crear la clave de Gemini (capa gratuita).
6. Crear la app de Instagram Login en Meta y añadir la cuenta profesional de pruebas del equipo como *tester*.
7. Completar la primera versión de `contrato/openapi.yaml`. Ya tiene las rutas de Places; faltan `/salud`, `/auth` y `/restaurantes`.
