---
title: 'Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y SAP Cloud for Customer.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 90154dab-eba2-4563-bcf0-f2acc797ea97
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/14/2017
ms.author: jeedes
ms.openlocfilehash: 59bbcbf9aaef17394151e7db1471b63b87a46288
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39445724"
---
# <a name="tutorial-azure-active-directory-integration-with-sap-cloud-for-customer"></a>Tutorial: Integración de Azure Active Directory con SAP Cloud for Customer

En este tutorial, aprenderá a integrar SAP Cloud for Customer con Azure Active Directory (Azure AD).

La integración de SAP Cloud for Customer con Azure AD proporciona las siguientes ventajas:

- En Azure AD se puede controlar quién tiene acceso a SAP Cloud for Customer.
- Es posible habilitar que los usuarios inicien sesión automáticamente en SAP Cloud for Customer (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con SAP Cloud for Customer, se necesitan los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción a SAP Cloud for Customer con inicio de sesión único habilitado

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, aquí puede obtener una versión de evaluación de un mes: [Oferta de prueba](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Adición de SAP Cloud for Customer desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-sap-cloud-for-customer-from-the-gallery"></a>Adición de SAP Cloud for Customer desde la galería
Para configurar la integración de SAP Cloud for Customer en Azure AD, es preciso agregar SAP Cloud for Customer desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar SAP Cloud for Customer desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **SAP Cloud for Customer**.

    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_search.png)

1. En el panel de resultados, seleccione **SAP Cloud for Customer** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, se configura y se prueba el inicio de sesión único de Azure AD con SAP Cloud for Customer con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, es preciso que Azure AD sepa cuál es el usuario homólogo de SAP Cloud for Customer para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de SAP Cloud for Customer.

Para establecer la relación de vínculo, en SAP Cloud for Customer, asigne el valor de **nombre de usuario** de Azure AD como valor de **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con SAP Cloud for Customer, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de SAP Cloud for Customer](#creating-a-sap-cloud-for-customer-test-user)**: para tener un homólogo de Britta Simon en SAP Cloud for Customer que esté vinculado a su representación en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación SAP Cloud for Customer.

**Para configurar el inicio de sesión único de Azure AD con SAP Cloud for Customer, siga estos pasos:**

1. En la página de integración de la aplicación **SAP Cloud for Customer** de Azure Portal, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_samlbase.png)

