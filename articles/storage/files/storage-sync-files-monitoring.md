---
title: Supervisión de Azure File Sync | Microsoft Docs
description: Cómo supervisar Azure File Sync.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/28/2019
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: ac09f9b59bc6f47adc9311cc910352c1a0d73b5d
ms.sourcegitcommit: 800f961318021ce920ecd423ff427e69cbe43a54
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/31/2019
ms.locfileid: "68699281"
---
# <a name="monitor-azure-file-sync"></a>Supervisión de Azure File Sync

Use Azure File Sync para centralizar los recursos compartidos de archivos de su organización en Azure Files sin renunciar a la flexibilidad, el rendimiento y la compatibilidad de un servidor de archivos local. Azure File Sync transforma Windows Server en una caché rápida de los recursos compartidos de archivos de Azure. Puede usar cualquier protocolo disponible en Windows Server para acceder a sus datos localmente, como SMB, NFS y FTPS. Puede tener todas las cachés que necesite en todo el mundo.

En este artículo se describe cómo supervisar la implementación de Azure File Sync mediante Azure Monitor, el Servicio de sincronización de almacenamiento y Windows Server.

Las siguientes opciones de supervisión están disponibles actualmente.

## <a name="azure-monitor"></a>Azure Monitor

Use [Azure Monitor](https://docs.microsoft.com/azure/azure-monitor/overview) para ver las métricas y configurar alertas para la sincronización, nube por niveles y conectividad del servidor.  

### <a name="metrics"></a>Métricas

Las métricas de Azure File Sync están habilitadas de forma predeterminada y se envían a Azure Monitor cada 15 minutos.

Para ver las métricas de Azure File Sync en Azure Monitor, seleccione el tipo de recurso de **Servicios de sincronización de almacenamiento**.

Las siguientes métricas para Azure File Sync están disponibles en Azure Monitor:

| Nombre de métrica | DESCRIPCIÓN |
|-|-|
| Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga).<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Recuperación de niveles de la nube | Tamaño de los datos que se recuperan.<br><br>**Nota**: esta métrica se eliminará en el futuro. Use la métrica del tamaño de recuperación de nube por niveles para supervisar el tamaño de los datos que se recuperan.<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: Nombre del servidor |
| Tamaño de recuperación de nube por niveles | Tamaño de los datos que se recuperan.<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: nombre del servidor, nombre del grupo de sincronización |
| Tamaño de recuperación de nube por niveles por aplicación | Tamaño de los datos recuperados por aplicación<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: nombre de la aplicación, nombre del servidor, nombre del grupo de sincronización |
| Rendimiento de recuperación de nube por niveles | Tamaño del rendimiento de recuperación de datos.<br><br>Unidad: Bytes<br>Tipo de agregación: Suma<br>Dimensión aplicable: nombre del servidor, nombre del grupo de sincronización |
| Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar.<br><br>Unidad: Count<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga).<br><br>Unidad: Count<br>Tipo de agregación: Suma<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |
| Estado en línea del servidor | Recuento de latidos recibido del servidor.<br><br>Unidad: Count<br>Tipo de agregación: Máxima<br>Dimensión aplicable: Nombre del servidor |
| Sync session result (Resultado de la sesión de sincronización) | Resultado de la sesión de sincronización (1 = sesión de sincronización correcta; 0 = sesión de sincronización con errores)<br><br>Unidad: Count<br>Tipos de agregación: Máxima<br>Dimensiones aplicables: Nombre de punto de conexión del servidor, dirección de sincronización, nombre del grupo de sincronización |

### <a name="alerts"></a>Alertas

