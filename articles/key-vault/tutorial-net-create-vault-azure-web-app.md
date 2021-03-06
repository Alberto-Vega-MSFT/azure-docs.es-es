---
title: 'Tutorial: Uso de Azure Key Vault con una aplicación web de Azure en .NET | Microsoft Docs'
description: 'Tutorial: Configuración de una aplicación ASP.NET Core para leer un secreto de Key Vault'
services: key-vault
documentationcenter: ''
author: prashanthyv
manager: rajvijan
ms.assetid: 0e57f5c7-6f5a-46b7-a18a-043da8ca0d83
ms.service: key-vault
ms.workload: identity
ms.topic: tutorial
ms.date: 09/05/2018
ms.author: pryerram
ms.custom: mvc
ms.openlocfilehash: 50a7f3166d677fe1af961866ccae4445a3d810b8
ms.sourcegitcommit: e37fa6e4eb6dbf8d60178c877d135a63ac449076
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53322148"
---
# <a name="tutorial-use-azure-key-vault-with-an-azure-web-app-in-net"></a>Tutorial: Uso de Azure Key Vault con una aplicación web de Azure en .NET

Azure Key Vault ayuda a proteger los secretos, como las claves de API y las cadenas de conexión de base de datos. Además, proporciona acceso a sus aplicaciones, servicios y recursos de TI.

En este tutorial, aprenderá a crear una aplicación web de Azure que puede leer información de un almacén de claves de Azure. El proceso usa Managed Identities for Azure Resources. Para más información sobre aplicaciones web de Azure, consulte [Azure Web Apps](../app-service/app-service-web-overview.md).

En este artículo se muestra cómo:

> [!div class="checklist"]
> * Cree un almacén de claves.
> * Almacenar un secreto en el almacén de claves.
> * Recuperar un secreto del almacén de claves.
> * Crear una aplicación web de Azure.
> * Habilite una [identidad administrada](../active-directory/managed-identities-azure-resources/overview.md) para la aplicación web.
> * Conceder los permisos necesarios para que la aplicación web lea datos del almacén de claves.
> * Ejecutar la aplicación web en Azure.

