---
title: 'Requisitos de suscripción para usar PIM: Azure | Microsoft Docs'
description: Describe los requisitos de suscripción y licencia necesarios para usar Azure AD Privileged Identity Management (PIM).
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
editor: markwahl-msft
ms.assetid: 34367721-8b42-4fab-a443-a2e55cdbf33d
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.component: pim
ms.date: 06/01/2017
ms.author: rolyon
ms.custom: pim
ms.openlocfilehash: 1554895dcba0c09a3a2e19c284a1cd6f0416cfe1
ms.sourcegitcommit: 63613e4c7edf1b1875a2974a29ab2a8ce5d90e3b
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 08/29/2018
ms.locfileid: "43190617"
---
# <a name="subscription-requirements-to-use-pim"></a>Requisitos de la suscripción para usar PIM

Azure AD Privileged Identity Management está disponible como parte de la edición Premium P2 de Azure AD. Para más información sobre las demás características de P2 y compararlo con Premium P1, consulte [Ediciones de Azure Active Directory](../active-directory-editions.md).

>[!NOTE]
Cuando Azure Active Directory (Azure AD) Privileged Identity Management estaba en versión preliminar, no había ninguna comprobación de licencia para un inquilino para probar el servicio.  Ahora que Azure AD Privileged Identity Management ha pasado a disponibilidad general, debe haber una suscripción de prueba o de pago para el inquilino para poder seguir usando Privileged Identity Management después de diciembre de 2016.
  

## <a name="confirm-your-trial-or-paid-subscription"></a>Confirmación de su suscripción de prueba o de pago

Si no está seguro de si su organización tiene una suscripción de prueba o de pago, puede comprobar si hay una suscripción en su inquilino mediante el uso de los comandos incluidos en el módulo Azure Active Directory para Windows PowerShell V1. 
1. Abra una ventana de PowerShell.
2. Escriba `Connect-MsolService` para autenticarse como un usuario en el inquilino.
3. Escriba `Get-MsolSubscription | ft SkuPartNumber,IsTrial,Status`.

Este comando recupera una lista de las suscripciones en el inquilino. Si no se devuelven líneas, tendrá que obtener una prueba de Azure AD Premium P2 o comprar una suscripción de Azure AD Premium P2 o de EMS E5 para usar Azure AD Privileged Identity Management.  Para obtener una prueba y comenzar a usar Azure AD Privileged Identity Management, lea [Introducción a Azure AD Privileged Identity Management](pim-getting-started.md).

Si este comando devuelve una línea en la que SkuPartNumber es "AAD_PREMIUM_P2" o "EMSPREMIUM" e IsTrial es "True", significa que hay una prueba de Azure AD Premium P2 en el inquilino.  Si el estado de la suscripción no está habilitado y no tiene una compra de suscripción de Azure AD Premium P2 o EMS E5, debe adquirir una suscripción de Azure AD Premium P2 o EMS E5 para seguir usando Azure AD Privileged Identity Management.

Azure AD Premium P2 se encuentra disponible a través del [Contrato Microsoft Enterprise](https://www.microsoft.com/en-us/licensing/licensing-programs/enterprise.aspx), el programa de [licencias por volumen abiertas](https://www.microsoft.com/en-us/licensing/licensing-programs/open-license.aspx) y el [programa de proveedores de soluciones en la nube](https://partner.microsoft.com/cloud-solution-provider). Los suscriptores de Azure y Office 365 también pueden comprar Azure AD Premium P2 en línea.  Encontrará más información sobre precios de Azure AD Premium y cómo realizar la solicitud en línea en [Precios de Azure Active Directory](https://azure.microsoft.com/pricing/details/active-directory/).

## <a name="azure-ad-privileged-identity-management-is-not-available-in-tenant"></a>Azure AD Privileged Identity Management no está disponible en el inquilino

Azure AD Privileged Identity Management ya no estará disponible en el inquilino si:
- Su organización estaba usando Azure AD Privileged Identity Management cuando estaba en versión preliminar y no compra la suscripción de Azure AD Premium P2 o EMS E5.
- Su organización tenía una prueba de Azure AD Premium P2 o EMS E5 que expiró.
- Su organización tenía una suscripción comprada que expiró.

Cuando una suscripción de Azure AD Premium P2 o EMS E5 expira, o una organización que estaba usando Azure AD Privileged Identity Management en versión preliminar no obtiene una suscripción de Azure AD Premium P2 o EMS E5:

- Las asignaciones de roles permanentes para roles de Azure AD no se verán afectadas.
- La extensión de Azure AD Privileged Identity Management en Azure Portal, así como los cmdlets de Graph API y las interfaces de PowerShell de Azure AD Privileged Identity Management, ya no estarán disponibles para que los usuarios activen roles con privilegios, administren el acceso con privilegios o realicen revisiones de acceso de roles con privilegios.
- Se quitarán las asignaciones de roles elegibles de roles de Azure AD, puesto que los usuarios ya no podrán activar roles con privilegios.
- Las revisiones de acceso en curso de roles de Azure AD finalizarán y las opciones de configuración de Azure AD Privileged Identity Management se quitarán.
- Azure AD Privileged Identity Management no volverá a enviar correos electrónicos cuando se produzcan cambios de asignación de rol.

## <a name="next-steps"></a>Pasos siguientes

- [Primer uso de PIM](pim-getting-started.md)
- [Roles de directorio de Azure AD que se pueden administrar en PIM](pim-roles.md)
