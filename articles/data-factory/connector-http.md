---
title: Copia de datos desde un origen HTTP mediante Azure Data Factory | Microsoft Docs
description: Obtenga información sobre cómo copiar datos desde un origen HTTP —en la nube o en un entorno local— a almacenes de datos receptores compatibles a través de una actividad de copia de una canalización de Azure Data Factory.
services: data-factory
documentationcenter: ''
author: linda33wj
manager: craigg
ms.reviewer: douglasl
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 08/24/2018
ms.author: jingwang
ms.openlocfilehash: 1f3e9be3a0048c4bf2e87ac23cbdc76b1aaa649f
ms.sourcegitcommit: c282021dbc3815aac9f46b6b89c7131659461e49
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/12/2018
ms.locfileid: "49166413"
---
# <a name="copy-data-from-an-http-endpoint-by-using-azure-data-factory"></a>Copia de datos desde un punto de conexión HTTP mediante Azure Data Factory

> [!div class="op_single_selector" title1="Select the version of Data Factory service you are using:"]
> * [Versión 1](v1/data-factory-http-connector.md)
> * [Versión actual](connector-http.md)

En este artículo se explica el uso de la actividad de copia de Azure Data Factory para copiar datos desde un punto de conexión HTTP. El artículo se basa en [Actividad de copia en Azure Data Factory](copy-activity-overview.md), en el que se ofrece información general acerca de la actividad de copia.

## <a name="supported-capabilities"></a>Funcionalidades admitidas

Puede copiar datos desde un origen HTTP a cualquier almacén de datos receptor compatible. Para obtener una lista de almacenes de datos que la actividad de copia admite como orígenes y receptores, consulte [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).

Puede usar este conector HTTP para:

- Recuperar datos desde un punto de conexión HTTP o HTTPS mediante los métodos HTTP **GET** o **POST**.
- Recuperar datos con uno de los siguientes tipos de autenticación: **Anónima**, **Básica**, **Implícita**, **Windows** o **ClientCertificate**.
- Copiar la respuesta HTTP tal cual, o bien analizarla con los [códecs de compresión y los formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md).

La diferencia entre este conector y el de [tabla web](connector-web-table.md) es que el último se usa para extraer contenido de la tabla desde una página web HTML.

> [!TIP]
> Para probar una solicitud HTTP para la recuperación de datos antes de configurar el conector HTTP en Data Factory, obtenga información acerca de la especificación de API para los requisitos del cuerpo y del encabezado. Puede usar herramientas como Postman o un explorador web para validar.

## <a name="get-started"></a>Introducción

[!INCLUDE [data-factory-v2-connector-get-started](../../includes/data-factory-v2-connector-get-started.md)]

En las secciones siguientes se proporcionan detalles sobre las propiedades que puede usar para definir entidades de Data Factory específicas del conector HTTP.

## <a name="linked-service-properties"></a>Propiedades del servicio vinculado

Las siguientes propiedades son compatibles con el servicio vinculado de HTTP:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** debe establecerse en: **HttpServer**. | SÍ |
| URL | La dirección URL base para el servidor web. | SÍ |
| enableServerCertificateValidation | Especifique si desea habilitar la validación de certificados SSL al conectarse al punto de conexión HTTP. Si el servidor HTTPS usa un certificado autofirmado, establezca esta propiedad en **false**. | Sin <br /> (El valor predeterminado es: **true**) |
| authenticationType | Especifica el tipo de autenticación. Los valores permitidos son: **Anonymous**, **Basic**, **Digest**, **Windows** y **ClientCertificate**. <br><br> Consulte las secciones que se encuentran después de esta tabla para obtener más propiedades y ejemplos de JSON para estos tipos de autenticación. | SÍ |
| connectVia | Instancia de [Integration Runtime](concepts-integration-runtime.md) que se usará para conectarse al almacén de datos. Se puede usar Azure Integration Runtime o un IR autohospedado (si el almacén de datos se encuentra en una red privada). Si no se especifica, esta propiedad se usará Azure Integration Runtime. |Sin  |

