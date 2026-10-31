# Arquitectura de SOCIETY

Este documento y los dos diagramas de la carpeta [`diagramas/`](diagramas/README.md) fijan la arquitectura del TFG para empezar a programar. Actualizado el 01/11/2026.

> **Pendiente de actualizar.** Este documento y los diagramas describen la API como una Supabase Edge Function (Deno + Hono). La decisión vigente es un único backend Node.js con Express y Prisma: ver [0003](../decisiones/0003-backend-unico-node.md). La actualización está en la tarea SOCIETY-128 de Jira.

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

Los dos permiten dar de alta el local, analizarlo, crear carruseles e historias con fotos reales, publicarlos en Instagram y gestionar oportunidades. Lo que se hace en uno aparece en el otro.

```
   App Android (Kotlin)                        Panel web (Angular, en Vercel)
   supabase-kt · Retrofit                      supabase-js · HttpClient
          │  sesión y fotos      │  API propia (HTTPS + JWT)      │
          ▼                      ▼                                ▼
 ┌──────────────────────────── Supabase ──────────────────────────────┐
 │ Auth · Storage · Postgres con RLS · Edge Function «api» (TypeScript)│
 │ tabla «trabajo» (cola) ── Database Webhook + pg_cron ──┐            │
 └────────────────────────────────────────────────────────┼───────────┘
        │ Places (búsqueda) · Instagram (OAuth)            ▼
        ▼                                  Worker Node en Vercel (trabajos largos)
  Google Places · Instagram                 ├─ Google Places · web del local
                                            ├─ Gemini (texto y visión)
                                            ├─ Higgsfield · Nano Banana Pro (imágenes)
                                            └─ Instagram (publicar, renovar tokens)
```

---

## 2. Servicios y tecnologías

| Pieza | Tecnología | Para qué | Plan |
|---|---|---|---|
| **Base de datos** | Supabase · Postgres con RLS (*Row Level Security*) | Todos los datos de negocio de los dos clientes | Gratuito |
| **Cuentas** | Supabase Auth (email + contraseña) | Registro, inicio de sesión, JWT | Gratuito |
| **Archivos** | Supabase Storage (buckets privados `recursos`, `referencias`, `generadas`) | Fotos del local, referencias, imágenes generadas | Gratuito |
| **API propia** | Supabase **Edge Functions** (TypeScript sobre Deno, router Hono) | Todo lo que responde rápido: CRUD, búsqueda en Places, crear trabajos, OAuth de Instagram | Gratuito |
| **Trabajos largos** | **Worker Node** (TypeScript) en **Vercel** (plan Hobby) | Análisis, plan con IA, imágenes, publicar en Instagram, renovar tokens | Gratuito |
| **Activación de trabajos** | Supabase **Database Webhooks**, `pg_cron` y `pg_net` | Avisar al worker al crear un trabajo y hacer un barrido cada minuto | Gratuito |
| **App Android** | Kotlin, Jetpack Compose, Material 3, Navigation Compose, Hilt, Retrofit + OkHttp, kotlinx.serialization, Coil, supabase-kt, DataStore, Custom Tabs | Cliente móvil | — |
| **Panel web** | Angular (componentes *standalone* y *signals*), RxJS, supabase-js, Angular CDK; alojado en **Vercel** | Cliente de escritorio | Gratuito |
| **Contrato de la API** | OpenAPI 3.1 (`contrato/openapi.yaml`) | Una sola definición para Android y Angular | — |

**Presupuesto:** 0 € en servicios, salvo el saldo de Higgsfield, que paga el equipo. Todo es para pruebas del equipo. La demo usa un **local ficticio** con imágenes propias.

### Repositorios

| Repositorio | Contenido |
|---|---|
| `Society-Kotlin` | App Android |
| `Society-TypeScript` | `contrato/` (OpenAPI) · `supabase/` (migraciones y Edge Functions) · `worker/` (proyecto de Vercel) · `web/` (panel Angular, otro proyecto de Vercel) |
| `Society` | Documentación (`docs/arquitectura`) y registro de decisiones |

---

## 3. Reglas de comunicación

1. Los clientes solo hablan con Supabase para la **sesión** (Auth) y la **subida de fotos** (Storage). Todo lo demás pasa por la API propia.
2. Cada llamada a la API lleva `Authorization: Bearer <JWT>`. La única excepción es `GET /instagram/callback`, protegida con un `state` de un solo uso.
3. **Ningún secreto en los clientes.** La app y el panel solo llevan la URL de Supabase y su clave pública; la seguridad la dan el JWT y RLS.
4. Las operaciones largas responden **202** con un ID. El cliente consulta el estado cada 3 s y muestra «No salgas de la aplicación» con el tiempo restante estimado.
5. Errores: código HTTP + `{"error": {"code", "message", "retryable"}}`. Los GET se reintentan hasta 3 veces; los POST nunca de forma automática.
6. Ningún cambio de la API sin cambiar antes `openapi.yaml`, en una *pull request* que revisen la parte Android y la parte Angular.

