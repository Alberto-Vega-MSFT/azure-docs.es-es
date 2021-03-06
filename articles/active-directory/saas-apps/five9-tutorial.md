---
title: 'Tutorial: Integración de Azure Active Directory con Five9 Plus Adapter (CTI, Contact Center Agents) | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Five9 Plus Adapter (CTI, Contact Center Agents).
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 88dc82ab-be0b-4017-8335-c47d00775d7b
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/18/2017
ms.author: jeedes
ms.openlocfilehash: c8d3364f9de298d19df35846af5f421978fea960
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842176"
---
# <a name="tutorial-azure-active-directory-integration-with-five9-plus-adapter-cti-contact-center-agents"></a>Tutorial: Integración de Azure Active Directory con Five9 Plus Adapter (CTI, Contact Center Agents)

En este tutorial aprenderá a integrar Five9 Plus Adapter (CTI, Contact Center Agents) con Azure Active Directory (Azure AD).

La integración de Five9 Plus Adapter (CTI, Contact Center Agents) con Azure AD le proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Five9 Plus Adapter (CTI, Contact Center Agents)
- Puede permitir que los usuarios inicien sesión automáticamente en Five9 Plus Adapter (CTI, Contact Center Agents) (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents), necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único de Five9 Plus Adapter (CTI, Contact Center Agents)

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de evaluación de un mes aquí: [Oferta de evaluación](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar Five9 Plus Adapter (CTI, Contact Center Agents) desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-five9-plus-adapter-cti-contact-center-agents-from-the-gallery"></a>Agregar Five9 Plus Adapter (CTI, Contact Center Agents) desde la galería
Para configurar la integración de Five9 Plus Adapter (CTI, Contact Center Agents) en Azure AD, deberá agregar Five9 Plus Adapter (CTI, Contact Center Agents) desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Five9 Plus Adapter (CTI, Contact Center Agents) desde la galería, lleve a cabo los siguientes pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Five9 Plus Adapter (CTI, Contact Center Agents)**.

    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/tutorial_five9_search.png)

1. En el panel de resultados, seleccione **Five9 Plus Adapter (CTI, Contact Center Agents)** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/tutorial_five9_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección puede configurar y probar el inicio de sesión único de Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents) con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Five9 Plus Adapter (CTI, Contact Center Agents) para un usuario de Azure AD. Es decir, es preciso establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Five9 Plus Adapter (CTI, Contact Center Agents).

Para establecer la relación de vínculo, en Five9 Plus Adapter (CTI, Contact Center Agents), asigne el valor del **nombre de usuario** de Azure AD como valor del **nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents), es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Five9 Plus Adapter (CTI, Contact Center Agents)](#creating-a-five9-plus-adapter-cti-contact-center-agents-test-user)**: para disponer de un usuario homólogo de Britta Simon en Five9 Plus Adapter (CTI, Contact Center Agents) que esté vinculado a la representación de Azure AD del usuario.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Five9 Plus Adapter (CTI, Contact Center Agents).

**Para configurar el inicio de sesión único de Azure AD con Five9 Plus Adapter (CTI, Contact Center Agents), lleve a cabo los siguientes pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Five9 Plus Adapter (CTI, Contact Center Agents)**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_five9_samlbase.png)

