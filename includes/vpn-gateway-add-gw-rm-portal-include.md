---
title: archivo de inclusión
description: archivo de inclusión
services: vpn-gateway
author: cherylmc
ms.service: vpn-gateway
ms.topic: include
ms.date: 10/04/2019
ms.author: cherylmc
ms.custom: include file
ms.openlocfilehash: 12c28969ed00a1370b90e06e9d57cd666c50ea29
ms.sourcegitcommit: c22327552d62f88aeaa321189f9b9a631525027c
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/04/2019
ms.locfileid: "73523800"
---
1. En el menú de [Azure Portal](https://portal.azure.com), seleccione **Crear un recurso**. 

   ![Crear un recurso en Azure Portal](./media/vpn-gateway-add-gw-rm-portal-include/azure-portal-create-resource.png)
2. En el campo **Buscar en el Marketplace**, escriba "Puerta de enlace de Virtual Network". Busque **Puerta de enlace de red virtual** en los resultados de la búsqueda y haga clic en la entrada. En la página **Puerta de enlace de red virtual**, haga clic en **Crear**. Se abre la página **Crear puerta de enlace de red virtual**.

   ![Campos de la página Crear puerta de enlace de red virtual](./media/vpn-gateway-add-gw-rm-portal-include/p2sgw.png "Campos de la página Crear puerta de enlace de red virtual")

   ![Campos de la página Crear puerta de enlace de red virtual](./media/vpn-gateway-add-gw-rm-portal-include/p2sgw2.png "Campos de la página Crear puerta de enlace de red virtual")
3. En la página **Crear puerta de enlace de red virtual**, rellene los valores de la puerta de enlace de red virtual.

   **Detalles del proyecto**

   - **Suscripción**: seleccione la suscripción que desea usar en la lista desplegable.
   - **Grupo de recursos**: esta configuración se rellena automáticamente cuando selecciona la red virtual en esta página.

   **Detalles de instancia**

   - **Nombre**: Asigne un nombre a la puerta de enlace. Asignar nombre a la puerta de enlace no es lo mismo que asignar nombre a una subred de puerta de enlace. Este es el nombre del objeto de puerta de enlace que va a crear.
   - **Región**: Seleccione la región en la que quiere crear este recurso. La región de la puerta de enlace debe ser la misma que la red virtual.
   - **Tipo de puerta de enlace**: Seleccione **VPN**. Las puertas de enlace VPN usan el tipo de puerta de enlace de red virtual **VPN**. 
   - **Tipo de VPN**: seleccione el tipo de VPN que se especifica para la configuración. La mayoría de las configuraciones requieren un tipo de VPN basada en enrutamiento.
   - **SKU**: seleccione la SKU de puerta de enlace en la lista desplegable. Las SKU que aparecen en la lista desplegable dependen del tipo de VPN que seleccione. Para más información acerca de las SKU de puerta de enlace, consulte [SKU de puerta de enlace](../articles/vpn-gateway/vpn-gateway-about-vpn-gateway-settings.md#gwsku).

      **Red virtual**: Elija la red virtual a la que quiera agregar esta puerta de enlace.

      **Intervalo de direcciones de subred de puerta de enlace**: Este campo solo aparece si la red virtual no tiene una subred de puerta de enlace. Si es posible, intente que el intervalo sea /27, o incluso mayor (/26, /25, etc.). No se recomienda crear un intervalo inferior a /28. Si ya tiene una subred de puerta de enlace y desea ver los detalles de GatewaySubnet, vaya a la red virtual. Haga clic en **Subnets** (Subredes) para ver el intervalo. Si desea cambiar el intervalo, puede eliminar y volver a crear GatewaySubnet.

   **Dirección IP pública**: esta configuración especifica el objeto de dirección IP pública que se asocia a la puerta de enlace de VPN. La dirección IP pública se asigna dinámicamente a este objeto cuando se crea la puerta de enlace de VPN. La única vez que la dirección IP pública cambia es cuando la puerta de enlace se elimina y se vuelve a crear. No cambia cuando se cambia el tamaño, se restablece o se realizan actualizaciones u otras operaciones de mantenimiento interno de una puerta de enlace VPN.

     - **Dirección IP pública**: Mantenga la opción **Crear nueva** seleccionada.
     - **Nombre de dirección IP pública**: En el cuadro de texto, escriba un nombre para la dirección IP pública.
     - **Asignación**: VPN Gateway solo admite Dinámica.

   **Modo activo/activo**: Seleccione **Habilitar el modo activo/activo** solo si va a crear una configuración de puerta de enlace activa/activa. En caso contrario, deje este valor sin seleccionar.

   Deje la opción **Configurar ASN BGP** sin seleccionar, a menos que su configuración requiera específicamente este valor. Si necesita esta configuración, el valor predeterminado del ASN es 65515, aunque esto se puede cambiar.

4. Haga clic en **Revisar y crear** para ejecutar la validación. Una vez superada la validación, haga clic en **Crear** para implementar VPN Gateway. Una puerta de enlace puede tardar hasta 45 minutos en crearse e implementarse completamente. Puede ver el estado de implementación en la página Información general de la puerta de enlace.

Una vez creada la puerta de enlace, puede ver la dirección IP que se le ha asignado consultando la red virtual en el portal. La puerta de enlace aparece como un dispositivo conectado.
