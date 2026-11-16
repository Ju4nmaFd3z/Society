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
| Permitir registros nuevos (*Allow new users to sign up*) | **Desactivado** desde el 16 de noviembre de 2026 (temporal) | Registro cerrado mientras solo prueba el equipo |

### Registro cerrado temporalmente

Para que nadie ajeno al equipo cree una cuenta y use la búsqueda de Google a través de la API. Hay dos puertas:

1. **Supabase Auth directo:** ✅ cerrada. *Allow new users to sign up* está desactivado, así que no se puede crear una cuenta llamando a Supabase Auth sin pasar por la API.
2. **`POST /auth/registro` de la API:** ⚠️ **sigue abierta**. Crea los usuarios con la API de administrador (`auth.admin.createUser`), que ignora ese ajuste. Cerrarla necesita un cambio en la API.

Los usuarios que ya existen siguen pudiendo entrar y renovar la sesión. Si el equipo necesita una cuenta nueva mientras tanto, se crea desde el panel (Authentication → Users → *Add user*, marcando *Auto Confirm User*). Esa cuenta no tendrá fila en `perfil_usuario`, porque solo la crea `/auth/registro`.

**Para reabrir el registro:** volver a activar *Allow new users to sign up* y deshacer el cierre de la API.
