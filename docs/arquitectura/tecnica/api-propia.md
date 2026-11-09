# API propia

**Base:** `https://society-api-xi.vercel.app/api/v1` · JSON en camelCase · fechas ISO 8601 en UTC. El código está en [`Society-TypeScript/api`](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/api) y el contrato en [`Society-TypeScript/contrato/openapi.yaml`](https://github.com/Ju4nmaFd3z/Society-TypeScript/blob/main/contrato/openapi.yaml).

| Método y ruta | Pantallas | Devuelve |
|---|---|---|
| `GET /salud` | — | `{ok: true}`. Comprueba que la API está en marcha. **Hecha** |
| `POST /auth/registro` · `POST /auth/acceso` · `POST /auth/refresco` · `POST /auth/salir` · `POST /auth/recuperar` [P1] | S01A, S13 | `{accessToken, refreshToken, expiraEn}` · 401 · 409 (email ya registrado) · 429 |
| `GET /perfil` | S13 | email y términos aceptados |
| `GET /restaurantes/mio` | Arranque, S01A | `{restaurante \| null, altaEstado, analisisEnCursoId?}`. `altaEstado`: `SIN_LOCAL` (aún no tiene local) o el `alta_estado` del local: `CREADO` · `ANALIZANDO` · `PENDIENTE_CONFIRMAR` · `COMPLETADA` |
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

## Errores

Todas las respuestas de error tienen el formato `{"error": {"code", "message", "retryable"}}`. Los códigos son los mismos que traducen Android y la web a `ErrorSociety`:

| `code` | HTTP | Cuándo |
|---|---|---|
| `VALIDACION` | 400 | Datos de entrada no válidos o JSON mal formado |
| `SESION_CADUCADA` | 401 | JWT ausente, caducado o no válido |
| `CREDENCIALES_INCORRECTAS` | 401 | Email o contraseña incorrectos en `/auth/acceso`. A diferencia de `SESION_CADUCADA`, el cliente no intenta renovar la sesión |
| `NO_ENCONTRADO` | 404 | No existe el recurso o la ruta |
| `DUPLICADO` | 409 | El recurso ya existe (por ejemplo, el local ya está dado de alta) |
| `LIMITE_ALCANZADO` | 429 | Demasiadas peticiones o límite de cambios o imágenes |
| `MODERACION` | 422 | La IA ha bloqueado el contenido |
| `PROVEEDOR_NO_DISPONIBLE` | 503 | Google, Gemini, Higgsfield o Instagram no responden (`retryable: true`) |
| `INTERNO` | 500 | Error inesperado de la API; no se dan detalles al cliente |

En el código, cualquier capa lanza `new ErrorApi(status, code, mensaje)` y un único *middleware* de errores construye la respuesta.
