---
title: (EN DESUSO) Supervisión de un clúster de Azure Container Service con Sysdig
description: Supervise un clúster del servicio de contenedores de Azure con Sysdig.
services: container-service
author: sauryadas
manager: jeconnoc
ms.service: container-service
ms.topic: article
ms.date: 08/08/2016
ms.author: saudas
ms.custom: mvc
ms.openlocfilehash: 3b6781a6b87f9f11b341cf3cf2bcf6668b640c2b
ms.sourcegitcommit: 2469b30e00cbb25efd98e696b7dbf51253767a05
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/06/2018
ms.locfileid: "52992198"
---
# <a name="deprecated-monitor-an-azure-container-service-cluster-with-sysdig"></a>(EN DESUSO) Supervisión de un clúster de Azure Container Service con Sysdig

[!INCLUDE [ACS deprecation](../../../includes/container-service-deprecation.md)]

En este artículo, implementaremos agentes Sysdig en todos los nodos de agente del clúster del servicio de contenedores de Azure. Necesita una cuenta con Sysdig para esta configuración. 

## <a name="prerequisites"></a>Requisitos previos
[Implemente](container-service-deployment.md) y [conecte](../container-service-connect.md) un clúster configurado por Azure Container Service. Explore la [interfaz de usuario de Marathon](container-service-mesos-marathon-ui.md). Vaya a [http://app.sysdigcloud.com](http://app.sysdigcloud.com) para configurar una cuenta de Sysdig en la nube. 

## <a name="sysdig"></a>Sysdig
Sysdig es un servicio de supervisión que le permite supervisar sus contenedores dentro del clúster. Sysdig es conocido por servir para solucionar problemas, pero además incluye métricas de supervisión básicas para la CPU, la red, la memoria y la E/S. Con Sysdig resulta más fácil ver qué contenedores hacen más trabajo o esencialmente cuáles utilizan más memoria y CPU. Esta vista se encuentra en la sección "Overview" (Información general), actualmente en versión beta. 

![Interfaz de usuario de Sysdig](./media/container-service-monitoring-sysdig/sysdig6.png) 

## <a name="configure-a-sysdig-deployment-with-marathon"></a>Configuración de una implementación de Sysdig con Marathon
Estos pasos muestran cómo configurar e implementar aplicaciones de Sysdig en su clúster con Marathon. 

Acceda a la interfaz de usuario de DC/OS mediante [http://localhost:80/](http://localhost:80/). Una vez en la interfaz de usuario de DC/OS, vaya a "Universe" (Universo) en la parte inferior izquierda y busque "Sysdig".

![Sysdig en Universe en DC/OS](./media/container-service-monitoring-sysdig/sysdig1.png)

Ahora, para completar la configuración, necesita una cuenta en la nube de Sysdig o una cuenta de evaluación gratuita. Una vez que haya iniciado sesión en el sitio web de la nube de Sysdig, haga clic en su nombre de usuario; debería aparecer en la página la clave de acceso (Access Key). 

![Clave de API de Sysdig](./media/container-service-monitoring-sysdig/sysdig2.png) 

Después, escriba la clave de acceso en la configuración de Sysdig dentro de Universe (Universo) en DC/OS. 

![Configuración de Sysdig en Universe en DC/OS](./media/container-service-monitoring-sysdig/sysdig3.png)

Ahora establezca las instancias en 10000000 para que, siempre que se agregue un nuevo nodo al clúster de Sysdig, se implemente automáticamente un agente en ese nodo nuevo. Se trata de una solución provisional para asegurarse de que Sysdig se implemente en todos los agentes nuevos dentro del clúster. 

![Configuración de Sysdig en instancias de Universe en DC/OS](./media/container-service-monitoring-sysdig/sysdig4.png)

Una vez que haya instalado el paquete, vuelva a la interfaz de usuario de Sysdig y podrá explorar las diferentes métricas de uso para los contenedores dentro del clúster. 

También puede paneles específicos de Mesos y Marathon a través del [nuevo Asistente de panel](https://app.sysdigcloud.com/#/dashboards/new).
