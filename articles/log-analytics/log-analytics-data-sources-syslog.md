---
title: Mensajes de Syslog en Log Analytics | Microsoft Docs
description: Syslog es un protocolo de registro de eventos que es común a Linux.   En este artículo se describe cómo configurar la recopilación de mensajes de Syslog en Log Analytics y detalles de los registros que crean en el repositorio de OMS.
services: log-analytics
documentationcenter: ''
author: bwren
manager: jwhit
editor: tysonn

ms.service: log-analytics
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/06/2016
ms.author: bwren

---
# <a name="syslog-data-sources-in-log-analytics"></a>Orígenes de datos de Syslog en Log Analytics
Syslog es un protocolo de registro de eventos que es común a Linux.  Las aplicaciones envían mensajes que pueden almacenarse en la máquina local o entregarse a un recopilador de Syslog.  Al instalar el agente de OMS para Linux, este configura el demonio Syslog local para que reenvíe mensajes al agente.  En ese momento, el agente envía el mensaje a Log Analytics, donde se crea un registro correspondiente en el repositorio de OMS.  

> [!NOTE]
> Log Analytics permite recopilar mensajes enviados por rsyslog o syslog-ng. El demonio predeterminado de Syslog en la versión 5 de Red Hat Enterprise Linux, CentOS y Oracle Linux (Sysklog) no se admite para la recopilación de eventos de Syslog. Para recopilar datos de Syslog de esta versión de estas distribuciones, es necesario instalar el [demonio rsyslog](http://rsyslog.com) y configurarlo para reemplazar Sysklog.
> 
> 

![Recopilación de Syslog](media/log-analytics-data-sources-syslog/overview.png)

## <a name="configuring-syslog"></a>Configuración de Syslog
El agente de OMS para Linux solo recopilará los eventos con los recursos y los niveles de gravedad que se especifican en su configuración.  Puede configurar Syslog a través del portal de OMS o mediante la administración de archivos de configuración de sus agentes de Linux.

### <a name="configure-syslog-in-the-oms-portal"></a>Configuración de Syslog en el portal de OMS
Configure Syslog desde el [menú de datos en la configuración de Log Analytics](log-analytics-data-sources.md#configuring-data-sources).  Esta configuración se entrega al archivo de configuración de cada agente de Linux.

Para agregar un nuevo recurso, escriba su nombre y haga clic en **+**.  Para cada recurso, solo se recopilarán los mensajes con los niveles de gravedad seleccionados.  Compruebe los niveles de gravedad del recurso determinado que desea recopilar.  No puede proporcionar criterios adicionales para filtrar mensajes.

![Configuración de Syslog](media/log-analytics-data-sources-syslog/configure.png)

De forma predeterminada, todos los cambios realizados en la configuración se insertan automáticamente en todos los agentes.  Si desea configurar Syslog manualmente en cada uno de los agentes de Linux, desactive la casilla *Apply below configuration to my Linux machines*(Aplicar la configuración siguiente a mis máquinas Linux).

### <a name="configure-syslog-on-linux-agent"></a>Configuración de Syslog en agente de Linux
Cuando el [agente de OMS se instala en un cliente Linux](log-analytics-linux-agents.md), instala un archivo de configuración de Syslog predeterminado que define el recurso y la gravedad de los mensajes que se recopilan.  Puede modificar este archivo para cambiar la configuración.  El archivo de configuración es diferente según el demonio Syslog que ha instalado el cliente.

> [!NOTE]
> Si modifica la configuración de Syslog, tiene que reiniciar el demonio Syslog para que los cambios surtan efecto.
> 
> 

#### <a name="rsyslog"></a>rsyslog
El archivo de configuración de rsyslog se encuentra en **/etc/rsyslog.d/95-omsagent.conf**.  A continuación se muestra su contenido predeterminado.  Recopila mensajes de Syslog enviados desde el agente local para todos los recursos con, al menos, un nivel de advertencia.

    kern.warning       @127.0.0.1:25224
    user.warning       @127.0.0.1:25224
    daemon.warning     @127.0.0.1:25224
    auth.warning       @127.0.0.1:25224
    syslog.warning     @127.0.0.1:25224
    uucp.warning       @127.0.0.1:25224
    authpriv.warning   @127.0.0.1:25224
    ftp.warning        @127.0.0.1:25224
    cron.warning       @127.0.0.1:25224
    local0.warning     @127.0.0.1:25224
    local1.warning     @127.0.0.1:25224
    local2.warning     @127.0.0.1:25224
    local3.warning     @127.0.0.1:25224
    local4.warning     @127.0.0.1:25224
    local5.warning     @127.0.0.1:25224
    local6.warning     @127.0.0.1:25224
    local7.warning     @127.0.0.1:25224

Para quitar un recurso, elimine su sección del archivo de configuración.  Puede limitar los niveles de gravedad que se recopilan para un recurso determinado mediante la modificación de la entrada de ese recurso.  Por ejemplo, para limitar el recurso del usuario a mensajes con una gravedad de error o superior, debe modificar esa línea del archivo de configuración de la siguiente manera:

    user.error  @127.0.0.1:25224


#### <a name="syslog-ng"></a>syslog-ng
El archivo de configuración de rsyslog se encuentra en **/etc/syslog-ng/syslog-ng.conf**.  A continuación se muestra su contenido predeterminado.  Recopila mensajes de Syslog enviados desde el agente local para todos los recursos y todos los niveles de gravedad.   

    #
    # Warnings (except iptables) in one file:
    #
    destination warn { file("/var/log/warn" fsync(yes)); };
    log { source(src); filter(f_warn); destination(warn); };

    #OMS_Destination
    destination d_oms { udp("127.0.0.1" port(25224)); };

    #OMS_facility = auth
    filter f_auth_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(auth); };
    log { source(src); filter(f_auth_oms); destination(d_oms); };

    #OMS_facility = authpriv
    filter f_authpriv_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(authpriv); };
    log { source(src); filter(f_authpriv_oms); destination(d_oms); };

    #OMS_facility = cron
    filter f_cron_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(cron); };
    log { source(src); filter(f_cron_oms); destination(d_oms); };

    #OMS_facility = daemon
    filter f_daemon_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(daemon); };
    log { source(src); filter(f_daemon_oms); destination(d_oms); };

    #OMS_facility = kern
    filter f_kern_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(kern); };
    log { source(src); filter(f_kern_oms); destination(d_oms); };

    #OMS_facility = local0
    filter f_local0_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local0); };
    log { source(src); filter(f_local0_oms); destination(d_oms); };

    #OMS_facility = local1
    filter f_local1_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(local1); };
    log { source(src); filter(f_local1_oms); destination(d_oms); };

    #OMS_facility = mail
    filter f_mail_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(mail); };
    log { source(src); filter(f_mail_oms); destination(d_oms); };

    #OMS_facility = syslog
    filter f_syslog_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(syslog); };
    log { source(src); filter(f_syslog_oms); destination(d_oms); };

    #OMS_facility = user
    filter f_user_oms { level(alert,crit,debug,emerg,err,info,notice,warning) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };

