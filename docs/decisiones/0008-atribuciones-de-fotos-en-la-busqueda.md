# 0008. Las atribuciones de las fotos viajan en la búsqueda

- **Fecha:** 2026-11-18
- **Estado:** aceptada

## Contexto

Google obliga a mostrar el autor de cada foto de un local. El contrato de la API decía que `GET /places/search` devolvía solo los nombres de las fotos (`fotoNames`) y que `GET /places/photo` devolvía la URL de la imagen junto con los autores (`{photoUri, atribuciones[]}`).

Al implementarlo en SOCIETY-51 se vio que no se puede cumplir:

- La llamada de Google que da la imagen (`/media`) solo devuelve `photoUri`, sin autores.
- Los autores (`authorAttributions`) vienen en los datos del lugar. Pedirlos otra vez en `/places/photo` no funciona, porque el `name` de una foto cambia de una llamada a otra y no hay forma de emparejarla. Se comprobó con una foto real: el mismo `name` tenía ids distintos en la búsqueda y en Place Details.
- La respuesta de la búsqueda ya trae los autores de cada foto, pero los descartábamos.

## Decisión

- `GET /places/search` devuelve en cada resultado `fotos`, hasta 5 elementos `{name, atribuciones[]}`, en lugar de `fotoNames`. Cada atribución es `{nombre, uri?}`.
- `GET /places/photo` devuelve solo `{photoUri}`.
- Los clientes pintan los autores con los datos de la búsqueda, junto a la foto.
- Se actualizan el contrato (`openapi.yaml`), el README de la API y los documentos de arquitectura.

## Alternativas consideradas

- **Mantener el contrato y devolver `atribuciones: []`:** descartada, porque incumple la obligación de Google de mostrar al autor.
- **Pedir los autores en `/places/photo` con una segunda llamada a Place Details:** descartada, porque los nombres de foto no coinciden entre llamadas y además supondría una llamada de pago más por foto.

## Consecuencias

- Es un cambio de contrato: Android y la web lo consumen. Todavía no hay código que use `fotoNames` (S02 es del sprint 3), así que el coste es mínimo, pero Pablo y Juanma deben revisarlo en la pull request.
- `FotoGoogle(name, atribuciones)` del diccionario de datos ya tenía esta forma, y el diagrama de la pantalla S02 también.
- `/places/photo` pasa a hacer una sola llamada a Google, más sencilla y más barata.
- El `name` de la foto caduca y no se guarda: hay que pedir la foto mientras se muestra la lista.
