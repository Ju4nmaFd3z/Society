# 01A. Acceso

Pantalla de registro e inicio de sesión. No tiene número en Figma.

## Objetivo

Que el usuario cree su cuenta o entre en ella para guardar su local y su contenido.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Detalle: página «1 · S01A · Acceso» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio). Decisión D-04.

## Contenido

- **Cabecera:** botón atrás, el título «Tu cuenta» y la mascota.
- **Selector** con dos pestañas: Crear cuenta e Iniciar sesión.
- **Correo electrónico:** obligatorio, con formato de correo y 254 caracteres como máximo.
- **Contraseña:** mínimo 8 caracteres, con opción de mostrarla. Nunca se guarda en la app ni en los registros.
- **Casilla de términos:** «Acepto los términos y la política de privacidad». Solo al crear cuenta, y obligatoria.
- **Botón «Continuar».**
- **Enlace «¿Olvidaste tu contraseña?»:** es P1.

## Estados

Formulario, con los errores por campo, enviando, error y hecho.

Errores posibles: credenciales incorrectas, correo ya registrado (la pantalla cambia a Iniciar sesión), demasiados intentos y falta de conexión.

## Navegación

Se llega desde [01. Inicio](01-inicio.md) o cuando caduca la sesión. Al terminar, la app va a [02. Empezamos](02-empezamos.md), [03. Análisis](03-analisis.md), [04. Así te ve Internet](04-asi-te-ve-internet.md) o [05. Home](05-home.md), según el estado del alta del local.

## Implementación

- Android: `feature/acceso`, con la ruta `AccesoRoute`, issues SOCIETY-52 y SOCIETY-53. Tiene el botón atrás, la mascota, el selector, la opción de mostrar la contraseña, la casilla de términos solo al crear cuenta y los estados formulario, enviando, error y hecho. Está conectada a la API: guarda la sesión en DataStore y, al terminar, va a 02, 03, 04 o 05 según el estado del alta. Faltan los errores por campo, los límites del correo y de la contraseña, pasar a Iniciar sesión cuando el correo ya está registrado y «¿Olvidaste tu contraseña?». Ver [Society-Kotlin](https://github.com/Ju4nmaFd3z/Society-Kotlin/blob/main/docs/pantallas.md).
- Web: ruta `/acceso` en `features/acceso`, issue SOCIETY-55. Tiene el selector, las validaciones por campo, la opción de mostrar la contraseña, la casilla de términos solo al crear cuenta y los errores de credenciales incorrectas y de correo ya registrado. De momento usa una sesión simulada, sin la API. Faltan el botón atrás, la mascota, «¿Olvidaste tu contraseña?» y los estados enviando, demasiados intentos y sin conexión, que llegan al conectar la API (SOCIETY-99). Ver [Society-TypeScript](https://github.com/Ju4nmaFd3z/Society-TypeScript/tree/main/web).
