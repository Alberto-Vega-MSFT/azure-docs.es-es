---
title: Explorador de búsqueda en Azure Portal para consultar índices en Azure Search
description: Utilice las herramientas de Azure Portal como el explorador de búsqueda para realizar consultas de índices en Azure Search. Escriba los términos de búsqueda o cadenas de búsqueda completa con sintaxis avanzada.
manager: cgronlun
author: HeidiSteen
services: search
ms.service: search
ms.topic: conceptual
ms.date: 07/10/2018
ms.author: heidist
ms.custom: seodec2018
ms.openlocfilehash: 11f102fcb2a24f9062313f9a3234c29e70a3dfe0
ms.sourcegitcommit: eb9dd01614b8e95ebc06139c72fa563b25dc6d13
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 12/12/2018
ms.locfileid: "53315678"
---
# <a name="how-to-use-search-explorer-to-query-indexes-in-azure-search"></a>Cómo usar el Explorador de búsqueda para realizar consultas de índices en Azure Search 

En este artículo se muestra cómo consultar un índice existente de Azure Search mediante el **Explorador de búsqueda** de Azure Portal. El Explorador de búsqueda se puede usar para enviar cadenas de consulta de Lucene simples o completas para cualquier índice existente en el servicio.

## <a name="open-the-service-dashboard"></a>Abrir el panel del servicio
1. Haga clic en **Todos los recursos** en la barra de acceso rápido del menú izquierdo de [Azure Portal](https://portal.azure.com/#blade/HubsExtension/BrowseResourceBlade/resourceType/Microsoft.Search%2FsearchServices).
2. Seleccione el servicio Azure Search.

## <a name="select-an-index"></a>Seleccionar un índice

Seleccione el índice en el que desea buscar con el icono **Índices**.

   ![](./media/search-explorer/pick-index.png)

## <a name="open-search-explorer"></a>Abrir el Explorador de búsqueda

Haga clic en el icono del Explorador de búsqueda para abrir la barra de búsqueda y el panel de resultados.

   ![](./media/search-explorer/search-explorer-tile.png)

## <a name="start-searching"></a>Inicio de la búsqueda

Cuando utilice el Explorador de búsqueda, puede especificar los [parámetros de consulta](https://docs.microsoft.com/rest/api/searchservice/Search-Documents) para formular la consulta.

1. En **Cadena de consulta**, escriba una consulta y pulse **Buscar**. 

   La cadena de consulta se analiza automáticamente en la dirección URL de solicitud adecuada para enviar una solicitud HTTP con la API de REST de Azure Search.   
   
   Puede usar cualquier sintaxis de consulta de Lucene simple o completa válida para crear la solicitud. El carácter `*` equivale a una búsqueda vacía o sin especificar que devuelve todos los documentos sin ningún orden determinado.

2. En **Resultados**, los resultados de las consultas se presentan en formato JSON sin formato, que son idénticos a la carga útil que se devuelve en un cuerpo de respuesta HTTP al emitir solicitudes mediante programación.

   ![](./media/search-explorer/search-bar.png)

## <a name="next-steps"></a>Pasos siguientes

Los siguientes recursos proporcionan ejemplos y la información de la sintaxis de consulta adicionales.

 + [Sintaxis de consulta simplificada](https://docs.microsoft.com/rest/api/searchservice/simple-query-syntax-in-azure-search) 
 + [Sintaxis de consulta de Lucene](https://docs.microsoft.com/rest/api/searchservice/lucene-query-syntax-in-azure-search) 
 + [Ejemplos de sintaxis de consulta de Lucene para la creación de consultas en Azure Search](https://docs.microsoft.com/azure/search/search-query-lucene-examples) 
 + [OData Expression Syntax for Azure Search](https://docs.microsoft.com/rest/api/searchservice/odata-expression-syntax-for-azure-search) (Sintaxis de expresiones de OData en Azure Search) 