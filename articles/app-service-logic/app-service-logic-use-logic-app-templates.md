---
title: Plantillas de aplicaciones lógicas | Microsoft Docs
description: Aprenda a usar plantillas de aplicaciones lógicas creadas previamente que le ayudarán a comenzar.
author: kevinlam1
manager: dwrede
editor: ''
services: app-service\logic
documentationcenter: ''

ms.service: app-service-logic
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/24/2016
ms.author: klam

---
# Plantillas de aplicaciones lógicas
## ¿Qué son las plantillas de aplicaciones lógicas?
Una plantilla de aplicación lógica es una aplicación lógica pregenerada que se puede usar para comenzar rápidamente a crear su propio flujo de trabajo.

Estas plantillas son una buena forma de detectar patrones distintos que se pueden crear mediante aplicaciones lógicas. Puede usar estas plantillas tal cual o modificarlas para adaptarlas a su escenario.

## Información general de plantillas disponibles
Hay muchas plantillas disponibles publicadas actualmente en la plataforma de aplicaciones lógicas. A continuación se muestran algunas categorías de ejemplo, así como los tipos de conectores que utilizan.

### Plantillas de empresa en la nube
Plantillas que integran Dynamics CRM, Salesforce, Box, Blob de Azure y otros conectores para sus necesidades empresariales en la nube. Entre los ejemplos de lo que se puede hacer con estas plantillas se incluye la organización de sus clientes potenciales y la copia de seguridad de los datos de archivos corporativos.

### Plantillas de Enterprise Integration Pack
Configuraciones de canalizaciones VETER (Validación, Extracción, Transformación, Enriquecimiento, Enrutamiento), recepción de documentos X12 EDI a través de AS2 y transformación en XML así como control de mensajes X12 y AS2.

### Plantillas de patrón de protocolo
Estas plantillas constan de aplicaciones lógicas que contienen patrones de protocolo como solicitud-respuesta a través de HTTP así como integraciones a través de FTP y SFTP. Utilícelas tal y como están o como base para crear patrones más complejos de protocolo.

### Plantillas de productividad personal
Entre los patrones para ayudar a mejorar la productividad personal se incluyen plantillas que permiten establecer avisos diarios, convierten los elementos de trabajo importantes en listas de tareas pendientes y automatizan las tareas largas con un único paso de aprobación de usuario.

### Plantillas en la nube del consumidor
Plantillas simples que se integran con los servicios de las redes sociales como Twitter, Slack o el correo electrónico, capaces a la larga de reforzar las iniciativas de marketing de las redes sociales. También se incluyen plantillas como la de copia repetitiva que permiten aumentar la productividad mediante el ahorro del tiempo invertido en tareas habitualmente repetitivas.

## Creación de una aplicación lógica mediante una plantilla
Para comenzar a usar la plantilla de aplicaciones lógicas, vaya al diseñador de aplicaciones lógicas. Si accede al diseñador abriendo una aplicación lógica que ya existe, esta se cargará automáticamente en la vista del diseñador. Sin embargo, si va a crear una nueva aplicación lógica, verá la pantalla siguiente. ![](../../includes/media/app-service-logic-templates/template7.png)

En esta pantalla, puede elegir empezar con una aplicación lógica en blanco o una plantilla predefinida. Si selecciona una de las plantillas, se proporcionará información adicional. En este ejemplo, se utilizará la plantilla *When a new file is created in Dropbox, copy it to OneDrive* (Si se crea un nuevo archivo en Dropbox, cópiese en OneDrive). ![](../../includes/media/app-service-logic-templates/template2.png)

Si decide utilizar la plantilla, solo tiene que seleccionar el botón *Usar esta plantilla*. Se le pedirá que inicie sesión en sus cuentas en función de los conectores que utiliza la plantilla. O, si ya ha establecido previamente una conexión con estos conectores, puede seleccionar el botón Continuar tal y como se muestra a continuación. ![](../../includes/media/app-service-logic-templates/template3.png)

Después de establecer la conexión y seleccionar el botón *Continuar*, la aplicación lógica se abre en la vista del diseñador. ![](../../includes/media/app-service-logic-templates/template4.png)

En el ejemplo anterior, como sucede con muchas plantillas, algunos de los campos de propiedades obligatorios pueden rellenarse dentro de los conectores; sin embargo, puede que algunos requieran un valor antes de poder implementar correctamente la aplicación lógica. Si intenta implementar sin rellenar algunos de los campos que faltan, se le notificará con un mensaje de error.

Si desea volver al visor de plantillas, seleccione el botón *Plantillas* en la barra de navegación superior. Al volver al visor de plantillas, perderá cualquier progreso no guardado. Antes de volver al visor de plantillas, verá un mensaje de advertencia que le avisará de esto. ![](../../includes/media/app-service-logic-templates/template5.png)

## Implementación de una aplicación lógica creada desde una plantilla
Una vez que ha cargado la plantilla y realizado los cambios deseados, seleccione el botón Guardar situado en la esquina superior izquierda. Esto permite guardar y publicar la aplicación lógica. ![](../../includes/media/app-service-logic-templates/template6.png)

Para más información sobre cómo agregar más pasos en una plantilla de aplicación lógica existente o realizar modificaciones en general, consulte [Creación de una aplicación lógica](app-service-logic-create-a-logic-app.md).

<!---HONumber=AcomDC_0831_2016-->