---
title: Contenedores sin servidor en Azure
description: El servicio Azure Container Instances ofrece la forma más rápida y sencilla de ejecutar contenedores aislados, sin tener que administrar máquinas virtuales y sin necesidad de adoptar un orquestador de nivel superior.
ms.topic: overview
ms.date: 04/25/2019
ms.custom: seodec18, mvc
ms.openlocfilehash: c871c09e29b64c4f0dcd107361154efdce306481
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/11/2020
ms.locfileid: "75888040"
---
# <a name="what-is-azure-container-instances"></a>¿Qué es Azure Container Instances?

Los contenedores están convirtiéndose en la manera preferida de empaquetar, implementar y administrar aplicaciones en la nube. Azure Container Instances ofrece la forma más rápida y sencilla de ejecutar un contenedor en Azure, sin tener que administrar ninguna máquina virtual y sin necesidad de adoptar un servicio de nivel superior.

Azure Container Instances es una excelente solución para cualquier escenario que pueda funcionar en contenedores aislados, incluidas las aplicaciones simples, la automatización de tareas y los trabajos de compilación. En aquellos escenarios donde se necesita una orquestación completa de contenedores, incluida la detección de servicios en varios contenedores, el escalado automático y las actualizaciones de aplicaciones coordinadas, se recomienda [Azure Kubernetes Service (AKS)](../aks/index.yml).

## <a name="fast-startup-times"></a>Tiempos de inicio rápido

Los contenedores ofrecen importantes ventajas de inicio sobre las máquinas virtuales (VM). Azure Container Instances puede iniciar un contenedor en Azure en segundos sin que sea necesario aprovisionar y administrar máquinas virtuales.

## <a name="container-access"></a>Acceso a contenedores

Azure Container Instances permite exponer los grupos de contenedores directamente a Internet con una dirección IP y un nombre de dominio completo (FQDN). Al crear una instancia de contenedor, se puede especificar una etiqueta de nombre DNS personalizada, con el fin de que se pueda acceder a la aplicación en *customlabel*. *azureregion*. azurecontainer.io.

Azure Container Instances también admite la ejecución de comandos en un contenedor en ejecución, para lo que proporciona un shell interactivo que ayuda en el desarrollo y la solución de problemas de las aplicaciones. El acceso se realiza a través de HTTPS y se usa TLS para proteger las conexiones de cliente.

> [!IMPORTANT]
> A partir del 13 de enero de 2020, Azure Container Instances requerirá que todas las conexiones seguras de servidores y aplicaciones utilicen TLS 1.2. Se retirará la compatibilidad con TLS 1.0 y 1.1.

## <a name="hypervisor-level-security"></a>Seguridad de nivel de hipervisor

Históricamente, los contenedores han ofrecido aislamiento a la dependencia entre aplicaciones y gobernanza de recursos, pero no se han considerado suficientemente protegidos para el uso de varios inquilinos hostiles. Azure Container Instances garantiza que la aplicación está tan aislada en un contenedor como lo estaría en una máquina virtual.


## <a name="custom-sizes"></a>Tamaños personalizados

Los contenedores normalmente están optimizados para ejecutar una sola aplicación, pero las necesidades exactas de esas aplicaciones pueden diferir considerablemente. Azure Container Instances proporciona un uso óptimo al permitir especificaciones exactas de los núcleos y la memoria de la CPU. El usuario paga según lo que necesita y se le factura por segundo, para que pueda optimizar con precisión los gastos según sus necesidades reales.

Para los trabajos de proceso intensivo, como los de aprendizaje automático, Azure Container Instances puede programar los contenedores de Linux para usar [recursos de GPU](container-instances-gpu.md) de NVIDIA Tesla (versión preliminar).

## <a name="persistent-storage"></a>Almacenamiento persistente

Para recuperar y conservar el estado con Azure Container Instances, ofrecemos el [montaje directo de los recursos compartidos de Azure Files](container-instances-mounting-azure-files-volume.md) con el respaldo de Azure Storage.

## <a name="linux-and-windows-containers"></a>Contenedores de Linux y Windows

Azure Container Instances puede programar los contenedores Windows y Linux con la misma API. Simplemente especifique el tipo de sistema operativo al crear los [grupos de contenedor](container-instances-container-groups.md).

Algunas características están restringidas actualmente a los contenedores Linux:

* Varios contenedores por grupo de contenedor
* Montaje del volumen ([Azure Files](container-instances-volume-azure-files.md), [emptyDir](container-instances-volume-emptydir.md), [GitRepo](container-instances-volume-gitrepo.md), [secreto](container-instances-volume-secret.md))
* [Métrica de uso de recursos](container-instances-monitor.md) con Azure Monitor
* [Implementación de Virtual Network](container-instances-vnet.md)
* [Recursos de GPU](container-instances-gpu.md) (versión preliminar)

En el caso de las implementaciones de contenedores de Windows, use imágenes basadas en [imágenes base de Windows](container-instances-faq.md#what-windows-base-os-images-are-supported) comunes.

> [!NOTE]
> El uso de imágenes basadas en Windows Server 2019 en Azure Container Instances está en versión preliminar.

## <a name="co-scheduled-groups"></a>Grupos con programación compartida

Azure Container Instances admite la programación de [grupos con varios contenedores](container-instances-container-groups.md) que comparten una máquina host, la red local, el almacenamiento y el ciclo de vida. Esto le permite combinar el contenedor de la aplicación principal con otros contenedores que actúan en una función auxiliar, como los patrones sidecar de registro.

## <a name="virtual-network-deployment"></a>Implementación de Virtual Network

Actualmente disponible para cargas de trabajo de producción en un subconjunto de regiones de Azure, esta característica de Azure Container Instances permite la [implementación de instancias de contenedor en una red virtual de Azure](container-instances-vnet.md). Al implementar instancias de contenedor en una subred dentro de la red virtual, pueden comunicarse de forma segura con otros recursos de la red virtual, incluidos los que están en el entorno local (mediante [VPN Gateway](../vpn-gateway/vpn-gateway-about-vpngateways.md) o [ ExpressRoute](../expressroute/expressroute-introduction.md)).

## <a name="next-steps"></a>Pasos siguientes

Pruebe a implementar un contenedor en Azure con un único comando mediante nuestra Guía de inicio rápido:

> [!div class="nextstepaction"]
> [Guía de inicio rápido de Azure Container Instances](container-instances-quickstart.md)

<!-- LINKS - External -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/
