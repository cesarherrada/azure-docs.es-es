---
title: "Introducción a Azure Data Lake Analytics mediante Azure PowerShell | Microsoft Docs"
description: "Aprenda a usar Azure PowerShell para crear una cuenta de Almacén de Data Lake, crear un trabajo de Análisis de Data Lake mediante U-SQL y enviar el trabajo. "
services: data-lake-analytics
documentationcenter: 
author: edmacauley
manager: jhubbard
editor: cgronlun
ms.assetid: 8a4e901e-9656-4a60-90d0-d78ff2f00656
ms.service: data-lake-analytics
ms.devlang: na
ms.topic: get-started-article
ms.tgt_pltfrm: na
ms.workload: big-data
ms.date: 09/21/2016
ms.author: edmaca
translationtype: Human Translation
ms.sourcegitcommit: 73d3e5577d0702a93b7f4edf3bf4e29f55a053ed
ms.openlocfilehash: 59efa050944059c737654a3f039a058c50865ea6


---
# <a name="tutorial-get-started-with-azure-data-lake-analytics-using-azure-powershell"></a>Tutorial: Introducción a Análisis de Azure Data Lake mediante Azure PowerShell
[!INCLUDE [get-started-selector](../../includes/data-lake-analytics-selector-get-started.md)]

Aprenda a usar Azure PowerShell para crear cuentas de Análisis de Azure Data Lake, definir trabajos de Análisis de Data Lake en [U-SQL](data-lake-analytics-u-sql-get-started.md)y enviar trabajos a cuentas de Análisis de Data Lake. Para más información acerca de Data Lake Analytics, consulte [Información general sobre Azure Data Lake Analytics](data-lake-analytics-overview.md).

En este tutorial, desarrollará un trabajo que lee un archivo de valores separados por tabulaciones (TSV) y lo convierte en un otro de valores separados por comas (CSV). Para recorrer el mismo tutorial con otras herramientas compatibles, haga clic en las pestañas situadas en la parte superior de esta sección.

## <a name="prerequisites"></a>Requisitos previos
Antes de empezar este tutorial, debe contar con lo siguiente:

