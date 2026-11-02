# Trabajos largos en Vercel

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
