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
Actualizar definición de factura recurrente (cambio de la fecha de expedición de la factura siguiente)

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
<a name="view_url"/>

## Enlace para ver factura y descargarla en PDF

Después de descargar los datos de factura a través de, por ejemplo: 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/invoices/100.json?api_token=API_TOKEN
```

API devuelve, entre otros, campo `token`. A base de ese campo podemos obtener enlaces de la vista previa de factura para descargar PDF. Estos enlaces permiten ver la factura sin tener que iniciar la sesión. Podemos pasarlos al cliente para que pueda ver el documento y descargarlo en formato PDF.
zwraca nam m.in. pole `token` na podstawie którego możemy otrzymać linki do podglądu faktury oraz do pobrania PDF-a z wygenrowaną fakturą.


Los enlaces tienen la siguiente estructura:

vista previa: `https://YOUR_DOMAIN.bitfactura.com/invoice/{{token}}`
PDF: `https://YOUR_DOMAIN.bitfactura.com/invoice/{{token}}.pdf`

Por ejemplo, para el token `HBO3Npx2OzSW79RQL7XV2` PDF público será disponible con enlace `https://YOUR_DOMAIN.bitfactura.com/invoice/HBO3Npx2OzSW79RQL7XV2.pdf`

<a name="use_case1"/>

## Ejemplos del uso - compra de un curso

`TODO`

Ejemplo del flow de un Portal que genera factura proforma, la manda al cliente y, después de realizar el pago, le manda invitación al curso.

* Cliente rellena los datos en Portal
* Portal llama a API de bitfactura.com y crea factura 
* Portal manda al cliente una factura proforma en forma de PDF con enlace paga el pago 
* Cliente realiza el pago por la factura proforma (p. ej. a través de PayPal) 
* BitFactura.com recibe la información sobre el pago, crea una factura final, la manda al cliente y llama al API del Portal
* Después de recibir la información sobre el pago (a través de API) Portal manda al Cliente la invitación al curso. 


<a name="invoices"/>

## Facturas


* `GET /invoices/1.json` descargar factura
* `POST /invoices.json` añadir nueva factura
* `PUT /invoices/1.json` actualizar una factura
* `DELETE /invoices/1.json` eliminar una factura


Ejemplo: añadir nueva factura (versión mínima; no hay que introducir los datos enteros si tenemos ID de producto, cliente y vendedor). Se generará factura con IVA del día actual y el plazo de pago de 5 días. El campo department_id define la empresa (o el departamento) que genera la facutra (se encuentra en Ajustes > Empresa/Departamento).

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

Campos en factura

