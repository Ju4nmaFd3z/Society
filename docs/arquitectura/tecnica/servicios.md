# Servicios y tecnologías

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

## Repositorios

| Repositorio | Contenido |
|---|---|
| `Society-Kotlin` | App Android |
| `Society-TypeScript` | `contrato/` (OpenAPI) · `api/` (Express + Prisma + worker, un proyecto de Vercel) · `web/` (panel Angular, otro proyecto de Vercel) |
| `Society` | Documentación (`docs/arquitectura`) y registro de decisiones |
