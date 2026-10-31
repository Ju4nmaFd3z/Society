# 0002. Flujo de trabajo con una rama por persona

- **Fecha:** 2026-10-19
- **Estado:** aceptada (actualizada el 2026-10-31 al incorporarse Pablo Jiménez)

## Contexto

Tres personas trabajan en los mismos repositorios. Hace falta que nadie pise el trabajo de los demás y que la página principal de cada repositorio se vea siempre bien.

## Decisión

Se aplica en todos los repositorios del proyecto:

- Cada persona tiene su rama: `juanma`, `victor` y `pablo`.
- `main` contiene siempre la versión presentable.
- Los cambios llegan a `main` mediante pull request, revisada por otra persona del equipo.
- Antes de empezar a trabajar, cada persona trae `main` a su rama.

## Alternativas consideradas

- **Trabajar todos directamente en `main`:** descartada, por el riesgo de pisarse y de dejar `main` en un estado a medias.

## Consecuencias

- La revisión cruzada mejora la calidad y deja un historial claro.
- Hay que sincronizar con `main` a menudo; si pasa mucho tiempo, los conflictos crecen.
