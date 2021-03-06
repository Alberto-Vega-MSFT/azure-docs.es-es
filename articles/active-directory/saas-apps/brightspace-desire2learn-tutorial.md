---
title: 'Tutorial: integración de Azure Active Directory con Brightspace by Desire2Learn | Microsoft Docs'
description: Obtenga información sobre cómo configurar el inicio de sesión único entre Azure Active Directory y Brightspace by Desire2Learn.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: e2d3065b-1f6c-4c45-af78-0d5da3266999
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: 6e1f586350a47b70da1b297b608feb9011363dec
ms.sourcegitcommit: 1d850f6cae47261eacdb7604a9f17edc6626ae4b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/02/2018
ms.locfileid: "39437536"
---
# <a name="tutorial-azure-active-directory-integration-with-brightspace-by-desire2learn"></a>Tutorial: Integración de Azure Active Directory con Brightspace by Desire2Learn

En este tutorial, obtendrá información sobre cómo integrar Brightspace by Desire2Learn con Azure Active Directory (Azure AD).

La integración de Brightspace by Desire2Learn con Azure AD proporciona las siguientes ventajas:

- En Azure AD puede controlar quién tiene acceso a Brightspace by Desire2Learn
- Puede permitir que los usuarios inicien sesión automáticamente en Brightspace by Desire2Learn (inicio de sesión único) con sus cuentas de Azure AD
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Brightspace by Desire2Learn, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único de Brightspace by Desire2Learn

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Brightspace by Desire2Learn desde la galería
1. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-brightspace-by-desire2learn-from-the-gallery"></a>Incorporación de Brightspace by Desire2Learn desde la galería
Para configurar la integración de Brightspace by Desire2Learn en Azure AD, deberá agregar Brightspace by Desire2Learn desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Brightspace by Desire2Learn desde la galería, siga estos pasos:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

1. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
1. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

1. En el cuadro de búsqueda, escriba **Brightspace by Desire2Learn**.

    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_search.png)

1. En el panel de resultados, seleccione **Brightspace by Desire2Learn** y, luego, haga clic en **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Brightspace by Desire2Learn con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Brightspace by Desire2Learn para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Brightspace by Desire2Learn.

Para establecer la relación de vínculo, en Brightspace by Desire2Learn, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Brightspace by Desire2Learn, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
1. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
1. **[Creación de un usuario de prueba de Brightspace by Desire2Learn](#creating-a-brightspace-by-desire2learn-test-user)**: para tener un homólogo de Britta Simon en Brightspace by Desire2Learn que esté vinculado a la representación del usuario en Azure AD.
1. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
1. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación Brightspace by Desire2Learn.

**Para configurar el inicio de sesión único de Azure AD con Brightspace by Desire2Learn, siga estos pasos:**

1. En Azure Portal, en la página de integración de la aplicación **Brightspace by Desire2Learn**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

1. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_samlbase.png)

1. En la sección **Dominio y direcciones URL de Brightspace by Desire2Learn**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_url.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón:
    | |
    |--|
    | `https://<companyname>.tenants.brightspace.com/samlLogin`|
    | `https://<companyname>.desire2learn.com/shibboleth-sp`|

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.desire2learn.com/d2l/lp/auth/login/samlLogin.d2l`.

    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Brightspace by Desire2Learn](https://www.d2l.com/contact/) para obtener estos valores.
 


1. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_certificate.png) 

1. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/brightspace-desire2learn-tutorial/tutorial_general_400.png)

1. Para configurar el inicio de sesión único en **Brightspace by Desire2Learn**, necesita enviar el archivo **XML de metadatos** descargado al [equipo de soporte técnico de Brightspace by Desire2Learn](https://www.d2l.com/contact/).

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/create_aaduser_01.png) 

1. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/create_aaduser_02.png) 

1. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/create_aaduser_03.png) 

1. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/brightspace-desire2learn-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-brightspace-by-desire2learn-test-user"></a>Creación de un usuario de prueba de Brightspace by Desire2Learn

Para permitir que los usuarios de Azure AD inicien sesión en Brightspace by Desire2Learn, tienen que aprovisionarse en Brightspace by Desire2Learn.  

En el caso de Brightspace by Desire2Learn, las cuentas de usuario tiene que crearlas el [equipo de soporte técnico de Brightspace by Desire2Learn](https://www.d2l.com/contact/).

>[!NOTE]
>Puede usar cualquier otra API o herramienta de creación de cuentas de usuario de Brightspace by Desire2Learn que proporcione Brightspace by Desire2Learn para aprovisionar cuentas de usuario de Azure Active Directory. 

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Brightspace by Desire2Learn.

![Asignar usuario][200] 

**Para asignar a Britta Simon a Brightspace by Desire2Learn, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

1. En la lista de aplicaciones, seleccione **Brightspace by Desire2Learn**.

    ![Configurar inicio de sesión único](./media/brightspace-desire2learn-tutorial/tutorial_brightspacebydesire2learn_app.png) 

1. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

1. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

1. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

1. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

1. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de Brightspace by Desire2Learn en el Panel de acceso, iniciará sesión automáticamente en la aplicación Brightspace by Desire2Learn.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/brightspace-desire2learn-tutorial/tutorial_general_01.png
[2]: ./media/brightspace-desire2learn-tutorial/tutorial_general_02.png
[3]: ./media/brightspace-desire2learn-tutorial/tutorial_general_03.png
[4]: ./media/brightspace-desire2learn-tutorial/tutorial_general_04.png

[100]: ./media/brightspace-desire2learn-tutorial/tutorial_general_100.png

[200]: ./media/brightspace-desire2learn-tutorial/tutorial_general_200.png
[201]: ./media/brightspace-desire2learn-tutorial/tutorial_general_201.png
[202]: ./media/brightspace-desire2learn-tutorial/tutorial_general_202.png
[203]: ./media/brightspace-desire2learn-tutorial/tutorial_general_203.png

