---
title: Creación de una aplicación lógica mediante plantillas del Administrador de recursos de Azure en el Servicio de aplicaciones de Azure | Microsoft Docs
description: Use una plantilla del Administrador de recursos de Azure para implementar una aplicación lógica vacía para definir flujos de trabajo.
services: logic-apps
documentationcenter: ''
author: MSFTMan
manager: erikre
editor: ''

ms.service: logic-apps
ms.workload: integration
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/25/2016
ms.author: deonhe

---
# Creación de una aplicación lógica mediante una plantilla
Use una plantilla del Administrador de recursos de Azure para crear una aplicación lógica vacía que pueda utilizarse para definir los flujos de trabajo. Puede definir los recursos que se implementan y los parámetros que se especifican cuando se ejecuta la implementación. Puede usar esta plantilla para sus propias implementaciones o personalizarla para satisfacer sus necesidades.

Para obtener más detalles sobre las propiedades de la aplicación lógica, consulte [API de administración de flujos de trabajo de aplicaciones lógicas](https://msdn.microsoft.com/library/azure/mt643788.aspx).

Para obtener ejemplos de la propia definición, consulte [Creación de definiciones de aplicaciones lógicas](app-service-logic-author-definitions.md).

Para obtener más información sobre la creación de plantillas, consulte [Creación de plantillas de Administrador de recursos de Azure](../resource-group-authoring-templates.md).

Para la plantilla completa, consulte [Plantilla de aplicación lógica](https://github.com/Azure/azure-quickstart-templates/blob/master/101-logic-app-create/azuredeploy.json).

## Lo que implementará
Con esta plantilla, implementará una aplicación lógica.

Para ejecutar automáticamente la implementación, seleccione el botón siguiente:

[![Implementación en Azure](media/app-service-logic-arm-provision/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F101-logic-app-create%2Fazuredeploy.json)

## Parámetros
[!INCLUDE [app-service-logic-deploy-parameters](../../includes/app-service-logic-deploy-parameters.md)]

### testUri
     "testUri": {
        "type": "string",
        "defaultValue": "http://azure.microsoft.com/status/feed/"
      }

## Recursos para implementar
### Aplicación lógica
Crea la aplicación lógica.

Las plantillas utilizan un valor de parámetro para el nombre de aplicación lógica. Establece la ubicación de la aplicación lógica en la misma ubicación que el grupo de recursos.

Esta definición determinada se ejecuta una vez cada hora y hace ping en la ubicación especificada en el parámetro **testUri**.

    {
      "type": "Microsoft.Logic/workflows",
      "apiVersion": "2016-06-01",
      "name": "[parameters('logicAppName')]",
      "location": "[resourceGroup().location]",
      "tags": {
        "displayName": "LogicApp"
      },
      "properties": {
        "definition": {
          "$schema": "http://schema.management.azure.com/providers/Microsoft.Logic/schemas/2016-06-01/workflowdefinition.json#",
          "contentVersion": "1.0.0.0",
          "parameters": {
            "testURI": {
              "type": "string",
              "defaultValue": "[parameters('testUri')]"
            }
          },
          "triggers": {
            "recurrence": {
              "type": "recurrence",
              "recurrence": {
                "frequency": "Hour",
                "interval": 1
              }
            }
          },
          "actions": {
            "http": {
              "type": "Http",
              "inputs": {
                "method": "GET",
                "uri": "@parameters('testUri')"
              },
              "runAfter": {}
            }
          },
          "outputs": {}
        },
        "parameters": {}
      }
    }


## Comandos para ejecutar la implementación
[!INCLUDE [app-service-deploy-commands](../../includes/app-service-deploy-commands.md)]

### PowerShell
    New-AzureRmResourceGroupDeployment -TemplateUri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -ResourceGroupName ExampleDeployGroup

### Azure CLI
    azure group deployment create --template-uri https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-logic-app-create/azuredeploy.json -g ExampleDeployGroup




<!---HONumber=AcomDC_0803_2016-->