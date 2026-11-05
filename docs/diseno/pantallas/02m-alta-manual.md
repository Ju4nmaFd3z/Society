# 02M. Alta manual

Formulario para dar de alta el local a mano cuando no aparece en Google Maps. No tiene número en Figma.

## Objetivo

Que el usuario pueda usar Society aunque su local no tenga ficha en Google.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Detalle: página «3 · S02M · Alta manual» de [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Contenido

- **Cabecera:** botón atrás y el título «Añade tu local».
- **Nombre del local:** obligatorio, de 2 a 80 caracteres.
- **Tipo de local:** obligatorio. Restaurante, Bar, Cafetería u Otro.
- **Dirección:** obligatoria, de 3 a 120 caracteres.
- **Municipio:** obligatorio, de 2 a 60 caracteres.
- **Web:** opcional, una dirección `https://` válida.
- **Instagram:** opcional, sin la arroba y con letras, números, puntos o guiones bajos.
- **Casilla «Declaro que gestiono este local»:** obligatoria.
- **Botón «Guardar y analizar».**

## Estados

Formulario, con los errores por campo, guardando, error y hecho.

Errores posibles: campo inválido (422), «Ya tienes un local» (409) y fallo del servidor, con la opción de reintentar.

## Navegación

Se llega desde «¿No aparece tu local? Añádelo a mano» de [02. Empezamos](02-empezamos.md). Al guardar, el análisis salta el paso de Google y se pasa a [03. Análisis](03-analisis.md). En [04. Así te ve Internet](04-asi-te-ve-internet.md) se avisa de que sin ficha de Google no hay valoración ni reseñas.

## Implementación

- Android: pendiente. Issue SOCIETY-62.
- Web: pendiente. Ruta `/alta/manual`, issue SOCIETY-63.
