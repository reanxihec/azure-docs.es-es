---
title: Creación de un host de Azure Bastion | Microsoft Docs
description: En este artículo aprenderá a crear un host de Azure Bastion.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: f907dcb4427fd07a2c212e5de91ccce5e8198960
ms.sourcegitcommit: 4f6a7a2572723b0405a21fea0894d34f9d5b8e12
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/04/2020
ms.locfileid: "76990426"
---
# <a name="create-an-azure-bastion-host"></a>Creación de un host de Azure Bastion

En este artículo se muestra cómo crear un host de Azure Bastion mediante Azure Portal. Una vez que haya aprovisionado el servicio Azure Bastion en la red virtual, ya puede disponer de la completa experiencia de RDP/SSH en todas las máquinas virtuales de la misma red virtual. La implementación de Azure Bastion se realiza por red virtual, no por suscripción o cuenta, ni por máquina virtual.

Puede crear un recurso de host bastión en el portal mediante la especificación de todos los valores de configuración manualmente o mediante los valores de configuración que correspondan a una máquina virtual existente. Opcionalmente, puede usar [Azure PowerShell](bastion-create-host-powershell.md) para crear un host de Azure Bastion.

## <a name="before-you-begin"></a>Antes de empezar

Bastion está disponible en las regiones públicas de Azure siguientes:

[!INCLUDE [available regions](../../includes/bastion-regions-include.md)]

## <a name="createhost"></a>Creación de un host bastión: especificación de la configuración

Esta sección le ayuda a crear un recurso de Azure Bastion desde Azure Portal.

1. En el menú [Azure Portal](https://portal.azure.com) o en la página **Inicio**, seleccione **Crear un recurso**.

1. En la página **Nuevo**, en el campo *Buscar en Marketplace*, escriba **Bastion** y haga clic en **Entrar** para ir a los resultados de la búsqueda.

1. En los resultados, haga clic en **Bastion**. Asegúrese de que el publicador es *Microsoft* y la categoría es *Redes*.

1. En la página **Bastion**, haga clic en **Crear** para abrir la página **Crear un bastión**.

1. En la página **Create a bastion** (Crear un recurso de Bastion), configure un nuevo recurso de Bastion. Especifique las opciones de configuración del recurso de Bastion.

    ![Creación de un recurso de Bastion](./media/bastion-create-host-portal/settings.png)

    * **Suscripción**: suscripción de Azure que quiere usar para crear un recurso de Bastion.
    * **Grupo de recursos**: grupo de recursos en el que se creará el recurso de Bastion. Si no tiene un grupo de recursos existente, puede crear uno.
    * **Name**: nombre del nuevo recurso de Bastion.
    * **Región**: región pública de Azure en la que se creará el recurso.
    * **Red virtual**: red virtual en la que se creará el recurso de Bastion. Puede crear una red virtual en el portal durante este proceso, o bien usar una ya existente. Si usa una red virtual existente, asegúrese de que tenga suficiente espacio de direcciones libre para adaptarse a los requisitos de subred de Bastion.
    * **Subred**: subred de la red virtual en la que se implementará el nuevo recurso de host de Bastion. Debe crear una subred con el valor de nombre **AzureBastionSubnet**. Este valor permite a Azure saber en qué subred se deben implementar los recursos de Bastion. Esto no es lo mismo que una subred de puerta de enlace. Debe usar una subred de al menos/27 o mayor (/27,/26, etc.).
    
       Cree **AzureBastionSubnet** sin ninguna delegación ni tabla de rutas. Cuando use grupos de seguridad de red en **AzureBastionSubnet**, consulte el artículo [Trabajo con grupos de seguridad de red](bastion-nsg.md).
    * **Dirección IP pública**: dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). Cree una dirección IP pública o use una existente. La dirección debe estar en la misma región que el recurso de Bastion que está creando.
    * **Nombre de dirección IP pública**: nombre del recurso de la dirección IP pública.
    * **SKU de la dirección IP pública**: Esta configuración se rellena de forma predeterminada como **Estándar**. Azure Bastion solo usa o admite la SKU de IP pública estándar.
    * **Asignación**: Esta configuración se rellena de forma predeterminada como **Estática**.

1. Cuando termine de especificar la configuración, haga clic en **Revisar + Crear**. Esto valida los valores. Una vez que se apruebe la validación, podrá comenzar el proceso de creación.
1. En la página **Create a bastion** (Crear un recurso de Bastion), haga clic en **Crear**.
1. Verá un mensaje en el que se le indica que la implementación está en curso. El estado se mostrará en esta página a medida que se creen los recursos. El recurso de Bastion tarda aproximadamente cinco minutos en crearse e implementarse.

## <a name="createvmset"></a>Creación de un host bastion: uso de la configuración de máquina virtual

Si crea un host bastion en el portal mediante una máquina virtual existente, varios valores de configuración tendrán automáticamente como valores predeterminados los de la máquina virtual o la red virtual.

1. Abra [Azure Portal](https://portal.azure.com). Vaya a la máquina virtual y, a continuación, haga clic en **Conectar**.

   ![Conexión de la máquina virtual](./media/bastion-create-host-portal/vmsettings.png)
1. En la barra lateral derecha, haga clic en **Bastion** y en **Use Bastion** (Usar Bastion).

   ![Bastion](./media/bastion-create-host-portal/vmbastion.png)
1. En la página de Bastion, rellene los campos de configuración siguientes:

   * **Name**: nombre del host de Bastion que quiere crear.
   * **Subred**: subred de la red virtual en la que se implementará el recurso de Bastion. La subred debe crearse con el nombre **AzureBastionSubnet**. De este modo, Azure sabe en qué subred se debe implementar el recurso de Bastion. Esto no es lo mismo que una subred de puerta de enlace. Debe usar una subred de al menos/27 o mayor (/27,/26, etc.). Cree la subred sin grupos de seguridad de red, tablas de rutas ni delegaciones. Si más adelante decide usar grupos de seguridad de red en **AzureBastionSubnet**, consulte [Trabajo con grupos de seguridad de red](bastion-nsg.md).
   
     Haga clic en **Administrar configuración de subred** para crear **AzureBastionSubnet**.  Haga clic en **Crear** para crear la subred y, después, siga configurando los valores siguientes.
   * **Dirección IP pública**: dirección IP pública del recurso de Bastion en la que se accederá a RDP/SSH (a través del puerto 443). Cree una dirección IP pública o use una existente. La dirección debe estar en la misma región que el recurso de Bastion que está creando.
   * **Nombre de dirección IP pública**: nombre del recurso de la dirección IP pública.
1. En la pantalla de validación, haga clic en **Crear**. Espere unos cinco minutos a que el recurso de Bastion se cree e implemente.

## <a name="next-steps"></a>Pasos siguientes

* Para más información, lea las [P+F sobre Bastion](bastion-faq.md).

* Para usar grupos de seguridad de red con la subred de Azure Bastion, consulte [Trabajo con grupos de seguridad de red](bastion-nsg.md).