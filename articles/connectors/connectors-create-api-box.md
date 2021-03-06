---
title: Incorporación del conector de Box a las aplicaciones lógicas | Microsoft Docs
description: Información general del conector de Box con parámetros de la API de REST
services: ''
documentationcenter: ''
author: MandiOhlinger
manager: erikre
editor: ''
tags: connectors

ms.service: multiple
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/18/2016
ms.author: mandia

---
# Introducción al conector de Box
Conéctese a Box y cree y elimine archivos, entre muchas otras cosas.

> [!NOTE]
> Esta versión del artículo se aplica a la versión de esquema 2015-08-01-preview de las aplicaciones lógicas.
> 
> 

Con Box, puede hacer lo siguiente:

* Compilar el flujo de negocio en función de los datos que obtiene de Box.
* Usar desencadenadores para cuando se crea o actualiza un archivo.
* Usar acciones que copian o eliminan archivos, entre otras muchas cosas. Estas acciones obtienen una respuesta y luego dejan el resultado a disposición de otras acciones. Por ejemplo, cuando se modifique un archivo en Box, puede tomar ese archivo y enviarlo por correo electrónico mediante Office 365.

Para agregar una operación en aplicaciones lógicas, consulte [Creación de una nueva aplicación lógica mediante la conexión de servicios de SaaS](../app-service-logic/app-service-logic-create-a-logic-app.md).

## Desencadenadores y acciones
Box incluye los siguientes desencadenadores y acciones.

| Desencadenadores | Acciones |
| --- | --- |
| <ul><li>Cuando se crea un archivo</li><li>Cuando se modifica un archivo</li></ul> |<ul><li>Crear archivo</li><li>Cuando se crea un archivo</li><li>Copiar archivo</li><li>Eliminar archivo</li><li>Extraer archivo en la carpeta</li><li>Obtener contenido de archivo con el identificador</li><li>Obtener contenido de archivo mediante la ruta de acceso</li><li>Obtener metadatos de archivo con el identificador</li><li>Obtener metadatos de archivo mediante la ruta de acceso</li><li>Actualizar archivo</li><li>Cuando se modifica un archivo</li></ul> |

Todos los conectores admiten datos en formato JSON y XML.

## Creación de una conexión a Box
Al agregar este conector a las aplicaciones lógicas, debe autorizar a estas para que se conecten a su Box.

> [!INCLUDE [Pasos para crear una conexión a Box](../../includes/connectors-create-api-box.md)]
> 
> 

Después de crear la conexión, escriba las propiedades de Box. La **referencia de la API de REST** de este tema describe estas propiedades.

> [!TIP]
> Puede usar esta misma conexión de Box en otras aplicaciones lógicas.
> 
> 

## Referencia de la API de REST de Swagger
Se aplica a la versión: 1.0.

### Crear archivo
Carga un archivo en Box. ```POST: /datasets/default/files```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| folderPath |string |Sí |query |None |Ruta de acceso de la carpeta para cargar el archivo en Box |
| name |string |Sí |query |None |Nombre del archivo que se va a crear en Box |
| body |string(binary) |Sí |body |None |Contenido del archivo que se va a cargar en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Cuando se crea un archivo
Desencadena un flujo cuando se crea un nuevo archivo en una carpeta de Box. ```GET: /datasets/default/triggers/onnewfile```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| folderId |string |Sí |query |None |Identificador único de la carpeta en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Copiar archivo
Copia un archivo en Box. ```POST: /datasets/default/copyFile```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| de origen |string |Sí |query |None |Dirección URL al archivo de origen |
| de destino |string |Sí |query |None |Ruta de acceso al archivo de destino en Box, incluido el nombre del archivo de destino |
| overwrite |boolean |No |query |None |Sobrescribe el archivo de destino si está establecido en 'true' |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Eliminar archivo
Elimina un archivo de Box. ```DELETE: /datasets/default/files/{id}```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| id |string |Sí |path |None |Identificador único del archivo que se va a eliminar de Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Extraer archivo en la carpeta
Extrae un archivo de almacenamiento en una carpeta de Box (ejemplo: .zip). ```POST: /datasets/default/extractFolderV2```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| de origen |string |Sí |query | |Ruta de acceso al archivo de almacenamiento |
| de destino |string |Sí |query | |Ruta de acceso de Box para extraer el contenido del archivo |
| overwrite |boolean |No |query | |Sobrescribe los archivos de destino si está establecido en 'true' |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Obtener contenido de archivo mediante el identificador
Recupera el contenido del archivo de Box mediante el identificador. ```GET: /datasets/default/files/{id}/content```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| id |string |Sí |path |None |Identificador único del archivo en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Obtener contenido de archivo mediante la ruta de acceso
Recupera el contenido del archivo de Box mediante la ruta de acceso. ```GET: /datasets/default/GetFileContentByPath```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| path |string |Sí |query |None |Ruta de acceso única al archivo en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Obtener metadatos de archivo mediante el identificador
Recupera los metadatos del archivo de Box mediante el identificador de archivo. ```GET: /datasets/default/files/{id}```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| id |string |Sí |path |None |Identificador único del archivo en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Obtener metadatos de archivo mediante la ruta de acceso
Recupera los metadatos del archivo de Box mediante la ruta de acceso. ```GET: /datasets/default/GetFileByPath```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| path |string |Sí |query |None |Ruta de acceso única al archivo en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Actualizar archivo
Actualiza un archivo en Box. ```PUT: /datasets/default/files/{id}```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| id |string |Sí |path |None |Identificador único del archivo que se va a actualizar en Box |
| body |string(binary) |Sí |body |None |Contenido del archivo que se va a actualizar en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

### Cuando se modifica un archivo
Desencadena un flujo cuando se modifica un archivo en una carpeta de Box. ```GET: /datasets/default/triggers/onupdatedfile```

| Nombre | Tipo de datos | Obligatorio | Ubicado en | Valor predeterminado | Description |
| --- | --- | --- | --- | --- | --- |
| folderId |string |Sí |query |None |Identificador único de la carpeta en Box |

#### Response
| Nombre | Descripción |
| --- | --- |
| 200 |OK |
| default |Error en la operación. |

## Definiciones de objeto
#### DataSetsMetadata
| Nombre de propiedad | Tipo de datos | Obligatorio |
| --- | --- | --- |
| tabular |not defined |no |
| blob |not defined |no |

#### TabularDataSetsMetadata
| Nombre de propiedad | Tipo de datos | Obligatorio |
| --- | --- | --- |
| de origen |string |no |
| DisplayName |string |no |
| urlEncoding |string |no |
| tableDisplayName |string |no |
| tablePluralName |string |no |

#### BlobDataSetsMetadata
| Nombre de propiedad | Tipo de datos | Obligatorio |
| --- | --- | --- |
| de origen |string |no |
| DisplayName |string |no |
| urlEncoding |string |no |

#### BlobMetadata
| Nombre de propiedad | Tipo de datos | Obligatorio |
| --- | --- | --- |
| Id |string |no |
| Nombre |string |no |
| DisplayName |string |no |
| Ruta de acceso |string |no |
| LastModified |string |no |
| Tamaño |integer |no |
| MediaType |string |no |
| IsFolder |boolean |no |
| ETag |string |no |
| FileLocator |string |no |

## Pasos siguientes
[Creación de una aplicación lógica](../app-service-logic/app-service-logic-create-a-logic-app.md)

<!---HONumber=AcomDC_0824_2016-->