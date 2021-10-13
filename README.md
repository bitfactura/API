# BITFACTURA API
Manual para integrar propia aplicación con el sistema <a href="https://bitfactura.com">Bitfactura.com</a>

Gracias a API puedes generar y administrar facturas/recibos/documentos contables desde otro sistema. Puedes también gestionar productos y clientes.

Los ejemplos de API de BitFactura se encuentran tambien en el sistema de BitFactura después de iniciar la sesión en menú <strong> Ajustes > API </strong> y en la página https://bitfactura.bitfactura.com/api 

## ÍNDICE
* API Token
* Parámetros adicionales disponibles al descargar la lista de récords
* Facturas - ejemplos del uso
  * Descargar facturas del mes actual
  * Descargar facturas con las posiciones
  * Facturas de un cliente
  * Descargar facturas por ID
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
  * lista de clientes
  * buscar de clientes por nombre, e-mail, nombre corto o número NIF
  * descargar un cliente seleccionado por ID
  * descargar un cliente seleccionado por ID externo
  * añadir un cliente
  * actualizar un cliente
  * eliminar un cliente
* Productos
  * lista de productos
  * lista de productos con el estado del stock seleccionado
  * descargar un producto seleccionado por ID
  * descargar un producto seleccionado por ID con el estado del stock escogido
  * añadir un producto
  * actualizar un producto
