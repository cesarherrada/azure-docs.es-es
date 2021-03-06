---
title: Movimiento de datos desde tabla web | Microsoft Docs
description: Obtenga información sobre cómo mover datos desde una tabla local en una página web mediante Factoría de datos de Azure
services: data-factory
documentationcenter: ''
author: linda33wj
manager: jhubbard
editor: monicar

ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/26/2016
ms.author: jingwang

---
# <a name="move-data-from-a-web-table-source-using-azure-data-factory"></a>Movimiento de datos de un origen de tabla web mediante Factoría de datos de Azure
En este artículo se describe cómo se puede usar la actividad de copia en Factoría de datos de Azure para copiar datos de una tabla en una página web a otro almacén de datos. Este artículo se basa en el artículo sobre [actividades de movimiento de datos](data-factory-data-movement-activities.md) que presenta una introducción general del movimiento de datos con la actividad de copia y las combinaciones del almacén de datos admitidas.

Factoría de datos solo admite actualmente el movimiento de datos desde una tabla web a otros almacenes de datos, pero no de otros almacenes de datos a un destino de tabla web.

> [!NOTE]
> Actualmente, este conector web solo permite extraer contenido de tablas de una página HTML.
> 
> 

## <a name="sample:-copy-data-from-web-table-to-azure-blob"></a>Ejemplo: copia de datos de una tabla web a un blob de Azure
El ejemplo siguiente muestra:

