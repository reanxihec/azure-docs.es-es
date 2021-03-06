---
title: Visualización de eventos del registro de actividad de Azure en Azure Monitor
description: Vea el registro de actividad de Azure en Azure Monitor y recupérelo con PowerShell, la CLI y la API REST.
author: bwren
services: azure-monitor
ms.service: azure-monitor
ms.topic: conceptual
ms.date: 12/07/2019
ms.author: johnkem
ms.subservice: logs
ms.openlocfilehash: 46d26aa5dccd32438b2028e21eaa94f7993944d1
ms.sourcegitcommit: 380e3c893dfeed631b4d8f5983c02f978f3188bf
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/08/2020
ms.locfileid: "75749523"
---
# <a name="view-and-retrieve-azure-activity-log-events"></a>Visualización y recuperación de eventos del registro de actividad de Azure

El [registro de actividad de Azure](platform-logs-overview.md) es un registro que proporciona información de los eventos de nivel de suscripción que se han producido en Azure. En este artículo se proporcionan detalles sobre los diferentes métodos para ver y recuperar eventos del registro de actividad.

## <a name="azure-portal"></a>Portal de Azure
Vea el registro de actividad de todos los recursos del menú **Supervisión** de Azure Portal. Vea el registro de actividad de un recurso determinado en la opción **Registro de actividad** del menú de ese recurso.

![Visualización del registro de actividad](./media/activity-logs-overview/view-activity-log.png)

Puede filtrar los eventos del registro de actividad por los campos siguientes:

