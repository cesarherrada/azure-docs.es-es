---
title: Uso de Análisis de transmisiones de Azure con Almacenamiento de datos SQL | Microsoft Docs
description: Sugerencias para usar Análisis de transmisiones de Azure con Almacenamiento de datos SQL de Azure para el desarrollo de soluciones.
services: sql-data-warehouse
documentationcenter: NA
author: kevinvngo
manager: barbkess
editor: ''

ms.service: sql-data-warehouse
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: data-services
ms.date: 08/16/2016
ms.author: kevin;barbkess;sonyama

---
# Uso de Análisis de transmisiones de Azure con Almacenamiento de datos SQL
Análisis de transmisiones de Azure es un servicio totalmente administrado que proporciona un procesamiento completo de eventos de baja latencia, alta disponibilidad y escalable a través el streaming de datos en la nube. Para aprender los conceptos básicos, lea [Introducción a Análisis de transmisiones de Azure][Introducción a Análisis de transmisiones de Azure]. Después puede aprender a crear una solución de extremo a extremo siguiendo el tutorial [Introducción al uso de Azure Stream Analytics][Introducción al uso de Azure Stream Analytics].

En este artículo, aprenderá a usar la base de datos de Almacenamiento de datos SQL de Azure como receptor de salida para los trabajos de Análisis de transmisiones.

## Requisitos previos
En primer lugar, ejecute los pasos siguientes del tutorial [Introducción al uso de Azure Stream Analytics][Introducción al uso de Azure Stream Analytics].

1. Creación de una entrada de Centro de eventos
2. Configuración e inicio de la aplicación del generador de eventos
3. Aprovisionamiento de un trabajo de Stream Analytics
4. Especificación de una consulta y entrada de trabajo

Luego cree una base de datos de Almacenamiento de datos SQL de Azure.

## Especifique la salida de trabajo: base de datos de Almacenamiento de datos SQL de Azure.
### Paso 1
En el trabajo de Análisis de transmisiones, haga clic en **SALIDA** en la parte superior de la página y luego en **AGREGAR SALIDA**.

### Paso 2
Seleccione Base de datos SQL y haga clic en Siguiente.

![][add-output]

### Paso 3
Escriba estos valores en la página siguiente:

* *Alias de salida*: escriba un nombre descriptivo para esta salida de trabajo.
* *Suscripción*:
  * Si la base de datos del Almacenamiento de datos SQL está en la misma suscripción que el trabajo de Análisis de transmisiones, seleccione Usar la base de datos SQL de la suscripción actual".
  * Si la base de datos está en una suscripción diferente, seleccione Usar la base de datos SQL de otra suscripción.
* *Base de datos*: especifique el nombre de una base de datos de destino.
* *Nombre del servidor*: especifique el nombre del servidor para la base de datos especificada. Para encontrarlo, puede usar el Portal de Azure clásico.

![][server-name]

* *Nombre de usuario*: especifique el nombre de usuario de una cuenta que tenga permisos de escritura para la base de datos.
* *Contraseña*: proporcione la contraseña de la cuenta de usuario especificada.
* *Tabla*: especifique el nombre de la tabla de destino en la base de datos.

![][add-database]

### Paso 4
Haga clic en el botón de comprobación para agregar esta salida de trabajo y comprobar que Análisis de transmisiones puede conectarse correctamente a la base de datos.

![][test-connection]

Cuando la conexión a la base de datos se realice correctamente, verá una notificación en la parte inferior del portal. Puede hacer clic en Probar conexión en la parte inferior para probar la conexión con la base de datos.

## Pasos siguientes
Para obtener información general sobre la integración, consulte [Información general de la integración de Almacenamiento de datos SQL][Información general de la integración de Almacenamiento de datos SQL].

Para obtener más sugerencias sobre desarrollo, consulte la [información general sobre desarrollo de Almacenamiento de datos SQL][información general sobre desarrollo de Almacenamiento de datos SQL].

<!--Image references-->

[add-output]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-output.png
[server-name]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/dw-server-name.png
[add-database]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/add-database.png
[test-connection]: ./media/sql-data-warehouse-integrate-azure-stream-analytics/test-connection.png

<!--Article references-->

[Introducción a Análisis de transmisiones de Azure]: ../stream-analytics/stream-analytics-introduction.md
[Introducción al uso de Azure Stream Analytics]: ../stream-analytics/stream-analytics-get-started.md
[información general sobre desarrollo de Almacenamiento de datos SQL]: ./sql-data-warehouse-overview-develop.md
[Información general de la integración de Almacenamiento de datos SQL]: ./sql-data-warehouse-overview-integrate.md

<!--MSDN references-->

<!--Other Web references-->
[Azure Stream Analytics documentation]: http://azure.microsoft.com/documentation/services/stream-analytics/

<!---HONumber=AcomDC_0817_2016-->