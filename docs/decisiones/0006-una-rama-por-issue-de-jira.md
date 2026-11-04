# 0006. Una rama por issue de Jira

- **Fecha:** 2026-11-04
- **Estado:** aceptada. Sustituye a la [0002](0002-flujo-de-trabajo-con-ramas.md)

## Contexto

Con una rama por persona (`juanma`, `victor` y `pablo`), cada rama mezcla trabajos de issues distintas, y desde Jira no se ve qué rama corresponde a cada tarea. Además, como la rotación de carriles cambia cada semana lo que hace cada uno, el nombre de la persona ya no dice nada del contenido de la rama.

## Decisión

Se aplica en todos los repositorios del proyecto:

- Cada issue de Jira se trabaja en su propia rama, que se llama igual que la clave de la issue: `SOCIETY-37`.
- La rama se crea desde `main` actualizada, justo antes de empezar la issue.
- Los cambios llegan a `main` mediante pull request, revisada por otra persona del equipo. El título de la pull request empieza por la clave de la issue: `SOCIETY-37 rename feature/inicio to feature/empezamos`.
- `main` contiene siempre la versión presentable.
- Si la rama dura varios días, se trae `main` a la rama antes de seguir trabajando.
- Cuando la pull request se fusiona, la rama se borra.
- Las ramas por persona dejan de usarse.

## Alternativas consideradas

- **Mantener una rama por persona:** descartada, porque mezcla issues y no deja relacionar el código con Jira.
- **Trabajar todos directamente en `main`:** descartada, por el riesgo de pisarse y de dejar `main` en un estado a medias.

## Consecuencias

- Todo el trabajo queda centralizado en Jira: cada issue tiene su rama y su pull request, y Jira las enlaza solas porque llevan la clave.
- Las pull requests son más pequeñas y más fáciles de revisar.
- Hay más ramas abiertas a la vez, así que hay que borrarlas al fusionarlas.
- Todo trabajo necesita una issue en Jira antes de empezar.