1. Un servicio vinculado de tipo [Web](#web-linked-service-properties).
2. Un servicio vinculado de tipo [AzureStorage](data-factory-azure-blob-connector.md#azure-storage-linked-service-properties)
3. Un [conjunto de datos](data-factory-create-datasets.md) de entrada de tipo [WebTable](#WebTable-dataset-properties).
4. Un [conjunto de datos](data-factory-create-datasets.md) de salida de tipo [AzureBlob](data-factory-azure-blob-connector.md#azure-blob-dataset-type-properties).
5. Una [canalización](data-factory-create-pipelines.md) con la actividad de copia que usa [WebSource](#websource-copy-activity-type-properties) y [BlobSink](data-factory-azure-blob-connector.md#azure-blob-copy-activity-type-properties).

El ejemplo copia los datos de una tabla web a un blob de Azure cada hora. Las propiedades JSON usadas en estos ejemplos se describen en las secciones que aparecen después de los ejemplos. 

En el ejemplo siguiente se muestra cómo copiar datos de una tabla web a un blob de Azure. Sin embargo, los datos se pueden copiar directamente en cualquiera de los receptores indicados en el artículo [Actividades de movimiento de datos](data-factory-data-movement-activities.md) mediante la actividad de copia de Data Factory de Azure. 

**Servicio vinculado de tipo Web** : este ejemplo utiliza el servicio vinculado de tipo Web con la autenticación anónima. Consulte la sección [Propiedades del servicio vinculado de Web](#web-linked-service-properties) para conocer los diferentes tipos de autenticación que se pueden usar. 

    {
        "name": "WebLinkedService",
        "properties":
        {
            "type": "Web",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "url" : "https://en.wikipedia.org/wiki/"
            }
        }
    }


**Servicio vinculado de Almacenamiento de Azure**

    {
      "name": "AzureStorageLinkedService",
      "properties": {
        "type": "AzureStorage",
        "typeProperties": {
          "connectionString": "DefaultEndpointsProtocol=https;AccountName=<accountname>;AccountKey=<accountkey>"
        }
      }
    }

**Conjunto de datos de entrada WebTable**: si se establece **external** en **true**, se informa al servicio Data Factory que la tabla es externa a la factoría de datos y no se produce por ninguna actividad de la misma.

> [!NOTE]
> Consulte la sección [Obtención de índice de una tabla en una página HTML](#get-index-of-a-table-in-an-html-page) para saber los pasos necesarios para obtener el índice de una tabla en una página HTML.  
> 
> 

    {
        "name": "WebTableInput",
        "properties": {
            "type": "WebTable",
            "linkedServiceName": "WebLinkedService",
            "typeProperties": {
                "index": 1,
                "path": "AFI's_100_Years...100_Movies"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }



**Conjunto de datos de salida de blob de Azure**

Los datos se escriben en un nuevo blob cada hora (frecuencia: hora, intervalo: 1). 

    {
        "name": "AzureBlobOutput",
        "properties":
        {
            "type": "AzureBlob",
            "linkedServiceName": "AzureStorageLinkedService",
            "typeProperties":
            {
                "folderPath": "adfgetstarted/Movies"
            },
            "availability":
            {
                "frequency": "Hour",
                "interval": 1
            }
        }
    }




**Canalización con actividad de copia**

La canalización contiene una actividad de copia que está configurada para usar los conjuntos de datos de entrada y de salida y está programada para ejecutarse cada hora. En la definición del JSON de la canalización, el tipo **source** se establece en **WebSource** y el tipo **sink**, en **BlobSink**. 

Consulte [Propiedades de tipo WebSource](#websource-copy-activity-type-properties) para obtener la lista de propiedades que admite WebSource. 

    {  
        "name":"SamplePipeline",
        "properties":{  
        "start":"2014-06-01T18:00:00",
        "end":"2014-06-01T19:00:00",
        "description":"pipeline with copy activity",
        "activities":[  
          {
            "name": "WebTableToAzureBlob",
            "description": "Copy from a Web table to an Azure blob",
            "type": "Copy",
            "inputs": [
              {
                "name": "WebTableInput"
              }
            ],
            "outputs": [
              {
                "name": "AzureBlobOutput"
              }
            ],
            "typeProperties": {
              "source": {
                "type": "WebSource"
              },
              "sink": {
                "type": "BlobSink"
              }
            },
           "scheduler": {
              "frequency": "Hour",
              "interval": 1
            },
            "policy": {
              "concurrency": 1,
              "executionPriorityOrder": "OldestFirst",
              "retry": 0,
              "timeout": "01:00:00"
            }
          }
          ]
       }
    }


## <a name="web-linked-service-properties"></a>Propiedades del servicio vinculado de Web
En la tabla siguiente se proporciona la descripción de los elementos JSON específicos del servicio vinculado de Web.

| Propiedad | Descripción | Obligatorio |
| --- | --- | --- |
| type |La propiedad type debe establecerse en: **Web** |Sí |
| URL |Dirección URL para el origen de Web |Sí |
| authenticationType |Anonymous o Basic. |Sí |
| userName |Nombre de usuario en autenticación básica |Sí (para la autenticación básica) |
| contraseña |Contraseña en autenticación básica |Sí (para la autenticación básica) |

### <a name="using-anonymous-authentication"></a>Uso de autenticación anónima
    {
        "name": "web",
        "properties":
        {
            "type": "Web",
            "typeProperties":
            {
                "authenticationType": "Anonymous",
                "url" : "https://en.wikipedia.org/wiki/"
            }
        }
    }


### <a name="using-basic-authentication"></a>Uso de la autenticación básica
    {
        "name": "web",
        "properties":
        {
            "type": "Web",
            "typeProperties":
            {
                "authenticationType": "basic",
                "url" : "http://myit.mycompany.com/",
                "userName": "Administrator",
                "password": "password"
            }
        }
    }


## <a name="webtable-dataset-properties"></a>Propiedades del conjunto de datos WebTable
Para una lista completa de las secciones y propiedades disponibles para definir conjuntos de datos, vea el artículo [Creación de conjuntos de datos](data-factory-create-datasets.md). Las secciones como structure, availability y policy del código JSON del conjunto de datos son similares para todos los tipos de conjunto de datos (SQL Azure, blob de Azure, tabla de Azure, etc.).

La sección **typeProperties** es diferente en cada tipo de conjunto de datos y proporciona información acerca de la ubicación de los datos en el almacén de datos. La sección typeProperties del conjunto de datos de tipo **WebTable** tiene las propiedades siguientes:

| Propiedad | Descripción | Obligatorio |
|:--- |:--- |:--- |
| type |Tipo del conjunto de datos. Debe establecerse en **WebTable** |Sí |
| path |Dirección URL relativa al recurso que contiene la tabla. |No. Cuando no se especifica la ruta de acceso, se solo se usa la dirección URL especificada en la definición de servicio vinculado. |
| index |Índice de la tabla en el recurso. Consulte la sección [Obtención de índice de una tabla en una página HTML](#get-index-of-a-table-in-an-html-page) para saber los pasos necesarios para obtener el índice de una tabla en una página HTML. |Sí |

**Ejemplo:**

    {
        "name": "WebTableInput",
        "properties": {
            "type": "WebTable",
            "linkedServiceName": "WebLinkedService",
            "typeProperties": {
                "index": 1,
                "path": "AFI's_100_Years...100_Movies"
            },
            "external": true,
            "availability": {
                "frequency": "Hour",
                "interval":  1
            }
        }
    }

## <a name="websource---copy-activity-type-properties"></a>WebSource - Propiedades de tipo de actividad de copia
Para ver una lista completa de las secciones y propiedades disponibles para definir actividades, consulte el artículo [Creación de canalizaciones](data-factory-create-pipelines.md). Las propiedades (como nombre, descripción, tablas de entrada y salida, y directivas) están disponibles para todos los tipos de actividades. 

Por otra parte, las propiedades disponibles en la sección typeProperties de la actividad varían con cada tipo de actividad. Para la actividad de copia, varían en función de los tipos de orígenes y receptores.

En este momento, cuando el origen de la actividad de copia es de tipo **WebSource**, no se admite ninguna propiedad adicional. 

## <a name="get-index-of-a-table-in-an-html-page"></a>Obtención de índice de una tabla en una página HTML
1. Inicie **Excel 2016** y cambie a la pestaña **Datos**.  
2. Haga clic en **Nueva consulta** en la barra de herramientas, elija **De otros orígenes** y haga clic en **Desde Web**.
   
    ![Menú de Power Query](./media/data-factory-web-table-connector/PowerQuery-Menu.png) 
3. En el cuadro de diálogo **Desde Web**, escriba la **dirección URL** que se use en el JSON del servicio vinculado (por ejemplo: https://en.wikipedia.org/wiki/) junto con la ruta de acceso que se especifique para el conjunto de datos (por ejemplo: AFI%27s_100_Years...100_Movies) y haga clic en **Aceptar**. 
   
    ![Cuadro de diálogo Desde Web](./media/data-factory-web-table-connector/FromWeb-DialogBox.png) 
   
    Dirección URL usada en este ejemplo: https://en.wikipedia.org/wiki/AFI%27s_100_Years...100_Movies 
4. Si ve el cuadro de diálogo **Acceso a contenido web**, seleccione la **dirección URL** correcta, la **autenticación** y haga clic en **Conectar**. 
   
   ![Cuadro de diálogo Acceso a contenido web](./media/data-factory-web-table-connector/AccessWebContentDialog.png)
5. Haga clic en un elemento de **tabla** en la vista de árbol para ver el contenido de la tabla y después en el botón **Editar** ubicado en la parte inferior.  
   
   ![Cuadro de diálogo Navegador](./media/data-factory-web-table-connector/Navigator-DialogBox.png) 
6. En la ventana **Editor de consultas**, haga clic en el botón **Editor avanzado** de la barra de herramientas.
   
    ![Botón Editor avanzado](./media/data-factory-web-table-connector/QueryEditor-AdvancedEditorButton.png)
7. En el cuadro de diálogo Editor avanzado, el número que aparece junto a "Origen" es el índice.
   
    ![Editor avanzado - Índice](./media/data-factory-web-table-connector/AdvancedEditor-Index.png) 

Si usa Excel 2013, use [Microsoft Power Query para Excel](https://www.microsoft.com/download/details.aspx?id=39379) para obtener el índice. Consulte el artículo [Conectarse a una página web](https://support.office.com/article/Connect-to-a-web-page-Power-Query-b2725d67-c9e8-43e6-a590-c0a175bd64d8) para más información. Los pasos son similares si usa [Microsoft Power BI Desktop](https://powerbi.microsoft.com/desktop/). 

[!INCLUDE [data-factory-column-mapping](../../includes/data-factory-column-mapping.md)]

[!INCLUDE [data-factory-structure-for-rectangualr-datasets](../../includes/data-factory-structure-for-rectangualr-datasets.md)]

## <a name="performance-and-tuning"></a>Rendimiento y optimización
Consulte [Guía de optimización y rendimiento de la actividad de copia](data-factory-copy-activity-performance.md) para más información sobre los factores clave que afectan al rendimiento del movimiento de datos (actividad de copia) en Azure Data Factory y las diversas formas de optimizarlo.

<!--HONumber=Oct16_HO2-->


