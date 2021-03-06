---
title: Preguntas frecuentes sobre Avere vFXT for Azure
description: Preguntas frecuentes sobre Avere vFXT for Azure
author: ekpgh
ms.service: avere-vfxt
ms.topic: conceptual
ms.date: 10/31/2018
ms.author: v-erkell
ms.openlocfilehash: 21b5dae6fda857a3a69aa06e461fdf3b716b5eb8
ms.sourcegitcommit: ebf2f2fab4441c3065559201faf8b0a81d575743
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 11/20/2018
ms.locfileid: "52163324"
---
# <a name="avere-vfxt-for-azure-faq"></a>Preguntas frecuentes acerca de Avere vFXT for Azure

En este artículo encontrará respuestas que pueden ayudarle a decidir si la solución de Avere vFXT es adecuada para sus necesidades. Proporciona información básica sobre las funcionalidades de Avere vFXT y explica cómo funciona con otros componentes de Azure y con productos de proveedores externos. 

## <a name="general"></a>General 

### <a name="what-is-avere-vfxt-for-azure"></a>¿Qué es Avere vFXT for Azure?

Avere vFXT for Azure es un sistema de archivos de alto rendimiento que almacena en caché datos activos en Azure Compute para un procesamiento eficaz de cargas de trabajo críticas.

### <a name="is-the-avere-vfxt-a-storage-solution"></a>¿Es Avere vFXT una solución de almacenamiento?

 No. Avere vFXT es una **caché** de sistema de archivos que se adjunta a los entornos de almacenamiento, como NAS de EMC o NetApp o un contenedor de blobs. vFXT optimiza las solicitudes de datos de los clientes y almacena en caché los datos que sirve para mejorar el rendimiento a escala y a lo largo del tiempo. vFXT en sí no almacena datos. No tiene información sobre la cantidad de datos que se almacenan tras de sí.

### <a name="is-the-avere-vfxt-a-tiering-solution"></a>¿Es Avere vFXT una solución de niveles?

Avere vFXT no apila automáticamente datos entre los niveles de acceso frecuente y esporádico.  

### <a name="how-do-i-know-if-an-environment-is-right-for-the-avere-vfxt"></a>¿Cómo se puede saber si un entorno es adecuado para Avere vFXT?

La mejor manera de pensar acerca de esta pregunta es: "¿Puede almacenarse en caché la carga de trabajo?". Es decir, ¿tiene la carga de trabajo un alto índice de lectura a escritura, por ejemplo, 80/20 o 70/30 lecturas/escrituras?

Tenga en cuenta Avere vFXT for Azure si tiene una canalización de análisis basada en archivos que se ejecuta en un gran número de máquinas virtuales de Azure y cumple una o varias de las condiciones siguientes:

* El rendimiento general es lento o incoherente debido a largos tiempos de acceso a archivos (decenas de milisegundos o segundos, dependiendo de los requisitos). Esta latencia es inaceptable para el cliente final.

* Los datos necesarios para el procesamiento se encuentran en el extremo de un entorno WAN y mover esos datos de forma permanente no resulta práctico. Los datos podrían estar en otra región de Azure o en un centro de datos del cliente.

* Un número significativo de clientes solicita los datos: por ejemplo, en un clúster de informática de alto rendimiento (HPC). El gran número de solicitudes simultáneas puede aumentar la latencia.

* El cliente quiere ejecutar la canalización actual "tal cual" en máquinas virtuales de Azure y necesita una solución de almacenamiento compartido (o almacenamiento en caché) basada en POSIX para la escalabilidad. Al usar Avere vFXT for Azure, no es necesario rediseñar la canalización de trabajo para hacer llamadas nativas al almacenamiento de Azure Blob.

* La aplicación HPC se basa en clientes NFSv3. (En algunas circunstancias, se pueden usar clientes SMB 2.1, pero el rendimiento es limitado).

   El gráfico siguiente intenta simplificar la respuesta a esta pregunta. Cuanto más se acerque el flujo de trabajo a la esquina superior derecha, es más probable que la solución de almacenamiento en caché de Avere sea adecuada para su entorno.

   ![diagrama que muestra que las cargas con mucha actividad de lectura y miles de clientes son más adecuadas para Avere vFXT](media/avere-vfxt-fit-assessment.png)

### <a name="at-what-scale-of-clients-does-the-avere-vfxt-solution-make-the-most-sense"></a>¿Con qué escala de clientes tiene más sentido la solución Avere vFXT?

La solución de caché vFXT está diseñada para controlar cientos, miles o decenas de miles de núcleos de proceso. Si tiene algunas máquinas con poco trabajo, Avere vFXT no es la solución adecuada.

