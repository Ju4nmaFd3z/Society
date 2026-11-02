# Secretos y despliegue

## Secretos

| Secreto | Dónde vive |
|---|---|
| `DATABASE_URL` y `DIRECT_URL` (ver [Datos](datos.md#dos-conexiones-a-la-base-de-datos)) | Variables de Vercel de la API y `.env` local |
| `SUPABASE_URL` (solo `https://<proyecto>.supabase.co`) y `SUPABASE_SECRET_KEY` | API |
| `GOOGLE_MAPS_API_KEY` (restringida a Places y con cuota diaria) | API |
| `GEMINI_API_KEY` | API (worker) |
| `HF_API_KEY_ID`, `HF_API_KEY_SECRET` y secreto del *webhook* de Higgsfield | API (worker) |
| `IG_APP_SECRET` e `IG_TOKEN_KEY` (cifrado AES-256-GCM de los tokens) | API |
| `TRABAJOS_SECRETO` (Supabase → `/trabajos`) | Vault de Supabase y variables de Vercel |
| URL de la API | App (`BuildConfig`) y panel (`environment.ts`): no es secreta |

Ningún secreto se sube a los repositorios: cada parte tiene su `.env.example` con los nombres de las variables y sin valores. Casi todos los valores de Supabase se copian del panel del proyecto, al que tiene acceso todo el equipo. La contraseña de la base de datos se comparte una sola vez con un enlace que caduca.

## Despliegue

| Pieza | Dónde |
|---|---|
| Base de datos, Auth, Storage, webhooks y `pg_cron` | Un proyecto de Supabase en Frankfurt (plan gratuito; se pausa tras una semana sin uso) |
| API Express + worker | Proyecto de Vercel con raíz en `api/`: <https://society-api-xi.vercel.app/api/v1/salud> |
| Panel web | Proyecto de Vercel con raíz en `web/` |
| App Android | APK en el móvil de la demo |
| Desarrollo | `npm run dev` en `api/` contra el proyecto de Supabase · `npm run db:migrar` para aplicar migraciones · `ng serve` en `web/` |

Vercel detecta Express sin configuración: despliega `api/src/app.ts`, que exporta la app. Cada fusión en `main` despliega de nuevo.
