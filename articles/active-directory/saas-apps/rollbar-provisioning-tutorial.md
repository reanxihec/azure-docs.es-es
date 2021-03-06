---
title: 'Tutorial: Configuración de Rollbar para el aprovisionamiento automático de usuarios con Azure Active Directory | Microsoft Docs'
description: Aprenda a configurar Azure Active Directory para aprovisionar y cancelar automáticamente el aprovisionamiento de cuentas de usuario en Rollbar.
services: active-directory
documentationcenter: ''
author: zchia
writer: zchia
manager: beatrizd
ms.assetid: d737aa16-8ab4-4c0c-a68b-2911623b41eb
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/26/2019
ms.author: zhchia
ms.openlocfilehash: d9720ca769eab8cf0e4ee763c720f6ba12ebb1d9
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 02/07/2020
ms.locfileid: "77063312"
---
# <a name="tutorial-configure-rollbar-for-automatic-user-provisioning"></a>Tutorial: Configuración de Rollbar para el aprovisionamiento automático de usuarios

El objetivo de este tutorial es mostrar los pasos que se realizan en Rollbar y Azure Active Directory (Azure AD) para configurar Azure AD con el objetivo de aprovisionar y desaprovisionar automáticamente usuarios o grupos en Rollbar.

> [!NOTE]
> Este tutorial describe un conector que se crea sobre el servicio de aprovisionamiento de usuarios de Azure AD. Para obtener información importante acerca de lo que hace este servicio, cómo funciona y ver preguntas frecuentes al respecto, consulte [Automatización del aprovisionamiento y desaprovisionamiento de usuarios para aplicaciones SaaS con Azure Active Directory](../app-provisioning/user-provisioning.md).
>
> Este conector está actualmente en versión preliminar pública. Para más información sobre los términos de uso generales de Microsoft Azure para las características en versión preliminar, consulte [Términos de uso complementarios para las versiones preliminares de Microsoft Azure](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="prerequisites"></a>Prerrequisitos

En el escenario descrito en este tutorial se supone que ya cuenta con los requisitos previos siguientes:

* Un inquilino de Azure AD.
* [Un inquilino de Rollbar](https://rollbar.com/pricing/) que tenga un plan empresarial.
* Una cuenta de usuario de Rollbar con permisos de administrador.

## <a name="assigning-users-to-rollbar"></a>Asignación de usuarios a Rollbar

Azure Active Directory usa un concepto denominado *asignaciones* para determinar qué usuarios deben recibir acceso a determinadas aplicaciones. En el contexto del aprovisionamiento automático de usuarios, solo se sincronizan los usuarios y grupos que se han asignado a una aplicación en Azure AD.

Antes de configurar y habilitar el aprovisionamiento automático de usuarios, debe decidir qué usuarios o grupos de Azure AD necesitan acceder a Rollbar. Una vez que lo decida, puede seguir estas instrucciones para asignar dichos usuarios o grupos a Rollbar:
* [Asignar un usuario o grupo a una aplicación empresarial](../manage-apps/assign-user-or-group-access-portal.md)

## <a name="important-tips-for-assigning-users-to-rollbar"></a>Sugerencias importantes para asignar usuarios a Rollbar

* Se recomienda asignar un único usuario de Azure AD a Rollbar para probar la configuración de aprovisionamiento automático de usuarios. Más tarde, se pueden asignar otros usuarios o grupos.

* Al asignar un usuario a Rollbar, debe seleccionar un rol válido específico de la aplicación (si está disponible) en el cuadro de diálogo de asignación. Los usuarios con el rol de **Acceso predeterminado** quedan excluidos del aprovisionamiento.

## <a name="setup-rollbar-for-provisioning"></a>Configuración de Rollbar para el aprovisionamiento

Antes de configurar Rollbar para el aprovisionamiento automático de usuarios con Azure AD, deberá habilitar el aprovisionamiento SCIM en Rollbar.

1. Inicie sesión en la [consola de administración de Rollbar](https://rollbar.com/login/). Haga clic en **Configuración de la cuenta**.

    ![Consola de administración de Rollbar](media/rollbar-provisioning-tutorial/image00.png)

2. Navegue hasta **Rollbar Tenant Name > Account Access Tokens** (Nombre de inquilino de Rollbar > 
Tokens de acceso de cuenta).

    ![Consola de administración de Rollbar](media/rollbar-provisioning-tutorial/account.png)

3. Copie el valor de **SCIM**. Este valor se escribirá en el campo Token secreto de la pestaña Aprovisionamiento en la aplicación Rollbar en Azure Portal.

    ![Consola de administración de Rollbar](media/rollbar-provisioning-tutorial/scim.png)

## <a name="add-rollbar-from-the-gallery"></a>Incorporación de Rollbar desde la galería

Para configurar Rollbar para el aprovisionamiento automático de usuarios con Azure AD, es preciso agregar Rollbar desde la galería de aplicaciones de Azure AD a la lista de aplicaciones SaaS administradas.

**Para agregar Rollbar desde la galería de aplicaciones de Azure AD, siga estos pasos:**

1. En **[Azure Portal](https://portal.azure.com)** , en el panel de navegación izquierdo, seleccione **Azure Active Directory**.

    ![Botón Azure Active Directory](common/select-azuread.png)

2. Vaya a **Aplicaciones empresariales** y seleccione **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

3. Para agregar una nueva aplicación, seleccione el botón **Nueva aplicación** en la parte superior del panel.

    ![Botón Nueva aplicación](common/add-new-app.png)

4. En el cuadro de búsqueda, escriba **Rollbar**, seleccione **Rollbar** en el panel de resultados y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Rollbar en la lista de resultados](common/search-new-app.png)

## <a name="configuring-automatic-user-provisioning-to-rollbar"></a>Configuración del aprovisionamiento automático de usuarios en Rollbar 

Esta sección le guía por los pasos necesarios para configurar el servicio de aprovisionamiento de AD Azure para crear, actualizar y deshabilitar usuarios o grupos en Rollbar en función de las asignaciones de grupos y usuarios de Azure AD.

> [!TIP]
> También puede habilitar el inicio de sesión único basado en SAML para Rollbar siguiendo las instrucciones del [tutorial de inicio de sesión único de Rollbar](rollbar-tutorial.md). El inicio de sesión único puede configurarse independientemente del aprovisionamiento automático de usuarios, aunque estas dos características se complementan entre sí.

### <a name="to-configure-automatic-user-provisioning-for-rollbar-in-azure-ad"></a>Para configurar el aprovisionamiento automático de usuarios para Rollbar en Azure AD:

1. Inicie sesión en [Azure Portal](https://portal.azure.com). Seleccione **Aplicaciones empresariales** y luego **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales](common/enterprise-applications.png)

2. En la lista de aplicaciones, seleccione **Rollbar**.

    ![Vínculo a Rollbar en la lista de aplicaciones](common/all-applications.png)

3. Seleccione la pestaña **Aprovisionamiento**.

    ![Pestaña Aprovisionamiento](common/provisioning.png)

4. Establezca el **modo de aprovisionamiento** en **Automático**.

    ![Pestaña Aprovisionamiento](common/provisioning-automatic.png)

5. En la sección **Credenciales de administrador**, escriba el valor del **token de acceso de la cuenta** recuperado anteriormente en **Token secreto**. Haga clic en **Probar conexión** para asegurarse de que Azure AD puede conectarse a Rollbar. Si la conexión no se establece, asegúrese de que la cuenta de Rollbar tiene permisos de administrador y pruebe de nuevo.

    ![Consola de administración de Rollbar](media/rollbar-provisioning-tutorial/admin.png)

6. En el campo **Correo electrónico de notificación**, escriba la dirección de correo electrónico de una persona o grupo que debe recibir las notificaciones de error de aprovisionamiento y active la casilla **Enviar una notificación por correo electrónico cuando se produzca un error**.

    ![Correo electrónico de notificación](common/provisioning-notification-email.png)

7. Haga clic en **Save**(Guardar).

8. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Users to Rollbar** (Sincronizar usuarios de Azure Active Directory con Rollbar).

    ![Asignaciones de usuario de Rollbar](media/rollbar-provisioning-tutorial/usermapping.png)

9. Examine los atributos de usuario que se sincronizan entre Azure AD y Rollbar en la sección **Asignaciones de atributos**. Los atributos seleccionados como propiedades de **coincidencia** se usan para buscar coincidencias con las cuentas de usuario de Rollbar con el objetivo de realizar operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de usuario de Rollbar](media/rollbar-provisioning-tutorial/userattribute.png)

10. En la sección **Asignaciones**, seleccione **Synchronize Azure Active Directory Groups to Rollbar** (Sincronizar grupos de Azure Active Directory con Rollbar).

    ![Asignaciones de grupos de Rollbar](media/rollbar-provisioning-tutorial/groupmapping.png)

11. Examine los atributos de grupo que se sincronizan entre Azure AD y Rollbar en la sección **Asignaciones de atributos**. Los atributos seleccionados como propiedades de **coincidencia** se usan para buscar coincidencias con los grupos de Rollbar para las operaciones de actualización. Seleccione el botón **Guardar** para confirmar los cambios.

    ![Atributos de grupo de Rollbar](media/rollbar-provisioning-tutorial/groupattribute.png)

12. Para configurar filtros de ámbito, consulte las siguientes instrucciones, que se proporcionan en el artículo [Aprovisionamiento de aplicaciones basado en atributos con filtros de ámbito](../app-provisioning/define-conditional-rules-for-provisioning-user-accounts.md).

13. Para habilitar el servicio de aprovisionamiento de Azure AD para Rollbar, cambie **Estado de aprovisionamiento** a **Activado** en la sección **Configuración**.

    ![Estado de aprovisionamiento activado](common/provisioning-toggle-on.png)

14. Elija los valores deseados en **Ámbito**, en la sección **Configuración**, para definir los usuarios o grupos que desea que se aprovisionen en Rollbar.

    ![Ámbito del aprovisionamiento](common/provisioning-scope.png)

15. Cuando esté listo para realizar el aprovisionamiento, haga clic en **Guardar**.

    ![Guardar la configuración de aprovisionamiento](common/provisioning-configuration-save.png)

    Esta operación inicia la sincronización inicial de todos los usuarios o grupos definidos en **Ámbito** en la sección **Configuración**. La sincronización inicial tarda más tiempo en realizarse que las posteriores, que se producen aproximadamente cada 40 minutos si el servicio de aprovisionamiento de Azure AD está ejecutándose. Puede usar la sección **Detalles de sincronización** para supervisar el progreso y seguir los vínculos al informe de actividad de aprovisionamiento, donde se describen todas las acciones que ha llevado a cabo el servicio de aprovisionamiento de Azure AD en Rollbar.

    Para más información sobre cómo leer los registros de aprovisionamiento de Azure AD, consulte [Creación de informes sobre el aprovisionamiento automático de cuentas de usuario](../app-provisioning/check-status-user-account-provisioning.md).
    
## <a name="additional-resources"></a>Recursos adicionales

* [Administración del aprovisionamiento de cuentas de usuario para aplicaciones empresariales](../app-provisioning/configure-automatic-user-provisioning-portal.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

## <a name="next-steps"></a>Pasos siguientes

* [Aprenda a revisar los registros y a obtener informes sobre la actividad de aprovisionamiento](../app-provisioning/check-status-user-account-provisioning.md)
