---
title: Administración de amenazas en los recursos y los datos en Azure Active Directory B2C | Microsoft Docs
description: Obtenga información sobre las técnicas de detección y mitigación de los ataques por denegación de servicio y los ataques a contraseñas en Azure Active Directory B2C.
services: active-directory-b2c
author: davidmu1
manager: mtillman
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 11/01/2018
ms.author: davidmu
ms.component: B2C
ms.openlocfilehash: ee99e7346d438e81a0cd25f8c522838524732568
ms.sourcegitcommit: 799a4da85cf0fec54403688e88a934e6ad149001
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/02/2018
ms.locfileid: "50912856"
---
# <a name="manage-threats-to-resources-and-data-in-azure-active-directory-b2c"></a>Administración de amenazas en los recursos y los datos en Azure Active Directory B2C

Azure Active Directory (Azure AD) B2C tiene características integradas que pueden ayudarle a protegerse frente a amenazas en los recursos y los datos. Estas amenazas incluyen los ataques de denegación de servicio y contraseña. Los ataques por denegación de servicio pueden hacer que los recursos no estén disponibles para los usuarios previstos. Los ataques a contraseñas pueden dar lugar a un acceso no autorizado a los recursos. 

## <a name="denial-of-service-attacks"></a>Ataques por denegación de servicio

Azure AD B2C protege frente a ataques de inundación SYN mediante una cookie SYN. Azure AD B2C también protege frente a ataques de denegación de servicio mediante límites en las tasas y conexiones.

## <a name="password-attacks"></a>Ataques a contraseñas

Las contraseñas establecidas por los usuarios deben tener una complejidad razonable. Azure AD B2C también dispone de técnicas de mitigación para los ataques a contraseñas. La mitigación incluye ataques a contraseñas por fuerza bruta y ataques de diccionario. Mediante el uso de diversas señales, Azure AD B2C analiza la integridad de las solicitudes. Azure AD B2C está diseñado para diferenciar de forma inteligente los usuarios previstos frente a los hackers y las redes de robots (botnets). 

Azure AD B2C usa una sofisticada estrategia para bloquear las cuentas. Las cuentas se bloquean en función de la dirección IP de la solicitud y las contraseñas escritas. El tiempo que dura el bloqueo también aumenta en función de la probabilidad de que sea un ataque. Una vez que se ha intentado una contraseña sin éxito diez veces, se produce un bloqueo de un minuto. La próxima vez que un inicio de sesión sea incorrecto tras desbloquearse la cuenta, se produce otro bloqueo de un minuto y continúa durante cada inicio de sesión incorrecto. Escribir repetidamente la misma contraseña no cuenta como varios inicios de sesión incorrectos. 

Los diez primeros períodos de bloqueo son de un minuto. Los diez períodos de bloqueo siguientes son ligeramente más largos y la duración aumenta después de cada diez períodos de bloqueo. El contador de bloqueos se restablece en cero tras un inicio de sesión correcto, cuando la cuenta no está bloqueada. Los períodos de bloqueo pueden durar hasta cinco horas. 

Actualmente, no se puede:

- Desencadenar un bloqueo con menos de diez inicios de sesión erróneos
- Recuperar una lista de cuentas bloqueadas
- Configurar la directiva de bloqueo

Para obtener más información, visite el [Centro de confianza de Microsoft](https://www.microsoft.com/trustcenter/default.aspx).
