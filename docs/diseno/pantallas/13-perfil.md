# 13. Perfil

Perfil del usuario y conexión con Instagram.

## Objetivo

Reunir los datos de la cuenta, la conexión con Instagram y los accesos legales.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Figma: pestaña v3 · Cristal líquido. Ver [Diseño](../README.md).
- Detalle: página «13 · S13 · Perfil e Instagram» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).
- Decisiones D-09, D-30 y D-31.

## Contenido

- **Cabecera:** botón atrás y el título «Perfil».
- **Cuenta:** nombre del local y correo.
- **Datos del local:** abre [04. Así te ve Internet](04-asi-te-ve-internet.md) en modo edición.
- **Instagram:** usuario, tipo de cuenta, estado de la conexión y cuándo caduca. Con los botones «Conectar» o «Desconectar». La cuenta debe ser profesional.
- **Legal:** términos y condiciones, y política de privacidad.
- **Botón «Cerrar sesión».**
- **Enlace «Eliminar cuenta».** Es P1.

## Estados

Cargando, con datos, conectando Instagram, error y cerrando sesión. La conexión con Instagram puede estar conectada, caducada o revocada.

## Navegación

Se llega desde la barra inferior de [05. Home](05-home.md). «Conectar» sale a Instagram y vuelve a esta pantalla. «Cerrar sesión» abre un modal de confirmación y vuelve a [01. Inicio](01-inicio.md). Ver [Modales y estados](modales-y-estados.md).

## Implementación

- Android: pantalla provisional en `feature/perfil`, con la ruta `PerfilRoute`. Muestra el título y los botones que llevan a la siguiente pantalla. Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