1. En la sección **Dominio y direcciones URL de Five9 Plus Adapter (CTI, Contact Center Agents)**, lleve a cabo los siguientes pasos:

    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_five9_url.png)
    
     a. En el cuadro de texto **Identificador**, escriba una dirección URL con los siguientes patrones:

    |    Entorno      |       URL      |
    | :-- | :-- |
    | Para "Five9 Plus Adapter for Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/metadata/alias/msdc` |
    | Para "Five9 Plus Adapter for Zendesk" | `https://app.five9.com/appsvcs/saml/metadata/alias/zd` |
    | Para "Five9 Plus Adapter for Agent Desktop Toolkit" | `https://app.five9.com/appsvcs/saml/metadata/alias/adt` |

    b. En el cuadro de texto **URL de respuesta** , escriba una dirección URL con el siguiente patrón:

    |      Entorno     |      URL      |
    | :--                  | :--           |
    | Para "Five9 Plus Adapter for Microsoft Dynamics CRM" | `https://app.five9.com/appsvcs/saml/SSO/alias/msdc` |
    | Para "Five9 Plus Adapter for Zendesk" | `https://app.five9.com/appsvcs/saml/SSO/alias/zd` |
    | Para "Five9 Plus Adapter for Agent Desktop Toolkit" | `https://app.five9.com/appsvcs/saml/SSO/alias/adt` |

1. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_five9_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_general_400.png)

1. En la sección **Configuración de Five9 Plus Adapter (CTI, Contact Center Agents)**, haga clic en **Configurar Five9 Plus Adapter (CTI, Contact Center Agents)** para que se abra la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_five9_configure.png) 

1. Para configurar el inicio de sesión único en **Five9 Plus Adapter (CTI, Contact Center Agents)**, debe enviar el **certificado descargado (Base64), la dirección URL de cierre de sesión, el identificador de entidad de SAML y la dirección URL del servicio de inicio de sesión único de SAML** al [equipo de soporte técnico de Five9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact). Además, para configurar aún más el inicio de sesión único, puede seguir los pasos descritos a continuación en función del adaptador:

     a. Guía del administrador de "Five9 Plus Adapter for Agent Desktop Toolkit": [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/agent-desktop-toolkit/plus-agent-desktop-toolkit-administrators-guide.pdf)
    
    b. Guía del administrador de "Five9 Plus Adapter for Microsoft Dynamics CRM": [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/microsoft/microsoft-administrators-guide.pdf)
    
    c. Guía del administrador de "Five9 Plus Adapter for Zendesk": [https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf](https://webapps.five9.com/assets/files/for_customers/documentation/integrations/zendesk/zendesk-plus-administrators-guide.pdf)


> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/five9-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-five9-plus-adapter-cti-contact-center-agents-test-user"></a>Creación de un usuario de prueba de Five9 Plus Adapter (CTI, Contact Center Agents)

En esta sección creará un usuario llamado Britta Simon en Five9 Plus Adapter (CTI, Contact Center Agents). Colabore con el  [equipo de soporte técnico de withFive9 Plus Adapter (CTI, Contact Center Agents)](https://www.five9.com/about/contact)  para agregar los usuarios a la plataforma de Five9 Plus Adapter (CTI, Contact Center Agents). Los usuarios se tienen que crear y activar antes de usar el inicio de sesión único.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Five9 Plus Adapter (CTI, Contact Center Agents).

![Asignar usuario][200] 

**Para asignar a Britta Simon a Five9 Plus Adapter (CTI, Contact Center Agents), lleve a cabo los siguientes pasos:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Five9 Plus Adapter (CTI, Contact Center Agents)**.

    ![Configurar inicio de sesión único](./media/five9-tutorial/tutorial_five9_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Five9 Plus Adapter (CTI, Contact Center Agents) del panel de acceso, debería iniciarse de forma automática la sesión de la aplicación Five9 Plus Adapter (CTI, Contact Center Agents).
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/five9-tutorial/tutorial_general_01.png
[2]: ./media/five9-tutorial/tutorial_general_02.png
[3]: ./media/five9-tutorial/tutorial_general_03.png
[4]: ./media/five9-tutorial/tutorial_general_04.png

[100]: ./media/five9-tutorial/tutorial_general_100.png

[200]: ./media/five9-tutorial/tutorial_general_200.png
[201]: ./media/five9-tutorial/tutorial_general_201.png
[202]: ./media/five9-tutorial/tutorial_general_202.png
[203]: ./media/five9-tutorial/tutorial_general_203.png

