---
title: Conexión de Configuration Manager con Log Analytics | Microsoft Docs
description: En este artículo se muestran los pasos para conectar Configuration Manager con Log Analytics y empezar a analizar datos.
services: log-analytics
documentationcenter: ''
author: bandersmsft
manager: jwhit
editor: ''

ms.service: log-analytics
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/29/2016
ms.author: banders

---
# <a name="connect-configuration-manager-to-log-analytics"></a>Conexión de Configuration Manager con Log Analytics
Puede conectar System Center Configuration Manager a Log Analytics de OMS para sincronizar datos de recopilación de dispositivos. De este modo, los datos de su implementación de Configuration Manager estarán disponibles en OMS.

Hay una serie de pasos necesarios para conectar Configuration Manager con OMS, así que presentamos un resumen del proceso general:

1. En el Portal de administración de Azure, registre Configuration Manager como una aplicación web o una aplicación de API web y asegúrese de que tiene el identificador de cliente y la clave secreta de cliente en el registro de Azure Active Directory. Consulte [Uso del portal para crear una aplicación de Active Directory y una entidad de servicio con acceso a los recursos](../resource-group-create-service-principal-portal.md) para obtener información detallada sobre cómo realizar este paso.
2. En el Portal de administración de Azure, [proporcione a Configuration Manager (la aplicación web registrada) permiso para acceder a OMS](#provide-configuration-manager-with-permissions-to-oms).
3. En Configuration Manager, [agregue una conexión mediante el Asistente para agregar una conexión de OMS](#add-an-oms-connection-to-configuration-manager).
4. En Configuration Manager, puede [actualizar las propiedades de conexión](#update-oms-connection-properties) si la clave secreta de cliente o la contraseña nunca expira o se pierde.
5. Con la información del portal de OMS, [descargue e instale el agente de supervisión de Microsoft](#download-and-install-the-agent) en el equipo que ejecuta el rol de sistema de sitio de punto de conexión del servicio de Configuration Manager. El agente envía datos de Configuration Manager a OMS.
6. En OMS, [importe recopilaciones de Configuration Manager](#import-collections) como grupos de equipos.
7. En OMS, vea los datos de Configuration Manager como [grupos de equipos](log-analytics-computer-groups.md).

Puede leer más información sobre cómo conectar Configuration Manager a OMS en [Sync data from Configuration Manager to the Microsoft Operations Management Suite](https://technet.microsoft.com/library/mt757374.aspx) (Sincronización de datos de Configuration Manager con Microsoft Operations Management Suite).

## <a name="provide-configuration-manager-with-permissions-to-oms"></a>Concesión de permisos a Configuration Manager para acceder a OMS
El procedimiento siguiente proporciona al Portal de administración de Azure permisos para acceder a OMS. En concreto, debe conceder el *rol Colaborador* a los usuarios del grupo de recursos. A su vez, esto permite que el Portal de administración de Azure conecte Configuration Manager con OMS.

> [!NOTE]
> Debe especificar permisos a OMS para Configuration Manager. De lo contrario, recibirá un mensaje de error cuando utilice el Asistente para configuración de Configuration Manager.
> 
> 

1. Abra [Azure Portal](https://portal.azure.com/) y haga clic en **Examinar** > **Log Analytics (OMS)** para abrir la hoja Log Analytics (OMS).  
2. En la hoja **Log Analytics (OMS)**, haga clic en **Agregar** para abrir la hoja **OMS Workspace** (Área de trabajo de OMS).  
   ![Hoja OMS](./media/log-analytics-sccm/sccm-azure01.png)
3. En la hoja **OMS Workspace** (Área de trabajo de OMS), proporcione la información siguiente y, después, haga clic en **Aceptar**.
   
   * **Área de trabajo de OMS**
   * **Suscripción**
   * **Grupos de recursos**
   * **Ubicación**
   * **Plan de tarifa**  
     ![Hoja OMS](./media/log-analytics-sccm/sccm-azure02.png)  
     
     > [!NOTE]
     > En el ejemplo siguiente se crea un grupo de recursos nuevo. El grupo de recursos solo se utiliza para proporcionar a Configuration Manager permisos para el área de trabajo de OMS en este ejemplo.
     > 
     > 
4. Haga clic en **Examinar** > **Grupos de recursos** para abrir la hoja **Grupos de recursos**.
5. En la hoja **Grupos de recursos**, haga clic en el grupo de recursos que creó anteriormente para abrir la hoja de configuración &lt;Nombre del grupo de recursos&gt;.  
   ![Hoja de configuración del grupo de recursos](./media/log-analytics-sccm/sccm-azure03.png)
6. En la hoja de configuración &lt;Nombre del grupo de recursos&gt;, haga clic en Control de acceso (IAM) para abrir la hoja Usuarios de &lt;Nombre del grupo de recursos&gt;.  
   ![Hoja de usuarios del grupo de recursos](./media/log-analytics-sccm/sccm-azure04.png)  
7. En la hoja Usuarios de &lt;Nombre del grupo de recursos&gt;, haga clic en **Agregar** para abrir la hoja **Agregar acceso**.
8. En la hoja **Agregar acceso**, haga clic en **Seleccionar un rol** y, después, seleccione, el rol **Colaborador**.  
   ![Seleccionar un rol](./media/log-analytics-sccm/sccm-azure05.png)  
9. Haga clic en **Agregar usuarios**, seleccione el usuario de Configuration Manager, haga clic en **Seleccionar**, y, después, en **Aceptar**.  
   ![Agregar usuarios](./media/log-analytics-sccm/sccm-azure06.png)  

## <a name="add-an-oms-connection-to-configuration-manager"></a>Adición de una conexión de OMS a Configuration Manager
Para agregar una conexión de OMS, su entorno de Configuration Manager debe tener un [punto de conexión de servicio](https://technet.microsoft.com/library/mt627781.aspx) configurado para el modo en línea.

1. En el área de trabajo **Administración** de Configuration Manager, seleccione **OMS Connector** (Conector de OMS). Se abrirá el **Asistente para agregar conexiones de OMS**. Seleccione **Siguiente**.
2. En la pantalla **General**, confirme que ha realizado las siguientes acciones y que tiene los detalles de cada elemento; después, seleccione **Siguiente**.
   
   1. En el Portal de administración de Azure, ha registrado Configuration Manager como una aplicación web o una aplicación de API web y asegúrese de que tiene el [identificador de cliente en el registro](../active-directory/active-directory-integrating-applications.md).
   2. En el Portal de administración de Azure, ha creado una clave secreta de la aplicación para la aplicación registrada en Azure Active Directory.  
   3. En el Portal de administración de Azure, ha proporcionado a la aplicación web registrada permiso para acceder a OMS.  
      ![Conexión a la página general del Asistente para OMS](./media/log-analytics-sccm/sccm-console-general01.png)
3. En la pantalla **Azure Active Directory**, establezca la configuración de conexión a OMS proporcionando su **inquilino** , el **identificador de cliente** y la **clave secreta de cliente**; después, seleccione **Siguiente**.  
   ![Conexión a la página de Azure Active Directory del Asistente para OMS](./media/log-analytics-sccm/sccm-wizard-tenant-filled03.png)
4. Si ha realizado todos los procedimientos correctamente, la información de la pantalla **OMS Connection Configuration** (Configuración de conexión de OMS) aparecerá automáticamente en esta página. Debe aparecer la información de la configuración de conexión para su **suscripción de Azure** , **grupo de recursos de Azure** y **área de trabajo de Operations Management Suite**.  
   ![Conexión a la página de conexión de OMS del Asistente para OMS](./media/log-analytics-sccm/sccm-wizard-configure04.png)
5. El asistente se conecta al servicio de OMS usando la información que ha introducido. Seleccione las recopilaciones de dispositivos que desea sincronizar con OMS y, después, haga clic en **Agregar**.  
   ![Selección de recopilaciones](./media/log-analytics-sccm/sccm-wizard-add-collections05.png)
6. Compruebe la configuración de conexión de la pantalla **Resumen** y, después, seleccione **Siguiente**. La pantalla **Progreso** muestra el estado de conexión, que debería ser **Completado**.

> [!NOTE]
> Debe conectar OMS al sitio de nivel superior de la jerarquía. Si conecta OMS a un sitio primario independiente y, después, agrega un sitio de administración central a su entorno, tendrá que eliminar y volver a crear la conexión de OMS dentro de la nueva jerarquía.
> 
> 

Después de haber vinculado Configuration Manager a OMS, puede agregar o quitar recopilaciones y ver las propiedades de la conexión de OMS.

## <a name="update-oms-connection-properties"></a>Actualización de las propiedades de conexión de OMS
Si la clave secreta de cliente o la contraseña nunca expira o se pierde, tendrá que actualizar manualmente las propiedades de conexión de OMS.

1. En Configuration Manager, acceda a **Cloud Services** , después, seleccione **OMS Connector** (Conector de OMS) para abrir la página **OMS Connection Properties** (Propiedades de conexión de OMS).
2. En esta página, haga clic en la pestaña **Azure Active Directory** para ver su **inquilino**, **identificador de cliente** y **expiración de clave secreta de cliente**. **Compruebe** si ha expirado la **clave secreta de cliente**.

## <a name="download-and-install-the-agent"></a>Descarga e instalación del agente
1. En el portal de OMS, [descargue el archivo de instalación del agente desde OMS](log-analytics-windows-agents.md#download-the-agent-setup-file-from-oms).
2. Utilice uno de los métodos siguientes para instalar y configurar al agente en el equipo que ejecuta el rol de sistema de sitio de punto del servicio de Configuration Manager:
   * [Instalación del agente con el programa de instalación](log-analytics-windows-agents.md#install-the-agent-using-setup)
   * [Instalación del agente a través de la línea de comandos](log-analytics-windows-agents.md#install-the-agent-using-the-command-line)
   * [Instalación del agente utilizando DSC en Azure Automation](log-analytics-windows-agents.md#install-the-agent-using-dsc-in-azure-automation)

## <a name="import-collections"></a>Importación de recopilaciones
Después de agregar una conexión de OMS en Configuration Manager y de instalar el agente en el equipo que ejecuta la conexión al servicio de Configuration Manager, seleccione el rol de sistema de sitio. Después, tiene que importar las recopilaciones de Configuration Manager en OMS como grupos de equipos.

Cuando se habilite la importación, la información de pertenencia de recopilaciones se recupera cada 3 horas para mantener las pertenencias a las recopilaciones actuales. Puede deshabilitar la importación en cualquier momento.

1. En el portal de OMS, haga clic en **Configuración**.
2. Haga clic en la pestaña **Grupos de equipos** y, después, en la pestaña **SCCM**.
3. Seleccione **Importar pertenencias de la recopilación de Configuration Manager** y, después, haga clic en **Guardar**.  
   ![Grupos de equipos: pestaña SCCM](./media/log-analytics-sccm/sccm-computer-groups01.png)

## <a name="view-data-from-configuration-manager"></a>Visualización de datos de Configuration Manager
Después de agregar una conexión de OMS en Configuration Manager y de instalar el agente en el equipo que ejecuta la conexión al servicio de Configuration Manager, seleccione el rol de sistema de sitio. En OMS, las recopilaciones de Configuration Manager aparecen como [grupos de equipos](log-analytics-computer-groups.md). Puede ver los grupos de la página **Configuration Manager** en **Grupos de equipos**, que se encuentra en **Configuración**.

Después de importar las recopilaciones, puede ver cuántos equipos con pertenencias a las recopilaciones se han detectado. También puede ver el número de recopilaciones que se han importado.

![Grupos de equipos: pestaña SCCM](./media/log-analytics-sccm/sccm-computer-groups02.png)

Al hacer clic en cualquiera de ellos, se abre Búsqueda, que muestra todos los grupos importados o todos los equipos que pertenecen a cada grupo. Mediante [Búsqueda de registros](log-analytics-log-searches.md), puede iniciar un análisis exhaustivo de datos de Configuration Manager.

## <a name="next-steps"></a>Pasos siguientes
* Use [Búsqueda de registros](log-analytics-log-searches.md) para ver información detallada sobre los datos de Configuration Manager.

<!--HONumber=Oct16_HO2-->