* **Intervalo de tiempo**: la hora de inicio y finalización para los eventos.
* **Categoría**: la categoría de eventos tal y como se describe en [Categorías del registro de actividad](activity-log-view.md#categories-in-the-activity-log).
* **Suscripción**: uno o más nombres de suscripción de Azure.
* **Grupo de recursos**: uno o más grupos de recursos dentro de las suscripciones seleccionadas.
* **Recurso (nombre)** : el nombre de un recurso específico.
* **Tipo de recurso**: el tipo de recurso, por ejemplo, _Microsoft.Compute/virtualmachines_.
* **Nombre de la operación**: el nombre de una operación de Azure Resource Manager, por ejemplo, _Microsoft.SQL/servers/Write_.
* **Gravedad**: Nivel de gravedad del evento. Los valores disponibles son _Informativo_, _Advertencia_, _Error_, _Crítico_.
* **Evento iniciado por**: el usuario que realizó la operación.
* **Abrir búsqueda**: se trata de un cuadro de búsqueda de texto abierto que busca esa cadena en todos los campos de todos los eventos.

## <a name="categories-in-the-activity-log"></a>Categorías del registro de actividad
Cada evento del registro de actividad tiene una categoría determinada que se describe en la tabla siguiente. Para obtener todos los detalles sobre los esquemas de estas categorías, consulte [Esquema de eventos del registro de actividad de Azure](activity-log-schema.md). 

| Category | Descripción |
|:---|:---|
| Administrativo | Contiene el registro de todas las operaciones de creación, actualización, eliminación y acción realizadas mediante Resource Manager. Algunos ejemplos de eventos administrativos incluyen la _creación de una máquina virtual_ y la _eliminación de un grupo de seguridad de red_.<br><br>Cada acción realizada por un usuario o aplicación mediante Resource Manager se modela como una operación en un tipo de recurso determinado. Si el tipo de operación es _Write_, _Delete_ o _Action_, los registros de inicio y corrección o error de esa operación se registran en la categoría Administrativo. Los eventos de la categoría Administrativo también incluyen los cambios realizados en el control de acceso basado en rol de una suscripción. |
| Service Health | Contiene el registro de los incidentes de estado del servicio que se han producido en Azure. Ejemplo de un evento de Service Health: _SQL Azure está experimentando un tiempo de inactividad en la región Este de EE. UU._ . <br><br>Los eventos de Service Health pueden encuadrarse dentro de seis variedades: _Acción requerida_, _Recuperación asistida_, _Incidente_, _Mantenimiento_, _Información_ o _Seguridad_. Estos eventos solo se crean si tiene un recurso en la suscripción que se puede ver afectado por el evento.
| Estado de los recursos | Contiene el registro de los eventos de estado de los recursos que se han producido en los recursos de Azure. Ejemplo de un evento de Resource Health: _Cambio del estado de mantenimiento de una máquina virtual a No disponible_.<br><br>Los eventos de Resource Health pueden representar uno de los cuatro estados de mantenimiento siguientes: _Disponible_, _No disponible_, _Degradado_ y _Desconocido_. Además, los eventos de Resource Health se pueden clasificar como _iniciados por la plataforma_ o _por el usuario_. |
| Alerta | Contiene el registro de activaciones de alertas de Azure. Ejemplo de un evento de alerta: _% de CPU en myVM ha estado por encima de 80 durante los últimos 5 minutos_.|
| Escalado automático | Contiene el registro de los eventos relacionados con el funcionamiento del motor de escalado automático en función de cualquier configuración de escalado automático que haya definido en la suscripción. Ejemplo de un evento de escalado automático: _Error durante la acción de escalado vertical_. |
| Recomendación | Contiene eventos de recomendación de Azure Advisor. |
| Seguridad | Contiene el registro de todas las alertas generadas por Azure Security Center. Ejemplo de un evento de seguridad: _Se ha ejecutado un archivo de extensión doble_. |
| Directiva | Contiene registros de todas las operaciones de acción de efecto realizadas por Azure Policy. Ejemplos de eventos de directiva: _Auditar_ y _Denegar_. Cada acción llevada a cabo por Azure Policy se modela como una operación en un recurso. |

## <a name="view-change-history"></a>Visualización del historial de cambios

La revisión del registro de actividad puede ayudarle a ver qué cambios ocurrieron durante ese evento. Puede consultar esta información con el **historial de cambios**. Seleccione un evento del registro de actividad del que desee obtener información más detallada. Seleccione la pestaña **Historial de cambios (versión preliminar)** para ver los cambios asociados a ese evento.

![Lista del historial de cambios para un evento](media/activity-logs-overview/change-history-event.png)

Si hay cambios asociados con el evento, verá una lista de cambios que puede seleccionar. Se abrirá la página **Historial de cambios (versión preliminar)** . En esta página verá los cambios realizados en el recurso. Como puede ver en el siguiente ejemplo, podemos ver no solo que la máquina virtual cambió de tamaño, sino también cuál era el tamaño de la máquina virtual antes de ese cambio y a qué cambió.

![Página de historial de cambios que muestra las diferencias](media/activity-logs-overview/change-history-event-details.png)

Para obtener más información sobre el historial de cambios, vea [Obtención de los cambios del recurso](../../governance/resource-graph/how-to/get-resource-changes.md).






## <a name="powershell"></a>PowerShell
Use el cmdlet [Get-AzLog](https://docs.microsoft.com/powershell/module/az.monitor/get-azlog) para recuperar el registro de actividad de PowerShell. A continuación se muestran algunos ejemplos comunes.

> [!NOTE]
> `Get-AzLog` solo proporciona 15 días de historial. Use el parámetro **-MaxEvents** para consultar los últimos N eventos más allá de 15 días. Para acceder a eventos de hace más de 15 días, use el SDK o la API REST. Si no incluye **StartTime**, el valor predeterminado será **EndTime** menos una hora. Si no incluye **EndTime**, el valor predeterminado será la hora actual. Todas las horas se muestran en UTC.


Obtención de entradas de registro creadas después de una determinada fecha y hora:

```powershell
Get-AzLog -StartTime 2016-03-01T10:30
```

Obtención de entradas de registro entre un intervalo de fecha y hora:

```powershell
Get-AzLog -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro de un grupo de recursos específico:

```powershell
Get-AzLog -ResourceGroup 'myrg1'
```

Obtención de entradas de registro de un proveedor de recursos específico entre un intervalo de fecha y hora:

```powershell
Get-AzLog -ResourceProvider 'Microsoft.Web' -StartTime 2015-01-01T10:30 -EndTime 2015-01-01T11:30
```

Obtención de entradas de registro con un autor de llamada concreto:

```powershell
Get-AzLog -Caller 'myname@company.com'
```

Obtención de los últimos 1000 eventos:

```powershell
Get-AzLog -MaxEvents 1000
```


## <a name="cli"></a>CLI
Use [az monitor activity-log](cli-samples.md#view-activity-log-for-a-subscription) para recuperar el registro de actividad de la CLI. A continuación se muestran algunos ejemplos comunes.


Vea todas las opciones disponibles.

```azurecli
az monitor activity-log list -h
```

Obtención de entradas de registro de un grupo de recursos específico:

```azurecli
az monitor activity-log list --resource-group <group name>
```

Obtención de entradas de registro con un autor de llamada concreto:

```azurecli
az monitor activity-log list --caller myname@company.com
```

Obtención de registros por autor de llamada en un tipo de recurso, dentro de un intervalo de fechas:

```azurecli
az monitor activity-log list --resource-provider Microsoft.Web \
    --caller myname@company.com \
    --start-time 2016-03-08T00:00:00Z \
    --end-time 2016-03-16T00:00:00Z
```

## <a name="rest-api"></a>API DE REST
Use la [API REST de Azure Monitor](https://docs.microsoft.com/rest/api/monitor/) para recuperar el registro de actividad de un cliente de REST. A continuación se muestran algunos ejemplos comunes.

Obtención de registros de actividad con filtro:

``` HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2018-01-21T20:00:00Z' and eventTimestamp le '2018-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'
```

Obtención de registros de actividad con filtro y selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$filter=eventTimestamp ge '2015-01-21T20:00:00Z' and eventTimestamp le '2015-01-23T20:00:00Z' and resourceGroupName eq 'MSSupportGroup'&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtención de registros de actividad con selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01&$select=eventName,id,resourceGroupName,resourceProviderName,operationName,status,eventTimestamp,correlationId,submissionTimestamp,level
```

Obtención de registros de actividad sin filtro ni selección:

```HTTP
GET https://management.azure.com/subscriptions/089bd33f-d4ec-47fe-8ba5-0753aa5c5b33/providers/microsoft.insights/eventtypes/management/values?api-version=2015-04-01
```


## <a name="activity-logs-analytics-monitoring-solution"></a>Solución de supervisión de Activity Log Analytics
La solución de supervisión de Azure Log Analytics incluye varias vistas y consultas de registros para analizar las entradas del registro de actividad en el área de trabajo de Log Analytics.

### <a name="prerequisites"></a>Prerequisites
Debe crear una configuración de diagnóstico para enviar el registro de actividad de la suscripción a un área de trabajo de Log Analytics. Consulte el artículo sobre la [recopilación de registros de plataforma de Azure en el área de trabajo de Log Analytics de Azure Monitor](resource-logs-collect-workspace.md)

### <a name="install-the-solution"></a>Instalar la solución
Use el procedimiento [Instalación de una solución de supervisión](../insights/solutions.md#install-a-monitoring-solution) para instalar la solución **Activity Log Analytics**. No se necesita ninguna configuración adicional.

### <a name="use-the-solution"></a>Uso de la solución
Haga clic en **Registros** en la parte superior de la página **Registro de actividad** para abrir la [solución de supervisión de Activity Log Analytics](activity-log-collect.md) para la suscripción. También puede acceder a todas las soluciones de supervisión de la suscripción en Azure Portal, mediante el menú **Supervisar**. Seleccione **Más** en la sección **Información detallada** para abrir la página **Información general** con los iconos de la solución. El icono **Registros de actividad de Azure** muestra un recuento del número de registros **AzureActivity** del área de trabajo.

![Icono Registros de actividad de Azure](media/collect-activity-logs/azure-activity-logs-tile.png)


Haga clic en el icono **Registros de actividad de Azure** para abrir la vista **Registros de actividad de Azure**. La vista incluye los elementos de visualización de la tabla siguiente. Cada uno de ellos muestra hasta otros diez elementos que coinciden con sus criterios para el intervalo de tiempo especificado. Puede ejecutar una consulta de registros que devuelve todos los registros coincidentes; para ello, haga clic en **Ver todo** en la parte inferior del elemento.

![Panel Registros de actividad de Azure](media/collect-activity-logs/activity-log-dash.png)

| Elemento de visualización | Descripción |
| --- | --- |
| Entradas del registro de actividad de Azure | Muestra un gráfico de barras de los principales totales del registro de entrada del registro de actividad del intervalo de fechas seleccionado y una lista de los 10 principales llamadores de actividad. Haga clic en el gráfico de barras para ejecutar una búsqueda de registros de `AzureActivity`. Haga clic en un elemento de llamador para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad de ese elemento. |
| Registros de actividad por estado | Muestra un gráfico de anillos del estado del registro de actividad de Azure para el intervalo de fechas seleccionado y una lista de los diez principales registros de estado. Haga clic en el gráfico para ejecutar una consulta de registros de `AzureActivity | summarize AggregatedValue = count() by ActivityStatus`. Haga clic en un elemento de estado para ejecutar una búsqueda de registros que devuelva todas las entradas de registro de actividad de ese registro de estado. |
| Registros de actividad por recurso | Muestra el número total de recursos con registros de actividad y enumera los diez principales recursos con recuentos de registro para cada recurso. Haga clic en el área total para ejecutar una búsqueda de registros de `AzureActivity | summarize AggregatedValue = count() by Resource`, que muestra todos los recursos de Azure disponibles para la solución. Haga clic en un recurso para ejecutar una consulta de registros que devuelva todos los registros de actividad de ese recurso. |
| Registros de actividad por proveedor de recursos | Muestra el número total de proveedores de recursos que producen registros de actividad y enumera los diez principales. Haga clic en el área total para ejecutar una consulta de registros de `AzureActivity | summarize AggregatedValue = count() by ResourceProvider`, que muestra todos los proveedores de recursos de Azure. Haga clic en un proveedor de recursos para ejecutar una consulta de registros que devuelva todos los registros de actividad para el proveedor. |




## <a name="next-steps"></a>Pasos siguientes

* [Introducción a los registros de plataforma Azure](platform-logs-overview.md)
* [Creación de la configuración de diagnóstico para enviar registros de actividad a otros destinos](diagnostic-settings.md)
