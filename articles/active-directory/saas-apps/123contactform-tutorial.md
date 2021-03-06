---
title: 'Tutorial: Integración de Azure Active Directory con 123ContactForm | Microsoft Docs'
description: Aprenda a configurar el inicio de sesión único entre Azure Active Directory y 123ContactForm.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.assetid: 5211910a-ab96-4709-959a-524c4d57c43e
ms.service: active-directory
ms.component: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 06/23/2017
ms.author: jeedes
ms.openlocfilehash: ecbe627697fc4f8b5fbfecf96c3cb65d9ffe4607
ms.sourcegitcommit: 7208bfe8878f83d5ec92e54e2f1222ffd41bf931
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 07/14/2018
ms.locfileid: "39054359"
---
# <a name="tutorial-azure-active-directory-integration-with-123contactform"></a>Tutorial: integración de Azure Active Directory con 123ContactForm

En este tutorial, obtendrá información sobre cómo integrar 123ContactForm con Azure Active Directory (Azure AD).

La integración de 123ContactForm con Azure AD proporciona las siguientes ventajas:

- Puede controlar en Azure AD quién tiene acceso a 123ContactForm.
- Puede permitir que los usuarios inicien sesión automáticamente en 123ContactForm (inicio de sesión único) con sus cuentas de Azure AD.
- Puede administrar sus cuentas en una ubicación central: el nuevo Azure Portal.

Si desea saber más sobre la integración de aplicaciones SaaS con Azure AD, consulte [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md).

## <a name="prerequisites"></a>Requisitos previos

Para configurar la integración de Azure AD con 123ContactForm, necesita los siguientes elementos:

- Una suscripción de Azure AD
- Una suscripción habilitada para el inicio de sesión único en 123ContactForm

> [!NOTE]
> Para probar los pasos de este tutorial, no se recomienda el uso de un entorno de producción.

Para probar los pasos de este tutorial, debe seguir estas recomendaciones:

- No use el entorno de producción, salvo que sea necesario.
- Si no dispone de un entorno de prueba de Azure AD, puede obtener una versión de prueba de un mes [aquí](https://azure.microsoft.com/pricing/free-trial/).

## <a name="scenario-description"></a>Descripción del escenario
En este tutorial, puede probar el inicio de sesión único de Azure AD en un entorno de prueba. El escenario descrito en este tutorial consta de dos bloques de creación principales:

1. Agregar 123ContactForm desde la galería
2. Configuración y comprobación del inicio de sesión único de Azure AD

## <a name="adding-123contactform-from-the-gallery"></a>Agregar 123ContactForm desde la galería
Para configurar la integración de 123ContactForm en Azure AD, será preciso que agregue 123ContactForm desde la galería a la lista de aplicaciones SaaS administradas.

**Para agregar 123ContactForm desde la galería, realice los pasos siguientes:**

1. En el panel de navegación izquierdo de **[Azure Portal](https://portal.azure.com)**, haga clic en el icono de **Azure Active Directory**. 

    ![Active Directory][1]

2. Vaya a **Aplicaciones empresariales**. A continuación, vaya a **Todas las aplicaciones**.

    ![APLICACIONES][2]
    
3. Para agregar una nueva aplicación, haga clic en el botón **Nueva aplicación** de la parte superior del cuadro de diálogo.

    ![APLICACIONES][3]

4. En el cuadro de búsqueda, escriba **123ContactForm**.

    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/tutorial_123contactform_search.png)

5. En el panel de resultados, seleccione **123ContactForm** y haga clic en el botón **Agregar** para agregar la aplicación.

    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/tutorial_123contactform_addfromgallery.png)

##  <a name="configuring-and-testing-azure-ad-single-sign-on"></a>Configuración y comprobación del inicio de sesión único de Azure AD
En esta sección, podrá configurar y probar el inicio de sesión único de Azure AD con 123ContactForm con un usuario de prueba llamado "Britta Simon".

Para que el inicio de sesión único funcione, Azure AD debe saber cuál es el usuario homólogo de 123ContactForm para un usuario de Azure AD. Es decir, es necesario establecer una relación de vínculo entre un usuario de Azure AD y el usuario relacionado de 123ContactForm.

Para establecer la relación de vínculo, en 123ContactForm, asigne el valor de **nombre de usuario** de Azure AD como valor de **Nombre de usuario**.

Para configurar y probar el inicio de sesión único de Azure AD con 123ContactForm, es preciso completar los siguientes bloques de creación:

1. **[Configuración del inicio de sesión único de Azure AD](#configuring-azure-ad-single-sign-on)** : para permitir a los usuarios usar esta característica.
2. **[Creación de un usuario de prueba de Azure AD](#creating-an-azure-ad-test-user)** : para probar el inicio de sesión único de Azure AD con Britta Simon.
3. **[Creación de un usuario de prueba de 123ContactForm](#creating-a-123contactform-test-user)**: para tener un homólogo de Britta Simon en 123ContactForm que esté vinculado a la representación de Azure AD de usuario.
4. **[Asignación del usuario de prueba de Azure AD](#assigning-the-azure-ad-test-user)** : para permitir que Britta Simon use el inicio de sesión único de Azure AD.
5. **[Testing Single Sign-On](#testing-single-sign-on)** : para comprobar si funciona la configuración.

### <a name="configuring-azure-ad-single-sign-on"></a>Configuración del inicio de sesión único de Azure AD

En esta sección, habilitará el inicio de sesión único de Azure AD en Azure Portal y configurará el inicio de sesión único en la aplicación 123ContactForm.

**Para configurar el inicio de sesión único de Azure AD con 123ContactForm, realice los pasos siguientes:**

1. En Azure Portal, en la página de integración de la aplicación **123ContactForm**, haga clic en **Inicio de sesión único**.

    ![Configurar inicio de sesión único][4]

2. En el cuadro de diálogo **Inicio de sesión único**, en **Modo** seleccione **Inicio de sesión basado en SAML** para habilitar el inicio de sesión único.
 
    ![Configurar inicio de sesión único](./media/123contactform-tutorial/tutorial_123contactform_samlbase.png)

3. En la sección **Dominio y direcciones URL de 123ContactForm**, si quiere configurar la aplicación en el **modo iniciado por IDP**, realice los siguientes pasos:

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/url1.png)

    a. En el cuadro de texto **Identificador**, escriba una dirección URL con el siguiente patrón: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/metadata`

    b. En el cuadro de texto **URL de respuesta**, escriba una dirección URL con el siguiente patrón: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/acs`.

4. Si quiere configurar la aplicación en **modo iniciado por SP**, realice los siguientes pasos:

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/url2.png)

    a. Haga clic en la opción **Mostrar configuración avanzada de URL**.

    b. En el cuadro de texto **URL de inicio de sesión**, escriba una dirección URL como: `https://www.123contactform.com/saml/azure_ad/<tenant_id>/sso`

    > [!NOTE] 
    > Estos valores no son reales. Debe actualizar este valor con las direcciones URL y el identificador reales, como se explica más adelante en el tutorial.
    
5. En la sección **Certificado de firma de SAML**, haga clic en **XML de metadatos** y luego guarde el archivo de metadatos en el equipo.

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/tutorial_123contactform_certificate.png) 

6. Haga clic en el botón **Guardar** .

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/tutorial_general_400.png)

7. Para configurar el inicio de sesión único en **123ContactForm**, vaya a [https://www.123contactform.com/form-2709121/](https://www.123contactform.com/form-2709121/) y siga estos pasos:

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/submit.png) 

    a. En el cuadro de texto **Correo electrónico**, escriba el correo electrónico del usuario, es decir, **BrittaSimon@Contoso.com**.

    b. Haga clic en **Cargar** y vaya al archivo XML de metadatos que ha descargado de Azure Portal.

    c. Haga clic en **ENVIAR FORMULARIO**.

8. En **Microsoft Azure AD - Inicio de sesión único - Configurar las opciones de la aplicación**, realice los pasos siguientes:
    
    ![Configurar inicio de sesión único](./media/123contactform-tutorial/url3.png)

    a. Si quiere configurar la aplicación en **modo iniciado por IDP**, copie el valor de **IDENTIFICADOR** de la instancia y péguelo en el cuadro de texto **Identificador** en la sección **Dominio y direcciones URL de 123ContactForm** en Azure Portal.
    
    b. Si quiere configurar la aplicación en **modo iniciado por IDP**, copie el valor de **URL DE RESPUESTA** de la instancia y péguelo en el cuadro de texto **URL de respuesta** en la sección **Dominio y direcciones URL de 123ContactForm** en Azure Portal.

    c. Si quiere configurar la aplicación en **modo iniciado por SP**, copie el valor de **URL de inicio de sesión** de la instancia y péguelo en el cuadro de texto **URL de inicio de sesión** en la sección **Dominio y direcciones URL de 123ContactForm** en Azure Portal.

> [!TIP]
> Ahora puede leer una versión resumida de estas instrucciones dentro de [Azure Portal](https://portal.azure.com) mientras configura la aplicación.  Después de agregar esta aplicación desde la sección **Active Directory > Aplicaciones empresariales**, simplemente haga clic en la pestaña **Inicio de sesión único** y acceda a la documentación insertada a través de la sección **Configuración** de la parte inferior. Puede leer más sobre la característica de documentación insertada aquí: [Vista previa: Administración de inicio de sesión único para aplicaciones empresariales en el nuevo Azure Portal]( https://go.microsoft.com/fwlink/?linkid=845985)
> 

### <a name="creating-an-azure-ad-test-user"></a>Creación de un usuario de prueba de Azure AD
El objetivo de esta sección es crear un usuario de prueba en Azure Portal llamado "Britta Simon".

![Creación de un usuario de Azure AD][100]

**Siga estos pasos para crear un usuario de prueba en Azure AD:**

1. En el panel de navegación izquierdo de **Azure Portal**, haga clic en el icono de **Azure Active Directory**.

    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/create_aaduser_01.png) 

2. Para mostrar la lista de usuarios, vaya a **Usuarios y grupos** y haga clic en **Todos los usuarios**.
    
    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/create_aaduser_02.png) 