Los clientes habituales de Avere vFXT ejecutan cargas de trabajo exigentes, a partir de aproximadamente 1000 núcleos de CPU. Estos entornos pueden llegar a 50 000 núcleos o más. Dado que vFXT es escalable, puede agregar nodos para admitir estas cargas de trabajo a medida que crecen para requerir más rendimiento o más IOPS.

### <a name="how-much-data-can-an-avere-vfxt-environment-store"></a>¿Cuántos datos puede almacenar un entorno de Avere vFXT?

Avere vFXT es una memoria caché, no almacena datos en concreto. Usa una combinación de RAM y SSD para almacenar los datos en caché. Los datos se almacenan de forma permanente en un sistema de almacenamiento back-end (por ejemplo, un sistema NAS de NetApp o un contenedor de blobs). El sistema de Avere vFXT no tiene información sobre la cantidad de datos almacenados detrás de él; vFXT solo almacena en caché el subconjunto de los datos que los clientes solicitan.  

### <a name="what-regions-are-supported"></a>¿Qué regiones se admiten?

A partir del 1 de noviembre de 2018, Avere vFXT for Azure se admite en todas las regiones, excepto para regiones soberana (China, Alemania) y las regiones de gobierno. Asegúrese de que la región que quiere usar sea compatible con la gran cantidad de núcleos de proceso, así como con las instancias de VM necesarias para crear el clúster de Avere vFXT. Aún no se admiten regiones soberanas y nubes de gobierno.

### <a name="how-do-i-get-help-with-the-avere-vfxt"></a>¿Cómo puedo obtener ayuda con Avere vFXT?

Un grupo de soporte técnico especializado ofrece ayuda con Avere vFXT for Azure. Siga las instrucciones de [Ayuda con el sistema](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt) para abrir una incidencia de soporte técnico desde Azure Portal. 

### <a name="is-the-avere-vfxt-highly-available"></a>¿Tiene Avere vFXT Avere alta disponibilidad?

Sí, Avere vFXT se ejecuta exclusivamente como solución de alta disponibilidad.

### <a name="does-avere-vfxt-for-azure-also-support-other-cloud-services"></a>¿Admite Avere vFXT for Azure también otros servicios en la nube?

Sí, los clientes pueden usar más de un proveedor de nube con el clúster de Avere vFXT. Es compatible con cubos estándar de AWS S3 y cubos estándar de Google Cloud Services, así como contenedores de Azure Blob. 

> [!NOTE] 
> Se aplica una tarifa de software para usar Avere vFXT en AWS o Google Cloud, pero no con Azure.

## <a name="technical---compute"></a>Preguntas técnicas: proceso

### <a name="can-you-describe-what-an-avere-vfxt-environment-looks-like"></a>¿Cómo puede describirse el "aspecto" de un entorno de Avere vFXT?

Avere vFXT es una aplicación en clúster conformada por varias máquinas virtuales de Azure. Una biblioteca de Python controla la creación, eliminación y modificación de clústeres. Lea [¿Qué es Avere vFXT for Azure?](avere-vfxt-overview.md) para más información. 

### <a name="what-kind-of-azure-virtual-machines-does-the-avere-vfxt-run-on"></a>¿En qué tipo de máquinas virtuales de Azure se ejecuta Avere vFXT?  

El clúster de Avere vFXT for Azure usa máquinas virtuales Microsoft Azure E32s_v3 o D16s_v3. 

### <a name="can-i-mix-and-match-virtual-machine-types-for-my-cluster"></a>¿Puedo mezclar y asociar tipos de máquina virtual para el clúster?

No, debe elegir un tipo de máquina virtual u otro.
    
### <a name="can-i-move-between-virtual-machine-types"></a>¿Puedo cambiar entre tipos de máquina virtual?

