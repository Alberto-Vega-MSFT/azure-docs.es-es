---
title: ClaimsProviders - Azure Active Directory B2C | Microsoft Docs
description: Especifique el elemento ClaimsProvider de una directiva personalizada en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: reference
ms.date: 09/10/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: b226aeac86084c56a49d00c2a2f8deddaf7b0f68
ms.sourcegitcommit: 7b0778a1488e8fd70ee57e55bde783a69521c912
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 10/10/2018
ms.locfileid: "49068781"
---
# <a name="claimsproviders"></a>ClaimsProviders 

[!INCLUDE [active-directory-b2c-advanced-audience-warning](../../includes/active-directory-b2c-advanced-audience-warning.md)]

Los proveedores de notificaciones tienen un conjunto de [perfiles técnicos](technicalprofiles.md). Cada proveedor de notificaciones necesita tener uno o más perfiles técnicos que determinen los puntos de conexión y protocolos necesarios para establecer comunicación con el proveedor de notificaciones. Un proveedor de notificaciones puede tener varios perfiles técnicos. Por ejemplo, se pueden definir varios perfiles técnicos, ya que el proveedor de notificaciones admite varios protocolos y varios puntos de conexión con distintas funcionalidades o libera diversas notificaciones en diferentes niveles de seguridad. Puede ser aceptable liberar notificaciones confidenciales en un recorrido del usuario, pero no en otro.

```XML
<ClaimsProviders>
  <ClaimsProvider>
    <Domain>Domain name</Domain>
    <DisplayName>Display name</DisplayName>
    <TechnicalProfiles>
      </TechnicalProfile>
        ...
      </TechnicalProfile>
        ...
    </TechnicalProfiles>
  </ClaimsProvider>
</ClaimsProvider>
  ...
</ClaimsProviders>
```

El elemento **ClaimsProviders** contiene el elemento siguiente:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ----------- | ----------- |
| ClaimsProvider | 1:n | Proveedor de notificaciones acreditado que se puede utilizar en varios recorridos de usuario. |

## <a name="claimsprovider"></a>ClaimsProvider

El elemento **ClaimsProvider** contiene los siguientes elementos secundarios:

| Elemento | Repeticiones | DESCRIPCIÓN |
| ------- | ---------- | ----------- |
| Dominio | 0:1 | Cadena que contiene el nombre de dominio del proveedor de notificaciones. Por ejemplo, si el proveedor de notificaciones incluye el perfil técnico de Facebook, el nombre de dominio será Facebook.com. Este nombre de dominio se utilizará con todos los perfiles técnicos definidos en el proveedor de notificaciones, a menos que se reemplace por el perfil técnico. También se puede hacer referencia al nombre de dominio en **domain_hint**. Para más información, consulte la sección **Redirección del inicio de sesión a un proveedor social** de [Configuración de inicio de sesión directo con Azure Active Directory B2C](direct-signin.md). |
| DisplayName | 0:1 | Cadena que contiene el nombre del proveedor de notificaciones que se muestra a los usuarios. |
| [TechnicalProfiles](technicalprofiles.md) | 0:1 | Conjunto de perfiles técnicos admitidos por el proveedor de notificaciones. |

**ClaimsProvider** organiza los perfiles técnicos relacionados con el proveedor de notificaciones. En el ejemplo siguiente, se muestra el proveedor de notificaciones de Azure Active Directory con los perfiles técnicos de Azure Active Directory: 

```XML
<ClaimsProvider>
  <DisplayName>Azure Active Directory</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="AAD-Common">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingAlternativeSecurityId-NoError">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingEmailAddress">
      ...
    </TechnicalProfile>
      ...
    <TechnicalProfile Id="AAD-UserWritePasswordUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWriteProfileUsingObjectId">
      ...    
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserReadUsingObjectId">
      ...
    </TechnicalProfile>
    <TechnicalProfile Id="AAD-UserWritePhoneNumberUsingObjectId">
      ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```

En el ejemplo siguiente, se muestra el proveedor de notificaciones de Facebook con el perfil técnico **Facebook OAUTH**.

```XML
<ClaimsProvider>
  <Domain>facebook.com</Domain>
  <DisplayName>Facebook</DisplayName>
  <TechnicalProfiles>
    <TechnicalProfile Id="Facebook-OAUTH">
      <DisplayName>Facebook</DisplayName>
      <Protocol Name="OAuth2" />
        ...
    </TechnicalProfile>
  </TechnicalProfiles>
</ClaimsProvider>
```
 
