# Modales y estados

Modales, estados vacíos y errores que el diagrama oficial define para las pantallas con datos.

## Objetivo

Que cada pantalla tenga resuelto qué ve el usuario cuando algo sale distinto de lo esperado.

## Diseño

- Flujo: [Flujo de navegación](../flujo-de-navegacion/README.md).
- Detalle: páginas de cada pantalla en [`diagrama-pantallas-detalle.drawio`](../../arquitectura/diagramas/diagrama-pantallas-detalle.drawio).

## Modales

- **M02 · ¿Es tu local?:** se abre al pulsar «Es mi local» en [02. Empezamos](02-empezamos.md). Lleva la casilla obligatoria «Soy propietario o gestor». Confirmar crea el local y pasa a [03. Análisis](03-analisis.md). Si el local ya está dado de alta con otra cuenta, avisa del error.
- **M03 · ¿Cancelar el análisis?:** se abre al pulsar «Atrás» en [03. Análisis](03-analisis.md). Confirmar cancela el análisis y vuelve a [02. Empezamos](02-empezamos.md).
- **M04 · Edición de un campo:** se abre con el lápiz de [04. Así te ve Internet](04-asi-te-ve-internet.md). Guarda un solo campo, y el campo pasa a tener el origen «Tú».
- **M13 · ¿Cerrar sesión?:** se abre desde [13. Perfil](13-perfil.md). Confirmar borra la sesión y vuelve a [01. Inicio](01-inicio.md).
- **M09P · Conecta tu Instagram para publicar:** se abre al publicar sin Instagram conectado. Ofrece conectar la cuenta o publicar a mano.

## Estados vacíos y de error

- **02 sin resultados:** «No encontramos tu local», con la salida «Añádelo a mano».
- **05 sin oportunidades:** tarjeta «Crea tu primera pieza».
- **15 sin piezas este mes:** «Aún no hay piezas este mes» y el botón «Crear contenido». Es P1.
- **Error de conexión:** «Revisa tu conexión» y el botón «Reintentar». Las peticiones de lectura se reintentan tres veces. Las de escritura nunca se repiten solas.
- **Cargando:** la Home muestra esqueletos mientras llegan los datos.

## Implementación

- Android: pendiente. Cada modal se implementa con su pantalla.
