---
title: Habilitación de la replicación de máquinas virtuales de VMware para la recuperación ante desastres de VMware en Azure con Azure Site Recovery| Microsoft Docs'
description: En este artículo, se describe cómo se habilita la replicación de máquinas virtuales de VMware para la recuperación ante desastres en Azure utilizando Azure Site Recovery.
author: asgang
ms.service: site-recovery
ms.date: 11/27/2018
ms.topic: conceptual
ms.author: asgang
ms.openlocfilehash: 51470e9f8e0bffe18d1dc4007433246d084a5cb2
ms.sourcegitcommit: 11d8ce8cd720a1ec6ca130e118489c6459e04114
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/04/2018
ms.locfileid: "52846673"
---
# <a name="enable-replication-to-azure-for-vmware-vms"></a>Habilitación de máquinas virtuales de VMware en Azure


En este artículo se describe cómo habilitar la replicación de máquinas virtuales locales de VMware en Azure.

## <a name="prerequisites"></a>Requisitos previos

En este artículo se supone que ha:

1.  [Configurado el entorno de origen local](vmware-azure-set-up-source.md).
2.  [Configurado el entorno de destino en Azure](vmware-azure-set-up-target.md).


## <a name="before-you-start"></a>Antes de comenzar
Al replicar máquinas virtuales de VMware:

* Su cuenta de usuario de Azure debe tener ciertos [permisos](site-recovery-role-based-linked-access-control.md#permissions-required-to-enable-replication-for-new-virtual-machines) para habilitar la replicación de una nueva máquina virtual en Azure.
* Las máquinas virtuales de VMware se detectan cada 15 minutos. Es posible que tarden 15 minutos o más en aparecer en Azure Portal tras la detección. Del mismo modo, la detección podría tardar 15 minutos o más si agrega un servidor vCenter o host vSphere nuevos.
* Los cambios de entorno en la máquina virtual (como la instalación de herramientas de VMware) pueden tardar 15 minutos o más en actualizarse en el portal.
* Puede consultar la hora de la detección más reciente de las máquinas virtuales de VMware en el campo **Último contacto a las** correspondiente al servidor vCenter/host de vSphere en la página **Servidores de configuración**.
* Para agregar máquinas para la replicación sin esperar a la detección programada, resalte el servidor de configuración (no haga clic en él) y haga clic en el botón **Actualizar**.
* Al habilitar la replicación, si la máquina está preparada, el servidor de procesos instala automáticamente Mobility Service en ella.


## <a name="enable-replication"></a>Habilitar replicación

1. Haga clic en **Paso 2: Replicar la aplicación** > **Origen**. Después de habilitar la replicación por primera vez, haga clic en **+Replicar** en el almacén para habilitar la replicación de más máquinas.
2. En la página **Origen** > **Origen**, seleccione el servidor de configuración.
3. En **Tipo de máquina**, seleccione **Máquinas virtuales** o **Máquinas físicas**.
4. En **vCenter/vSphere Hypervisor**, seleccione el servidor vCenter que administra el host de vSphere, o bien seleccione el host. Esta configuración es relevante si va a replicar máquinas físicas.
5. Seleccione el servidor de procesos, que será el nombre del servidor de configuración si no ha creado ningún servidor de proceso adicional. A continuación, haga clic en **Aceptar**.

    ![Habilitación del origen de replicación](./media/vmware-azure-enable-replication/enable-replication2.png)

6. En **Destino**, seleccione la suscripción y el grupo de recursos donde desee crear las máquinas virtuales conmutadas por error. Elija el modelo de implementación que desee usar en Azure para las máquinas virtuales conmutadas por error.

7. Seleccione la cuenta de Azure Storage que desea usar para los datos de replicación. 

    > [!NOTE]

    >   * Puede seleccionar una cuenta de almacenamiento Estándar o Premium. Si selecciona una cuenta Premium, debe especificar una cuenta de almacenamiento Estándar adicional para los registros de replicación en curso. Las cuentas deben estar en la misma región que el almacén de Recovery Services.
    >   * Si desea utilizar una cuenta de almacenamiento diferente de las que tiene, puede [crear una](../storage/common/storage-create-storage-account.md). Para crear una cuenta de almacenamiento mediante Resource Manager, haga clic en **Crear nueva**. 

8. Seleccione la red y la subred de Azure a la que se conectarán las máquinas virtuales de Azure cuando se pongan en marcha después de la conmutación por error. La red virtual de Azure debe estar en la misma región que el almacén de Recovery Services. Seleccione la opción **Configurar ahora para las máquinas seleccionadas** con el fin de aplicar la configuración de red a todas las máquinas que seleccione para su protección. Seleccione **Configurar más tarde** para seleccionar la red de Azure por máquina. Si no dispone de una red, debe [crear una](#set-up-an-azure-network). Para crear una red mediante Resource Manager, haga clic en **Crear nueva**. Seleccione una subred si es posible y después haga clic en **Aceptar**.

    ![Habilitar replicación - Configuración de destino](./media/vmware-azure-enable-replication/enable-rep3.png)
9. En **Máquinas virtuales** > **Seleccionar máquinas virtuales**, seleccione cada máquina que desea replicar. Solo puede seleccionar aquellas máquinas en las que se pueda habilitar la replicación. A continuación, haga clic en **Aceptar**.

    ![Habilitar replicación - Seleccionar máquinas virtuales](./media/vmware-azure-enable-replication/enable-replication5.png)
10. En **Propiedades** > **Configurar propiedades**, seleccione la cuenta que ha usado el servidor de procesos para instalar automáticamente el servicio de movilidad en la máquina.  
11. De manera predeterminada se replican todos los discos. Para excluir discos de la replicación, haga clic en **Todos los discos** y desactive los discos que no quiera replicar.  A continuación, haga clic en **Aceptar**. Puede establecer propiedades adicionales más adelante. [Obtenga más información](vmware-azure-exclude-disk.md) sobre cómo excluir discos.

    ![Habilitar replicación - Configurar propiedades](./media/vmware-azure-enable-replication/enable-replication6.png)

12. En **Configuración de la replicación** > **Establecer configuración de replicación**, compruebe que se haya seleccionado la directiva de replicación correcta. Puede modificar la configuración de la directiva de replicación en **Configuración** > **Directivas de replicación** > (nombre de directiva) > **Editar configuración**. Los cambios aplicados a una directiva también se aplican también a las máquinas nuevas y replicadas.
13. Habilite la **coherencia de múltiples máquinas virtuales** si desea recopilar las máquinas en un grupo de replicación. Especifique un nombre para el grupo y, después, haga clic en **Aceptar**. 

    > [!NOTE]

    >    * Todas las máquinas de un grupo de replicación se replican al mismo tiempo y comparten puntos de recuperación coherentes con los bloqueos y coherentes con la aplicación cuando conmutan por error.
    >    * Recopile las máquinas virtuales y los servidores físicos juntos para que reflejen las cargas de trabajo. La habilitación de la coherencia entre varias VM puede afectar al rendimiento de la carga de trabajo. Úselo únicamente si las máquinas ejecutan la misma carga de trabajo y necesita coherencia.

    ![Habilitar replicación](./media/vmware-azure-enable-replication/enable-replication7.png)
14. Haga clic en **Enable Replication**. Puede hacer un seguimiento del progreso del trabajo **Habilitar protección** en **Configuración** > **Trabajos** > **Trabajos de Site Recovery**. La máquina estará preparada para la conmutación por error después de que finalice el trabajo **Finalizar la protección**.



## <a name="view-and-manage-vm-properties"></a>Visualización y administración de las propiedades de la máquina virtual

Después, compruebe las propiedades de la máquina de origen. Recuerde que el nombre de la máquina virtual de Azure debe cumplir los [requisitos para las máquinas virtuales de Azure](vmware-physical-azure-support-matrix.md#replicated-machines).

1. Haga clic en **Configuración** > **Elementos replicados** > y después seleccione la máquina. En la página **Información esencial** se detalla la configuración y el estado de las máquinas.
2. En **Propiedades** puede ver la información de replicación y conmutación por error de la máquina virtual.
3. En **Compute y Network** > **Propiedades de Compute**, puede especificar el nombre y el tamaño de destino de la máquina virtual de Azure. Modifique el nombre para que cumpla con los requisitos de Azure si es necesario.

    ![Propiedades de proceso y red](./media/vmware-azure-enable-replication/vmproperties.png)

4.  Puede seleccionar un [grupo de recursos](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-resource-groups-guidelines) del cual la máquina formará parte de la conmutación por error posterior. Puede cambiar esta configuración en cualquier momento antes de una conmutación por error. Después de la conmutación por error, si se migra la máquina a otro grupo de recursos, la configuración de protección para dicha máquina se interrumpirá.
5. Puede seleccionar un [conjunto de disponibilidad](https://docs.microsoft.com/azure/virtual-machines/windows/infrastructure-availability-sets-guidelines) si su máquina debe formar parte de una conmutación por error posterior. Al seleccionar un conjunto de disponibilidad, tenga en cuenta lo siguiente:

    * Solo se mostrarán los conjuntos de disponibilidad que pertenecen al grupo de recursos especificado.  
    * Las máquinas con distintas redes virtuales no pueden formar parte del mismo conjunto de disponibilidad.
    * Solo las máquinas virtuales del mismo tamaño pueden formar parte de un conjunto de disponibilidad.
5. También puede ver y agregar la información sobre la red, la subred y la dirección IP de destino que se asigna a la máquina virtual de Azure.
6. En **Discos** puede ver los discos de datos y del sistema operativo en la máquina virtual que se va a replicar.

### <a name="configure-networks-and-ip-addresses"></a>Configuración de redes y direcciones IP

- Puede establecer la dirección IP de destino. Si no proporciona ninguna dirección, la máquina conmutada por error usará DHCP. Si establece una dirección que no esté disponible en el momento de la conmutación por error, esta operación no funcionará. Si la dirección está disponible en la red de conmutación por error, se puede utilizar la misma dirección IP de destino para la conmutación por error de prueba.
- El número de adaptadores de red viene determinado por el tamaño que especifique para la máquina virtual de destino, de la siguiente manera:
    - Si el número de adaptadores de red en el equipo de origen es menor o igual al número de adaptadores permitido para el tamaño de la máquina de destino, el destino tendrá el mismo número de adaptadores que el origen.
    - Si el número de adaptadores para la máquina virtual de origen supera el número permitido para el tamaño de destino, entonces se utilizará el tamaño máximo de destino.
    Por ejemplo, si una máquina de origen tiene dos adaptadores de red y el tamaño de la máquina de destino admite cuatro, el equipo de destino tiene dos adaptadores. Si la máquina de origen tiene dos adaptadores pero el tamaño de destino compatible solo admite uno, la máquina de destino tiene solo un adaptador.
    - Si la máquina virtual tiene varios adaptadores de red, todos ellos se conectan a la misma red. Además, el primero de ellos que se muestre en la lista se convertirá en el adaptador de red *predeterminado* en la máquina virtual de Azure.

### <a name="azure-hybrid-benefit"></a>Ventaja híbrida de Azure

Los clientes de Microsoft Software Assurance pueden usar la Ventaja híbrida de Azure para ahorrar en los costos de licencia de máquinas Windows Server que se migran a Azure o usar Azure para la recuperación ante desastres. Si está autorizado a usar la Ventaja híbrida de Azure, puede especificar que la máquina virtual asignada a este beneficio sea la que crea Azure Site Recovery en caso de una conmutación por error. Para ello, siga estos pasos:
- Vaya a la sección de propiedades de Compute y Network de la máquina virtual replicada.
- Responda a la pregunta sobre si tiene una licencia de Windows Server que le permita ser apto para la Ventaja híbrida de Azure.
- Active la casilla para confirmar que tiene una licencia de Windows Server con Software Assurance que puede usar para aplicar la Ventaja híbrida de Azure en la máquina que se creará en la conmutación por error.
- Guarde la configuración de la máquina replicada.

Más información sobre la [Ventaja híbrida de Azure](https://aka.ms/azure-hybrid-benefit-pricing).

## <a name="common-issues"></a>Problemas comunes

* Cada disco debe tener menos de 1 TB de tamaño.
* El disco del sistema operativo debe ser un disco básico y no uno dinámico.
* Para las máquinas virtuales de generación 2 o habilitadas para UEFI, la familia del sistema operativo debe ser Windows y el disco de arranque debe ser menor de 300 GB.

## <a name="next-steps"></a>Pasos siguientes

Una vez finalizada la protección y que la máquina ha alcanzado un estado protegido, puede intentar una [conmutación por error](site-recovery-failover.md) para comprobar si su aplicación aparece en Azure o no.

Si desea deshabilitar la protección, consulte cómo [borrar el registro y la configuración de la protección](site-recovery-manage-registration-and-protection.md).