```shell
"number" : "13/2012", - número de facura (si no se rellena ese campo el número se genera automáticamente) 
"kind" : "vat", - tipo de factura (con IVA, proforma, bill, receipt, advance, correction, vat_mp, invoice_other, vat_margin, final, estimate)
"income" : "1", - factura de ingresos (1) o gastos (0)
"issue_date" : "2013-01-16", - fecha de expedición
"place" : "Madrid", - lugar de expedición
"sell_date" : "2013-01-16", - fecha de venta (puede ser una fecha o mes en formato 2012-12)
"category_id" : "", - id de la categoría
"department_id" : "1", - id del departamento (en Ajustes > Empresa/Departamento hay que hacer clic en un departamento para ver el ID en el enlace); Se descargarán los datos predeterminados después de dejar vacío este campo y el campo 'seller_name'
"accounting_kind": "", - tipo de gasto para facturas de gasto - (purchases, expenses, media, salary, incident, fuel0, fuel_expl50, fuel_expl75, fuel_expl100, fixed_assets, fixed_assets50, no_vat_deduction)
"seller_name" : "Emprea xx", - vendedor
"seller_tax_no" : "525-244-57-67", - número de identificación fiscal del vendedor (NIF)
"seller_tax_no_kind" : "", - tipo del número de identificación fiscal del vendedor; campo vacío (por defecto) se interpreta como NIF; en otros casos se entiende como inscripción personalizada (CIF etc.)
"seller_bank_account" : "24 1140 1977 0000 5921 7200 1001", - número de cuenta del vendedor
"seller_bank" : "BRE Bank",
"seller_post_code" : "02-548",
"seller_city" : "Madrid",
"seller_street" : "calle xxx",
"seller_country" : "", - país del vendedor (ISO 3166)
"seller_email" : "pagos@radgost123.com",
"seller_www" : "",
"seller_fax" : "",
"seller_phone" : "",
"client_id" : "-1" - id del cliente (con -1 se creará un cliente en el sistema)
"buyer_name" : "Nombre del cliente" - cliente
"buyer_tax_no" : "525-244-57-67", - úmero de identificación fiscal del cliente (NIF)
"buyer_tax_no_kind" : "", - tipo del número de identificación fiscal del cliente; pole puste (domyślnie) jest interpretowane jako "NIP"; campo vacío (por defecto) se interpreta como NIF; en otros casos se entiende como inscripción personalizada (CIF etc.)
"disable_tax_no_validation" : "",
"buyer_post_code" : "30-314", - código postal del cliente
"buyer_city" : "Warszawa", - ciudad del cliente
"buyer_street" : "Nowa 44", - calle del cliente
"buyer_country" : "PL", - país del cliente (ISO 3166)
"buyer_note" : "", - comentario adicional del cliente
"buyer_email" : "", - e-mail del cliente
"recipient_id" : "", - id del receptor (id del cliente desde el systemu)
"recipient_name" : "", - nombre del receptor
"recipient_street" : "", - calle del receptor
"recipient_post_code" : "", - código postal del receptor
"recipient_city" : "", - ciudad del receptor 
"recipient_country" : "", - país del receptor (ISO 3166)
"recipient_email" : "", - e-mail del receptor
"recipient_phone" : "", - móvil del receptor
"recipient_note" : "", - comentario adicional del receptor
"additional_info" : "0" - mostrar o no campo adicional en factura
"additional_info_desc" : "xxx" - nombre de columna adicional en posición de factura 
"show_discount" : "0" - mostrar o no descuento
"payment_type" : "transfer",
"payment_to_kind" : fecha de pago. Se puede establecer una fecha concreta en el campo "payment_to" si aparece aquí "other_date". Una cifra significa el número de días: 5 son 5 días para pagar 
"payment_to" : "2013-01-16",
"status" : "issued",
"paid" : "0,00",
"oid" : "pedido", - número del pedido (p. ej. desde un sistema para pedidos externo)
"oid_unique" : si ese campo contiene'yes' el sistema no permitirá crear dos facturas con el mismo ID (puede ser útil al sincronizar con la tienda online)
"warehouse_id" : "1090",
"seller_person" : "Nombre Apellido",
"buyer_first_name" : "Nombre",
"buyer_last_name" : "Apellido",
"paid_date" : "",
"currency" : "EUR",
"lang" : "es",
"use_moss" : "0" - usar o no Factura OSS
"exchange_currency" : "", - moneda convertida (se convierte la suma y el impuesto a otra moneda), p.ej. "EUR"
"exchange_kind" : "", - fuente de tasa de cambio de moneda ("ecb", "nbp", "cbr", "nbu", "nbg", "own")
"exchange_currency_rate" : "", - propia tasa de cambio de moneda (utilizada cuando el parámetro exchange_kind está ajustado "own")
"invoice_template_id" : "1",
"description" : "- descripción de factura",
"description_footer" : "", - descripción en el pie de página
"description_long" : "", - descripción en lado reverso de factura
"invoice_id" : "" - campo con el id del documento vinculado, por ejemplo pedido o factura base en caso de factura recurrente,
"from_invoice_id" : "" - id de facutra base (útil al generar factura final a base de factura proforma),
"delivery_date" : "" - fecha de recibir el documento (solo en el caso de gastos),
"buyer_company" : "1" - si el cliente es una empresa
"additional_invoice_field" : "" - valor del campo adicional en factura, Ajustes > Ajustes de cuenta > Configuración > Facturas y documentos > Campos adicionales en factura)
"internal_note" : "" - contenida de la nota privada en factura (no visible en impresión)
"exclude_from_stock_level" : "" - información si el sistema debería incluir la factura en estado de almacén (true p.ej cuando factura se genera a base de recibo)
"procedure_designations" : [] - indicaciones sobre los procedimientos
"positions":
   		"product_id" : "1",
   		"name" : "BitFactura Básico",
   		"additional_info" : "", - información adicional en la posición de factura
   		"discount_percent" : "", - % del descuento (ojo: para que se calcule el descuento hay que ajustar el campo 'show_discount' a 1 y verificar que el campo "Forma de cálculo de descuento" está ajustado a "porcentaje")
   		"discount" : "", - descuento a base de cuota (ojo: para que se calcule el descuento hay que ajustar el campo 'show_discount' a 1 y verificar que el campo "Forma de cálculo de descuento" está ajustado a "cuota"
   		"quantity" : "1",
   		"quantity_unit" : "cantidad",
   		"price_net" : "59,00", - si no se introduce el sistema lo se calculará
   		"tax" : "23", - puede ser un tipo o N/A (no se aplica) o exento de impuesto
   		"price_gross" : "72,57", - si no se introduce el sistema lo se calculará
   		"total_price_net" : "59,00", - si no se introduce el sistema lo se calculará
   		"total_price_gross" : "72,57",
   		"code" : "" - código del producto,
"calculating_strategy":
{
  "position": "default" lub "keep_gross" - método de calcular cuotas en posiciones de factura 
  "sum": "sum" lub "keep_gross" lub "keep_net" - método de sumar cuotas de posiciones 
  "invoice_form_price_kind": "net" lub "gross" - precio unitario en el formulario de factura
},
"split_payment" : "1" - 1 o 0 dependiendo si factura está sujeto a split payment o no
"accounting_vat_tax_date": "2020-12-18", fecha de contabilizar el impuesto (si activado en ajustes)
"accounting_income_tax_date": "2020-12-18", fecha de contabilizar el impuesto sobre la renta (si activado en ajustes)
```

