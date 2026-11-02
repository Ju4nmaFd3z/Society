# Arquitectura

Decisiones técnicas, estructura del sistema y diagramas.

## Contenido

- [Arquitectura técnica](tecnica/README.md): servicios, comunicación, trabajos largos, API externas, API propia, datos, secretos y despliegue, cada uno en su archivo.
- [Diagramas](diagramas/README.md): flujo general de la app y detalle de cada pantalla.
- [Repositorios](repositorios.md): qué repositorio hay por tecnología y qué contiene cada uno.

## Estado

La arquitectura está definida: dos clientes, la app Android y el panel web Angular, con las mismas funciones sobre un único backend Node.js con Express y Prisma. Ver la decisión [0003](../decisiones/0003-backend-unico-node.md).

La arquitectura técnica ya describe la API Express con Prisma, que está desplegada en Vercel. Los diagramas todavía muestran la API como una Supabase Edge Function; su actualización sigue pendiente en Jira (SOCIETY-128).

El código de cada tecnología se documenta en su propio repositorio. Cada decisión relevante se registra además en [Decisiones](../decisiones/README.md).