### Cómo se ejecutan los trabajos largos en Vercel

Vercel (plan Hobby) no mantiene procesos encendidos: cada función dura **como máximo 300 s**, y sus tareas programadas solo pueden ejecutarse una vez al día. Por eso es Supabase quien avisa al worker:

| Disparador | Cuándo | Llama a |
|---|---|---|
| *Database Webhook* de Supabase | Cada vez que se inserta un trabajo | `POST /api/ejecutar` del worker |
| `pg_cron` + `pg_net` | Cada minuto | `POST /api/barrido`: reintentos, esperas vencidas y trabajos colgados |
| `pg_cron` | Una vez al día | Encola la renovación de tokens de Instagram |
| *Webhook* de Higgsfield | Cuando termina una imagen | `POST /api/webhooks/higgsfield` (firma HMAC) |

Ninguna función espera dentro de su ejecución: los trabajos largos se dividen en **fases**, cada una se guarda en la tabla `trabajo` y se retoma en la siguiente llamada.

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
| **Google Places API (New)** | Buscar el local, leer sus datos públicos y su foto | Edge Function (búsqueda, fotos) y worker (detalles) | `POST places.googleapis.com/v1/places:searchText` · `GET /v1/places/{id}` · `GET /v1/{photo}/media` | Clave de API en el servidor (`X-Goog-Api-Key`) y `X-Goog-FieldMask` obligatoria | Gratis hasta 1.000 llamadas al mes por tipo (Enterprise); **exige cuenta de facturación con tarjeta** | Aprobada |
| **Gemini API** (`gemini-3.8-flash`) | Análisis, ficha de marca, oportunidades, plan de cada pieza, pie y hashtags (texto y visión) | Worker | SDK `@google/genai`, salida JSON con esquema | Clave de API en el servidor | Capa gratuita, solo para pruebas del equipo | Aprobada |
| **Higgsfield API** (Nano Banana Pro) | Imagen de cada diapositiva a partir de la foto real y una referencia | Worker | `POST https://api.higgsfield.ai/<modelo>` · `GET /requests/{id}/status` · `DELETE /requests/{id}` · *webhook* · `POST /estimate/<modelo>` | `Authorization: Key ID:SECRET` en el servidor | Prepago (lo paga el equipo). No cobra los fallos ni las imágenes bloqueadas | Aprobada con condición: comprobar que la cuenta tiene Nano Banana Pro por API |
| **Instagram API with Instagram Login** (Meta) | Conectar la cuenta profesional y publicar carruseles e historias | Edge Function (OAuth) y worker (publicar, renovar) | `instagram.com/oauth/authorize` · `POST api.instagram.com/oauth/access_token` · `GET graph.instagram.com/access_token` · `POST /{ig-id}/media` · `POST /{ig-id}/media_publish` · `GET refresh_access_token` | OAuth del propietario; token de 60 días cifrado en el servidor | Gratis. Con acceso estándar, solo cuentas con rol en la app (cuenta de pruebas del equipo) | Aprobada |
| **Web del local** (HTTP GET) | Texto, enlaces a redes, colores y letras de la web del local | Worker | `GET` a la URL (10 s, 2 MB, respetando `robots.txt`) | — | Gratis | Aprobada |
| **Supabase Auth y Storage** | Sesión y subida de fotos desde los clientes | App (supabase-kt) y panel (supabase-js) | SDK oficiales | Clave pública + JWT del usuario | Gratuito | Aprobada |

**Respaldo sin IA de imagen:** si Higgsfield falla o no hay saldo, el worker compone la diapositiva con una **plantilla**: la foto real recortada, el texto y los colores de la ficha, con `sharp`.

**Descartadas:**
- Google Business Profile: el foco es Instagram.
- TikTok.
- *Scraping* de Google, Instagram o TikTok.
- Claude: se eligió Gemini por su capa gratuita.
- Imagen con Gemini (`gemini-3-pro-image`): no tiene capa gratuita.
- Firebase Cloud Messaging: no hay notificaciones en el MVP.

---

## 5. API propia

**Base:** `https://<proyecto>.supabase.co/functions/v1/api/v1` · JSON en camelCase · fechas ISO 8601 en UTC.

