# API externas

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
- Supabase Edge Functions y SDK de Supabase en los clientes: sustituidos por la API Express (decisión [0003](../../decisiones/0003-backend-unico-node.md)).