Valor de los campos

Pole: `kind`
```shell
	"vat" - factura con IVA
	"proforma" - factura Proforma
	"bill" - factura sin IVA
	"advance" - factura de anticipo
	"final" - factura final
	"correction" - factura rectificativa
	"invoice_other" - otra factura
	"vat_margin" - factura margen
	"estimate" - pedido
 "kp" - recibo de caja
	"correction_note" - credit note
	"accounting_note" - nota contable
	"client_order" - propio documento no contable
	"import_service" - import de servicio
	"import_service_eu" - import de servicio de UE
	"import_products" - import de mercancías - procedimiento simplificado)
	"export_products" - import de mercancías
```

Campo: `lang`
```shell
	"pl" - factura en polaco
	"en" - inglés
	"en-GB" - inglés UK
	"de" - alemán
	"fr" - francés
	"cz" - checo
	"ru" - ruso
	"es" - español
	"it" - italiano
	"nl" - neerlandés
	"hr" - croata
	"ar" - árabe
	"sk" - eslovaco
	"sl" - esloveno
	"el" - greco
	"et" - estonio
	"cn" - chino
	"hu" - húngaro
	"tr" - turco
	"fa" - idioma persa

	se puede generar facturas bilingües combinando los símbolos de idiomas a través de berra invertida, p.ej.:
	"es/en" - español e inglés
```

Campo: `income`
```shell
	"1" - factura de ingresos
	"0" - factura de gastos
```

Campo: `accounting_kind`
```shell
  "purchases" - compra de merciancías y materiales
  "expenses" - gastos de explotación
  "media" - medios y servicios de telecomunicación
  "salary" - remuneraciones
  "incident" - gastos secundarios de compra
  "fuel0" - compra de compustible para los vehículos 0%
  "fuel_expl50" - compra de compustible y uso del vehículo 50%
  "fuel_expl75" - compra de compustible y uso del vehículo 75%
  "fuel_expl100" - compra de compustible y uso del vehículo 100%
  "fixed_assets" - capital fijo
  "fixed_assets50" - capital fijo 50%
  "no_vat_deduction" - imposibilidad de deducir el impuesto IVA
```

Campo: `payment_type`
```shell
	"transfer" - transferencia
	"card" - tarjeta 
	"cash" -  efectivo
	"barter" - trueque
	"cheque" - cheque
	"bill_of_exchange" - letra de cambio
	"cash_on_delivery" - pago contrarrembolso
	"compensation" - compensación 
	"letter_of_credit" - carta de crédito
	"paypal" - PayPal
	"off" - "no mostrar"
	"cualquier_otro_texto"
```

