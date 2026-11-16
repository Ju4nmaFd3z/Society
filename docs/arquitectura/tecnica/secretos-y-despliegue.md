# Secretos y despliegue

## Secretos

| Secreto | Dónde vive |
|---|---|
| `DATABASE_URL` y `DIRECT_URL` (ver [Datos](datos.md#dos-conexiones-a-la-base-de-datos)) | Variables de Vercel de la API y `.env` local |
| `SUPABASE_URL` (solo `https://<proyecto>.supabase.co`) y `SUPABASE_SECRET_KEY` | API |
| `GOOGLE_PLACES_API_KEY` (restringida a Places API (New) y con cuota diaria) | API |
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

Vercel solo compila los archivos que se importan, directa o indirectamente, desde `app.ts`. Por eso los tipos que amplían Express (`src/types/express.ts`) se importan en `app.ts`: si no, la build de Vercel falla aunque `npm run typecheck` pase en local.

## Supabase Auth

Configuración del proyecto (Authentication → Sign In / Providers), comprobada el 16 de noviembre de 2026:

| Ajuste | Valor | Por qué |
|---|---|---|
| Email y contraseña | Activado | Único método de acceso del MVP |
| Confirmar email | Desactivado | D-04: la cuenta queda lista al registrarse |
| Permitir registros nuevos (*Allow new users to sign up*) | **Activado** | Pendiente de desactivar mientras el registro esté cerrado |

Para cerrar el registro hacen falta dos cambios, porque hay dos puertas:

1. **Supabase:** desactivar *Allow new users to sign up*. Impide registrarse llamando a Supabase Auth directamente, sin pasar por la API.
2. **API:** `POST /auth/registro` crea los usuarios con la API de administrador (`auth.admin.createUser`), que **ignora** ese ajuste. Para cerrarlo hace falta un cambio en la API.

Los usuarios que ya existen siguen pudiendo entrar y renovar la sesión en los dos casos.