Para quitar un recurso, elimine su sección del archivo de configuración.  Puede limitar los niveles de gravedad que se recopilan de un recurso determinado, para lo que debe eliminarlos de la lista.  Por ejemplo, para limitar el recurso del usuario solo a mensajes de alerta y críticos, debe modificar la sección del archivo de configuración de la siguiente manera:

    #OMS_facility = user
    filter f_user_oms { level(alert,crit) and facility(user); };
    log { source(src); filter(f_user_oms); destination(d_oms); };


### <a name="changing-the-syslog-port"></a>Modificación del puerto de Syslog
El agente de OMS escucha los mensajes de Syslog en el cliente local en el puerto 25224.  Puede cambiar este puerto agregando la siguiente sección al archivo de configuración del agente de OMS que se encuentra en **/etc/opt/microsoft/omsagent/conf/omsagent.conf**.  En la entrada del **puerto** , reemplace 25224 por el número de puerto que desee.  Tenga en cuenta que también deberá modificar el archivo de configuración del demonio Syslog para que envíe mensajes a este puerto.

    <source>
      type syslog
      port 25224
      bind 127.0.0.1
      protocol_type udp
      tag oms.syslog
    </source>


## <a name="data-collection"></a>Colección de datos
El agente de OMS escucha los mensajes de Syslog en el cliente local en el puerto 25224. El archivo de configuración del demonio Syslog reenvía mensajes de Syslog enviados desde la aplicación a este puerto, donde Log Analytics los recopila.

## <a name="syslog-record-properties"></a>Propiedades de registros de Syslog
Los registros de Syslog tienen un tipo **Syslog** y las propiedades que aparecen en la tabla siguiente.

| Propiedad | Descripción |
|:--- |:--- |
| Equipo |Nombre del equipo desde el que se recopiló el evento. |
| Facility |Define la parte del sistema que ha generado el mensaje. |
| HostIP |Dirección IP del sistema que envía el mensaje. |
| HostName |Nombre del sistema que envía el mensaje. |
| SeverityLevel |Nivel de gravedad del evento. |
| SyslogMessage |Texto del mensaje. |
| ProcessID |Identificador del proceso que ha generado el mensaje. |
| EventTime |Fecha y hora en que se generó el evento. |

## <a name="log-queries-with-syslog-records"></a>Consultas de registro con registros de Syslog
La tabla siguiente proporciona ejemplos distintos de consultas de registro que recuperan registros de Syslog.

| Consultar | Descripción |
|:--- |:--- |
| Type=Syslog |Todos los registros de Syslog. |
| Type=Syslog SeverityLevel=error |Todos los registros de Syslog con gravedad de error. |
| Type=Syslog &#124; measure count() by Computer |Número de registros de Syslog por equipo. |
| Type=Syslog &#124; measure count() by Facility |Número de registros de Syslog por recurso. |

## <a name="next-steps"></a>Pasos siguientes
* Obtenga información acerca de las [búsquedas de registros](log-analytics-log-searches.md) para analizar los datos recopilados de orígenes de datos y soluciones. 
* Use [Campos personalizados](log-analytics-custom-fields.md) para analizar datos de registros de Syslog en campos individuales.
* [Configure agentes de Linux](log-analytics-linux-agents.md) para recopilar otros tipos de datos. 

<!--HONumber=Oct16_HO2-->