Campo: `status`
```shell
	"issued" - expedida
	"sent" - enviada
	"paid" - pagada
	"partial" - pagada parcialmente
	"rejected" - rechazada
```

Campo: `discount_kind` - tipo del descuento
```shell
	"percent_unit" - calculado a base de precio unitario
	"percent_total" - calculado a base de precio total
	"amount" - calculado a base de cuota
```

Campo: `np_tax_kind` - base de aplicación de N/A
> ¡OJO! Para cada posición que no está sujeta a impuesto IVA hay que introducir el tipo N/A (p.ej. "tax": "na").

> Variantes del tipo "N/A" reconocidos por el sistema: "np", "n/a", "not applicable", "na" - o sus versiones en mayúscula.

```shell
    "export_service" - entrega de mercancías y prestación de servicios fuera del teritorio del país
    "export_service_eu" - incluyendo la prestación de servicios de acuerdo con las declaraciones de la UE
    "not_specified" - no defiido
```

## Clientes

<a name="k1"/>
Lista de clientes

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?api_token=API_TOKEN&page=1"
```

<a name="k1b"/>
Buscar de clientes por nombre, e-mail, nombre corto o número NIF

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?api_token=API_TOKEN&name=CLIENT_NAME"
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?api_token=API_TOKEN&email=EMAIL_ADDRESS"
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?api_token=API_TOKEN&shortcut=SHORT_NAME"
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?api_token=API_TOKEN&tax_no=TAX_NO"
```

<a name="k2"/>
Descargar un cliente seleccionado por su ID

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/clients/100.json?api_token=API_TOKEN"
```

<a name="k2b"/>
Descargar un cliente seleccionado por su ID externo

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/clients.json?external_id=100&api_token=API_TOKEN"
```

<a name="k3"/>
Añadir un cliente

```shell
curl https://YOUR_DOMAIN.bitfactura.com/clients.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Cliente1",
            "tax_no": "5252445767",
            "bank" : "banco1",
            "bank_account" : "bank_account1",
            "city" : "city1",
            "country" : "",
            "email" : "email@gmail.com",
            "person" : "person1",
            "post_code" : "post-code1",
            "phone" : "phone1",
            "street" : "street1"
        }}'
```

<a name="k4"/>
Actualizar un cliente

```shell
curl https://YOUR_DOMAIN.bitfactura.com/clients/111.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "client": {
            "name": "Klient2",
            "tax_no": "52524457672",
            "bank" : "bank2",
            "bank_account" : "bank_account2",
            "city" : "city2",
            "country" : "PL",
            "email" : "email@gmail.com",
            "person" : "person2",
            "post_code" : "post-code2",
            "phone" : "phone2",
            "street" : "street2"
        }}'
```
<a name="k5"/>
Eliminar un cliente

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.comclients/CLIENT_ID.json?api_token=API_TOKEN"
```

<a name="products"/>

## Productos

<a name="p1"/>
Lista de productos

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/products.json?api_token=API_TOKEN&page=1"
```

<a name="p2"/>
Lista de productos con el estado del stock seleccionado

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/products.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID&page=1"
```

<a name="p3"/>
Descargar un producto seleccionado por su ID

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/products/100.json?api_token=API_TOKEN"
```

<a name="p4"/>
Descargar un producto seleccionado por su ID con el estado del stock escogido

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/products/100.json?api_token=API_TOKEN&warehouse_id=WAREHOUSE_ID"
```

<a name="p5"/>
Añadir un producto

```shell
curl https://YOUR_DOMAIN.bitfactura.com/products.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA",
            "code": "A001",
            "price_net": "100",
            "tax": "23"
        }}'
```

<a name="p6"/>
Actualizar un producto

```shell
curl https://YOUR_DOMAIN.bitfactura.comproducts/333.json \
    -X PUT \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json'  \
    -d '{"api_token": "API_TOKEN",
        "product": {
            "name": "PoroductAA2",
            "code": "A0012",
            "price_gross": "102",
	    "tax": "23"
        }}'