Antes de continuar, lea los [conceptos básicos de Key Vault](key-vault-whatis.md#basic-concepts).

## <a name="prerequisites"></a>Requisitos previos

* En Windows:
  * [SDK de .NET Core 2.1 o posterior](https://www.microsoft.com/net/download/windows)

* En Mac:
  * [Visual Studio para Mac](https://visualstudio.microsoft.com/vs/mac/)

* Todas las plataformas:
  * [Git](https://git-scm.com/downloads)
  * Una suscripción de Azure <br />(Si no tiene una suscripción a Azure, cree una [cuenta gratuita](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) antes de empezar).
  * Versión 2.0.4 o posterior de la [CLI de Azure](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest), disponible para Windows, Mac y Linux.
  * [.NET Core](https://www.microsoft.com/net/download/dotnet-core/2.1)

## <a name="managed-service-identity-and-how-it-works"></a>Managed Service Identity y cómo funciona

Azure Key Vault almacena las credenciales de forma segura, de modo que no están en el código. Sin embargo, debe autenticarse en Azure Key Vault para recuperar las claves. Para autenticarse en Key Vault, necesita una credencial. Se trata de un dilema de arranque clásico. Managed Service Identity (MSI) soluciona este problema al proporcionar una _identidad de arranque_ que simplifica el proceso.

Al habilitar MSI para un servicio de Azure (por ejemplo: Virtual Machines, App Service o Functions), Azure crea una [entidad de servicio](key-vault-whatis.md#basic-concepts). MSI hace esto para la instancia del servicio en Azure Active Directory (Azure AD) e inserta las credenciales de la entidad de servicio en esa instancia.

![Diagrama de MSI](media/MSI.png)

A continuación, el código llama a un servicio local de metadatos disponible en el recurso de Azure para obtener un token de acceso. El código usa el token de acceso que obtiene del MSI_ENDPOINT local para autenticar una instancia del servicio Azure Key Vault.

## <a name="sign-in-to-azure"></a>Inicio de sesión en Azure

Para iniciar sesión en Azure mediante la CLI de Azure, escriba:

```azurecli
az login
```

## <a name="create-a-resource-group"></a>Crear un grupo de recursos

Un grupo de recursos de Azure es un contenedor lógico en el que se implementan y se administran los recursos de Azure.

1. Cree un grupo de recursos con el comando [az group create](/cli/azure/group#az-group-create).
1. Seleccione el nombre de un grupo de recursos y rellene el marcador de posición. En el siguiente ejemplo se crea un grupo de recursos en la ubicación Oeste de EE. UU:

   ```azurecli
   # To list locations: az account list-locations --output table
   az group create --name "<YourResourceGroupName>" --location "West US"
   ```

Usará este grupo de recursos a lo largo de este tutorial.

## <a name="create-a-key-vault"></a>Creación de un Almacén de claves

Para crear un almacén de claves en el grupo de recursos, proporcione la siguiente información:

* Nombre del almacén de claves: una cadena de entre 3 y 24 caracteres que puede contener solo números, letras y guiones (por ejemplo: 0-9, a-z, A-z y -)
* Definición de un nombre de grupo de recursos
* Ubicación: **Oeste de EE. UU.**

En la CLI de Azure, escriba el siguiente comando:

```azurecli
az keyvault create --name "<YourKeyVaultName>" --resource-group "<YourResourceGroupName>" --location "West US"
```

En este momento, su cuenta de Azure es la única autorizada para realizar operaciones en este nuevo almacén.

## <a name="add-a-secret-to-the-key-vault"></a>Incorporación de un secreto al almacén de claves

Ahora puede agregar un secreto. Podría ser una cadena de conexión de SQL o cualquier otra información que necesite mantener segura y disponible para la aplicación.

Escriba los siguientes comandos para crear un secreto en el almacén de claves denominado **AppSecret**. Este secreto almacena el valor **MySecret**.

```azurecli
az keyvault secret set --vault-name "<YourKeyVaultName>" --name "AppSecret" --value "MySecret"
```

Para ver el valor contenido en el secreto como texto sin formato, escriba el siguiente comando:

```azurecli
az keyvault secret show --name "AppSecret" --vault-name "<YourKeyVaultName>"
```

Este comando muestra la información secreta, URI incluido. Después de completar estos pasos debe tener un identificador URI para un secreto en un almacén de claves. Tome nota de esta información. La necesitará en otro paso más adelante.

## <a name="create-a-net-core-web-app"></a>Creación de una aplicación web .NET Core

Siga este [tutorial](../app-service/app-service-web-get-started-dotnet.md) para crear una aplicación web .NET Core y **publicarla** en Azure. También puede ver el vídeo siguiente:

>[!VIDEO https://www.youtube.com/embed/EdiiEH7P-bU]

## <a name="open-and-edit-the-solution"></a>Apertura y edición de la solución

1. Vaya a **Páginas** >  archivo **About.cshtml.cs**.
2. Instale estos paquetes NuGet:
   - [AppAuthentication](https://www.nuget.org/packages/Microsoft.Azure.Services.AppAuthentication)
   - [KeyVault](https://www.nuget.org/packages/Microsoft.Azure.KeyVault)
3. Importe el código siguiente en el archivo About.cshtml.cs:

   ```csharp
    using Microsoft.Azure.KeyVault;
    using Microsoft.Azure.KeyVault.Models;
    using Microsoft.Azure.Services.AppAuthentication;
   ```

4. El código de la clase AboutModel debe parecerse al siguiente:

   ```csharp
    public class AboutModel : PageModel
    {
        public string Message { get; set; }

        public async Task OnGetAsync()
        {
            Message = "Your application description page.";
            int retries = 0;
            bool retry = false;
            try
            {
                /* The below 4 lines of code shows you how to use AppAuthentication library to fetch secrets from your Key Vault*/
                AzureServiceTokenProvider azureServiceTokenProvider = new AzureServiceTokenProvider();
                KeyVaultClient keyVaultClient = new KeyVaultClient(new KeyVaultClient.AuthenticationCallback(azureServiceTokenProvider.KeyVaultTokenCallback));
                var secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                Message = secret.Value;

                /* The below do while logic is to handle throttling errors thrown by Azure Key Vault. It shows how to do exponential backoff which is the recommended client side throttling*/
                do
                {
                    long waitTime = Math.Min(getWaitTime(retries), 2000000);
                    secret = await keyVaultClient.GetSecretAsync("https://<YourKeyVaultName>.vault.azure.net/secrets/AppSecret")
                        .ConfigureAwait(false);
                    retry = false;
                } 
                while(retry && (retries++ < 10));
            }
            /// <exception cref="KeyVaultErrorException">
            /// Thrown when the operation returned an invalid status code
            /// </exception>
            catch (KeyVaultErrorException keyVaultException)
            {
                Message = keyVaultException.Message;
                if((int)keyVaultException.Response.StatusCode == 429)
                    retry = true;
            }
        }

        // This method implements exponential backoff incase of 429 errors from Azure Key Vault
        private static long getWaitTime(int retryCount)
        {
            long waitTime = ((long)Math.Pow(2, retryCount) * 100L);
            return waitTime;
        }

        // This method fetches a token from Azure Active Directory which can then be provided to Azure Key Vault to authenticate
        public async Task<string> GetAccessTokenAsync()
        {
            var azureServiceTokenProvider = new AzureServiceTokenProvider();
            string accessToken = await azureServiceTokenProvider.GetAccessTokenAsync("https://vault.azure.net");
            return accessToken;
        }
    }
    ```

## <a name="run-the-app"></a>Ejecución de la aplicación

1. En el menú principal de Visual Studio 2017, seleccione **Depurar** > **Iniciar** con o sin depuración. 
1. Cuando aparezca el explorador, vaya a la página **Acerca de**.
1. Aparecerá el valor de **AppSecret**.

## <a name="enable-a-managed-identity-for-the-web-app"></a>Habilitación de una identidad administrada para la aplicación web

Azure Key Vault proporciona una manera de almacenar de forma segura credenciales y otros secretos, pero el código tiene que autenticarse en Key Vault para recuperarlos. La información del artículo [Acerca de Managed Identities for Azure Resources](../active-directory/managed-identities-azure-resources/overview.md) ayuda a resolver este problema al proporcionar a los servicios de Azure una identidad administrada automáticamente en Azure AD. Puede usar esta identidad para autenticar cualquier servicio que admita la autenticación de Azure AD, incluido Key Vault, sin necesidad de tener credenciales en el código.

1. En la CLI de Azure, ejecute el comando assign-identity para crear la identidad de esta aplicación:

   ```azurecli

   az webapp identity assign --name "<YourAppName>" --resource-group "<YourResourceGroupName>"

   ```

   >[!NOTE]
   >Tiene que reemplazar \<YourAppName\> por el nombre de la aplicación publicada en Azure. Por ejemplo, si el nombre de la aplicación publicada era **MyAwesomeapp.azurewebsites.net**, reemplace \<YourAppName\> por **MyAwesomeapp**.

1. Anote el valor de `PrincipalId` cuando publique la aplicación en Azure. La salida del comando en el paso 1 debe tener el formato siguiente:

   ```json
   {
     "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
     "type": "SystemAssigned"
   }
   ```

>[!NOTE]
>El comando de este procedimiento equivale a ir al [portal](https://portal.azure.com) y cambiar la configuración de **Identity /System assigned** a **On** en las propiedades de la aplicación web.

## <a name="assign-permissions-to-your-application-to-read-secrets-from-key-vault"></a>Asignación de permisos a una aplicación para leer secretos de Key Vault

Reemplace \<YourKeyVaultName\> por el nombre del almacén de claves y \<PrincipalId\> por el valor de **PrincipalId** en el siguiente comando:

```azurecli
az keyvault set-policy --name '<YourKeyVaultName>' --object-id <PrincipalId> --secret-permissions get list
```

Este comando proporciona a la identidad (MSI) de App Service permisos para realizar operaciones **get** y **list** en el almacén de claves.

## <a name="publish-the-web-application-to-azure"></a>Publicación de la aplicación web en Azure

Publique la aplicación web en Azure una vez más para ver que la aplicación web activa puede capturar el valor del secreto.

1. En Visual Studio, seleccione el proyecto **key-vault-dotnet-core-quickstart**.
2. Seleccione **Publicar** > **Iniciar**.
3. Seleccione **Crear**.

Cuando ejecute la aplicación, verá que puede recuperar el valor del secreto.

Ahora, ha creado una aplicación web en .NET que almacena sus secretos en Key Vault y los recupera de ahí.

## <a name="next-steps"></a>Pasos siguientes

>[!div class="nextstepaction"]
>[Guía del desarrollador de Azure Key Vault](https://docs.microsoft.com/en-us/azure/key-vault/key-vault-developers-guide)
