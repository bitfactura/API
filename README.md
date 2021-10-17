# BITFACTURA API
Manual para integrar propia aplicación con el sistema <a href="https://bitfactura.com">Bitfactura.com</a>

Gracias a API puedes generar y administrar facturas/recibos/documentos contables desde otro sistema. Puedes también gestionar productos y clientes.

Los ejemplos de API de BitFactura se encuentran tambien en el sistema de BitFactura después de iniciar la sesión en menú <strong> Ajustes > API </strong> y en la página https://bitfactura.bitfactura.com/api 

## ÍNDICE
* [API Token](#token)
* [Parámetros adicionales disponibles al descargar la lista de récords](#list_params)
* [Facturas - ejemplos del uso](#examples)
  * Descargar facturas del mes actual
  * Descargar facturas con las posiciones
  * Facturas de un cliente
  * Descargar facturas por su ID
  * Descargar PDF
  * Enviar facturas al cliente por e-mail
  * Añadir nueva factura (utilizando ID del cliente, producto, vendedor)
  * Añadir factura similar (utilizando ID de otra factura, por ejemplo de anticipo, final etc.)
  * Añadir factura rectificativa
  * Actualizar factura
  * Actualizar posiciones en factura 
  * Eliminar posiciones de factura
  * Añadir posiciones a factura
  * Cambiar el estado de factura
  * Descargar la lista de definiciones de factura recurrente
  * Eliminar factura
  * Vincular factura existente con recibo
  * Descargar archivos adjuntos en el formato ZIP
  * Añadir archivo adjunto
* Enlace para ver factura y descargarla en PDF
* Ejemplos del uso - compra del curso
* Facturas - especificación, tipos de campos
* Clientes
  * Lista de clientes
  * Buscar de clientes por nombre, e-mail, nombre corto o número NIF
  * Descargar un cliente seleccionado por su ID
  * Descargar un cliente seleccionado por su ID externo
  * Añadir un cliente
  * Actualizar un cliente
  * Eliminar un cliente
* Productos
  * Lista de productos
  * Lista de productos con el estado del stock seleccionado
  * Descargar un producto seleccionado por su ID
  * Descargar un producto seleccionado por su ID con el estado del stock escogido
  * Añadir un producto
  * Actualizar un producto
* Listas de precios
  * Conjunto de listas de precios
  * Añadir una lista de precios
  * Actualizar una lista de precios
  * Eliminar una lista de precios
* Documentos de almacén
  * Todos documentos de almacén
  * Descargar un documento de almacén por su ID
  * Añadir documento de almacén TA
  * Añadir documento de almacén IM
  * Añadir documento de almacén SM
  * Añadir documento de almacén IM para un cliente, departamento y producto ya existente 
  * Actualizar un documento
  * Eliminar un documento
  * Vincular las facturas existentes con el documento de almacén
* Pagos
  * Todos los pagos
  * Descargar un pago concreto por su ID
  * Añadir nuevo pago
  * Descargar pagos con los datos de facturas vinculadas
  * Añadir nuevo pago vinculado con la factura existente
  * Actualizar un pago
  * Eliminar un pago
* Categorías
  * Lista de categorías
  * Descargar nueva categoría por su ID
  * Añadir nueva categoría
  * Actualizar categoría
  * Eliminar categoría de un ID indicado
* Almacenes
  * Lista de almacenes
  * Descargar un almacén seleccionado por su ID
  * Añadir nuevo almacén
  * Actualizar almacén
  * Eliminar almacén de un ID indicado
* Departamentos
  * Lista de departamentos
  * Descargar un departamento por su ID
  * Añadir nuevo departamento
  * Actualizar un departamento
  * Eliminar departamento de un ID indicado
* Inicio de sesión y descargar del Token por API
* Cuentas del sistema
* Ejemplos en PHP y Ruby

<a name="token"/>

## API Token
<code>API_TOKEN</code> token hay que descargar de los ajustes de aplicación (Ajustes > Ajustes de cuenta> Integración > Código de autorización de API)

<a name="list_params"/>

## Parámetros adicionales disponibles al descargar la lista de récords
Se puede utilizar paramentros adicionales, los mismos utilizados en la aplicación, por ejemplo `page=`, `period=` etc.

Parámetro `page=` permite iterar registtros paginados. EL valor predeterminado es de `1` y muestra primeros N de records, donde N es el límite de cantidad de records. Para obtener N posteriores hay que causar una acción con parámentro `page=2`, etc.

Parámetro `period=` facilita escoger records de un período específico. 
Puede tener los siguientes valores:
- last_12_months
- this_month
- last_30_days
- last_month
- this_year
- last_year
- all
- more (aquí hay que introducir parámentros adicionales date_from (p. ej. "2018-12-16") y date_to (p. ej. "2018-12-21"))

Parámetro `include_positions=` con el valor `true` facilita descargar la lista de records con las posiciones 

Parámetro `income=` con el valor `no` facilita descargar facturas de gasto 

<a name="examples"/>

