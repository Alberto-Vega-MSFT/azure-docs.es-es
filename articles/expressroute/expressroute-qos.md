---
title: 'Requisitos de QoS: ExpressRoute: Azure | Microsoft Docs'
description: En esta página se proporcionan requisitos detallados para configurar y administrar QoS. Se analizan Skype Empresarial y los servicios de voz.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: cherylmc
ms.custom: seodec18
ms.openlocfilehash: e478475ea9b5a34315c9d46c2201263566398008
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53336669"
---
# <a name="expressroute-qos-requirements"></a>Requisitos de QoS ExpressRoute
Skype Empresarial tiene varias cargas de trabajo que requieren tratamiento diferenciado de QoS. Si piensa consumir servicios de voz a través de ExpressRoute, debe cumplir los requisitos descritos a continuación.

![](./media/expressroute-qos/expressroute-qos.png)

> [!NOTE]
> Los requisitos de QoS solo se aplican al emparejamiento de Microsoft. Los valores de DSCP en el tráfico de red recibido en el emparejamiento público y privado de Azure se restablecerán en 0. 
> 
> 

En la tabla siguiente se proporciona una lista de marcados de DSCP usados por Skype Empresarial. Consulte [Administración de QoS para Skype Empresarial](https://technet.microsoft.com/library/gg405409.aspx) para obtener más información.

| **Clase de tráfico** | **Tratamiento (marcado de DSCP)** | **Cargas de trabajo de Skype Empresarial** |
| --- | --- | --- |
| **Voz** |EF (46) |Voz de Skype o Lync |
| **Interactivo** |AF41 (34) |Video, VBSS |
| |AF21 (18) |Uso compartido de aplicaciones | 
| **Valor predeterminado** |AF11 (10) |Transferencia de archivos |
| |CS0 (0) |Nada más |

* Debe clasificar las cargas de trabajo y marcar los valores de DSCP correctos. Siga las instrucciones proporcionadas [aquí](https://docs.microsoft.com/SkypeForBusiness/manage/network-management/qos/configuring-port-ranges-for-your-skype-clients#configure-quality-of-service-policies-for-clients-running-on-windows-10) sobre cómo establecer marcados de DSCP en la red.
* Debe configurar y admitir varias colas de QoS dentro de la red. La voz debe ser una clase independiente y recibir el tratamiento EF especificado en [RFC 3246](https://www.ietf.org/rfc/rfc3246.txt). 
* Puede decidir el mecanismo de puesta en cola, la directiva de detección de congestión y la asignación de ancho de banda por clase de tráfico. Sin embargo, se debe preservar el marcado de DSCP para cargas de trabajo de Skype Empresarial. Si utiliza marcados de DSCP no enumerados anteriormente, por ejemplo, AF31 (26), debe reescribir este valor de DSCP a 0 antes de enviar el paquete a Microsoft. Microsoft solo envía paquetes marcados con el valor de DSCP que se muestra en la tabla anterior. 

## <a name="next-steps"></a>Pasos siguientes
* Consulte los requisitos de [enrutamiento](expressroute-routing.md) y [NAT](expressroute-nat.md).
* Consulte los vínculos siguientes para configurar la conexión ExpressRoute.
  
  * [Creación de un circuito ExpressRoute](expressroute-howto-circuit-classic.md)
  * [Configuración del enrutamiento](expressroute-howto-routing-classic.md)
  * [Vinculación de redes virtuales a circuitos ExpressRoute](expressroute-howto-linkvnet-classic.md)

