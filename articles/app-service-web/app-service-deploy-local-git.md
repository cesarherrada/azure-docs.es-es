---
title: Implementación de Git local en el Servicio de aplicaciones de Azure
description: Aprenda a habilitar la implementación de Git local en el Servicio de aplicaciones de Azure.
services: app-service
documentationcenter: ''
author: dariagrigoriu
manager: wpickett
editor: mollybos

ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/13/2016
ms.author: dariagrigoriu

---
# Implementación de Git local en el Servicio de aplicaciones de Azure
En este tutorial se muestra cómo implementar la aplicación en el [Servicio de aplicaciones de Azure] desde un repositorio de Git en el equipo local. El Servicio de aplicaciones admite este enfoque con la opción de implementación **Git local** en el [Portal de Azure]. Muchos de los comandos de Git que se describen en este artículo se ejecutan automáticamente al crear una aplicación del Servicio de aplicaciones con la [interfaz de la línea de comandos de Azure], tal como se describe [aquí](app-service-web-get-started.md).

## Requisitos previos
Para completar este tutorial, necesita:

* Git. Puede descargar el archivo binario de instalación [aquí](http://www.git-scm.com/downloads).
* Conocimientos básicos de Git.
* Una cuenta de Microsoft Azure. Si aún no tiene ninguna, puede [registrarse para obtener una prueba gratuita](https://azure.microsoft.com/pricing/free-trial) o bien [activar las ventajas de suscriptor de Visual Studio](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details).

> [!NOTE]
> Si desea empezar a trabajar con el Servicio de aplicaciones de Azure antes de inscribirse para abrir una cuenta de Azure, vaya a [Probar Servicio de aplicaciones](http://go.microsoft.com/fwlink/?LinkId=523751), donde podrá crear inmediatamente una aplicación de inicio de corta duración en el Servicio de aplicaciones. No es necesario proporcionar ninguna tarjeta de crédito ni asumir ningún compromiso.
> 
> 

## <a id="Step1"></a>Paso 1: Creación de un repositorio local
Realice las tareas siguientes al crear un nuevo repositorio Git.

1. Inicie una herramienta de línea de comandos, como **GitBash** (Windows) o **Bash** (shell de Unix). En los sistemas OS X puede tener acceso a la línea de comandos mediante la aplicación **Terminal**.
2. Desplácese al directorio donde se ubicará el contenido que se va a implementar.
3. Utilice el comando siguiente para inicializar un nuevo repositorio Git:
   
        git init

## <a id="Step2"></a>Paso 2: Confirmación del contenido
El Servicio de aplicaciones admite aplicaciones creadas en varios lenguajes de programación.

1. Si el repositorio ya incluye contenido, omita este punto y vaya al punto 2. Si el repositorio todavía no incluye contenido, simplemente llénelo con un archivo .html estático, tal como se indica a continuación:
   
   * Con un editor de texto, cree un archivo nuevo denominado **index.html** en la raíz del repositorio Git.
   * Agregue el texto siguiente como contenido para el archivo index.htm y guárdelo: *Hello Git!*
2. Desde la línea de comandos, compruebe que está en la raíz de su repositorio de Git. Use el comando siguiente para agregar archivos al repositorio:
   
        git add -A 
3. A continuación, confirme los cambios en el repositorio mediante el siguiente comando:
   
        git commit -m "Hello Azure App Service"

## <a id="Step3"></a>Paso 3: Habilitación del repositorio de aplicaciones del Servicio de aplicaciones
Lleve a cabo los pasos siguientes para habilitar un repositorio de Git para su aplicación del Servicio de aplicaciones.

1. Inicie sesión en el [Portal de Azure].
2. En la hoja de su aplicación del Servicio de aplicaciones, haga clic en **Configuración > Origen de implementación**. Haga clic en **Elegir origen**, **Repositorio de Git local** y **Aceptar**.
   
    ![Repositorio de Git local](./media/app-service-deploy-local-git/local_git_selection.png)
3. Si esta es la primera vez que configura un repositorio en Azure, tendrá que crear unas credenciales de inicio de sesión para él. Las usará para iniciar sesión en el repositorio de Azure y aplicar cambios desde su repositorio Git local. En la hoja de la aplicación, haga clic en **Configuración > Credenciales de implementación** y configure el nombre de usuario y la contraseña para la implementación. Cuando haya terminado, haga clic en **Guardar**.
   
    ![](./media/app-service-deploy-local-git/deployment_credentials.png)

## <a id="Step4"></a>Paso 4: Implementación del proyecto
Siga los pasos que se indican a continuación para publicar una aplicación en el Servicio de aplicaciones mediante un Git local.

1. En la hoja de su aplicación del Portal de Azure, haga clic en **Configuración > Propiedades** para la **Dirección URL de Git**.
   
    ![](./media/app-service-deploy-local-git/git_url.png)
   
    **Dirección URL de Git** es la referencia remota en la que se realizará la implementación desde el repositorio local. Usará esta dirección URL en los pasos siguientes.
2. Mediante la línea de comandos, compruebe que está en la raíz de su repositorio de Git local.
3. Use `git remote` para agregar la referencia remota que aparecía en **Dirección URL de Git** en el paso 1. El comando será similar al siguiente:
   
        git remote add azure https://<username>@localgitdeployment.scm.azurewebsites.net:443/localgitdeployment.git         
   > [!NOTE]
   > El comando **remote** agrega una referencia con nombre a un repositorio remoto. En este ejemplo, se crea una referencia llamada "azure" para el repositorio de la aplicación web.
   > 
   > 
4. Inserte el contenido en el Servicio de aplicaciones con el nuevo comando remoto **azure** que acaba de crear.
   
        git push azure master
   
    Se le solicitará la contraseña que ha creado antes al restablecer las credenciales de implementación en el Portal de Azure. Escriba la contraseña (tenga en cuenta que Gitbash no envía los asteriscos a la consola mientras escribe la contraseña).
5. Vuelva a la aplicación en el Portal de Azure. En la hoja **Implementaciones** debería aparecer una entrada de registro de la última inserción.
   
    ![](./media/app-service-deploy-local-git/deployment_history.png)
6. Haga clic en el botón **Examinar** situado en la parte superior de la hoja de la aplicación para comprobar que el contenido se haya implementado.

## <a id="Step5"></a>Solución de problemas
Estos son los errores o problemas que suelen aparecer al usar Git para publicar en una aplicación del Servicio de aplicaciones en Azure:

- - -
**Síntoma**: no es posible tener acceso a ’[siteURL]’: error al conectarse a [scmAddress]

**Causa**: este error puede producirse si la aplicación no está en funcionamiento.

**Resolución**: inicie la aplicación en el Portal de Azure. La implementación de Git no funcionará a menos que se esté ejecutando la aplicación.

- - -
**Síntoma**: no se pudo resolver el host "nombre de host".

**Causa**: este error puede ocurrir si la información de dirección introducida al crear el repositorio remoto "azure" no era correcta.

**Resolución**: use el comando `git remote -v` para obtener un listado de todos los remotos, junto con la dirección URL asociada. Compruebe que la URL para el repositorio correcto "azure" es correcta. Si lo necesita, suprima y vuelva a crear este repositorio remoto utilizando la URL correcta.

- - -
**Síntoma**: no hay referencias en común y no se ha especificado nada; sin hacer nada. Quizá hay que especificar una rama como "principal".

**Causa**: este error puede ocurrir si no especifica una rama al realizar una operación de inserción en Git y no hay establecido el valor push.default utilizado por Git.

**Resolución**: vuelva a realizar la operación de inserción, especificando la rama principal. Por ejemplo:

    git push azure master

- - -
**Síntoma**: src refspec [branchname] no coincide con ninguna.

**Causa**: este error puede tener lugar si intenta insertar una rama que no es la principal en el repositorio remoto "azure".

**Resolución**: vuelva a realizar la operación de inserción, especificando la rama principal. Por ejemplo:

    git push azure master

- - -
**Síntoma**: Error: los cambios se han confirmado en el repositorio remoto, pero la aplicación web no se ha actualizado.

**Causa**: este error puede ocurrir si está implementando una aplicación Node.js que contiene un archivo package.json que especifica módulos requeridos adicionales.

**Resolución**: los mensajes adicionales que contienen "npm ERR!" deberían registrarse antes de este error y pueden proporcionar contexto adicional sobre el error. A continuación se indican las causas conocidas de este error y el mensaje 'npm ERR!' correspondiente:

* **Archivo package.json con estructura incorrecta**: npm ERR! No se pudieron leer las dependencias.
* **Módulo nativo que no tiene una distribución binaria para Windows**:
  
  * npm ERR! `cmd "/c" "node-gyp rebuild"` failed with 1
    
      OR
  * npm ERR! [modulename@version] preinstall: `make || gmake`

## Recursos adicionales
* [Documentación de Git](http://git-scm.com/documentation)
* [Documentación de Project Kudu](https://github.com/projectkudu/kudu/wiki)
* [Implementación continua en el Servicio de aplicaciones de Azure](app-service-continuous-deployment.md)
* [Uso de PowerShell para Azure](../powershell-install-configure.md)
* [Cómo utilizar la interfaz de línea de comandos de Azure](../xplat-cli-install.md)

[Servicio de aplicaciones de Azure]: https://azure.microsoft.com/documentation/articles/app-service-changes-existing-services/
[Azure Developer Center]: http://azure.microsoft.com/develop/overview/
[Portal de Azure]: https://portal.azure.com
[Git website]: http://git-scm.com
[Installing Git]: http://git-scm.com/book/en/Getting-Started-Installing-Git
[interfaz de la línea de comandos de Azure]: https://azure.microsoft.com/documentation/articles/xplat-cli-azure-resource-manager/

[Using Git with CodePlex]: http://codeplex.codeplex.com/wikipage?title=Using%20Git%20with%20CodePlex&referringTitle=Source%20control%20clients&ProjectName=codeplex
[Quick Start - Mercurial]: http://mercurial.selenic.com/wiki/QuickStart

<!---HONumber=AcomDC_0803_2016-->