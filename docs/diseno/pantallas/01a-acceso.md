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

- Android: pendiente. Issue SOCIETY-52.
- Web: pendiente. Ruta `/acceso`, issue SOCIETY-55.
