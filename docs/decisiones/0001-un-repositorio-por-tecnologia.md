# 0001. Un repositorio por tecnología y uno general

- **Fecha:** 2026-10-19
- **Estado:** aceptada

## Contexto

Society lo desarrollan dos autores con varias tecnologías. El proyecto exige tener un repositorio por cada una y, además, una referencia común con la documentación.

## Decisión

- Un repositorio general, `Society`, con la documentación común: producto, diseño, arquitectura, marca y decisiones. Enlaza al resto de repositorios.
- Un repositorio por tecnología: `Society-Kotlin` para la app Android y `Society-TypeScript` para la parte desarrollada en TypeScript.
- Cada repositorio de tecnología documenta su código y enlaza al general para lo común.
- Si se incorpora una tecnología nueva, se crea su repositorio y se enlaza desde el general.

## Consecuencias

- Cada tecnología se versiona, se construye y se publica de forma independiente.
- La documentación común está en un solo lugar y no se duplica.
- Los cambios que afectan a varias tecnologías hay que coordinarlos entre repositorios.
- Los enlaces entre repositorios hay que mantenerlos al día.
