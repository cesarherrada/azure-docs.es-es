---
title: Etiquetado de una máquina virtual Linux | Microsoft Docs
description: Infórmese acerca del etiquetado de una máquina virtual Linux en Azure creada con el modelo de implementación de Resource Manager.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: timlt
editor: tysonn
tags: azure-resource-manager

ms.service: virtual-machines-linux
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 07/05/2016
ms.author: memccror

---
# Etiquetado de una máquina virtual Linux en Azure
En este artículo se describen diferentes maneras de etiquetar una máquina virtual en Azure el modelo de implementación de Resource Manager. Las etiquetas son pares clave-valor definidos por el usuario que se pueden colocar directamente en un recurso o un grupo de recursos. Actualmente, Azure admite un máximo de 15 etiquetas por recurso y grupo de recursos. Las etiquetas se pueden colocar en un recurso en el momento de su creación, o bien se pueden agregar a un recurso existente. Tenga en cuenta que las etiquetas solo son compatibles con los recursos creados mediante el modelo de implementación de Resource Manager.

[!INCLUDE [virtual-machines-common-tag](../../includes/virtual-machines-common-tag.md)]

## Etiquetado con la CLI de Azure
Para comenzar, [instale y configure la CLI de Azure](../xplat-cli-azure-resource-manager.md) y asegúrese de que está en modo de Resource Manager (`azure config mode arm`).

Con este comando es posible ver todas las propiedades de una máquina virtual dada, incluidas las etiquetas:

        azure vm show -g MyResourceGroup -n MyTestVM

Para agregar una nueva etiqueta de máquina virtual a través de la CLI de Azure, puede usar el comando `azure vm set` junto con el parámetro de etiqueta **-t**:

        azure vm set -g MyResourceGroup -n MyTestVM –t myNewTagName1=myNewTagValue1;myNewTagName2=myNewTagValue2

Para quitar todas las etiquetas, puede usar el parámetro **-T** en el comando `azure vm set`.

        azure vm set – g MyResourceGroup –n MyTestVM -T


Ahora que hemos aplicado etiquetas a nuestros recursos a través de PowerShell, la CLI de Azure y el portal, analicemos los datos de uso para ver las etiquetas del portal de facturación.

[!INCLUDE [virtual-machines-common-tag-usage](../../includes/virtual-machines-common-tag-usage.md)]

## Pasos siguientes
* Para obtener más información sobre el etiquetado de los recursos de Azure, consulte [Información general de Azure Resource Manager][Información general de Azure Resource Manager] y [Uso de etiquetas para organizar los recursos de Azure][Uso de etiquetas para organizar los recursos de Azure].
* Para ver cómo las etiquetas pueden ayudarle a administrar el uso de recursos de Azure, consulte [Comprender la factura de Microsoft Azure][Comprender la factura de Microsoft Azure] y [Obtención de información sobre el consumo de recursos de Microsoft Azure][Obtención de información sobre el consumo de recursos de Microsoft Azure].

[Azure CLI environment]: ./xplat-cli-azure-resource-manager.md
[Información general de Azure Resource Manager]: ../resource-group-overview.md
[Uso de etiquetas para organizar los recursos de Azure]: ../resource-group-using-tags.md
[Comprender la factura de Microsoft Azure]: ../billing-understand-your-bill.md
[Obtención de información sobre el consumo de recursos de Microsoft Azure]: ../billing-usage-rate-card-overview.md

<!---HONumber=AcomDC_0706_2016-->