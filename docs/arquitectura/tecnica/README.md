# Arquitectura técnica

Cómo está construido Society: servicios, comunicación, API, datos y despliegue. El backend es una API Express con Prisma en Vercel, según la decisión [0003](../../decisiones/0003-backend-unico-node.md). Actualizado el 2 de noviembre de 2026.

Los [diagramas](../diagramas/README.md) muestran lo mismo de forma visual: el flujo general y el detalle de cada pantalla.

## Contenido

| Archivo | Qué contiene |
|---|---|
| [Servicios y tecnologías](servicios.md) | Qué pieza hace qué, con qué tecnología y en qué repositorio |
| [Clientes](clientes.md) | Capa de red y estructura por pantalla de la app Android; el panel web usa los mismos nombres |
| [Reglas de comunicación](comunicacion.md) | Cómo hablan los clientes con la API: JWT, errores, reintentos y contrato |
| [Trabajos largos](trabajos-largos.md) | Cómo se ejecutan en Vercel el análisis, las imágenes y la publicación |
| [API externas](apis-externas.md) | Google Places, Gemini, Higgsfield, Instagram y Supabase |
| [API propia](api-propia.md) | URL, rutas y códigos de error |
| [Datos](datos.md) | Tablas, RLS y conexiones de Prisma |
| [Secretos y despliegue](secretos-y-despliegue.md) | Dónde vive cada clave y dónde se despliega cada parte |

## Visión general

SOCIETY tiene **dos clientes con las mismas funciones** que comparten **un único backend**:

- **App Android** (Kotlin): parte Android del TFG.
- **Panel web** (Angular): parte TypeScript del TFG, con composición de escritorio (barra lateral).
- **API REST** (Express + Prisma): backend compartido, también parte TypeScript del TFG.

Los dos clientes permiten dar de alta el local, analizarlo, crear carruseles e historias con fotos reales, publicarlos en Instagram y gestionar oportunidades. Lo que se hace en uno aparece en el otro.

```
   App Android (Kotlin)                     Panel web (Angular + Tailwind, en Vercel)
   Retrofit + Gson · OkHttp                 HttpClient + interceptores
          │                                        │
          └──────── HTTPS + JWT · /api/v1 ─────────┘
                             ▼
 ┌──────────────── API Express + Prisma (TypeScript, Vercel) ─────────────────┐
 │ rutas → controladores → servicios · middlewares (auth, errores)             │
 │ worker: rutas internas /trabajos (las llama Supabase) y webhook Higgsfield  │
 └───────┬──────────────────────────────┬─────────────────────────────┬───────┘
         │ Prisma                       │ supabase-js (servidor)      │
         ▼                              ▼                             ▼
 ┌──────────── Supabase ─────────────────────────────┐   Google Places · web del local
 │ Postgres (tablas de negocio, cola «trabajo»)      │   Gemini (texto y visión)
 │ Auth (email + contraseña) · Storage (fotos)       │   Higgsfield · Nano Banana Pro
 │ Database Webhook + pg_cron → /trabajos            │   Instagram (OAuth y publicar)
 └───────────────────────────────────────────────────┘
   Fotos: el cliente sube con PUT a una URL firmada que le da la API
```

## Pasos previos

1. ~~Crear el proyecto de Supabase y el proyecto de Vercel de la API.~~ Hecho: API en Vercel y Supabase con el equipo invitado.
2. ~~Configurar Prisma con `DATABASE_URL` y `DIRECT_URL`, y hacer la primera migración.~~ Hecho.
3. Comprobar en [console.higgsfield.ai](https://console.higgsfield.ai) que la cuenta tiene Nano Banana Pro por API. Si no, elegir otro modelo con imágenes de referencia.
4. Google Cloud: asociar una tarjeta, crear la clave de Places restringida y poner una cuota diaria por debajo de lo gratuito. Sin tarjeta, la búsqueda usa datos de prueba y el alta se hace a mano.
5. Crear la clave de Gemini (capa gratuita).
6. Crear la app de Instagram Login en Meta y añadir la cuenta profesional de pruebas del equipo como *tester*.
7. Completar la primera versión de `contrato/openapi.yaml`. Ya tiene las rutas de Places; faltan `/salud`, `/auth` y `/restaurantes`.