Sí, hay una ruta de migración para moverse de un tipo de VM a otro. Para averiguar cómo, [abra una incidencia de soporte técnico](avere-vfxt-open-ticket.md#open-a-support-ticket-for-your-avere-vfxt).

### <a name="does-the-avere-vfxt-environment-scale"></a>¿Puede escalarse el entorno de Avere vFXT?

El clúster de Avere vFXT puede ser tan pequeña como tres nodos de máquina virtual o tan grandes como 24 nodos. Póngase en contacto con soporte técnico de Azure para obtener ayuda de planeación si cree que necesita un clúster de más de nueve nodos. Cuantos más nodos tenga, mayor será la arquitectura de implementación.

### <a name="does-the-avere-vfxt-environment-autoscale"></a>¿Puede "escalarse automáticamente" el entorno de Avere vFXT?

 No. Puede escalar o reducir verticalmente el tamaño del clúster, pero agregar o quitar nodos de clúster es un paso manual.

### <a name="can-i-run-the-vfxt-cluster-as-a-virtual-machine-scale-set"></a>¿Puedo ejecutar el clúster de vFXT como conjunto de escalado de máquinas virtuales?

Avere vFXT no admite la implementación de conjuntos de escalado de máquinas virtuales(VMSS). Se han diseñado varios mecanismos de soporte técnico de disponibilidad integrada únicamente para VM atómicas que participan en un clúster.  

### <a name="can-i-run-the-vfxt-cluster-on-low-priority-vms"></a>¿Puedo ejecutar el clúster de vFXT en VM de prioridad baja?

No, el sistema requiere un conjunto estable subyacente de máquinas virtuales.

### <a name="can-i-run-the-vfxt-cluster-in-containers"></a>¿Puedo ejecutar el clúster de vFXT en contenedores?

No, Avere vFXT debe implementarse como aplicación independiente.

### <a name="do-the-avere-vfxt-vms-count-against-my-compute-quota"></a>¿Cuentas las VM de Avere vFXT para la cuota de proceso?

Sí. Asegúrese de tener cuota suficiente en la región para admitir el clúster.  

### <a name="can-i-run-the-avere-vfxt-cluster-machines-in-different-availability-zones"></a>¿Puedo ejecutar las máquinas del clúster de Avere vFXT en zonas de disponibilidad diferentes?

 No. El modelo de alta disponibilidad que se usa en Avere vFXT actualmente no es compatible con los miembros individuales del clúster de vFXT ubicados en zonas de disponibilidad diferentes.

### <a name="can-i-clone-avere-vfxt-virtual-machines"></a>¿Puedo clonarse las máquinas virtuales de Avere vFXT?

No, debe usar el script de Python compatible para agregar o quitar nodos del clúster de Avere vFXT. Para más información, lea [Manage the Avere vFXT cluster](avere-vfxt-manage-cluster.md) (Administración del clúster de Avere vFXT).  

### <a name="is-there-a-vm-version-of-the-software-i-can-run-in-my-own-local-environment"></a>¿Existe una versión de "VM" del software que puedo ejecutar en mi propio entorno local?

No, el sistema se ofrece como dispositivo en clúster y se ha probado en tipos de máquina virtual específicos. Esta restricción ayuda a los clientes a evitar la creación de un sistema que no sea compatible con los requisitos de alto rendimiento de un flujo de trabajo típico de Avere vFXT. 

## <a name="technical---disks"></a>Preguntas técnicas: discos

### <a name="what-type-of-disks-are-supported-for-the-azure-vms"></a>¿Qué tipo de discos son compatibles con las VM de Azure?

Avere vFXT for Azure puede usar configuraciones de SSD Premium de 1 TB o 4 TB. La configuración de SSD Premium se puede implementar como varios discos administrados.

### <a name="does-the-cluster-support-unmanaged-disks"></a>¿Admite el clúster discos no administrados?

No, el clúster requiere discos administrados.

### <a name="does-the-system-support-local-attached-ssds"></a>¿Admite el sistema SSD locales (conectados)?

Avere vFXT for Azure no admite actualmente SSD locales. Los discos usados para Avere vFXT deben poder apagarse y reiniciarse, pero los SSD locales conectados en esta configuración solo se pueden finalizar.

### <a name="does-the-system-support-ultra-ssds"></a>¿Admite el sistema SSD ultra?

No, el sistema admite configuraciones de SSD Premium únicamente.

### <a name="can-i-detach-my-premium-ssds-and-reattach-them-later-to-preserve-cache-contents-between-use"></a>¿Puedo desasociar los SSD Premium y volver a conectarlos más adelante para conservar el contenido en caché entre un uso y otro?

No se admite desasociar y reconectar SSD. Los metadatos o el contenido de los archivos en el origen podrían haber cambiado entre los usos, lo que podrían provocar problemas de integridad de los datos.

### <a name="does-the-system-encrypt-the-cache"></a>¿El sistema cifra la memoria caché?

Los datos se reparten entre los discos, pero no se cifran. Sin embargo, puede cifrar los discos en sí. Puede encontrar más información [aquí](https://docs.microsoft.com/azure/virtual-machines/linux/security-policy#encryption).

## <a name="technical---networking"></a>Preguntas técnicas: redes

### <a name="what-network-is-recommended"></a>¿Qué red se recomienda?

Si se usa almacenamiento local con Avere vFXT, debe tener una conexión de red o de 1 Gbps o mejor. Si tiene una pequeña cantidad de datos y está dispuesto a copiar datos en la nube antes de ejecutar los trabajos, puede ser suficiente la conectividad VPN. 

> [!TIP] 
> Cuanto más lento sea el vínculo de red, más lentas serán las lecturas en frío iniciales. Las lecturas lentas aumentan la latencia de la canalización de trabajos. 

### <a name="can-i-run-the-avere-vfxt-in-a-different-virtual-network-than-my-compute-cluster"></a>¿Puedo ejecutar Avere vFXT en una red virtual diferente de mi clúster de proceso?

Sí, puede crear el sistema de Avere vFXT en una red virtual diferente. Lea [Planeamiento de un sistema de Avere vFXT](avere-vfxt-deploy-plan.md) para más detalles.

### <a name="does-the-avere-vfxt-require-its-own-subnet"></a>¿Requiere Avere vFXT su propia subred?

Sí. Avere vFXT se ejecuta estrictamente como clúster de alta disponibilidad y requiere varias direcciones IP para funcionar. Si el clúster está en su propia subred, se evita el riesgo de conflictos de direcciones IP, lo que puede causar problemas de instalación y de normal funcionamiento. La subred del clúster puede estar dentro de la red virtual existente, siempre y cuando no se superponga ninguna dirección IP.

### <a name="can-i-run-the-avere-vfxt-on-infiniband"></a>¿Puedo ejecutar Avere vFXT en Infiniband?

No, Avere vFXT usa solo Ethernet/IP.

### <a name="how-do-i-access-my-on-premises-nas-environment-from-the-avere-vfxt"></a>¿Cómo accedo a mi entorno de NAS local desde Avere vFXT?

El entorno de Avere vFXT es similar a cualquier otra VM de Azure, en la medida en que requiere acceso enrutado a través de una puerta de enlace de red o VPN al centro de datos del cliente (y a la inversa). Considere el uso de conectividad de Azure ExpressRoute si está disponible en su entorno.

### <a name="what-are-the-bandwidth-requirements-for-the-avere-vfxt"></a>¿Cuáles son los requisitos de ancho de banda para Avere vFXT?

El requisito de ancho de banda total depende de dos factores: 

* La cantidad de datos que se solicitan desde el origen 
* La tolerancia del sistema del cliente para la latencia durante la carga inicial de datos  

En entornos sensibles a la latencia, debe usar una solución de fibra con una velocidad de vínculo mínima de 1 Gbps. Use ExpressRoute si está disponible.  

### <a name="can-i-run-the-vfxt-with-public-ip-addresses"></a>¿Puedo ejecutar vFXT con direcciones IP públicas?

No, vFXT está pensada para funcionar dentro de un entorno de red protegido mediante los procedimientos recomendados.  

## <a name="technical---backend-storage-core-filers"></a>Preguntas técnicas, almacenamiento de back-end (archivadores de core)

### <a name="how-many-core-filers-does-a-single-avere-vfxt-environment-support"></a>¿Cuántos archivadores de core admite un único entorno de Avere vFXT?

Un clúster de Avere vFXT admite a un máximo de 20 archivadores de core. 

### <a name="how-does-the-avere-environment-store-data"></a>¿De qué manera el entorno de Avere almacena los datos?

Avere vFXT no es almacenamiento. Es una memoria caché que lee y escribe datos procedentes de varios destinos de almacenamiento denominados archivadores de core. Los datos almacenados en discos SSD Premium de Avere vFXT son transitorios y finalmente se vacían en el almacenamiento de los archivadores de core back-end.

### <a name="which-core-filers-does-avere-vfxt-support"></a>¿Qué archivadores de core admite Avere vFXT?

En términos generales, Avere vFXT for Azure admite los siguientes sistemas como archivadores de core: 

* Dell EMC Isilon (OneFS 7.1, 7.2, 8.0 y 8.1) 
* NetApp ONTAP (Clustered Mode 9.4, 9.3, 9.2, 9.1P1, 8.0 - 8.3) y (7-Mode 7.*, 8.0 - 8.3) 
* Contenedores de Azure Blob (solo LRS) 
* Cubos de AWS S3 
* Cubos de Google Cloud

### <a name="why-doesnt-the-avere-vfxt-support-all-nfs-filers"></a>¿Por qué Avere vFXT no admite todos los sistemas de archivadores NFS?

Si bien todas las plataformas NFS cumplen con los mismos estándares de IETF, en la práctica, cada implementación tiene sus propias particularidades. Estos detalles afecta a cómo Avere vFXT interactúa con el sistema de almacenamiento. Los sistemas admitidos son las plataformas más usadas en el mercado.

### <a name="does-avere-vfxt-support-private-object-storage-such-as-swiftstack"></a>¿Admite Avere vFXT almacenamiento de objetos privados (por ejemplo, Swiftstack)?

Avere vFXT no admite el almacenamiento de objetos privados.

### <a name="how-can-i-get-a-specific-storage-product-under-support"></a>¿Cómo puedo obtener compatibilidad para un producto de almacenamiento específico?

La compatibilidad se basa en la cantidad de demanda en el campo. Si hay suficientes solicitudes que generen ingresos para admitir una solución NAS determinada, se tendrá en cuenta. Haga las solicitudes a través del soporte técnico de Azure.

### <a name="can-i-use-azure-blob-storage-as-a-core-filer"></a>¿Puedo usar almacenamiento de Azure Blob como archivador de core?

Sí, Avere vFXT for Azure puede usar un contenedor de blobs de bloque como archivador de core en la nube.  

### <a name="what-are-the-storage-account-requirements-for-a-blob-core-filer"></a>¿Cuáles son los requisitos de la cuenta de almacenamiento para un archivador de core de Blob?

La cuenta de almacenamiento debe ser una cuenta de uso general v2 (GPv2) y estar configurada para el almacenamiento con redundancia local (LRS) únicamente. No se admiten GRS ni ZRS.

### <a name="can-i-use-archive-blob-storage"></a>¿Puedo usar almacenamiento de blobs de archivo?

 No. El SLA de almacenamiento de archivos no es compatible con las necesidades de acceso a directorios y archivos en tiempo real del sistema vFXT. 

### <a name="can-i-use-cool-blob-storage"></a>¿Puedo usar el almacenamiento de blobs esporádico?

Puede usar el nivel de acceso esporádico, pero tenga en cuenta que la tasa de operaciones será mucho mayor. 

### <a name="how-do-i-encrypt-the-blob-container"></a>¿Cómo se puede cifrar el contenedor de blobs?

Puede configurar el cifrado de blobs en Azure (opción preferida) o en el nivel del archivador de core de vFXT.  

### <a name="can-i-use-my-own-encryption-key-for-a-blob-core-filer"></a>¿Puedo usar mi propia clave de cifrado para un archivador de core de blobs?

De manera predeterminada, los datos se cifran mediante claves administradas de Microsoft para Azure Blobs, Tables, Files y Queues. Puede traer su propia clave de cifrado para Azure Blobs y Files. Si decide usar cifrado de vFXT, debe usar la clave generada por Avere y almacenarla localmente. 

## <a name="purchasing"></a>Compra

### <a name="how-do-i-get-avere-vfxt-for-azure-licensing"></a>¿Cómo se puede obtener licencias de Avere vFXT for Azure?

Obtener una licencia de Avere vFXT for Azure es fácil a través de Azure Marketplace. Registre una cuenta de Azure y, luego, siga las instrucciones de [Implementación del clúster de vFXT](avere-vfxt-deploy.md) para crear un clúster de Avere vFXT. 

### <a name="how-much-does-the-avere-vfxt-cost"></a>¿Cuánto cuesta Avere vFXT?

En Azure, no hay ningún cargo adicional de licencias para usar clústeres de Avere vFXT. Los clientes son responsables de los cargos de almacenamiento y otras cuotas de consumo de Azure.

### <a name="can-avere-vfxt-vms-be-run-as-low-priority"></a>¿Pueden las VM de Avere vFXT ejecutarse como de prioridad baja?

No, los clústeres de Avere vFXT requieren un servicio "siempre disponible". Los clústeres pueden desactivarse cuando no sean necesarios. 

## <a name="next-steps"></a>Pasos siguientes

Para empezar a trabajar con Avere vFXT for Azure, lea estos vínculos para obtener información sobre cómo planear e implementar su propio sistema:

* [Planeamiento de un sistema de Avere vFXT](avere-vfxt-deploy-plan.md)
* [Descripción general de la implementación](avere-vfxt-deploy-overview.md)
* [Preparación para la creación de Avere vFXT](avere-vfxt-prereqs.md)
* [Implementación del clúster de vFXT](avere-vfxt-deploy.md)

Para más información sobre las funcionalidades y casos de uso de Avere vFXT, visite [Avere vFXT for Azure](https://azure.microsoft.com/services/storage/avere-vfxt/).
