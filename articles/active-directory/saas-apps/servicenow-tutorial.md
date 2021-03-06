---
title: 'Tutorial: Integración de Azure Active Directory con ServiceNow | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y ServiceNow.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: joflore
ms.assetid: a5a1a264-7497-47e7-b129-a1b5b1ebff5b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/04/2018
ms.author: jeedes
ms.openlocfilehash: 5b7baffea8e718810a91ea9687a007d36c806aab
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52850013"
---
# <a name="tutorial-azure-active-directory-integration-with-servicenow"></a>Tutorial: Integración de Azure Active Directory con ServiceNow

En este tutorial, aprenderá a integrar ServiceNow con Azure Active Directory (Azure AD).

La integración de ServiceNow con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a ServiceNow.
- Puede habilitar que los usuarios inicien sesión automáticamente en ServiceNow (inicio de sesión único) con su cuenta de Azure AD.
- Puede administrar sus cuentas en una ubicación central: Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con ServiceNow, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Para ServiceNow, una instancia o inquilino de ServiceNow, versión Calgary o superior
- Para ServiceNow Express, una instancia de ServiceNow Express, versión Helsinki o superior
- El inquilino de ServiceNow debe tener habilitado el [complemento de inicio de sesión único en varios proveedores](https://wiki.servicenow.com/index.php?title=Multiple_Provider_Single_Sign-On#gsc.tab=0). Esto puede hacerse [enviando una solicitud de servicio](https://hi.service-now.com).
- Para la configuración automática, habilite el complemento de varios proveedores para ServiceNow.

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede [obtener una versión de prueba durante un mes](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario

En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba.
El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de ServiceNow desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-servicenow-from-the-gallery"></a>Adición de ServiceNow desde la galería

Para configurar la integración de ServiceNow en Azure AD, es preciso agregarlo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar ServiceNow desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Botón Azure Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![Hoja Aplicaciones empresariales][2]

3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![Botón Nueva aplicación][3]

4. En el cuadro de búsqueda, escriba **ServiceNow**, seleccione **ServiceNow** en el panel de resultados y haga clic en el botón **Agregar** para agregar la aplicación.

    ![ServiceNow en la lista de resultados](./media/servicenow-tutorial/tutorial_servicenow_addfromgallery.png)

## <a name="configure-and-test-azure-ad-single-sign-on"></a>Configuración y prueba del inicio de sesión único en Azure AD

En esta sección, configurará y probará el inicio de sesión único de Azure AD con ServiceNow con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de ServiceNow para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de ServiceNow.

Para establecer la relación de vínculo, en ServiceNow, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con ServiceNow, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD para ServiceNow](#configure-azure-ad-single-sign-on-for-servicenow)**: para permitir a los usuarios usar esta característica.
2. **[Configuración del inicio de sesión único de Azure AD para ServiceNow Express](#configure-azure-ad-single-sign-on-for-servicenow-express)**: para permitir a los usuarios usar esta característica.
3. **[Creación de un usuario de prueba de Azure AD](#create-an-azure-ad-test-user)**, para probar el inicio de sesión único de Azure AD con Britta Simon.
4. **[Creación de usuarios de prueba de ServiceNow](#create-a-servicenow-test-user)**: para tener un homólogo de Britta Simon en ServiceNow que esté vinculado a la representación de usuario en Azure AD.
5. **[Asignación del usuario de prueba de Azure AD](#assign-the-azure-ad-test-user)**, para permitir que Britta Simon use el inicio de sesión único de Azure AD.
6. **[Prueba del inicio de sesión único](#test-single-sign-on)**: para comprobar si la configuración funciona.

### <a name="configure-azure-ad-single-sign-on-for-servicenow"></a>Configuración del inicio de sesión único de Azure AD para ServiceNow

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación ServiceNow.

**Para configurar el inicio de sesión único de Azure AD con ServiceNow, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **ServiceNow**, haga clic en **Inicio de sesión único**.

    ![Vínculo Configurar inicio de sesión único][4]

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_300.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_301.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_302.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Información acerca del inicio de sesión único de dominio y direcciones URL de ServiceNow](./media/servicenow-tutorial/tutorial_servicenow_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com/navpage.do`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizar estos valores con la dirección URL de inicio de sesión y el identificador reales, como se explica más adelante en el tutorial.

6. En la sección **Certificado de firma de SAML**, realice los pasos siguientes:

    ![Vínculo de descarga del certificado](./media/servicenow-tutorial/tutorial_servicenow_certificate.png)

     a. Haga clic en el botón Copiar para copiar la **dirección URL de metadatos de federación de aplicación** y péguela en el Bloc de notas, se usará más adelante en el tutorial.

    b. Haga clic en **Descargar** para descargar el **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

7. Inicie sesión en la aplicación ServiceNow como administrador.

8. Active el complemento **Integration - Multiple Provider Single Sign-On Installer**, para lo que debe seguir estos pasos:

     a. En el panel de navegación del lado izquierdo, busque la sección **System Definition** (Definición del sistema) con la barra de búsqueda y haga clic en **Plugins** (Complementos).

    ![Activar complemento](./media/servicenow-tutorial/tutorial_servicenow_03.png "activar complemento")

     b. Busque **Integration - Multiple Provider Single Sign-On Installer**.

     ![Activar complemento](./media/servicenow-tutorial/tutorial_servicenow_04.png "activar complemento")

    c. Seleccione el complemento. Haga clic con el botón derecho y seleccione **Activate/Upgrade** (Activar o actualizar).

     ![Activar complemento](./media/servicenow-tutorial/tutorial_activate.png "activar complemento")

    d. Haga clic en el botón **Activate**.

     ![Activar complemento](./media/servicenow-tutorial/tutorial_activate1.png "activar complemento")

9. En el panel de navegación del lado izquierdo, busque la sección de **SSO de varios proveedores** de la barra de búsqueda y haga clic en **Properties** (Propiedades).

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/tutorial_servicenow_06.png "Configurar dirección URL de la aplicación")

10. En el cuadro de diálogo **Multiple Provider SSO Properties** (Propiedades de SSO de varias proveedores), realice los pasos siguientes:

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694981.png "Configurar dirección URL de la aplicación")

    * En **Enable multiple provider SSO** (Habilitar SSO de varios proveedores), seleccione **Yes** (Sí).
  
    * En **Enable Auto Importing of users from all identity providers into the user table** (Habilitar la importación automática de usuarios de todos los proveedores de identidades en la tabla de usuarios), seleccione **Yes** (Sí).

    * En **Enable debug logging for the multiple provider SSO integration** (Habilitar registro de depuración para la integración de SSO de varios proveedores), seleccione **Yes** (Sí).

    * En el cuadro de texto **The field on the user table that...** (El campo en la tabla de usuario que...), escriba **user_name** (nombre_usuario).
  
    * Haga clic en **Save**(Guardar).

11. Hay dos maneras de configurar **ServiceNow**: automática y manual.

12. Para configurar **ServiceNow** automáticamente, siga estos pasos:

    * Vuelva a la página de inicio de sesión único de **ServiceNow** en Azure Portal.

    * ServiceNow tiene el servicio de configuración con un clic, es decir, Azure AD configura automáticamente ServiceNow para la autenticación de SAML. Para habilitarlo, vaya a la sección **ServiceNow Configuration** (Configuración de ServiceNow), haga clic en **Configure ServiceNow** (Configurar ServiceNow) para abrir la ventana Configurar inicio de sesión.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

    * Escriba el nombre de la instancia de ServiceNow, el nombre de usuario y la contraseña de administrador en el formulario **Configurar inicio de sesión** y haga clic en **Configurar ahora**. Tenga en cuenta que el nombre de usuario de administrador proporcionado debe tener asignado el rol **security_admin** en ServiceNow para que esto funcione. Si no es así, para configurar manualmente ServiceNow para usar Azure AD como proveedor de identidades SAML, haga clic en **Configurar manualmente el inicio de sesión único** y copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** desde la sección Referencia rápida.

        ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/configure.png "Configurar dirección URL de la aplicación")

    * Inicie sesión en la aplicación ServiceNow como administrador.

    * En la configuración automática se configuran todos los parámetros necesarios de **ServiceNow** pero el **certificado X.509** no está habilitado de forma predeterminada. Tiene que asignarlo manualmente al proveedor de identidades en ServiceNow. Siga a continuación los pasos correspondientes:

    * En el panel de navegación del lado izquierdo, busque la sección de **Multi-Provider SSO** (Inicio de sesión único de varios proveedores) de la barra de búsqueda y haga clic en **Proveedores de identidades**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurar inicio de sesión único")

    * Haga clic en el proveedor de identidades generado automáticamente.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_08.png "Configurar inicio de sesión único")

    *  En la sección **Proveedor de identidades**, realice los siguientes pasos:

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/automatic_config.png "Configurar inicio de sesión único")

        * En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **Inicio de sesión único federado de Microsoft Azure**).

        * Quite el valor rellenado **Identity Provider's SingleLogoutRequest** (Solicitud de cierre de sesión único del proveedor de identidades) del cuadro de texto.

        * Copie el valor de **ServiceNow Homepage**, péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

            > [!NOTE]
            > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

        * Copie el valor de **Entity ID / Issuer** (Id. de entidad / emisor), péguelo en el cuadro de texto **Identificador** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

        * Asegúrese de que el valor de **NameID Policy** (Directiva de id. de nombres) esté establecido en el valor `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`. 

    * Desplácese hacia abajo a la sección **Certificado X.509** y seleccione **Editar**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_09.png "Configurar inicio de sesión único")

    * Seleccione el certificado y haga clic con el botón derecho en el icono de flecha para agregar el certificado.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_11.png "Configurar inicio de sesión único")

    * Haga clic en **Save**(Guardar).

    * Haga clic en **Prueba de conexión** en la esquina superior derecha de la página.

        ![Activar complemento](./media/servicenow-tutorial/tutorial_activate2.png "activar complemento")

    * Después de hacer clic en **Prueba de conexión**, aparecerá la ventana emergente donde deberá escribir las credenciales y se mostrará la página siguiente con los resultados. Se espera el error **SSO Logout Test Results** (Resultados de pruebas de cierre de sesión de SSO). Ignórelo y haga clic en el botón **Activar**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/servicenowactivate.png "Configurar inicio de sesión único")
  
13. Para configurar **ServiceNow** manualmente, siga los pasos siguientes:

    * Inicie sesión en la aplicación ServiceNow como administrador.

    * En el panel de navegación de la izquierda, haga clic en **Identity Providers**(Proveedores de identidades).

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_07.png "Configurar inicio de sesión único")

    * En el cuadro de diálogo **Proveedores de identidades**, haga clic en **Nuevo**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694977.png "Configurar inicio de sesión único")

    * En el cuadro de diálogo **Proveedores de identidades**, haga clic en **SAML**.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694978.png "Configurar inicio de sesión único")

    * En la ventana emergente **Import Identity Provider Metadata** (Importar metadatos del proveedor de identidades), realice los siguientes pasos:

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/idp.png "Configurar inicio de sesión único")

        * Escriba la **dirección URL de metadatos de federación de aplicación** que haya copiado de Azure Portal.

        * Haga clic en **Import**.

    * Se lee la dirección URL de metadatos de IdP y se rellena toda la información de campos.

        ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694982.png "Configurar inicio de sesión único")

        * En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **Inicio de sesión único federado de Microsoft Azure**).

        * Quite el valor rellenado **Identity Provider's SingleLogoutRequest** (Solicitud de cierre de sesión único del proveedor de identidades) del cuadro de texto.

        * Copie el valor de **ServiceNow Homepage**, péguelo en el cuadro de texto **Dirección URL de inicio de sesión** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

            > [!NOTE]
            > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

        * Copie el valor de **Entity ID / Issuer** (Id. de entidad / emisor), péguelo en el cuadro de texto **Identificador** de la sección **ServiceNow Domain and URLs** (Dominio y direcciones URL de ServiceNow) de Azure Portal.

        * Asegúrese de que el valor de **NameID Policy** (Directiva de id. de nombres) esté establecido en el valor `urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified`.

        * Haga clic en **Avanzadas**. En el cuadro de texto **Campo de usuario**, escriba **correo electrónico** o **user_name**, según qué campo se use para identificar de forma única a los usuarios en la implementación de ServiceNow.

            > [!NOTE]
            > Puede configurar Azure AD para que emita el identificador de usuario de Azure AD (nombre de usuario principal) o la dirección de correo electrónico como identificador único en el token SAML. Para ello, vaya a la sección **ServiceNow > Atributos > Inicio de sesión único** en Azure Portal y asigne el campo que desee al atributo **nameidentifier**. El valor almacenado para el atributo seleccionado en Azure AD (por ejemplo, nombre de usuario principal) debe coincidir con el valor almacenado en ServiceNow para el campo especificado (por ejemplo, user_name).

        * Haga clic en **Prueba de conexión** en la esquina superior derecha de la página.

        * Después de hacer clic en **Prueba de conexión**, aparecerá la ventana emergente donde deberá escribir las credenciales y se mostrará la página siguiente con los resultados. Se espera el error **SSO Logout Test Results** (Resultados de pruebas de cierre de sesión de SSO). Ignórelo y haga clic en el botón **Activar**.

          ![Configurar inicio de sesión único](./media/servicenow-tutorial/servicenowactivate.png "Configurar inicio de sesión único")

### <a name="configure-azure-ad-single-sign-on-for-servicenow-express"></a>Configuración del inicio de sesión único de Azure AD para ServiceNow Express

1. En Azure Portal, en la página de integración de la aplicación **ServiceNow**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. Haga clic en **Cambiar el modo de inicio de sesión único** en la parte superior de la pantalla para seleccionar el modo **SAML**.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_300.png)

3. En el cuadro de diálogo **Seleccione un método de inicio de sesión único**, haga clic en **Seleccionar** para el modo **SAML** para habilitar el inicio de sesión único.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_301.png)

4. En la página **Configurar el inicio de sesión único con SAML**, haga clic en el botón **Editar** para abrir el cuadro de diálogo **Configuración básica de SAML**.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_general_302.png)

5. En la sección **Configuración básica de SAML**, siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_url.png)

     a. En el cuadro de texto **URL de inicio de sesión**, escriba el valor con el siguiente patrón: `https://<instance-name>.service-now.com/navpage.do`

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<instance-name>.service-now.com`

    > [!NOTE]
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de atención al cliente de ServiceNow](https://www.servicenow.com/support/contact-support.html) para obtener estos valores.

6. En la sección **Certificado de firma SAML**, haga clic **Descargar** para descargar el **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_certificates.png)

7. ServiceNow tiene el servicio de configuración con un clic, es decir, Azure AD configura automáticamente ServiceNow para la autenticación de SAML. Para habilitar este servicio, vaya a la sección **Set up ServiceNow** (Configurar ServiceNow) y haga clic en **Ver instrucciones detalladas** para abrir la ventana Configurar inicio de sesión.

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/tutorial_servicenow_configure.png)

8. Escriba el nombre de la instancia de ServiceNow, el nombre de usuario y la contraseña de administrador en el formulario **Configurar inicio de sesión** y haga clic en **Configurar ahora**. Tenga en cuenta que el nombre de usuario de administrador proporcionado debe tener asignado el rol **security_admin** en ServiceNow para que esto funcione. Si no es así, para configurar manualmente ServiceNow para usar Azure AD como proveedor de identidades SAML, haga clic en **Configurar manualmente el inicio de sesión único** y copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** desde la sección Referencia rápida.

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/configure.png "Configurar dirección URL de la aplicación")

9. Inicie sesión en la aplicación ServiceNow Express como administrador.

10. En el panel de navegación del lado izquierdo, haga clic en **Inicio de sesión único**.

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694980ex.png "Configurar dirección URL de la aplicación")

11. En el cuadro de diálogo **Inicio de sesión único**, haga clic en el icono de configuración de la parte superior derecha y establezca las siguientes propiedades:

    ![Configurar dirección URL de la aplicación](./media/servicenow-tutorial/ic7694981ex.png "Configurar dirección URL de la aplicación")

     a. Cambie **Enable multiple provider SSO** (Habilitar SSO de varios proveedores), hacia la derecha.

    b. Cambie **Enable debug logging got the multiple provider SSO integration** (Habilitar registro de depuración para integración de SSO de varios proveedores) hacia la derecha.

    c. En el cuadro de texto **The field on the user table that...** (El campo en la tabla de usuario que...), escriba **user_name** (nombre_usuario).

12. En el cuadro de diálogo **Single Sign-On** (Inicio de sesión único), haga clic en **Add New Certificate** (Agregar nuevo certificado).

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694973ex.png "Configurar inicio de sesión único")

13. En el cuadro de diálogo **Certificados X.509** , realice los pasos siguientes:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694975.png "Configurar inicio de sesión único")

     a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **TestSAML2.0**).

    b. Seleccione **Active**(Activo).

    c. En **Format** (Formato), seleccione **PEM**.

    d. En **Type** (Tipo), seleccione **Trust Store Cert** (Confiar en certificados de almacén).

    e. Abra el certificado codificado en base 64 descargado de Azure Portal en el bloc de notas, copie su contenido en el portapapeles y péguelo en el cuadro de texto **PEM Certificate** (Certificado PEM).

    f. Haga clic en **Update** (Actualizar).

14. En el cuadro de diálogo **Single Sign-On** (Inicio de sesión único), haga clic en **Add New IdP** (Agregar nuevo IdP).

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694976ex.png "Configurar inicio de sesión único")

15. En el cuadro de diálogo **Add New Identity Provider** (Agregar nuevo proveedor de identidades), en **Configure Identity Provider** (Configurar proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694982ex.png "Configurar inicio de sesión único")

     a. En el cuadro de texto **Name** (Nombre), escriba el nombre de la configuración (por ejemplo, **SAML 2.0**).

    b. En el campo **Identity provider URL** (Dirección URL del proveedor de identidades), pegue el valor del **identificador del proveedor de identidades** que ha copiado de Azure Portal.

    c. En el campo **Identity Provider's AuthnRequest** (Solicitud de autenticación del proveedor de identidades), pegue el valor de la **dirección URL de solicitud de autenticación** que ha copiado de Azure Portal.

    d. En el campo **Identity Provider's SingleLogoutRequest** (Solicitud de cierre de sesión único del proveedor de identidades), pegue el valor de la **dirección URL del servicio de cierre de sesión único** que ha copiado de Azure Portal.

    e. Como **Certificado de proveedor de identidades**, seleccione el certificado que ha creado en el paso anterior.

16. Haga clic en **Advanced Settings** (Configuración avanzada) y en **Additional Identity Provider Properties** (Propiedades adicionales del proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694983ex.png "Configurar inicio de sesión único")

     a. En el cuadro de texto **Protocol Binding for the IDP's SingleLogoutRequest** (Vinculación de protocolo para la solicitud de cierre de sesión único del proveedor de identidades), escriba **urn:oasis:names:tc:SAML:2.0:bindings:HTTP-Redirect**.

    b. En el cuadro de texto **NameID Policy** (Directiva NameID), escriba **urn:oasis:names:tc:SAML:1.1:nameid-format:unspecified**.

    c. En **AuthnContextClassRef Method** (Método AuthnContextClassRef), escriba `http://schemas.microsoft.com/ws/2008/06/identity/authenticationmethod/password`.

    d. Anule la selección de **Create an AuthnContextClass**(Crear AuthnContextClass).

17. En **Additional Identity Provider Properties** (Propiedades adicionales del proveedor de identidades), siga estos pasos:

    ![Configurar inicio de sesión único](./media/servicenow-tutorial/ic7694984ex.png "Configurar inicio de sesión único")

     a. En el cuadro de texto **ServiceNow Homepage** (Página de inicio de ServiceNow), escriba la dirección URL de la página de inicio de instancia de ServiceNow.

    > [!NOTE]
    > La página principal de la instancia de ServiceNow es una concatenación de su **URL de inquilino de ServiceNow** y **/navpage.do** (por ejemplo: `https://fabrikam.service-now.com/navpage.do`).

    b. En el cuadro de texto **Entity ID / Issuer** (Id. de entidad / emisor), escriba la dirección URL de su inquilino de ServiceNow.

    c. En el cuadro de texto **Audience URI** (Identificador URI de audiencia), escriba la dirección URL de su inquilino ServiceNow.

    d. En el cuadro de diálogo **Clock Skew** (Desplazamiento del reloj), escriba **60**.

    e. En el cuadro de texto **Campo de usuario**, escriba **correo electrónico** o **user_name**, según qué campo se use para identificar de forma única a los usuarios en la implementación de ServiceNow.

    > [!NOTE]
    > Puede configurar Azure AD para que emita el identificador de usuario de Azure AD (nombre de usuario principal) o la dirección de correo electrónico como identificador único en el token SAML. Para ello, vaya a la sección **ServiceNow > Atributos > Inicio de sesión único** en Azure Portal y asigne el campo que desee al atributo **nameidentifier**. El valor almacenado para el atributo seleccionado en Azure AD (por ejemplo, nombre de usuario principal) debe coincidir con el valor almacenado en ServiceNow para el campo especificado (por ejemplo, user_name).

    f. Haga clic en **Save**(Guardar).

### <a name="create-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD

El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

1. En Azure Portal, en el panel izquierdo, seleccione **Azure Active Directory**, **Usuarios** y **Todos los usuarios**.

    ![Creación de un usuario de Azure AD][100]

2. Seleccione **Nuevo usuario** en la parte superior de la pantalla.

    ![Creación de un usuario de prueba de Azure AD](./media/servicenow-tutorial/create_aaduser_01.png) 

3. En las propiedades Usuario, siga estos pasos.

    ![Creación de un usuario de prueba de Azure AD](./media/servicenow-tutorial/create_aaduser_02.png)

     a. En el campo **Nombre**, escriba **BrittaSimon**.
  
    b. En el campo **Nombre de usuario**, escriba **brittasimon@yourcompanydomain.extension**  
    Por ejemplo: BrittaSimon@contoso.com

    c. Seleccione **Propiedades**, active la casilla **Mostrar contraseña** y escriba el valor que se muestra en el cuadro de contraseña.

    d. Seleccione **Crear**.

### <a name="create-a-servicenow-test-user"></a>Creación de un usuario de prueba de ServiceNow

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en ServiceNow. ServiceNow admite el aprovisionamiento automático de usuarios, que está habilitado de forma predeterminada. [Aquí](servicenow-provisioning-tutorial.md) puede encontrar más información sobre cómo configurar el aprovisionamiento automático de usuarios.

> [!NOTE]
> Si necesita crear manualmente un usuario, es preciso que se ponga en contacto con el [equipo de soporte técnico al cliente de ServiceNow](https://www.servicenow.com/support/contact-support.html)

### <a name="assign-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a ServiceNow.

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201]

2. En la lista de aplicaciones, seleccione **ServiceNow**.

    ![Vínculo a ServiceNow en la lista de aplicaciones](./media/servicenow-tutorial/tutorial_servicenow_app.png)  

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Vínculo "Usuarios y grupos"][202]

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Panel Agregar asignación][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista Usuarios y, luego, haga clic en el botón **Seleccionar** en la parte inferior de la pantalla.

6. En el cuadro de diálogo **Agregar asignación**, seleccione el botón **Asignar**.

### <a name="test-single-sign-on"></a>Prueba de inicio de sesión único

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de ServiceNow en el panel de acceso, debería iniciar sesión automáticamente en su aplicación ServiceNow.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)
* [Configuración del aprovisionamiento de usuarios](servicenow-provisioning-tutorial.md)

<!--Image references-->

[1]: ./media/servicenow-tutorial/tutorial_general_01.png
[2]: ./media/servicenow-tutorial/tutorial_general_02.png
[3]: ./media/servicenow-tutorial/tutorial_general_03.png
[4]: ./media/servicenow-tutorial/tutorial_general_04.png

[100]: ./media/servicenow-tutorial/tutorial_general_100.png

[200]: ./media/servicenow-tutorial/tutorial_general_200.png
[201]: ./media/servicenow-tutorial/tutorial_general_201.png
[202]: ./media/servicenow-tutorial/tutorial_general_202.png
[203]: ./media/servicenow-tutorial/tutorial_general_203.png
