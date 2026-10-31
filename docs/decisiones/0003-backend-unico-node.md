# 0003. Backend único en Node.js con Express y Prisma

- **Fecha:** 2026-10-31
- **Estado:** aceptada

## Contexto

Society tiene dos clientes con las mismas funciones: la app Android y el panel web Angular. Los requisitos del módulo de Acceso a Datos piden un backend Node.js con Express o Nest, un ORM (Prisma o TypeORM) con migraciones y seed, PostgreSQL y una API REST organizada en rutas, controladores y servicios.

## Decisión

- Un único backend: una API REST en Node.js con Express y TypeScript, organizada en rutas, controladores y servicios.
- Persistencia con Prisma sobre el PostgreSQL de Supabase, con migraciones y seed en el repositorio.
- Supabase se mantiene para las cuentas (Auth) y las fotos (Storage).
- La API se despliega en Vercel. Los trabajos largos (análisis, imágenes y publicación) se ejecutan en la misma API y los activa Supabase.
- El código vive en `Society-TypeScript`, junto al panel web y al contrato OpenAPI.

## Alternativas consideradas

- **API en Supabase Edge Functions (Deno + Hono), con un worker aparte:** es lo que describe la [arquitectura técnica](../arquitectura/arquitectura-tecnica.md). Descartada porque no usa Express ni un ORM, y los requisitos los exigen.

## Consecuencias

- Los dos clientes usan la misma API y el mismo contrato.
- La arquitectura técnica y los diagramas se tienen que actualizar a esta decisión (SOCIETY-128 en Jira).
