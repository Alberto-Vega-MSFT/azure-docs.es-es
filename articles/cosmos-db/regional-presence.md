---
title: Presencia regional con Azure Cosmos DB
description: En este artículo se explica la presencia regional de Azure Cosmos DB y los distintos entornos en la nube.
author: rimman
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 12/07/2018
ms.author: rimman
ms.custom: seodec18
ms.openlocfilehash: c3299b2ea777577281ede234cd1aa5137ce5a445
ms.sourcegitcommit: 78ec955e8cdbfa01b0fa9bdd99659b3f64932bba
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/10/2018
ms.locfileid: "53138116"
---
# <a name="regional-presence-of-azure-cosmos-db"></a>Presencia regional de Azure Cosmos DB

Actualmente, Azure está disponible en [54 regiones](https://azure.microsoft.com/global-infrastructure/regions/) en todo el mundo. Azure Cosmos DB es un servicio fundamental en Azure y está disponible en todas las regiones en las que Azure está disponible.

![Disponibilidad regional de Azure Cosmos DB](./media/regional-presence/regional-presence.png)

Cosmos DB está disponible en los cinco entornos en la nube de Azure disponibles para los clientes:

* La nube **pública de Azure**, que está disponible globalmente.

* **Azure China** está disponible gracias a una exclusiva asociación entre Microsoft y 21Vianet, uno de los proveedores de acceso a Internet más importantes del país.

* **Azure Alemania** proporciona servicios bajo un modelo de administrador de datos que garantiza que los datos de los clientes permanecen en Alemania bajo el control de T-Systems International GmbH, una subsidiaria de Deutsche Telekom, que actúa como administrador de datos para Alemania.

* **Azure Government** está disponible en cuatro regiones en los Estados Unidos para agencias gubernamentales de EE. UU. y sus asociados. 

* **Azure Government para el Departamento de Defensa (DoD)** está disponible en dos regiones de Estados Unidos para el Departamento de Defensa de EE. UU.

## <a name="regional-presence-with-global-distribution"></a>Presencia regional con distribución global

En todas las regiones de Azure están disponibles distintas API expuestas por Azure Cosmos DB (incluidas SQL, MongoDB, Cassandra, Gremlin y Azure Table Storage). Por ejemplo, puede tener MongoDB y Cassandra API expuestas por Azure Cosmos DB no solo en todas las regiones de Azure globales, sino también en las regiones soberanas de Azure, como China, Alemania, Government y del Departamento de Defensa (DoD).

Azure Cosmos DB es una base de datos [distribuida globalmente](distribute-data-globally.md). Puede asociar cualquier número de regiones de Azure con su cuenta de Azure Cosmos y los datos se replican de forma automática y transparente. Puede agregar o quitar una región a la cuenta de Azure Cosmos en cualquier momento. Con la funcionalidad de distribución global de llave en mano y el protocolo de replicación con arquitectura multimaestro, Azure Cosmos DB ofrece latencias de lectura y escritura de menos de 10 ms en el percentil 99, disponibilidad de lectura y escritura del 99,999 % y la capacidad de escalar de forma flexible el rendimiento aprovisionado para lectura y escritura en todas las regiones asociadas con su cuenta de Azure Cosmos. Azure Cosmos DB también ofrece cinco modelos de coherencia bien definidos y se puede aplicar un modelo específico a sus datos. Por último, Azure Cosmos DB es el único servicio de base de datos en el sector que proporciona un Acuerdo de Nivel de Servicio (SLA) completo que abarca rendimiento aprovisionado, latencia en el percentil 99, alta disponibilidad y coherencia.

## <a name="next-steps"></a>Pasos siguientes

Ahora puede obtener información acerca de los diferentes conceptos de Azure Cosmos DB con los siguientes artículos:

* [Aplicaciones distribuidas globalmente](distribute-data-globally.md)
* [Administración de cuentas de base de datos en Azure Cosmos DB](manage-account.md)
* [Aprovisionamiento del rendimiento de contenedores y bases de datos de Cosmos DB](set-throughput.md)
* [Acuerdo de Nivel de Servicio de Azure Cosmos DB](https://azure.microsoft.com/support/legal/sla/cosmos-db/v1_2/)