```
**OJO:** Precio bruto se calcula a base del valor neto e impuesto, no se lo puede editar directamente por API. 


<a name="price_lists"/>
## Listas de precios

<a name="warehouse_documents"/>

<a name="pricel1"/>
Conjunto de listas de precios

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/price_lists.json?api_token=API_TOKEN"
```
se puede utilizar los mismos parámetros que se utilizan en la aplicación (en la página con la lista de facturas) 

<a name="pricel2"/>
Añadir una lista de precios

```shell
curl https://YOUR_DOMAIN.bitfactura.com/price_lists.json
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "price_list": {
                    "name": "Nombre de lista de precios",
		    "description": "Descripción",
		    "currency": "EUR",
                    "price_list_positions_attributes": {
		    	"0": {
				"priceable_id": "ID del producto",
				"priceable_name": "Nombre del producto",
				"priceable_type": "Product",
				"use_percentage": "0",
				"percentage": "",
				"price_net": "111.0",
				"price_gross": "136.53",
				"use_tax": "1",
				"tax": "23"
			}
		    }
                }}'
```

<a name="pricel3"/>
Actualizar una lista de precios

```shell
curl https://YOUR_DOMAIN.bitfactura.com/price_lists/100.json
		-X PUT
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "price_list": {
                    "name": "Nombre de lista de precios",
		    "description": "Descripción",
		    "currency": "EUR",
                }}'
```

<a name="pricel4"/>
Eliminar una lista de precios

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/price_lists/100.json?api_token=API_TOKEN"
```

## Documentos de almacén

<a name="wd1"/>
WTodos documentos de almacén

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/warehouse_documents.json?api_token=API_TOKEN"
```
se puede utilizar los mismos parámetros que se utilizan en la aplicación (en la página con la lista de facturas) 

<a name="wd2"/>
Descargar un documento de almacén por su ID

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/warehouse_documents/555.json?api_token=API_TOKEN"
```

<a name="wd3a"/>
Añadir documento de almacén TA

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents.json
                -H 'Accept: application/json'
                -H 'Content-Type: application/json'
                -d '{
                "api_token": "API_TOKEN",
                "warehouse_document": {
                    "kind":"mm",
                    "number": null,
                    "warehouse_id": "1",
                    "issue_date": "2017-10-23",
                    "department_name": "Department1 SA",
                    "client_name": "Client1 SA",
                    "warehouse_actions":[
                        {"product_name":"Produkt A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1, "warehouse2_id":13}
                    ]
                }}'
```

<a name="wd3"/>
Añadir documento de almacén IM

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_name":"Producto A1", "purchase_tax":23, "purchase_price_net":10.23, "quantity":1},
						{"product_name":"Producto A2", "purchase_tax":0, "purchase_price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd4"/>
Añadir documento de almacén SM

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"wz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_name": "Department1 SA",
					"client_name": "Client1 SA",
					"warehouse_actions":[
						{"product_id":"333", "tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd5"/>
Añadir documento de almacén IM para un cliente, departamento y producto ya existente 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse_document": {
					"kind":"pz",
					"number": null,
					"warehouse_id": "1",
					"issue_date": "2017-10-23",
					"department_id": "222",
					"client_id": "111",
					"warehouse_actions":[
						{"product_id":"333", "purchase_tax":23, "price_net":10.23, "quantity":1},
						{"product_id":"333", "purchase_tax":0, "price_net":50, "quantity":2}
					]
				}}'
```

<a name="wd6"/>
Actualizar un documento

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents/555.json
			 	-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"warehouse_document": {
						"client_name": "New client name SA"
				    }}'
```

<a name="wd7"/>
Eliminar un documento

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/warehouse_documents/100.json?api_token=API_TOKEN"
```

<a name="wd8"/>
Vincular las facturas existentes con el documento de almacén

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouse_documents/555.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"warehouse_document": {
						"invoice_ids": [
							100,
							111
						]
					}}'
```

<a name="payments"/>

## Pagos

<a name="pl1"/>
Todos pagos

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/banking/payments.json?api_token=API_TOKEN"
```

se puede utilizar los mismos parámetros que se utilizan en la aplicación (en la página con la lista de facturas) 


<a name="pl2"/>
Descargar un pago concreto por su ID

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/banking/payment/100.json?api_token=API_TOKEN"
```

<a name="pl3"/>
Añadir nuevo pago

```shell
curl https://YOUR_DOMAIN.bitfactura.com/banking/payments.json \
				-H 'Accept: application/json' \
				-H 'Content-Type: application/json' \
				-d '{
					"api_token": "API_TOKEN",
					"banking_payment": {
						"name":"Payment 001",
						"price": 100.05,
						"invoice_id": null,
						"paid":true,
						"kind": "api"
				    }}'
```

<a name="pl4"/>
Descargar pagos con los datos de facturas vinculadas

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/banking/payments.json?include=invoices&api_token=API_TOKEN"
```
<a name="pl5"/>
Añadir nuevo pago vinculado con la factura existente

```shell
curl https://YOUR_DOMAIN.bitfactura.com/banking/payments.json \
				-H 'Accept: application/json' \
				-H 'Content-Type: application/json' \
				-d '{
					"api_token": "API_TOKEN",
					"banking_payment": {
						"name":"Payment 003",
						"price": 200,
						"invoice_ids": [555, 666],
						"paid":true,
						"kind": "api"
						}}'