3. Para abrir el cuadro de diálogo **Usuario**, haga clic en **Agregar** en la parte superior del cuadro de diálogo.
 
    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/create_aaduser_03.png) 

4. En la página de diálogo **Usuario**, realice los siguientes pasos:
 
    ![Creación de un usuario de prueba de Azure AD](./media/123contactform-tutorial/create_aaduser_04.png) 

    a. En el cuadro de texto **Nombre**, escriba **BrittaSimon**.

    b. En el cuadro de texto **Nombre de usuario**, escriba la **dirección de correo electrónico** de Britta Simon.

    c. Seleccione **Mostrar contraseña** y anote el valor del cuadro **Contraseña**.

    d. Haga clic en **Create**(Crear).
 
### <a name="creating-a-123contactform-test-user"></a>Crear un usuario de prueba de 123ContactForm

La aplicación admite aprovisionamiento de usuarios Just-In-Time y, tras la autenticación, los usuarios se crearán automáticamente en la aplicación.

### <a name="assigning-the-azure-ad-test-user"></a>Asignación del usuario de prueba de Azure AD

En esta sección, habilitará a Britta Simon para que use el inicio de sesión único de Azure concediéndole acceso a 123ContactForm.

![Asignar usuario][200] 

**Para asignar el usuario Britta Simon a 123ContactForm, realice los pasos siguientes:**

1. En Azure Portal, abra la vista de aplicaciones, navegue a la vista de directorio y vaya a **Aplicaciones empresariales**. Luego haga clic en **Todas las aplicaciones**.

    ![Asignar usuario][201] 

2. En la lista de aplicaciones, seleccione **123ContactForm**.

    ![Configurar inicio de sesión único](./media/123contactform-tutorial/tutorial_123contactform_app.png) 

3. En el menú de la izquierda, haga clic en **Usuarios y grupos**.

    ![Asignar usuario][202] 

4. Haga clic en el botón **Agregar**. Después, seleccione **Usuarios y grupos** en el cuadro de diálogo **Agregar asignación**.

    ![Asignar usuario][203]

5. En el cuadro de diálogo **Usuarios y grupos**, seleccione **Britta Simon** en la lista de usuarios.

6. Haga clic en el botón **Seleccionar** del cuadro de diálogo **Usuarios y grupos**.

7. Haga clic en el botón **Asignar** del cuadro de diálogo **Agregar asignación**.
    
### <a name="testing-single-sign-on"></a>Prueba del inicio de sesión único 

En esta sección, probará la configuración de inicio de sesión único de Azure AD mediante el Panel de acceso.

Al hacer clic en el icono de 123ContactForm en el Panel de acceso, debería iniciar sesión automáticamente en la aplicación 123ContactForm.
Para más información sobre el Panel de acceso, consulte [Introducción al Panel de acceso](../user-help/active-directory-saas-access-panel-introduction.md).

## <a name="additional-resources"></a>Recursos adicionales

* [Lista de tutoriales sobre cómo integrar aplicaciones SaaS con Azure Active Directory](tutorial-list.md)
* [¿Qué es el acceso a aplicaciones y el inicio de sesión único con Azure Active Directory?](../manage-apps/what-is-single-sign-on.md)



<!--Image references-->

[1]: ./media/123contactform-tutorial/tutorial_general_01.png
[2]: ./media/123contactform-tutorial/tutorial_general_02.png
[3]: ./media/123contactform-tutorial/tutorial_general_03.png
[4]: ./media/123contactform-tutorial/tutorial_general_04.png

[100]: ./media/123contactform-tutorial/tutorial_general_100.png

[200]: ./media/123contactform-tutorial/tutorial_general_200.png
[201]: ./media/123contactform-tutorial/tutorial_general_201.png
[202]: ./media/123contactform-tutorial/tutorial_general_202.png
[203]: ./media/123contactform-tutorial/tutorial_general_203.png