### <a name="using-basic-digest-or-windows-authentication"></a>Uso de la autenticación Basic, Digest o Windows

Establezca la propiedad **authenticationType** en **Basic**, **Digest** o **Windows**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| userName | El nombre de usuario para acceder al punto de conexión HTTP. | SÍ |
| contraseña | Contraseña del usuario (valor **userName**). Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | SÍ |

**Ejemplo**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "Basic",
            "url" : "<HTTP endpoint>",
            "userName": "<user name>",
            "password": {
                "type": "SecureString",
                "value": "<password>"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

### <a name="using-clientcertificate-authentication"></a>Uso de la autenticación ClientCertificate

Para usar la autenticación ClientCertificate, establezca la propiedad **authenticationType** en **ClientCertificate**. Además de las propiedades genéricas descritas en las secciones anteriores, especifique las siguientes:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| embeddedCertData | Datos del certificado con codificación Base64. | Especifique **embeddedCertData** o **certThumbprint**. |
| certThumbprint | La huella digital del certificado que se instaló en el almacén de certificados de la máquina Integration Runtime (autohospedado). Solo se aplica cuando se especifica el tipo autohospedado de un entorno Integration Runtime en la propiedad **connectVia**. | Especifique **embeddedCertData** o **certThumbprint**. |
| contraseña | La contraseña asociada con el certificado. Marque este campo como de tipo **SecureString** para almacenarlo de forma segura en Data Factory. También puede [hacer referencia a un secreto almacenado en Azure Key Vault](store-credentials-in-key-vault.md). | Sin  |

Si utiliza **certThumbprint** para la autenticación y el certificado está instalado en el almacén personal del equipo local, conceda permisos de lectura al entorno Integration Runtime (autohospedado):

1. Abra Microsoft Management Console (MMC). Agregue el complemento **Certificados** que tenga como destino **Equipo local**.
2. Expanda **Certificados** > **Personal** y, luego, seleccione **Certificados**.
3. Haga clic con el botón derecho en el certificado del almacén personal y seleccione **Todas las tareas** > **Administrar claves privadas**.
3. En la pestaña **Seguridad**, agregue la cuenta de usuario en que se ejecuta el servicio de host del entorno Integration Runtime (DIAHostService) con acceso de lectura al certificado.

**Ejemplo 1: Uso de certThumbprint**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "certThumbprint": "<thumbprint of certificate>"
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

**Ejemplo 2: Uso de embeddedCertData**

```json
{
    "name": "HttpLinkedService",
    "properties": {
        "type": "HttpServer",
        "typeProperties": {
            "authenticationType": "ClientCertificate",
            "url": "<HTTP endpoint>",
            "embeddedCertData": "<Base64-encoded cert data>",
            "password": {
                "type": "SecureString",
                "value": "password of cert"
            }
        },
        "connectVia": {
            "referenceName": "<name of Integration Runtime>",
            "type": "IntegrationRuntimeReference"
        }
    }
}
```

## <a name="dataset-properties"></a>Propiedades del conjunto de datos

En esta sección se proporciona una lista de las propiedades que admite el conjunto de datos de HTTP. 

Para ver una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, consulte [Conjuntos de datos y servicios vinculados](concepts-datasets-linked-services.md). 

Para copiar datos desde HTTP, establezca la propiedad **type** del conjunto de datos en **HttpFile**. Se admiten las siguientes propiedades:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del conjunto de datos debe establecerse en **HttpFile**. | SÍ |
| relativeUrl | Dirección URL relativa al recurso que contiene los datos. Cuando no se especifica la propiedad, solo se usa la dirección URL especificada en la definición del servicio vinculado. | Sin  |
| requestMethod | Método HTTP. Los valores permitidos son **Get** (valor predeterminado) y **Post**. | Sin  |
| additionalHeaders | Encabezados de solicitud HTTP adicionales. | Sin  |
| requestBody | Cuerpo de la solicitud HTTP. | Sin  |
| formato | Si desea recuperar datos tal cual desde el punto de conexión HTTP sin analizarlos y, después, copiarlos en un almacén basado en archivos, ignore la sección de **formato** de las definiciones de los conjuntos de datos de entrada y salida.<br/><br/>Si desea analizar el contenido de la respuesta HTTP durante la copia, se admiten los siguientes tipos de formato de archivos: **TextFormat**, **JsonFormat**, **AvroFormat**, **OrcFormat** y **ParquetFormat**. En **formato**, establezca la propiedad **type** de formato en uno de los siguientes valores. Para obtener más información, consulte las secciones sobre [formato JSON](supported-file-formats-and-compression-codecs.md#json-format), [formato de texto](supported-file-formats-and-compression-codecs.md#text-format), [formato Avro](supported-file-formats-and-compression-codecs.md#avro-format), [formato Orc](supported-file-formats-and-compression-codecs.md#orc-format) y [formato Parquet](supported-file-formats-and-compression-codecs.md#parquet-format). |Sin  |
| compresión | Especifique el tipo y el nivel de compresión de los datos. Para más información, consulte el artículo sobre [códecs de compresión y formatos de archivo compatibles](supported-file-formats-and-compression-codecs.md#compression-support).<br/><br/>Tipos admitidos: **GZip**, **Deflate**, **BZip2** y **ZipDeflate**.<br/>Niveles admitidos: **Optimal** y **Fastest**. |Sin  |

> [!NOTE]
> El tamaño de carga de la solicitud HTTP admitido es aproximadamente 500 KB. Si el que desea pasar al punto de conexión web supera los 500 KB, considere la posibilidad de agrupar la carga en fragmentos menores.

**Ejemplo 1: Uso del método Get (valor predeterminado)**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "additionalHeaders": "Connection: keep-alive\nUser-Agent: Mozilla/5.0\n"
        }
    }
}
```

**Ejemplo 2: Uso del método POST**

```json
{
    "name": "HttpSourceDataInput",
    "properties": {
        "type": "HttpFile",
        "linkedServiceName": {
            "referenceName": "<HTTP linked service name>",
            "type": "LinkedServiceReference"
        },
        "typeProperties": {
            "relativeUrl": "<relative url>",
            "requestMethod": "Post",
            "requestBody": "<body for POST HTTP request>"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Propiedades de la actividad de copia

En esta sección se proporciona una lista de las propiedades que admite el origen de HTTP.

Para ver una lista completa de las secciones y propiedades que hay disponibles para definir actividades, consulte [Canalizaciones](concepts-pipelines-activities.md). 

### <a name="http-as-source"></a>HTTP como origen

Para copiar datos desde HTTP, establezca el **tipo de origen** de la actividad de copia en **HttpSource**. Se admiten las siguientes propiedades en la sección **source** de la actividad de copia:

| Propiedad | DESCRIPCIÓN | Obligatorio |
|:--- |:--- |:--- |
| Tipo | La propiedad **type** del origen de la actividad de copia debe establecerse en: **HttpSource**. | SÍ |
| httpRequestTimeout | El tiempo de espera (el valor **TimeSpan**) para que la solicitud HTTP obtenga una respuesta. Este valor es el tiempo de espera para obtener una respuesta, no para leer los datos de la respuesta. El valor predeterminado es **00:01:40**.  | Sin  |

**Ejemplo**

```json
"activities":[
    {
        "name": "CopyFromHTTP",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<HTTP input dataset name>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset name>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "HttpSource",
                "httpRequestTimeout": "00:01:00"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```


## <a name="next-steps"></a>Pasos siguientes

Para ver una lista de los almacenes de datos que la actividad de copia admite como orígenes y receptores en Azure Data Factory, consulte los [Almacenes de datos y formatos que se admiten](copy-activity-overview.md#supported-data-stores-and-formats).