* **Una suscripción de Azure**. Consulte [Obtención de una versión de evaluación gratuita](https://azure.microsoft.com/pricing/free-trial/).
* **Una estación de trabajo con Azure PowerShell**. Consulte [Instalación y configuración de Azure PowerShell](../powershell-install-configure.md).

## <a name="create-data-lake-analytics-account"></a>Creación de una cuenta de Análisis de Data Lake
Debe tener una cuenta de Análisis de Data Lake para poder ejecutar trabajos. Para crear una cuenta de Análisis de Data Lake, debe especificar lo siguiente:

* **Grupo de recursos de Azure**: se debe crear una cuenta de Análisis de Data Lake dentro de un grupo de recursos de Azure. [Azure Resource Manager](../azure-resource-manager/resource-group-overview.md) permite trabajar con los recursos de la aplicación como un grupo. Puede implementar, actualizar o eliminar todos los recursos de la aplicación en una operación única y coordinada.  

    Para enumerar los grupos de recursos para la suscripción:

        Get-AzureRmResourceGroup

    Para crear un nuevo grupo de recursos:

        New-AzureRmResourceGroup `
            -Name "<Your resource group name>" `
            -Location "<Azure Data Center>" # For example, "East US 2"
* **Nombre de la cuenta de Análisis de Data Lake**
* **Ubicación**: uno de los centros de datos de Azure que admite Análisis de Data Lake.
* **Cuenta predeterminada de Data Lake**: cada cuenta de Análisis de Data Lake tiene una cuenta predeterminada de Data Lake.

    Para crear una nueva cuenta de Data Lake:

        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName "<Your Azure resource group name>" `
            -Name "<Your Data Lake account name>" `
            -Location "<Azure Data Center>"  # For example, "East US 2"

  > [!NOTE]
  > El nombre de cuenta de Data Lake solo debe contener letras minúsculas y números.
  >
  >

**Para crear una cuenta de Análisis de Data Lake**

1. Abra PowerShell ISE en la estación de trabajo de Windows.
2. Ejecute el siguiente script:

        $resourceGroupName = "<ResourceGroupName>"
        $dataLakeStoreName = "<DataLakeAccountName>"
        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $location = "East US 2"

        Write-Host "Create a resource group ..." -ForegroundColor Green
        New-AzureRmResourceGroup `
            -Name  $resourceGroupName `
            -Location $location

        Write-Host "Create a Data Lake account ..."  -ForegroundColor Green
        New-AzureRmDataLakeStoreAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeStoreName `
            -Location $location

        Write-Host "Create a Data Lake Analytics account ..."  -ForegroundColor Green
        New-AzureRmDataLakeAnalyticsAccount `
            -Name $dataLakeAnalyticsName `
            -ResourceGroupName $resourceGroupName `
            -Location $location `
            -DefaultDataLake $dataLakeStoreName

        Write-Host "The newly created Data Lake Analytics account ..."  -ForegroundColor Green
        Get-AzureRmDataLakeAnalyticsAccount `
            -ResourceGroupName $resourceGroupName `
            -Name $dataLakeAnalyticsName  

## <a name="upload-data-to-data-lake"></a>Carga de datos a Data Lake
En este tutorial, va a procesar algunos registros de búsqueda.  El registro de búsqueda se puede almacenar en el Almacén de Data Lake o en el almacenamiento de blobs de Azure.

Se ha copiado un archivo de registro de búsqueda de ejemplo en un contenedor de Blob de Azure público. Use el siguiente script de PowerShell para descargar el archivo en la estación de trabajo y después cargue el archivo a la cuenta predeterminada de Almacén de Data Lake de la cuenta de Análisis de Data Lake.

    $dataLakeStoreName = "<The default Data Lake Store account name>"

    $localFolder = "C:\Tutorials\Downloads\" # A temp location for the file.
    $storageAccount = "adltutorials"  # Don't modify this value.
    $container = "adls-sample-data"  #Don't modify this value.

    # Create the temp location    
    New-Item -Path $localFolder -ItemType Directory -Force

    # Download the sample file from Azure Blob storage
    $context = New-AzureStorageContext -StorageAccountName $storageAccount -Anonymous
    $blobs = Azure\Get-AzureStorageBlob -Container $container -Context $context
    $blobs | Get-AzureStorageBlobContent -Context $context -Destination $localFolder

    # Upload the file to the default Data Lake Store account    
    Import-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path $localFolder"SearchLog.tsv" -Destination "/Samples/Data/SearchLog.tsv"

El siguiente script de PowerShell muestra cómo obtener el nombre predeterminado de Almacén de Data Lake para una cuenta de Análisis de Data Lake:

    $resourceGroupName = "<ResourceGroupName>"
    $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticsName).Properties.DefaultDataLakeAccount
    echo $dataLakeStoreName

> [!NOTE]
> El Portal de Azure proporciona una interfaz de usuario para copiar los archivos de datos de ejemplo en la cuenta predeterminada de Almacén de Data Lake. Para obtener instrucciones, consulte [Introducción a Análisis de Azure Data Lake mediante el Portal de Azure](data-lake-analytics-get-started-portal.md#prepare-source-data).
>
>

Análisis de Data Lake también puede acceder al almacenamiento de blobs de Azure.  Para cargar datos al almacenamiento de blobs de Azure, consulte [Uso de Azure PowerShell con Almacenamiento de Azure](../storage/storage-powershell-guide-full.md).

## <a name="submit-data-lake-analytics-jobs"></a>Envío de trabajos de Análisis de Data Lake
Los trabajos de Análisis de Data Lake se escriben en el lenguaje U-SQL. Para más información sobre U-SQL, consulte la [introducción al lenguaje U-SQL](data-lake-analytics-u-sql-get-started.md) y la [referencia del lenguaje U-SQL](http://go.microsoft.com/fwlink/?LinkId=691348).

**Para crear un script de trabajo de Análisis de Data Lake**

* Cree un archivo de texto con el siguiente script U-SQL y guarde el archivo de texto en la estación de trabajo:

        @searchlog =
            EXTRACT UserId          int,
                    Start           DateTime,
                    Region          string,
                    Query           string,
                    Duration        int?,
                    Urls            string,
                    ClickedUrls     string
            FROM "/Samples/Data/SearchLog.tsv"
            USING Extractors.Tsv();

        OUTPUT @searchlog   
            TO "/Output/SearchLog-from-Data-Lake.csv"
        USING Outputters.Csv();

    Este script de U-SQL lee el archivo de datos de origen mediante **Extractors.Tsv()** y crea un archivo csv con **Outputters.Csv()**.

    No modifique ninguna de las dos rutas a menos que copie el archivo de origen en una ubicación diferente.  Análisis de Data Lake creará la carpeta de salida si no existe.

    Es más fácil usar rutas de acceso relativas para los archivos almacenados en cuentas predeterminadas de Data Lake. También puede usar rutas de acceso absolutas.  Por ejemplo:

        adl://<Data LakeStorageAccountName>.azuredatalakestore.net:443/Samples/Data/SearchLog.tsv

    Debe usar rutas de acceso absolutas para acceder a los archivos de las cuentas de almacenamiento vinculadas.  La sintaxis de los archivos almacenados en la cuenta de Almacenamiento de Azure vinculada es:

        wasb://<BlobContainerName>@<StorageAccountName>.blob.core.windows.net/Samples/Data/SearchLog.tsv

  > [!NOTE]
  > Los contenedores de blobs de Azure con permisos de acceso de contenedores públicos o blobs públicos no se admiten actualmente.    
  >
  >

**Para enviar el trabajo**

1. Abra PowerShell ISE en la estación de trabajo de Windows.
2. Ejecute el siguiente script:

        $dataLakeAnalyticsName = "<DataLakeAnalyticsAccountName>"
        $usqlScript = "c:\tutorials\data-lake-analytics\copyFile.usql"

        $job = Submit-AzureRmDataLakeAnalyticsJob -Name "convertTSVtoCSV" -AccountName $dataLakeAnalyticsName –ScriptPath $usqlScript

        Wait-AdlJob -Account $dataLakeAnalyticsName -JobId $job.JobId

        Get-AzureRmDataLakeAnalyticsJob -AccountName $dataLakeAnalyticsName -JobId $job.JobId

    En el script, el archivo de script U-SQL se almacena en c:\tutorials\data-lake-analytics\copyFile.usql. Actualice la ruta de acceso de archivo en consecuencia.

Después de finalizar el trabajo, puede usar los siguientes cmdlets para mostrar el archivo y descargarlo:

    $resourceGroupName = "<Resource Group Name>"
    $dataLakeAnalyticName = "<Data Lake Analytic Account Name>"
    $destFile = "C:\tutorials\data-lake-analytics\SearchLog-from-Data-Lake.csv"

    $dataLakeStoreName = (Get-AzureRmDataLakeAnalyticsAccount -ResourceGroupName $resourceGroupName -Name $dataLakeAnalyticName).Properties.DefaultDataLakeAccount

    Get-AzureRmDataLakeStoreChildItem -AccountName $dataLakeStoreName -path "/Output"

    Export-AzureRmDataLakeStoreItem -AccountName $dataLakeStoreName -Path "/Output/SearchLog-from-Data-Lake.csv" -Destination $destFile

## <a name="see-also"></a>Otras referencias
* Para ver el mismo tutorial con otras herramientas, haga clic en los selectores de pestañas en la parte superior de la página.
* Para ver una consulta más compleja, consulte la página sobre el [análisis de registros de sitio web mediante Análisis de Azure Data Lake](data-lake-analytics-analyze-weblogs.md).
* Para empezar a desarrollar aplicaciones con U-SQL, consulte [Desarrollo de scripts U-SQL mediante Data Lake Tools for Visual Studio](data-lake-analytics-data-lake-tools-get-started.md).
* Para obtener más información sobre U-SQL, consulte [Introducción al lenguaje U-SQL de Análisis de Azure Data Lake](data-lake-analytics-u-sql-get-started.md).
* Para las tareas de administración, consulte [Administración de Análisis de Azure Data Lake mediante el Portal de Azure](data-lake-analytics-manage-use-portal.md).
* Para obtener información general sobre Análisis de Data Lake, consulte [Información general sobre Análisis de Azure Data Lake](data-lake-analytics-overview.md).



<!--HONumber=Nov16_HO2-->


