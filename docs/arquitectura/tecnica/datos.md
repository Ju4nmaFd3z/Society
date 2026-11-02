# Datos (Postgres con Prisma)

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

## Dos conexiones a la base de datos

| Variable | Conexión de Supabase | La usa |
|---|---|---|
| `DATABASE_URL` | *Transaction pooler*, puerto 6543, con `?pgbouncer=true` | La API en marcha (en Vercel cada petición puede abrir una conexión nueva) |
| `DIRECT_URL` | *Session pooler*, puerto 5432 | El CLI de Prisma para las migraciones |