Para configurar alertas en Azure Monitor, seleccione el Servicio de sincronización de almacenamiento y luego seleccione la [métrica de Azure File Sync](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#metrics) que se usará en la alerta.  

En la tabla siguiente se muestran algunos escenarios de ejemplo que se van a supervisar y la métrica adecuada que se va a usar para la alerta:

| Escenario | Métrica que se va a usar para la alerta |
|-|-|
| Estado de punto de conexión del servidor en el portal = error | Sync session result (Resultado de la sesión de sincronización) |
| Los archivos no se pueden sincronizar con un servidor o un punto de conexión en la nube | Archivos que no se están sincronizando |
| El servidor registrado no se puede comunicar con el Servicio de sincronización de almacenamiento | Estado en línea del servidor |
| El tamaño de recuperación de nube por niveles ha superado 500 GiB en un día  | Tamaño de recuperación de nube por niveles |

Para obtener más información sobre cómo configurar alertas en Azure Monitor, consulte [Introducción sobre las alertas en Microsoft Azure]( https://docs.microsoft.com/azure/azure-monitor/platform/alerts-overview).

## <a name="storage-sync-service"></a>Servicio de sincronización de almacenamiento

Para ver el estado del servidor registrado y del punto de conexión del servidor, así como las métricas, vaya al Servicio de sincronización de almacenamiento en Azure Portal. Se puede ver el estado del servidor registrado en la hoja **Servidores registrados** y el del punto de conexión del servidor en la hoja **Grupos de sincronización**.

### <a name="registered-server-health"></a>Estado del servidor registrado

- Si el estado del **Servidor registrado** es **En línea**, el servidor se está comunicando correctamente con el servicio.
- Si el estado del **Servidor registrado** es **Aparece sin conexión**, verifique que el proceso del Supervisor de sincronización de almacenamiento (AzureStorageSyncMonitor.exe) en el servidor se está ejecutando. Si el servidor está detrás de un firewall o un proxy, consulte [este artículo](https://docs.microsoft.com/azure/storage/files/storage-sync-files-firewall-and-proxy) para configurar el firewall y el proxy.

### <a name="server-endpoint-health"></a>Estado del punto de conexión del servidor

- El estado de punto de conexión del servidor en el portal se basa en los eventos de sincronización que se registran en el registro de eventos de telemetría en el servidor (Id. 9102 y 9302). Si se produce un error en una sesión de sincronización debido a un error transitorio, como un error cancelado, es posible que en el portal siga apareciendo la sincronización como correcta, siempre y cuando la sesión de sincronización actual esté en progreso. El id. de evento 9302 se utiliza para determinar si se están aplicando los archivos. Para obtener más información, consulte [estado de la sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) y [progreso de la sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).
- Si el portal muestra un error de sincronización debido a que la sincronización no avanza, consulte la [documentación de solución de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

### <a name="metric-charts"></a>Gráficos de métricas

- Los siguientes gráficos de métricas están visibles en el portal del Servicio de sincronización de almacenamiento:

  | Nombre de métrica | DESCRIPCIÓN | Nombre de la hoja |
  |-|-|-|
  | Bytes sincronizados | Tamaño de los datos transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Recuperación de niveles de la nube | Tamaño de los datos que se recuperan | Servidores registrados |
  | Archivos que no se están sincronizando | Recuento de archivos que no se pueden sincronizar | Punto de conexión de servidor |
  | Archivos sincronizados | Recuento de los ficheros transferidos (carga y descarga) | Grupo de sincronización,punto de conexión de servidor |
  | Estado en línea del servidor | Recuento de latidos recibido del servidor | Servidores registrados |

- Para obtener más información, consulte [Azure Monitor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring#azure-monitor).

  > [!Note]  
  > Los gráficos del portal del servicio de sincronización de almacenamiento tienen un intervalo de tiempo de 24 horas. Para ver los diferentes intervalos de tiempo o dimensiones, use Azure Monitor.

## <a name="windows-server"></a>Windows Server

En Windows Server, puede ver la nube por niveles, el servidor registrado y el estado de la sincronización.

### <a name="event-logs"></a>Registros de eventos

Usar el registro Eventos de telemetría en el servidor para supervisar el servidor registrado, la sincronización y el estado de la nube por niveles. El registro de Eventos de telemetría se encuentra en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

Estado de la sincronización:

- El id. de evento 9102 se registra una vez finalizada una sesión de sincronización. Use este evento para determinar si las sesiones de sincronización son correctas (**HResult = 0**) y si hay errores de sincronización por elemento. Para obtener más información, consulte la documentación [estado de la sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#broken-sync) y [errores por elemento](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-see-if-there-are-specific-files-or-folders-that-are-not-syncing).

  > [!Note]  
  > A veces, las sesiones de sincronización producen un error general o tienen un elemento PerItemErrorCount distinto de cero. Sin embargo, aún así hacen progresos y algunos archivos se sincronizan correctamente. Se puede observar esto en campos Applied como, por ejemplo, AppliedFileCount, AppliedDirCount, AppliedTombstoneCount y AppliedSizeBytes. Estos campos indican qué parte de la sesión se ha realizado correctamente. Si ve varias sesiones de sincronización seguidas que producen un error pero que tienen un número cada vez mayor de campos Applied, debe dar tiempo para que se vuelva a intentar la sincronización antes de abrir una incidencia de soporte técnico.

- El id. de evento 9302 se registra cada 5 a 10 minutos si hay una sesión de sincronización activa. Use este evento para determinar si la sesión de sincronización actual está en progreso (**AppliedItemCount > 0**). Si la sincronización no avanza, la sesión de sincronización finalmente generará un error y se registrará el id. de evento 9102 con el error. Para obtener más información, consulte la [documentación sobre progreso de la sincronización](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=server%2Cazure-portal#how-do-i-monitor-the-progress-of-a-current-sync-session).

Estado del servidor registrado:

- El id. de evento 9301 se registra cada 30 segundos, cuando un servidor consulta al servicio por los trabajos. Si GetNextJob se completa con **status = 0**, el servidor puede comunicarse con el servicio. Si GetNextJob se completa con un error, compruebe la [documentación de solución de problemas](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#common-sync-errors) para obtener instrucciones.

Estado de la nube por niveles:

- Para supervisar la actividad por niveles en un servidor, use los id. de eventos 9003, 9016 y 9029 en el registro de Eventos de telemetría, ubicado en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

  - El identificador de evento 9003 proporciona una distribución de errores para un punto de conexión de servidor. Por ejemplo:  Recuento total de errores y ErrorCode. Se registra un evento por código de error.
  - El identificador de evento 9016 proporciona resultados de conversión en fantasma para un volumen. Por ejemplo:  el porcentaje de espacio libre, el Número de archivos reflejados en la sesión y el Número de archivos no reflejados.
  - El id. de evento 9029 proporciona información de la sesión de conversión en fantasma para el punto de conexión de un servidor. Por ejemplo:  el Número de archivos que se han intentado en la sesión, el Número de archivos organizados en niveles de la sesión y el Número de archivos ya organizados en niveles.
  
- Para supervisar la actividad de recuperación en un servidor, use los id. de evento 9005, 9006, 9009 y 9059 en el registro de Eventos de telemetría, ubicado en el Visor de eventos en *Applications and Services\Microsoft\FileSync\Agent*.

  - El identificador de evento 9005 proporciona confiabilidad de recuperación a un punto de conexión de servidor. Por ejemplo:  el Total de archivos únicos a los que se puede acceder y el Total de archivos únicos con acceso erróneo.
  - El identificador de evento 9006 proporciona una distribución de errores de recuperación a un punto de conexión de servidor. Por ejemplo:  Total de solicitudes con error y ErrorCode. Se registra un evento por código de error.
  - El id. de evento 9009 proporciona información de la sesión de recuperación para un punto de conexión de servidor. Por ejemplo:  DurationSeconds, CountFilesRecallSucceeded y CountFilesRecallFailed.
  - El id. de evento 9059 proporciona una distribución de recuperación de aplicación para un punto de conexión de servidor. Por ejemplo:  ShareId, Nombre de la aplicación y TotalEgressNetworkBytes.

### <a name="performance-counters"></a>contadores de rendimiento

Use los contadores de rendimiento de Azure File Sync en el servidor para supervisar la actividad de sincronización.

Para ver los contadores de rendimiento de Azure File Sync en el servidor, abra el Monitor de rendimiento (Perfmon.exe). Puede encontrar los contadores en los objetos **AFS Bytes Transferred** y **AFS Sync Operations**.

Los siguientes contadores de rendimiento para Azure File Sync están disponibles en el Monitor de rendimiento:

| Objeto de rendimiento\Nombre del contador | DESCRIPCIÓN |
|-|-|
| AFS Bytes Transferred\Downloaded Bytes/sec (Bytes descargados/s) | Número de bytes descargados por segundo. |
| AFS Bytes Transferred\Uploaded Bytes/sec (Bytes cargados/s) | Número de bytes cargados por segundo. |
| AFS Bytes Transferred\Total Bytes/sec (Total de bytes/s) | Número total de bytes por segundo (carga y descarga). |
| AFS Sync Operations\Downloaded Sync Files/sec (Archivos de sincronización descargados/s) | Número de archivos descargados por segundo. |
| AFS Sync Operations\Uploaded Sync Files/sec (Archivos de sincronización cargados/s) | Número de archivos cargados por segundo. |
| AFS Sync Operations\Total Sync File Operations/sec (Total de operaciones de sincronización de archivos/s) | Número total de archivos sincronizados (carga y descarga). |

## <a name="next-steps"></a>Pasos siguientes
- [Planeamiento de una implementación de Azure File Sync](storage-sync-files-planning.md)
- [Tenga en cuenta los valores de proxy y firewall](storage-sync-files-firewall-and-proxy.md)
- [Implementación de Azure File Sync](storage-sync-files-deployment-guide.md)
- [Solución de problemas de Azure File Sync](storage-sync-files-troubleshoot.md)
- [Preguntas más frecuentes de Azure Files](storage-files-faq.md)