| Método y ruta | Pantallas | Devuelve |
|---|---|---|
| `GET /restaurantes/mio` | Arranque, S01A | `{restaurante \| null, altaEstado, analisisEnCursoId?}` |
| `GET /places/search?q=&pageToken=` | S02 | `{resultados[], nextPageToken?}` (no se guarda) |
| `GET /places/photo?name=&maxWidthPx=` | S02 | `{photoUri, atribuciones[]}` |
| `POST /restaurantes` | S02, S02M | 201 · 409 `DUPLICADO` |
| `POST /restaurantes/{id}/analisis` | S02, S02M, S03 | 202 `{analisisId}` |
| `GET /analisis/{id}` · `POST /analisis/{id}/cancelar` | S03 | estado, pasos y `tiempoRestanteSeg` |
| `GET /restaurantes/{id}/ficha` · `PATCH /restaurantes/{id}/ficha` | S04, S07 | ficha con origen por campo y `version` |
| `GET /restaurantes/{id}/resumen-google` | S04, S05 | valoración y reseñas en vivo |
| `GET /restaurantes/{id}/oportunidades` · `GET /oportunidades/{id}` · `PATCH /oportunidades/{id}` | S05, S11 | oportunidades |
| `POST /campanas` · `GET /campanas/{id}` | S11, S06, S12 | campaña + idea sugerida |
| `GET /referencias-esteticas` · `POST /referencias-esteticas` | S05, S07 | catálogo y referencias propias |
| `GET /recursos` · `POST /recursos` | S08 | fotos del local |
| `POST /piezas` · `GET /piezas` · `GET /piezas/{id}` · `PATCH /piezas/{id}` | S05, S08, S09, S09P, S15 | pieza con diapositivas y `tiempoRestanteSeg` |
| `POST /piezas/{id}/generar` | S08 | 202 |
| `POST /piezas/{id}/diapositivas/{n}/regenerar` | S09 | 202 · 429 (3 cambios por diapositiva o 40 imágenes al día) |
| `POST /piezas/{id}/publicar-instagram` · `POST /piezas/{id}/marcar-publicada` | S09P | 202 · 409 si Instagram no está conectado |
| `POST /instagram/conexiones` · `GET /instagram/callback` · `GET /instagram/cuenta` · `DELETE /instagram/cuenta` | S04, S09P, S13 | URL de autorización · redirección · estado de la cuenta |

Rutas del worker (las llama Supabase o Higgsfield, nunca los clientes): `POST /api/ejecutar`, `POST /api/barrido`, `POST /api/webhooks/higgsfield`.

---

## 6. Datos (Postgres)

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

Valoración, reseñas y fotos de Google **nunca se guardan**: se piden en vivo. Todas las tablas tienen RLS: cada usuario solo ve las filas de su local.

---

## 7. Secretos

| Secreto | Dónde vive |
|---|---|
| `SUPABASE_SERVICE_ROLE_KEY` | Edge Function y worker |
| `GOOGLE_MAPS_API_KEY` (restringida a Places y con cuota diaria) | Edge Function y worker |
| `GEMINI_API_KEY` | Worker |
| `HF_API_KEY_ID`, `HF_API_KEY_SECRET` y secreto del *webhook* de Higgsfield | Worker |
| `IG_APP_SECRET` e `IG_TOKEN_KEY` (cifrado AES-256-GCM de los tokens) | Edge Function y worker |
| `TRABAJOS_SECRETO` (Supabase → worker) | Vault de Supabase y variables de Vercel |
| URL de Supabase + clave pública | App (`BuildConfig`) y panel (`environment.ts`): no son secretas |

Ningún secreto se sube a los repositorios.

---

## 8. Despliegue

| Pieza | Dónde |
|---|---|
| Base de datos, Auth, Storage, Edge Function, webhooks y `pg_cron` | Un proyecto de Supabase (plan gratuito; se pausa tras una semana sin uso) |
| Worker | Proyecto de Vercel con raíz en `worker/` |
| Panel web | Proyecto de Vercel con raíz en `web/` |
| App Android | APK en el móvil de la demo |
| Desarrollo | `supabase start` + `supabase functions serve` + `vercel dev` + `ng serve` en local |

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
| D-26 | Trabajos largos en Vercel |
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

1. Crear el proyecto de Supabase y los dos proyectos de Vercel (worker y panel).
2. Comprobar en [console.higgsfield.ai](https://console.higgsfield.ai) que la cuenta tiene Nano Banana Pro por API. Si no, elegir otro modelo con imágenes de referencia.
3. Google Cloud: asociar una tarjeta, crear la clave de Places restringida y poner una cuota diaria por debajo de lo gratuito. Sin tarjeta, la búsqueda usa datos de prueba y el alta se hace a mano.
4. Crear la clave de Gemini (capa gratuita).
5. Crear la app de Instagram Login en Meta y añadir la cuenta profesional de pruebas del equipo como *tester*.
6. Escribir la primera versión de `contrato/openapi.yaml`.
