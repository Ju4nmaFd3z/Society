# 0002. Flujo de trabajo con una rama por persona

- **Fecha:** 2026-10-19
- **Estado:** aceptada

## Contexto

Dos autores trabajan en los mismos repositorios. Hace falta que ninguno pise el trabajo del otro y que la página principal de cada repositorio se vea siempre bien.

## Decisión

Se aplica en todos los repositorios del proyecto:

- Cada autor tiene su rama: `juanma` y `victor`.
- `main` contiene siempre la versión presentable.
- Los cambios llegan a `main` mediante pull request, revisada por la otra persona.
- Antes de empezar a trabajar, cada autor trae `main` a su rama.

## Alternativas consideradas

- **Trabajar los dos directamente en `main`:** descartada, por el riesgo de pisarse y de dejar `main` en un estado a medias.

## Consecuencias

- La revisión cruzada mejora la calidad y deja un historial claro.
- Hay que sincronizar con `main` a menudo; si pasa mucho tiempo, los conflictos crecen.
