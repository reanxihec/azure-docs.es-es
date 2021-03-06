---
title: 'Aplicaciones o acciones en la nube en la directiva de acceso condicional: Azure Active Directory'
description: Qué son las aplicaciones o acciones en la nube en una directiva de acceso condicional de Azure AD
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 02/11/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: calebb
ms.collection: M365-identity-device-management
ms.openlocfilehash: a9d2780e09c099d76aa2ef4ec2638a410793481d
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/13/2020
ms.locfileid: "77186378"
---
# <a name="conditional-access-cloud-apps-or-actions"></a>Acceso condicional: Aplicaciones o acciones en la nube

Las aplicaciones o acciones en la nube son una señal esencial en una directiva de acceso condicional. Las directivas de acceso condicional permiten que los administradores asignen controles a determinadas aplicaciones o acciones.

- Los administradores pueden elegir en la lista de aplicaciones entre aplicaciones integradas de Microsoft y cualquier [aplicación integrada de Azure AD](../manage-apps/what-is-application-management.md), incluidas aplicaciones de la galería, aplicaciones que no son de la galería y aplicaciones publicadas a través de [Application Proxy](../manage-apps/what-is-application-proxy.md).
- Los administradores pueden optar por definir una directiva que no se base en una aplicación en la nube, sino en una acción del usuario. La única acción admitida es Registrar la información de seguridad (versión preliminar), que permite que el acceso condicional aplique controles en torno a la [experiencia de registro de la información de seguridad combinado](../authentication/howto-registration-mfa-sspr-combined.md).

![Definición de una directiva de acceso condicional y especificación de aplicaciones en la nube](./media/concept-conditional-access-cloud-apps/conditional-access-cloud-apps-or-actions.png)

## <a name="microsoft-cloud-applications"></a>Aplicaciones de nube de Microsoft

En la lista de aplicaciones que se pueden seleccionar están muchas de las aplicaciones en la nube de Microsoft existentes. 

Los administradores pueden asignar una directiva de acceso condicional a las siguientes aplicaciones en la nube de Microsoft. Algunas aplicaciones, como Office 365 (versión preliminar) y Administración de Microsoft Azure, incluyen varios servicios o aplicaciones secundarios relacionados. La lista siguiente no es exhaustiva y está sujeta a cambios.

- [Office 365 (versión preliminar)](#office-365-preview)
- Azure Analysis Services
- Azure DevOps
- [Azure SQL Database y Data Warehouse](../../sql-database/sql-database-conditional-access.md)
- Dynamics CRM Online
- Analytics de Microsoft Application Insights
- [Microsoft Azure Information Protection](https://docs.microsoft.com/azure/information-protection/faqs#i-see-azure-information-protection-is-listed-as-an-available-cloud-app-for-conditional-accesshow-does-this-work)
- [Administración de Microsoft Azure](#microsoft-azure-management)
- Administración de suscripciones de Microsoft Azure
- Microsoft Cloud App Security
- Portal de Control de acceso de las herramientas de Microsoft Commerce
- Servicio de autenticación de las herramientas de Microsoft Commerce
- Microsoft Flow
- Microsoft Forms
- Microsoft Intune
- [Inscripción en Microsoft Intune](https://docs.microsoft.com/intune/enrollment/multi-factor-authentication)
- Microsoft Planner
- Microsoft PowerApps
- Microsoft Search en Bing
- Microsoft StaffHub
- Microsoft Stream
- Equipos de Microsoft
- Office 365 Exchange Online
- Office 365 SharePoint Online
- Yammer para Office 365
- Office Delve
- Office Sway
- Outlook Groups
- Servicio Power BI
- Project Online
- Skype Empresarial Online
- Red privada virtual (VPN)
- ATP de Windows Defender

### <a name="office-365-preview"></a>Office 365 (versión preliminar)

Office 365 proporciona servicios de colaboración y productividad basados en la nube, como Exchange, SharePoint y Microsoft Teams. Los servicios en la nube de Office 365 están profundamente integrados para garantizar experiencias de colaboración fluidas. Esta integración puede producir confusión a la hora de crear directivas, ya que algunas aplicaciones, como Microsoft Teams, dependen de otras, como SharePoint o Exchange.

La aplicación Office 365 (versión preliminar) permite la segmentación de todos estos servicios a la vez. Se recomienda usar la nueva aplicación Office 365 (versión preliminar), en lugar de orientarse a aplicaciones en la nube individuales. Trabajar con este grupo de aplicaciones ayuda a evitar problemas que pueden surgir debido a directivas y dependencias incoherentes.

Los administradores pueden optar por excluir aplicaciones específicas de la directiva si lo desean, incluyendo la aplicación Office 365 (versión preliminar) y excluyendo las aplicaciones específicas que decidan.

Aplicaciones clave que se incluyen en la aplicación cliente Office 365 (versión preliminar):

   - Microsoft Flow
   - Microsoft Forms
   - Microsoft Stream
   - Microsoft To-Do
   - Equipos de Microsoft
   - Office 365 Exchange Online
   - Office 365 SharePoint Online
   - Búsqueda de Office 365
   - Yammer para Office 365
   - Office Delve
   - Office en línea
   - Office.com
   - OneDrive
   - PowerApps
   - Skype Empresarial Online
   - Sway

### <a name="microsoft-azure-management"></a>Microsoft Azure Management (Administración de Microsoft Azure)

La aplicación de administración de Microsoft Azure incluye varios servicios subyacentes. 

   - Portal de Azure
   - Proveedor de Azure Resource Manager
   - API del modelo de implementación clásica
   - Azure PowerShell
   - Portal de administrador de suscripciones de Visual Studio
   - Azure DevOps
   - Portal de Azure Data Factory

> [!NOTE]
> La aplicación de administración de Microsoft Azure se aplica a Azure PowerShell, que llama a la API de Azure Resource Manager. No se aplica a Azure AD PowerShell, que llama a Microsoft Graph.

## <a name="other-applications"></a>Otras aplicaciones

Además de las aplicaciones de Microsoft, los administradores pueden agregar cualquier aplicación registrada de Azure AD a las directivas de acceso condicional. Estas aplicaciones pueden incluir: 

- Aplicaciones web que se publican a través de[Azure AD Application Proxy](../manage-apps/what-is-application-proxy.md)
- [Aplicaciones agregadas desde la galería](../manage-apps/add-application-portal.md)
- [Aplicaciones personalizadas que no están en la galería](../manage-apps/add-non-gallery-app.md)
- [Aplicaciones heredadas publicadas a través de redes y controladores de entrega de aplicaciones](../manage-apps/secure-hybrid-access.md)

## <a name="user-actions"></a>Acciones del usuario

Las acciones del usuario son tareas que un usuario puede realizar. La única acción admitida actualmente es **registrar información de seguridad (versión preliminar)** , lo que permite que la directiva de acceso condicional se aplique cuando los usuarios que están habilitados para el registro combinado intentan registrar su información de seguridad. Para más información, consulte el artículo [Registro de información de seguridad combinado (versión preliminar)](../authentication/concept-registration-mfa-sspr-combined.md).

## <a name="next-steps"></a>Pasos siguientes

- [Acceso condicional: Condiciones](concept-conditional-access-conditions.md)

- [Directivas de acceso condicional habituales](concept-conditional-access-policy-common.md)
- [¿Cuáles son las dependencias de servicio del acceso condicional de Azure Active Directory?](service-dependencies.md)
