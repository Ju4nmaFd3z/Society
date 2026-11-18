# Odoo

ERP de Society para el Proyecto Intermodular: qué módulos tiene y qué datos ficticios contiene.

## Objetivo

Society se evalúa también en Odoo, con una empresa llamada Society y su actividad: un SaaS de marketing para hostelería. Todos los datos son inventados y sirven para enseñar cada módulo. No hay datos reales de clientes.

## Acceso

La instancia es [edu-society.odoo.com](https://edu-society.odoo.com). Las cuentas de las tres personas del equipo las gestiona Odoo. Las contraseñas no se guardan en este repositorio.

## Qué hay por bloques

El curso pide cuatro bloques. Todos están cubiertos.

### Operaciones

- **Inventario:** productos y movimientos de almacén, incluida una transferencia interna.
- **Compra:** 2 pedidos de compra confirmados y recibidos por completo.
- **Contabilidad:** 10 facturas de cliente, 2 facturas de proveedor y una factura rectificativa, todas contabilizadas.
- **Empleados:** tres departamentos, con un empleado en cada uno.
- **Proyecto:** 3 proyectos con 33 tareas en total.

### Ventas

- **CRM:** dos equipos, Captación y Fidelización, con una oportunidad y un cliente potencial de origen «Feria H&T Málaga».
- **Ventas y facturación:** 11 pedidos de venta confirmados (8 ya facturados y 3 pendientes de facturar) y 3 presupuestos en borrador.
- **TPV:** una caja con una sesión cerrada y un pedido.
- **Firma electrónica:** una plantilla de contrato y una solicitud firmada.
- **Alquiler:** un producto de alquiler y un pedido recogido y devuelto.

### Productividad

- **Conversaciones:** un canal del equipo.
- **Marketing social:** una publicación programada para el 18 de noviembre a las 9:00. Instagram no está conectado.
- **Marketing por correo:** una lista de contactos y un envío a 5 direcciones de `example.com`.
- **Partes de horas:** 35,5 horas imputadas en dos proyectos.
- **Documentos:** una carpeta con tres documentos.
- **Planificación:** 7 turnos publicados.
- **Ausencias:** 2 ausencias validadas.
- **Reclutamiento:** un puesto con 3 candidatos.

### Sitios web

- **Sitio web y comercio electrónico:** tienda con productos y un pedido de prueba.
- **Chat en vivo:** el canal «Soporte Society». Solo responde si hay un operador conectado.
- **Blog:** un blog con 2 artículos.
- **Foros:** un foro con una pregunta y su respuesta.
- **Eventos:** un evento el 26 de noviembre con 2 inscripciones.
- **eLearning:** un curso con 5 diapositivas y un test.
- **Encuestas:** una encuesta con 2 respuestas.

## Seguimiento en Jira

El trabajo está en las issues SOCIETY-140, SOCIETY-141 y SOCIETY-142.

## Cosas que conviene saber

- Los datos se han creado a mano desde la interfaz y la API de Odoo, no con un seed reproducible. Si se borra la base, hay que volver a cargarlos.
- Los envíos de correo y de marketing salen de verdad, pero a direcciones de `example.com`.
