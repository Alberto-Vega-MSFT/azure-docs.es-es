---
title: SDK de Azure Event Grid
description: Se describen los SDK de Azure Event Grid. Estos SDK proporcionan administración, publicación y consumo.
services: event-grid
author: tfitzmac
manager: timlt
ms.service: event-grid
ms.topic: reference
ms.date: 06/29/2018
ms.author: tomfitz
ms.openlocfilehash: 3c085074863aa166a5766116b6c63b7dc341ad96
ms.sourcegitcommit: 5892c4e1fe65282929230abadf617c0be8953fd9
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 06/29/2018
ms.locfileid: "37130842"
---
# <a name="event-grid-sdks-for-management-and-publishing"></a>SDK de Event Grid para administración y publicación

Event Grid proporciona SDK que permiten administrar los recursos y exponer eventos mediante programación.

## <a name="management-sdks"></a>SDK de administración

Los SDK de administración permiten crear, actualizar y eliminar suscripciones y temas de Event Grid. En la actualidad, están disponibles los siguientes SDK:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.Management.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://search.maven.org/#search%7Cga%7C1%7Cazure-mgmt-eventgrid)
* [Node](https://www.npmjs.com/package/azure-arm-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-mgmt-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_mgmt_event_grid)

## <a name="data-plane-sdks"></a>SDK de plano de datos

Los SDK de planos de datos permiten publicar eventos en temas, y se aseguran de autenticar, formar el evento y publicarlo de forma asincrónica en el punto de conexión especificado. También permiten consumir eventos de primera entidad. En la actualidad, están disponibles los siguientes SDK:

* [.NET](https://www.nuget.org/packages/Microsoft.Azure.EventGrid)
* [Go](https://github.com/Azure/azure-sdk-for-go)
* [Java](https://mvnrepository.com/artifact/com.microsoft.azure/azure-eventgrid)
* [Node](https://www.npmjs.com/package/azure-eventgrid)
* [Python](https://pypi.python.org/pypi/azure-eventgrid)
* [Ruby](https://rubygems.org/gems/azure_event_grid)

## <a name="next-steps"></a>Pasos siguientes

* Para obtener aplicaciones de ejemplo, consulte [Ejemplos de código de Event Grid](https://azure.microsoft.com/resources/samples/?sort=0&service=event-grid).
* Para ver una introducción a Event Grid, consulte el artículo acerca de [qué es Event Grid](overview.md).
* Para consultar los comandos de Event Grid en la CLI de Azure, vea [CLI de Azure](/cli/azure/eventgrid).
* Para consultar los comandos de Event Grid en PowerShell, vea [PowerShell](/powershell/module/azurerm.eventgrid).