```

Hay que recordar que las facturas se pagan según el orden del atributo `invoice_ids`. Si factura con ID = 555 se ha expedido con el valor de 100 EUR y factura con ID = 666 con el valor de 200 EUR, después de añadir pago la primera factura se pagará en su totalidad, y la segundo en la mitad. 


<a name="pl6"/>
Actualizar un pago

```shell
curl https://YOUR_DOMAIN.bitfactura.com/banking/payments/555.json
				-X PATCH
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{"api_token": "API_TOKEN",
					"banking_payment": {
						"name": "New payment name",
						"price": 100
						}}'
```

<a name="pl7"/>
Eliminzr un pago

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/banking/payments/555.json?api_token=API_TOKEN"
```

<a name="categories"/>

## Categorías

<a name="cat1"/>
Lista de categorías

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/categories.json?api_token=API_TOKEN"
```

<a name="cat2"/>
Descargar nueva categoría por su ID

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/categories/100.json?api_token=API_TOKEN"
```

<a name="cat3"/>
Añadir nueva categoría

```shell
curl https://YOUR_DOMAIN.bitfactura.com/categories.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": null
				}}'
```

<a name="cat4"/>
Actualizar categoría

```shell
curl https://YOUR_DOMAIN.bitfactura.com/categories/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"category": {
					"name":"my_category",
					"description": "new_description"
				}}'
```


<a name="cat5"/>
Eliminar categoría de un ID indicado

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/categories/100.json?api_token=API_TOKEN"
```

<a name="warehouses"/>

## Almacenes

<a name="wh1"/>
Lista de almacenes

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/warehouses.json?api_token=API_TOKEN"
```

<a name="wh2"/>
Descargar un almacén seleccionado por su ID

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/warehouses/100.json?api_token=API_TOKEN"
```

<a name="wh3"/>
Añadir nuevo almacén

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouses.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_warehouse",
					"kind": null,
					"description": null
				}}'
```

<a name="wh4"/>
Actualizar almacén

```shell
curl https://YOUR_DOMAIN.bitfactura.com/warehouses/100.json
				-X PUT
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"warehouse": {
					"name":"my_category",
					"kind": null,
					"description": "new_description"
				}}'
```


<a name="wh5"/>
Eliminar almacén de un ID indicado

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/warehouses/100.json?api_token=API_TOKEN"
```


<a name="departments"/>

## Departamentos

<a name="dep1"/>
Lista de departamentos

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/departments.json?api_token=API_TOKEN"
```

<a name="dep2"/>
Descargar un departamento por su ID

```shell
curl "http://YOUR_DOMAIN.bitfactura.com/departments/100.json?api_token=API_TOKEN"
```

<a name="dep3"/>
Añadir nuevo departamento

```shell
curl https://YOUR_DOMAIN.bitfactura.com/departments.json
				-H 'Accept: application/json'
				-H 'Content-Type: application/json'
				-d '{
				"api_token": "API_TOKEN",
				"department": {
					"name":"my_warehouse",
					"shortcut": "short_name",
					"tax_no": "-"
				}}'
```

<a name="dep4"/>
Actualizar un departamento