1. En la sección **Dominio y direcciones URL de SAP Cloud for Customer**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_url.png)

    a. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL con el siguiente patrón: `https://<server name>.crm.ondemand.com`.

    b. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<server name>.crm.ondemand.com`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizarlos con la dirección URL y el identificador reales de inicio de sesión. Póngase en contacto con el [equipo de soporte técnico de SAP Cloud for Customer](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) para obtener estos valores. 

1. En la sección **Agregar usuario**, realice estos pasos:

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_attribute.png)

    a. En la lista **Identificador de usuario**, seleccione la función **ExtractMailPrefix**.

    b. En la lista **Correo** , seleccione el atributo de usuario que desea usar en su implementación.
    Por ejemplo, si quiere usar EmployeeID como identificador de usuario único y ha almacenado el valor del atributo en ExtensionAttribute2, seleccione user.extensionattribute2.  

1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de SAP Cloud for Customer**, haga clic en **Configurar SAP Cloud for Customer** para abrir la ventana **Configurar inicio de sesión**. Copie la **dirección URL de servicio de inicio de sesión único de SAML** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_configure.png) 

1. Para configurar SSO, siga estos pasos:
   
    a. Inicie sesión en el portal de SAP Cloud for Customer con derechos de administrador.
   
    b. Navegue hasta **Application and User Management Common Task** (Tarea común de administración de usuarios y aplicaciones) y haga clic en la pestaña **Proveedor de identidades**.
   
    c. Haga clic en **New Identity Provider** (Nuevo proveedor de identidades) y seleccione el archivo XML de metadatos que ha descargado de Azure Portal. Al importar los metadatos, el sistema carga automáticamente el certificado de firma y el certificado de cifrado necesarios.
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_54.png)
   
    d. Azure Active Directory requiere la URL del Servicio de consumidor de aserciones del elemento en la solicitud SAML, así que active la casilla **Include Assertion Consumer Service URL** (Incluir URL del Servicio de consumidor de aserciones).
   
    e. Haga clic en **Activate Single Sign-On**(Activar inicio de sesión único).
   
    f. Guarde los cambios.
   
    g. Haga clic en la pestaña **My System** (Mi sistema).
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_52.png)
   
    h. En el cuadro de texto **Azure AD Sign On URL** (Dirección URL de inicio de sesión de Azure AD), pegue la **dirección URL del servicio de inicio de sesión único de SAML** que copió desde Azure Portal.
   
    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_53.png)
   
    i. Especifique si el empleado puede elegir manualmente entre iniciar sesión con el Id. de usuario y una contraseña o mediante SSO, para lo que debe seleccionar **Manual Identity Provider Selection** (Selección manual de proveedor de identidades).
   
    j. En la sección **SSO URL** (URL de SSO), especifique la dirección URL que deben utilizar los empleados para iniciar sesión en el sistema. 
    En la lista **URL Sent to Employee** (URL que se envía a empleado), puede elegir una de las siguientes opciones:
   
    **Non-SSO URL**
   
    El sistema solo envía la dirección URL del sistema normal al empleado. El empleado no puede iniciar sesión mediante SSO y debe usar una contraseña o un certificado en su lugar.
   
    **SSO URL** 
   
    El sistema solo envía la dirección URL de SSO al empleado. El empleado puede iniciar sesión mediante SSO. La solicitud de autenticación se redirige a través de IdP.
   
    **Automatic Selection**
   
    SI SSO no está activo, el sistema envía la dirección URL del sistema normal al empleado. Si SSO está activo, el sistema comprueba si el empleado tiene contraseña. Si existe una contraseña, se envían al empleado tanto la URL de SSO como la URL no de SSO. Sin embargo, si el empleado no tiene contraseña, solo se envía la primera al empleado.
   
    k. Guarde los cambios.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/sap-customer-cloud-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-sap-cloud-for-customer-test-user"></a>Creación de un usuario de prueba de SAP Cloud for Customer

En esta sección, creará un usuario llamado Britta Simon en SAP Cloud for Customer. Trabaje con el [equipo de soporte técnico de SAP Cloud for Customer](https://www.sap.com/about/agreements.sap-cloud-services-customers.html) para agregar usuarios en la plataforma SAP Cloud for Customer. 

> [!NOTE]
> Asegúrese de que el valor de NameID coincide con el campo de nombre de usuario de la plataforma SAP Cloud for Customer.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, concederá acceso a Britta Simon a SAP Cloud for Customer para que use el inicio de sesión único de Azure.

![Asignar usuario][200] 

**Para asignar Britta Simon a SAP Cloud for Customer, siga estos pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **SAP Cloud for Customer**.

    ![Configurar inicio de sesión único](./media/sap-customer-cloud-tutorial/tutorial_sapcloudforcustomer_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de SAP Cloud for Customer en el Panel de acceso, debería iniciar sesión automáticamente en su aplicación SAP Cloud for Customer.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/sap-customer-cloud-tutorial/tutorial_general_01.png
[2]: ./media/sap-customer-cloud-tutorial/tutorial_general_02.png
[3]: ./media/sap-customer-cloud-tutorial/tutorial_general_03.png
[4]: ./media/sap-customer-cloud-tutorial/tutorial_general_04.png

[100]: ./media/sap-customer-cloud-tutorial/tutorial_general_100.png

[200]: ./media/sap-customer-cloud-tutorial/tutorial_general_200.png
[201]: ./media/sap-customer-cloud-tutorial/tutorial_general_201.png
[202]: ./media/sap-customer-cloud-tutorial/tutorial_general_202.png
[203]: ./media/sap-customer-cloud-tutorial/tutorial_general_203.png

