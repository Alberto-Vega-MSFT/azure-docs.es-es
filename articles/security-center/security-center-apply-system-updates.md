---
title: Aplicación de actualizaciones del sistema en Azure Security Center | Microsoft Docs
description: En este documento se muestra cómo implementar las recomendaciones **Aplicar actualizaciones del sistema** y **Reiniciar tras actualizar el sistema** de Azure Security Center.
services: security-center
documentationcenter: na
author: rkarlin
manager: MBaldwin
editor: ''
ms.assetid: e5bd7f55-38fd-4ebb-84ab-32bd60e9fa7a
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 10/28/2018
ms.author: rkarlin
ms.openlocfilehash: f3c055d55da06e6410520084a1c96baa78b91496
ms.sourcegitcommit: edacc2024b78d9c7450aaf7c50095807acf25fb6
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/13/2018
ms.locfileid: "53341719"
---
# <a name="apply-system-updates-in-azure-security-center"></a>Aplicar actualizaciones del sistema en Azure Security Center
Azure Security Center supervisa diariamente los equipos y máquinas virtuales de Windows y Linux por si faltan actualizaciones de sistema operativo. Security Center recupera una lista de actualizaciones críticas y de seguridad disponibles desde Windows Update o Windows Server Update Services (WSUS), dependiendo de qué servicio está configurado en un equipo con Windows. Security Center comprueba también las últimas actualizaciones de los sistemas Linux. Si falta una actualización del sistema en la máquina virtual o en el equipo, Security Center le recomendará que aplique las actualizaciones del sistema.

## <a name="implement-the-recommendation"></a>Implementación de la recomendación
La aplicación de actualizaciones del sistema se presenta como una recomendación en Security Center. Si falta una actualización del sistema en la máquina virtual o equipo, se mostrará esta recomendación en **Recomendaciones** y en **Proceso**.  Al seleccionar la recomendación se abre el panel **Aplicar actualizaciones del sistema**.

En este ejemplo usaremos **Proceso**.

1. Seleccione **Proceso** en el menú principal de Security Center.

   ![Seleccionar Proceso][1]

2. En **Proceso**, seleccione **Faltan las actualizaciones del sistema**. Se abrirá el panel **Aplicar actualizaciones del sistema**.

   ![Panel Aplicar actualizaciones del sistema][2]

   En la parte superior del panel se muestra lo siguiente:

    - El número total de equipos y máquinas virtuales con Windows y Linux en los que faltan actualizaciones del sistema.
    - El número total de actualizaciones críticas que faltan en todas las máquinas virtuales y equipos.
    - El número total de actualizaciones de seguridad que faltan en todas las máquinas virtuales y equipos.

  En la parte inferior del panel se muestran todas las actualizaciones que faltan en todas las máquinas virtuales y equipos, así como la gravedad de la actualización que falta.  La lista incluye:

    - NOMBRE: nombre de la actualización que falta.
    - NÚMERO DE EQUIPOS Y VM: número total de VM y equipos que no tienen esta actualización.
    - ESTADO: el estado actual de la recomendación:

      - Abierta: la recomendación aún no se ha abordado.
      - En curso: la recomendación se está aplicando actualmente a esos recursos y no se requiere ninguna acción por su parte.
      - Resuelta: la recomendación ya terminó. (Si se ha resuelto el problema, la entrada aparecerá atenuada).

    - GRAVEDAD: describe la gravedad de una recomendación concreta:

      - Alta: existe una vulnerabilidad en un recurso importante (aplicación, máquina virtual o grupo de seguridad de red) y requiere atención.
      - Media: para completar un proceso o eliminar una vulnerabilidad se requieren pasos adicionales o no críticos.
      - Baja: es preciso abordar una vulnerabilidad, pero esta no requiere una atención inmediata. (De manera predeterminada no se muestran las recomendaciones bajas, pero si desea verlas, puede filtrar por ellas).

3. En la lista, seleccione una actualización que falta para ver los detalles.

   ![Actualización de seguridad que falta][3]

4. Seleccione el icono **Buscar** de la cinta de opciones superior.  Se abrirá una consulta de búsqueda de Log Analytics filtrada por los equipos a los que les falta la actualización.

   ![Búsqueda de Log Analytics][4]

5. Seleccione un equipo de la lista para más información. Se abre otro resultado de la búsqueda con la información filtrada solo para ese equipo.

    ![Búsqueda de Log Analytics][5]

## <a name="reboot-after-system-updates"></a>Reiniciar tras actualizar el sistema
1. Vuelva a la hoja **Recomendaciones** . Se genera una nueva entrada después de aplicar las actualizaciones del sistema, denominada **Reiniciar tras actualizar el sistema**. Esta entrada permite saber que tiene que reiniciar la máquina virtual para completar el proceso de aplicación de las actualizaciones del sistema.

   ![Reiniciar tras actualizar el sistema][6]
2. Seleccione **Reiniciar tras actualizar el sistema**. Se abre la hoja **Hay un reinicio pendiente para completar las actualizaciones del sistema** que muestra una lista de máquinas virtuales que deben reiniciarse para completar el proceso de aplicación de actualizaciones del sistema.

   ![Reinicio pendiente][7]

Reinicie la máquina virtual de Azure para completar el proceso.

## <a name="next-steps"></a>Pasos siguientes
Para más información sobre el Centro de seguridad, consulte los siguientes recursos:

* [Establecimiento de directivas de seguridad en Azure Security Center](tutorial-security-policy.md): aprenda a configurar directivas de seguridad para las suscripciones y los grupos de recursos de Azure.
* [Administración de recomendaciones de seguridad en Azure Security Center](security-center-recommendations.md): recomendaciones que le ayudan a proteger los recursos de Azure.
* [Supervisión del estado de seguridad en Azure Security Center](security-center-monitoring.md): obtenga información sobre cómo supervisar el mantenimiento de los recursos de Azure.
* [Administración y respuesta a las alertas de seguridad en Azure Security Center](security-center-managing-and-responding-alerts.md): obtenga información sobre cómo administrar y responder a alertas de seguridad.
* [Supervisión de las soluciones de asociados con Azure Security Center](security-center-partner-solutions.md): aprenda a supervisar el estado de mantenimiento de las soluciones de asociados.
* [Preguntas más frecuentes sobre Azure Security Center](security-center-faq.md) : busque las preguntas más frecuentes sobre cómo usar el servicio.
* [Blog de seguridad de Azure](https://blogs.msdn.com/b/azuresecurity/) : encuentre publicaciones de blog sobre el cumplimiento y la seguridad de Azure.

<!--Image references-->
[1]: ./media/security-center-apply-system-updates/missing-system-updates.png
[2]:./media/security-center-apply-system-updates/apply-system-updates.png
[3]: ./media/security-center-apply-system-updates/detail-on-missing-update.png
[4]: ./media/security-center-apply-system-updates/log-search.png
[5]: ./media/security-center-apply-system-updates/search-details.png
[6]: ./media/security-center-apply-system-updates/reboot-after-system-updates.png
[7]: ./media/security-center-apply-system-updates/restart-pending.png
