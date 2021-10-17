# BITFACTURA API
Manual para integrar propia aplicación con el sistema <a href="https://bitfactura.com">Bitfactura.com</a>

Gracias a API puedes generar y administrar facturas/recibos/documentos contables desde otro sistema. Puedes también gestionar productos y clientes.

Los ejemplos de API de BitFactura se encuentran tambien en el sistema de BitFactura después de iniciar la sesión en menú <strong> Ajustes > API </strong> y en la página https://bitfactura.bitfactura.com/api 

## ÍNDICE
* [API Token](#token)
* [Parámetros adicionales disponibles al descargar la lista de récords](#list_params)
* [Facturas - ejemplos del uso](#examples)
  * [Descargar facturas del mes actual](#f1)
  * [Descargar facturas con las posiciones](#f2)
  * [Facturas de un cliente](#f3)
  * [Descargar facturas por su ID](#f4)
  * [Descargar PDF](#f5)
  * [Enviar facturas al cliente por e-mail](#f6)
  * [Añadir nueva factura](#f7)
  * [Añadir nueva factura (utilizando ID del cliente, producto, vendedor)](#f8)
  * [Añadir factura similar (utilizando ID de otra factura, por ejemplo de anticipo, final etc.)](#f8b)
  * [Añadir factura rectificativa](#f9)
  * [Actualizar factura](#f10)
  * [Actualizar posiciones en factura](#f10b)
  * [Eliminar posiciones de factura](#f10c)
  * [Añadir posiciones a factura](#f10d)
  * [Cambiar el estado de factura](#f11)
  * [Descargar la lista de definiciones de facturas recurrentes](#f12)
  * [Añadir definición de factura recurrente](#f13)
  * [Actualizar definición de factura recurrente](#f14)
  * [Eliminar factura](#f15)
  * [Vincular factura existente con recibo](#f16)
  * [Descargar archivos adjuntos en el formato ZIP](#f17)
  * [Añadir archivo adjunto](#f17b)
* [Enlace para ver factura y descargarla en PDF](#view_url)
* [Ejemplos del uso - compra de un curso](#use_case1)
* [Facturas - especificación, tipos de campos](#invoices)
* [Clientes](#clients)
  * [Lista de clientes](#k1)
  * [Buscar de clientes por nombre, e-mail, nombre corto o número NIF](#k1b)
  * [Descargar un cliente seleccionado por su ID](#k2)
  * [Descargar un cliente seleccionado por su ID externo](#k2b)
  * [Añadir un cliente](#k3)
  * [Actualizar un cliente](#k4)
  * [Eliminar un cliente](#k5)
* [Productos](#products)
  * [Lista de productos](#p1)
  * [Lista de productos con el estado del stock seleccionado](#p2)
  * [Descargar un producto seleccionado por su ID](#p3)
  * [Descargar un producto seleccionado por su ID con el estado del stock escogido](#p4)
  * [Añadir un producto](#p5)
  * [Actualizar un producto](#p6)
* [Listas de precios](#price_lists)
  * [Conjunto de listas de precios](#pricel1)
  * [Añadir una lista de precios](#pricel2)
  * [Actualizar una lista de precios](#pricel3)
  * [Eliminar una lista de precios](#pricel4)
* [Documentos de almacén](#warehouse_documents)
  * [Todos documentos de almacén](#wd1)
  * [Descargar un documento de almacén por su ID](#wd2)
  * [Añadir documento de almacén TA](#wd3a)
  * [Añadir documento de almacén IM](#wd3)
  * [Añadir documento de almacén SM](#wd4)
  * [Añadir documento de almacén IM para un cliente, departamento y producto ya existente ](#wd5)
  * [Actualizar un documento](#wd6)
  * [Eliminar un documento](#wd7)
  * [Vincular las facturas existentes con el documento de almacén](#wd8)
* [Pagos](#payments)
  * [Todos los pagos](#pl1)
  * [Descargar un pago concreto por su ID](#pl2)
  * [Añadir nuevo pago](#pl3)
  * [Descargar pagos con los datos de facturas vinculadas](#pl4)
  * [Añadir nuevo pago vinculado con la factura existente](#pl5)
  * [Actualizar un pago](#pl6)
  * [Eliminar un pago](#pl7)
* [Categorías](#categories)
  * [Lista de categorías](#cat1)
  * [Descargar nueva categoría por su ID](#cat2)
  * [Añadir nueva categoría](#cat3)
  * [Actualizar categoría](#cat4)
  * [Eliminar categoría de un ID indicado](#cat5)
* [Almacenes](#warehouses)
  * [Lista de almacenes](#wh1)
  * [Descargar un almacén seleccionado por su ID](#wh2)
  * [Añadir nuevo almacén](#wh3)
  * [Actualizar almacén](#cat4)
  * [Eliminar almacén de un ID indicado](#cat5)
* [Departamentos](#departments)
  * [Lista de departamentos](#dep1)
  * [Descargar un departamento por su ID](#dep2)
  * [Añadir nuevo departamento](#dep3)
  * [Actualizar un departamento](#dep4)
  * [Eliminar departamento de un ID indicado](#dep5)
* [Inicio de sesión y descargar del Token por API](#get_token_by_api)
* [Cuentas del sistema](#accounts)
* [Ejemplos en PHP y Ruby](#codes)

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

##Facturas - ejemplos del uso

<a name="f1"/>
Descargar facturas del mes actual

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json?period=this_month&api_token=API_TOKEN&page=1
```

<a name="f2"/>
Descargar facturas con las posiciones

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json?include_positions=true&api_token=API_TOKEN&page=1
```

<a name="f3"/>
Facturas de un cliente

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json?client_id=ID_KLIENTA&api_token=API_TOKEN
```

<a name="f4"/>
Descargar facturas por su ID


```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/100.json?api_token=API_TOKEN
```

<a name="f5"/>
Descargar PDF


```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/100.pdf?api_token=API_TOKEN
```

<a name="f6"/>
Enviar facturas al cliente por e-mail (a la dirección del correo electrónico introducido al crear factura, campo "buyer_email")
```shell
curl -X POST https://YOUR_DOMAIN.bitfactura.com/invoices/100/send_by_email.json?api_token=API_TOKEN
```

Otras opciones PDF:
* print_option=original - Original
* print_option=copy - Copia
* print_option=original_and_copy - Original y copia
* print_option=duplicate Duplicado


<a name="f7"/>
Añadir nueva factura

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "kind":"vat",
            "number": null,
            "sell_date": "2013-01-16",
            "issue_date": "2013-01-16",
            "payment_to": "2013-01-23",
            "seller_name": "Vendedor",
            "seller_tax_no": "5252445767",
            "buyer_name": "Cliente1 Sp. z o.o.",
            "buyer_email": "buyer@testemail.com",
            "buyer_tax_no": "5252445767",
            "positions":[
                {"name":"Producto A1", "tax":23, "total_price_gross":10.23, "quantity":1},
                {"name":"Producto A2", "tax":0, "total_price_gross":50, "quantity":3}
            ]
        }
    }'
```

<a name="f8"/>
Añadir nueva factura - versión mínima (solo los campos obligatorios), no hay que introducir datos completos si tenemos ID de producto (product_id), cliente (client_id) y vendedor (department_id). Se puede introducir también ID del destinatario (recipient_id). 
Se generará factura con IVA del día actual y el plazo de pago de 5 días.

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "payment_to_kind": 5,
            "client_id": 1,
            "positions":[
                {"product_id": 1, "quantity":2}
            ]
        }}'
```

<a name="f8b"/>
Añadir factura similar utilizando ID de otra factura (copy_invoice_from).


Añadir una facura similar del mismo tipo

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DEL_DOCUMENTO_PRINCIPAL,
            "kind": "TIPO_DE_FACTURA"
        }
    }'
```

Añadir factura de anticipo a base del pedido - el % del importe total

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DEL_PEDIDO,
            "kind": "advance",
            "advance_creation_mode": "percent",
            "advance_value": "10",
            "position_name": "Anticipo por realizar el pedido PED-NR"
        }
    }'
```

Añaidr factura de anticipo a base del pedido - importe neto indicado 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DEL_PEDIDO,
            "kind": "advance",
            "advance_creation_mode": "amount",
            "advance_value": "150",
            "position_name": "Anticipo por realizar el pedido PED-NR"
        }
    }'
```

Añadir factura final a base de pedido y facturas de anticipo 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DEL_PEDIDO,,
            "kind": "final",
            "invoice_ids": [ID_DEL_ANTICIPO_1, ID_DEL_ANTICIPO_2, ...]
        }
    }'
```

Añadir factura con IVA a base de factura proforma 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "copy_invoice_from": ID_DE_PROFORMA,
            "kind": "iva"
        }
    }'
```

<a name="f9"/>
Añadir factura rectificativa

```shell
curl http://YOUR_DOMAIN.bitfactura.com/invoices.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "invoice": {
            "kind": "correction",
            "correction_reason": "Cantidad incorrecta",
            "invoice_id": "2432393",
            "from_invoice_id": "2432393",
            "client_id": 1,
            "positions":[
                {"name": "Product A1",
                "quantity":-1,
                "total_price_gross":"-10",
                "tax":"23",
                "kind":"correction",
                "correction_before_attributes": {
                    "name":"Product A1",
                    "quantity":"2",
                    "total_price_gross":"20",
                    "tax":"23",
                    "kind":"correction_before"
                },
                "correction_after_attributes": {
                    "name":"Product A1",
                    "quantity":"1",
                    "total_price_gross":"10",
                    "tax":"23",
                    "kind":"correction_after"
                }
            }]
        }}'
```

<a name="f10"/>
Actualizar factura

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/111.json \
 -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "buyer_name": "Nuevo nombre del cliente"
        }
    }'
```

<a name="f10b"/>
Actualizar posiciones en factura - para editar posición en factura hay que introducir el ID de la posición. 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "name":"test"}]
        }
    }'
```

<a name="f10c"/>
Eliminar posición de factura - para eliminar una posición de factura hay que introducir el ID de la posición con el parámetro _destroy=1.

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"id":32649087, "_destroy": 1}]
        }
    }'
```

<a name="f10d"/>
Añadir posición a facutra. La posición se añade como última.

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "positions": [{"name":"Producto A1", "tax":23, "total_price_gross":10.23, "quantity":1}]
        }
    }'
```

<a name="f11"/>
Cambiar el estado de factura

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/invoices/111/change_status.json?api_token=API_TOKEN&status=ESTADO" -X POST
```

<a name="f12"/>
Descargar la lista de definiciones de facturas recurrentes

```shell
curl https://YOUR_DOMAIN.bitfactura.com/recurrings.json?api_token=API_TOKEN
```

<a name="f13"/>
Añadir definición de factura recurrente

```shell
curl https://YOUR_DOMAIN.bitfactura.com/recurrings.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "recurring": {
            "name": "Nombre del ciclo",
            "invoice_id": 1,
            "start_date": "2016-01-01",
            "every": "1m",
            "issue_working_day_only": false,
            "send_email": true,
            "buyer_email": "mail1@mail.com, mail2@mail.com",
            "end_date": "null"
        }}'
```
<a name="f14"/>
AActualizar definición de factura recurrente (cambio de la fecha de expedición de la factura siguiente)

```shell
curl https://YOUR_DOMAIN.bitfactura.com/recurrings/111.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "recurring": {
            "next_invoice_date": "2016-02-01"
        }
    }'
```

<a name="f15"/>
Eliminar factura

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfacura.com/invoices/INVOICE_ID.json?api_token=API_TOKEN"
```

<a name="f16"/>
Vincular factura existente con recibo

```shell
curl https://YOUR_DOMAIN.bitfacura.test/invoices/ID_FAKTURY.json \
    -X PUT \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
        "api_token": "API_TOKEN",
        "invoice": {
            "from_invoice_id": ID_DEL_RECIBO,
            "invoice_id": ID_DEL_RECIBO,
            "exclude_from_stock_level": true
        }
    }'
```

<a name="f17"/>
Descargar archivos adjuntos en el formato ZIP

```shell
curl -o attachments.zip https://YOUR_DOMAIN.bitfactura.com/invoices/INVOICE_ID/attachments_zip.json?api_token=API_TOKEN
```

<a name="f17b"/>
Añadir nuevo archivo adjunto a factura 

1. Descargar datos necesarios para cargar el archivo:
    ```shell
    curl https://YOUR_DOMAIN.bitfactura.com/invoices/INVOICE_ID/get_new_attachment_credentials.json?api_token=API_TOKEN
    ```

2. Cargar el archivo:
    ```shell
    curl -F 'AWSAccessKeyId=received_AWSAccessKeyId' \
         -F 'key=received_key' \
         -F 'policy=received_policy' \
         -F 'signature=received_signature' \
         -F 'acl=received_acl' \
         -F 'success_action_status=received_success_action_status' \
         -F 'file=@/file_path/name.ext' \
         received_url
    ```

3. Añadir archivo (cargado anteriormente) a factura:
    ```shell
    curl -X POST https://YOUR_DOMAIN.bitfactura.com/invoices/INVOICE_ID/add_attachment.json?api_token=API_TOKEN&file_name=name.ext
    ```



