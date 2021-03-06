---
title: Formato de token de usuario externo para la vista previa de colaboración de Azure Active Directory B2B | Microsoft Docs
description: Azure Active Directory B2B posibilita las relaciones entre empresas al permitir que los partners empresariales accedan de forma selectiva a las aplicaciones corporativas.
services: active-directory
documentationcenter: ''
author: viv-liu
manager: cliffdi
editor: ''
tags: ''

ms.service: active-directory
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: NA
ms.workload: na
ms.date: 05/09/2016
ms.author: viviali

---
# Vista previa de la colaboración B2B de Azure AD: formato de token de usuario externo
Las notificaciones para un anuncio de Azure estándar token se describen en la [admite tokens y los tipos de notificación](active-directory-token-and-claims.md) artículo en azure.microsoft.com.

Las notificaciones que son diferentes para un usuario externo de colaboración B2B autenticado son las siguientes:<br/>

* **OID:** el identificador de objeto del inquilino de recursos<br/>
* **TID**: identificador de inquilino del inquilino de recursos<br/>
* **Emisor**: se trata del inquilino de recursos<br/>
* **IDP**: se trata del inquilino principal del usuario<br/>
* **AltSecId**: se trata del identificador de seguridad alternativo, que es opaco para usted<br/>

## Artículos relacionados
Examine nuestros otros artículos sobre la colaboración B2B de Azure AD:

* [¿Qué es la colaboración B2B de Azure AD?](active-directory-b2b-what-is-azure-ad-b2b.md)
* [Cómo funciona](active-directory-b2b-how-it-works.md)
* [Tutorial detallado](active-directory-b2b-detailed-walkthrough.md)
* [Referencia de formato de archivo CSV](active-directory-b2b-references-csv-file-format.md)
* [Cambios de atributo de objeto de usuario externo](active-directory-b2b-references-external-user-object-attribute-changes.md)
* [Limitaciones de la vista previa actual](active-directory-b2b-current-preview-limitations.md)
* [Índice de artículos sobre la administración de aplicaciones en Azure Active Directory](active-directory-apps-index.md)

<!---HONumber=AcomDC_0511_2016-->