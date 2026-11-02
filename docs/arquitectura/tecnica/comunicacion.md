# Reglas de comunicación

1. Los clientes **solo hablan con la API propia**, también para registrarse y entrar. La única excepción es **subir fotos**: la API devuelve una URL de subida firmada y el cliente hace `PUT` directo a Storage, así el archivo no pasa por Vercel (que limita el cuerpo a 4,5 MB).
2. Cada llamada a la API lleva `Authorization: Bearer <JWT>`. Las excepciones son `/salud`, `/auth/registro`, `/auth/acceso`, `/auth/refresco` y `GET /instagram/callback` (protegida con un `state` de un solo uso).
3. **Ningún secreto en los clientes.** La app y el panel solo llevan la URL de la API. Android guarda los tokens en DataStore y la web en `localStorage`.
4. Las operaciones largas responden **202** con un ID. El cliente consulta el estado cada 3 s y muestra «No salgas de la aplicación» con el tiempo restante estimado.
5. Errores: código HTTP + `{"error": {"code", "message", "retryable"}}`. Los GET se reintentan hasta 3 veces; los POST nunca de forma automática. Los códigos están en la [API propia](api-propia.md#errores).
6. Ningún cambio de la API sin cambiar antes `openapi.yaml`, en una *pull request* que revisen la parte Android y la parte Angular.
7. El contrato (`openapi.yaml`) documenta **nuestras** rutas (`/api/v1/...`), no las de Google, Gemini, Higgsfield o Instagram. Por ejemplo, la búsqueda se documenta como `GET /places/search` con nuestra respuesta; la llamada a Google Places va solo en la descripción de esa ruta.