```shell
curl https://YOUR_DOMAIN.bitfactura.com/departments/100.json
        -X PUT
        -H 'Accept: application/json'
        -H 'Content-Type: application/json'
        -d '{
        "api_token": "API_TOKEN",
        "department": {
          "name":"new_name",
          "shortcut": "new_short_name",
          "tax_no": "xxx-xxx-xx-xx"
        }}'
```

<a name="dep5"/>
Eliminar departamento de un ID indicado

```shell
curl -X DELETE "https://YOUR_DOMAIN.bitfactura.com/departments/100.json?api_token=API_TOKEN"
```


<a name="get_token_by_api"/>

## Inicio de sesión y descargar del Token por API

```shell
curl https://app.bitfactura.com/login.json \
    -H 'Accept: application/json'  \
    -H 'Content-Type: application/json' \
    -d '{
            "login": "login_or_email",
            "password": "password",
	    "integration_token": ""
    }'
```

Está solicitud devuelve el token y la información sobre URL de cuenta en BitFactura (campos `prefix` y `url`):

```shell
{
	"login":"juan",
	"email":"email@test.com",
	"prefix":"YYYYYYY",
	"url":"https://YYYYYYY.bitfactura.com",
	"first_name":"Juan",
	"last_name":"Perez",
	"api_token":"XXXXXXXXXXXXXX"
}
```

OJO: api_token se devuelve solo si el usuario tiene generado API Token (usuario lo puede añadir en ajustes de cuenta) 


<a name="accounts"/>

## Cuentas del sistema

La opción disponible para Socios que quieren crear cuentas en BitFactura desde su aplicación. Pueden ser, por ejemplo, proveedores de tiendas en línea, sistemas de reservación u otros sistemas que quieren compartir con sus usuarios la función de generar facturas. 

Un cliente en portal del Socio puede crear una cuenta a través de un botón y empezar a generar facuras de inmediato (no tiene que crear una cuenta en BitFactura.com por sí mismo).


Campos user.login, user.from_partner, user, company no son oblitatorios 

```shell
curl https://YOUR_DOMAIN.bitfactura.com/account.json \
    -H 'Accept: application/json' \
    -H 'Content-Type: application/json' \
    -d '{
            "api_token": "API_TOKEN",
            "account": {
                "prefix": "prefix1",
		"lang": "es"
            },
            "user": {
                "login": "login1",
                "email": "email1@email.com",
                "password": "password1",
                "from_partner": "PARTNER_CODE"
            },
            "company": {
                "name": "Company1",
                "tax_no": "5252445700",
                "post_code": "00-112",
                "city": "Madrid",
                "street": "Street 1/10",
                "person": "Juan Perez",
                "bank": "Banco1",
                "bank_account": "111222333444555666111"
            },
	    "integration_token": ""
        }'

```

OJO: parámetro ```integration_token``` es oblitatorio si queremos descargar api_token actual del usuario (para obtener _integration_token_ para tu aplicación integrada ponte en contacto con nosotros)


Después de crear cuenta se devuelven: 

```shell

{
	"prefix":"prefix126", - prefijo de la cuenta creada (puede ser diferente al  introducido si ya ha sido usado) 
	"api_token":"62YPJfIekoo111111", - código de acceso a cuenta creada 
	"url":"https://prefix126.bitfactura.com", - url de la cuenta creada 
	"login":"login1", - login del usuario  (puede ser diferente al introducido si ya ha sido usado)
	"email":"email1@email.com"
}
```

Otros campos disponibles al crear nueva cuenta (útil al integrar)

```shell
	"account": {
		"prefix": "prefijo-de-cuenta",
		"lang": "es",
		"integration_fast_login": true - permite el inico de sesión automático para tus usuarios en BitFactura)
		"integration_logout_url": "http://tudominio.com/" - permite a tus usuarios volver a tu página después de cerrar la sesión en BitFactura
	}
```

Descargar información sobre la cuenta:

```shell
curl "https://YOUR_DOMAIN.bitfactura.com/account.json?api_token=API_TOKEN&integration_token="
```

<a name="codes"/>

## Ejemplos en PHP y Ruby

<https://github.com/radgost/fakturownia-api/blob/master/example1.php/>

<https://github.com/radgost/fakturownia-api/blob/master/example1.rb/>

Ruby Gem para la integración con BitFactura.com: <https://github.com/kkempin/fakturownia/>



