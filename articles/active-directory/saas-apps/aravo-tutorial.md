---
title: 'Tutorial: Integración de Azure Active Directory con Aravo | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y Aravo.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 224939d8-2c9c-4561-968d-62722f5ab5ed
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/09/2017
ms.author: jeedes
ms.openlocfilehash: 6631df4764e16fd2399b231732df3fb1ee475869
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52842652"
---
# <a name="tutorial-azure-active-directory-integration-with-aravo"></a>Tutorial: Integración de Azure Active Directory con Aravo

En este tutorial, obtendrá información sobre cómo integrar Aravo con Azure Active Directory (Azure AD).

Integrar Aravo con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a Aravo.
- Puede permitir que los usuarios inicien sesión automáticamente en Aravo (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con Aravo, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para inicio de sesión único en Aravo

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Incorporación de Aravo desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-aravo-from-the-gallery"></a>Incorporación de Aravo desde la galería
Para configurar la integración de Aravo en Azure AD, deberá agregar Aravo desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar Aravo desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el cuadro de búsqueda, escriba **Aravo**.

    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/tutorial_aravo_search.png)

5. En el panel de resultados, seleccione **Aravo** y luego haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/tutorial_aravo_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con Aravo con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de Aravo para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de Aravo.

Para establecer la relación de vínculo, en Aravo, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con Aravo, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de Aravo](#creating-an-aravo-test-user)**: para tener un homólogo de Britta Simon en Aravo que esté vinculado a la representación de ella en Azure AD.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y lo configurará en la aplicación Aravo.

**Para configurar el inicio de sesión único de Azure AD con Aravo, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **Aravo** , haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_aravo_samlbase.png)

3. En la sección **Dominio y direcciones URL de Aravo**, lleve a cabo los pasos siguientes:

    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_aravo_url.png)

     a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.aravo.com`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://<companyname>.aravo.com/aems/login.do`.

    > [!NOTE] 
    > Estos valores no son reales. Actualice estos valores con el identificador y la URL de respuesta reales. Póngase en contacto con el [equipo de soporte técnico de Aravo](https://www.aravo.com/about-us/contact/) para obtener estos valores.
 
4. En la sección **Certificado de firma de SAML**, haga clic en **Certificado (Base64)** y, luego, guarde el archivo de certificado en el equipo.

    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_aravo_certificate.png) 

5. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_general_400.png)

6. En la sección **Configuración de Aravo**, haga clic en **Configurar Aravo** para abrir la ventana **Configurar inicio de sesión**. Copie la **URL del servicio de inicio de sesión único de SAML, el identificador de entidad de SAML y la dirección URL de cierre de sesión** de la sección **Referencia rápida**.

    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_aravo_configure.png) 

7. Para configurar el inicio de sesión único en **Aravo**, es preciso enviar los valores descargados de **Certificado (Base64)**, **Sign-Out URL (Dirección URL de cierre de sesión), SAML Entity ID (Identificador de entidad de SAML) y SAML Single Sign-On Service URL (Dirección URL del servicio de inicio de sesión único de SAML)** al [equipo de soporte técnico de Aravo](https://www.aravo.com/about-us/contact/). 


> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más aquí sobre la característica de documentación insertada: [Documentación insertada de Azure AD]( https://go.microsoft.com/fwlink/?linkid=845985)

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/create_aaduser_01.png) 

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/create_aaduser_02.png) 

3. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/create_aaduser_03.png) 

4. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/aravo-tutorial/create_aaduser_04.png) 

     a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-an-aravo-test-user"></a>Creación de un usuario de prueba de Aravo

El objetivo de esta sección es crear un usuario de prueba llamado Britta Simon en Aravo. Trabaje con el [equipo de soporte técnico de Aravo](https://www.aravo.com/about-us/contact/) para agregar usuarios a la cuenta de Aravo.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a Aravo.

![Asignar usuario][200] 

**Para asignar Britta Simon a Aravo, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **Aravo**.

    ![Configurar inicio de sesión único](./media/aravo-tutorial/tutorial_aravo_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

El objetivo de esta sección es probar la configuración del inicio de sesión único de Microsoft Azure AD mediante el panel de acceso.

Al hacer clic en el icono de Aravo en el panel de acceso, debería iniciar sesión automáticamente en su aplicación Aravo.

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)

<!--Image references-->

[1]: ./media/aravo-tutorial/tutorial_general_01.png
[2]: ./media/aravo-tutorial/tutorial_general_02.png
[3]: ./media/aravo-tutorial/tutorial_general_03.png
[4]: ./media/aravo-tutorial/tutorial_general_04.png

[100]: ./media/aravo-tutorial/tutorial_general_100.png

[200]: ./media/aravo-tutorial/tutorial_general_200.png
[201]: ./media/aravo-tutorial/tutorial_general_201.png
[202]: ./media/aravo-tutorial/tutorial_general_202.png
[203]: ./media/aravo-tutorial/tutorial_general_203.png

