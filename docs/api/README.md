# API

Documentación de las API que utiliza Society.

## API propia

Backend único de Society, con Express y Prisma. La usan la app Android y el panel web.

- **URL:** <https://society-api-xi.vercel.app/api/v1>. Para comprobar que funciona: [`/salud`](https://society-api-xi.vercel.app/api/v1/salud).
- **Contrato:** [`contrato/openapi.yaml`](https://github.com/Ju4nmaFd3z/Society-TypeScript/blob/main/contrato/openapi.yaml) en Society-TypeScript.
- **Código:** [`api/`](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/api) en Society-TypeScript.
- **Rutas, errores y datos:** [API propia](../arquitectura/tecnica/api-propia.md) en la arquitectura técnica.

## API externas

- [Google Maps (Places)](documentacion-maps.md): información que se extrae de la API de Google Maps.
