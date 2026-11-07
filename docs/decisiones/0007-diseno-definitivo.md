# 0007. Diseño definitivo: cristal sobre grafiti con texto legible

- **Fecha:** 2026-11-06
- **Estado:** aceptada. Completa la [0005](0005-estetica-de-cristal.md)

## Contexto

La decisión 0005 adoptó la estética de cristal de la versión 3 de Figma. Al completar todas las pantallas aparecieron cuatro problemas:

- El texto pequeño se perdía cuando caía sobre una mancha de grafiti.
- Algunas sombras tenían el borde duro.
- Las etiquetas eran píldoras de colores con un punto, un aspecto genérico que el equipo no quiere.
- La variante «Cristal legible» (v4) corregía la legibilidad, pero a costa de aplanar el cristal y de cubrir el grafiti con un degradado azul y amarillo. Perdía la identidad de la marca y no gustó a nadie del equipo.

## Decisión

- El diseño definitivo es el del [archivo de Figma](https://www.figma.com/design/G4bwEgiP3eRRlNoHK4ftVA/Society--Final-Version-?node-id=2027-1844), que pasa a ser el oficial.
- Se conservan la pared de grafitis original, sin filtros de color, y el glassmorfismo como estilo principal.
- El cristal tiene tres niveles de translucidez, con un filo de luz y sombras muy difuminadas.
- El texto pequeño va siempre sobre cristal o lleva un halo claro, y los textos secundarios son más oscuros.
- Las etiquetas son editoriales: categoría con icono, confianza con un medidor de tres barras, estados que se distinguen por su forma, y avisos con un trazo de rotulador. No hay píldoras de colores.
- La variante «Cristal legible» (v4) queda descartada.
- Los valores concretos están en `docs/diseno/sistema-de-diseno.md`, que pasa a ser la guía de estilos de la app y del panel web.

## Alternativas consideradas

- **Quedarse con la versión 3 tal cual:** descartada, por los problemas de legibilidad y de sombras.
- **Cristal legible (v4):** descartada, porque pierde el cristal y el grafiti, que son la identidad de la marca.

## Consecuencias

- El tema de la app Android y los estilos del panel web se adaptan a estos valores (SOCIETY-129 y SOCIETY-33 en Jira), no a los de la versión 3.
- Hay que cambiar tres cosas de la hoja de estilos actual de la web: los textos secundarios, las sombras y los niveles de cristal.
- Cualquier cambio posterior del diseño se hace en este archivo de Figma y se refleja en la guía de estilos.
