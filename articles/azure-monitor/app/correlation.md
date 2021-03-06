---
title: Correlación de Telemetría de Application Insights | Microsoft Docs
description: Correlación de Telemetría de Application Insights
services: application-insights
documentationcenter: .net
author: lgayhardt
manager: carmonm
ms.service: application-insights
ms.workload: TBD
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/31/2018
ms.reviewer: sergkanz
ms.author: lagayhar
ms.openlocfilehash: 946d0895ff25509a0e35695fe27c783b3c50784b
ms.sourcegitcommit: da69285e86d23c471838b5242d4bdca512e73853
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 01/03/2019
ms.locfileid: "53999585"
---
# <a name="telemetry-correlation-in-application-insights"></a>Correlación de Telemetría en Application Insights

En el mundo de los microservicios, todas las operaciones lógicas requieren que se realice el trabajo en diversos componentes del servicio. Cada uno de estos componentes puede supervisarse por separado mediante [Application Insights](../../application-insights/app-insights-overview.md). El componente de la aplicación web se comunica con el componente del proveedor de autenticación para validar las credenciales de usuario y con el componente de API a fin de obtener datos para su visualización. El componente de API, a su vez, puede consultar datos de otros servicios, usar componentes de proveedor de la caché e informar al componente de facturación sobre esta llamada. Application Insights admite la correlación de telemetría distribuida. Permite detectar qué componente es responsable de los errores o las degradaciones del rendimiento.

Este artículo explica el modelo de datos utilizado por Application Insights para poner en correlación la telemetría enviada por varios componentes. En él se tratan las técnicas y protocolos de propagación de contexto. También se trata la implementación de los conceptos de correlación en distintos idiomas y plataformas.

## <a name="telemetry-correlation-data-model"></a>Modelo de datos de correlación de telemetría

Application Insights define el [modelo de datos](../../azure-monitor/app/data-model.md) para la correlación de telemetría distribuida. Para asociar la telemetría con la operación lógica, todos los elementos de telemetría tienen el campo de contexto denominado `operation_Id`. Todos los elementos de telemetría comparten este identificador en el seguimiento distribuido. De este modo, aun con la pérdida de telemetría de una sola capa, todavía puede asociar la telemetría notificada por otros componentes.

Normalmente, la operación lógica distribuida consta de un conjunto de operaciones más pequeñas: solicitudes procesadas por uno de los componentes. La [telemetría de solicitudes](../../azure-monitor/app/data-model-request-telemetry.md) define esas operaciones. Cada telemetría de solicitudes cuenta con su propio `id`, que la identifica global y exclusivamente. Además, toda la telemetría (seguimientos, excepciones, etc.) asociada a esta solicitud debe establecer `operation_parentId` en el valor de la solicitud `id`.

La [telemetría de dependencia](../../azure-monitor/app/data-model-dependency-telemetry.md) representa todas las operaciones salientes (como, por ejemplo, la llamada HTTP a otro componente). La telemetría de dependencia también define su propio `id`, que es globalmente único. La telemetría de solicitudes, iniciada por esta llamada de dependencia, la usa como `operation_parentId`.

Puede crear la vista de la operación lógica distribuida usando `operation_Id`, `operation_parentId` y `request.id` con `dependency.id`. Esos campos también definen el orden de causalidad de las llamadas de telemetría.

En el entorno de los microservicios, los seguimientos de componentes pueden ir a los distintos almacenamientos. Cada componente puede tener su propia clave de instrumentación en Application Insights. A fin de obtener la telemetría para la operación lógica, es necesario consultar los datos de cada almacenamiento. Si el número de almacenamientos es muy elevado, debe tener una idea de dónde mirar a continuación.

El modelo de datos de Application Insights define dos campos, `request.source` y `dependency.target`, para solucionar este problema. El primer campo identifica el componente que inició la solicitud de dependencia y, el segundo, el componente que devolvió la respuesta de la llamada de dependencia.


## <a name="example"></a>Ejemplo

Tomemos como ejemplo una aplicación COTIZACIONES BURSÁTILES que muestra el precio de mercado actual de una acción con una API externa denominada API DE COTIZACIONES. La aplicación COTIZACIONES BURSÁTILES tiene una página `Stock page` abierta por el explorador web de cliente mediante `GET /Home/Stock`. La aplicación consulta la API de COTIZACIONES BURSÁTILES mediante el uso de una llamada HTTP `GET /api/stock/value`.

Puede analizar la telemetría resultante mediante la ejecución de una consulta:

```
(requests | union dependencies | union pageViews) 
| where operation_Id == "STYz"
| project timestamp, itemType, name, id, operation_ParentId, operation_Id
```

En la vista de resultados, observe que todos los elementos de telemetría comparten la raíz `operation_Id`. Al realizarse la llamada ajax desde la página, el nuevo y único id. `qJSXU` se asigna a la telemetría de dependencia y el id. de pageView se usa como `operation_ParentId`. A su vez, la solicitud de servidor utiliza el id. de ajax como `operation_ParentId`, etc.

| itemType   | Nombre                      | ID           | operation_ParentId | operation_Id |
|------------|---------------------------|--------------|--------------------|--------------|
| pageView   | Stock page                |              | STYz               | STYz         |
| dependency | GET /Home/Stock           | qJSXU        | STYz               | STYz         |
| request    | GET Home/Stock            | KqKwlrSt9PA= | qJSXU              | STYz         |
| dependency | GET /api/stock/value      | bBrf2L7mm2g= | KqKwlrSt9PA=       | STYz         |

Ahora, al realizarse la llamada `GET /api/stock/value` a un servicio externo, desea conocer la identidad de ese servidor. De este modo, puede establecer el campo `dependency.target` adecuadamente. Si el servicio externo no admite la supervisión, `target` se establece en el nombre de host del servicio como `stock-prices-api.com`. Sin embargo, si ese servicio se identifica devolviendo un encabezado HTTP predefinido, `target` contiene la identidad de servicio que permite a Application Insights crear un seguimiento distribuido consultando la telemetría de ese servicio. 

## <a name="correlation-headers"></a>Encabezados de correlación

Estamos trabajando en una propuesta de RFC para el [protocolo HTTP de correlación](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/HttpCorrelationProtocol.md). Esta propuesta define dos encabezados:

- `Request-Id`: lleva el id. único a nivel global de la llamada
- `Correlation-Context`: lleva la colección de pares nombre-valor de las propiedades de seguimiento distribuidas

El estándar también define dos esquemas de generación `Request-Id`: sin formato y jerárquico. Con el esquema sin formato, hay una clave de `Id` conocida definida para la colección `Correlation-Context`.

Application Insights define la [extensión](https://github.com/lmolkova/correlation/blob/master/http_protocol_proposal_v2.md) del protocolo HTTP de correlación. Usa pares nombre-valor `Request-Context` para propagar la colección de propiedades utilizadas por el autor o destinatario de la llamada. El SDK de Application Insights usa este encabezado para establecer los campos `dependency.target` y `request.source`.

### <a name="w3c-distributed-tracing"></a>Seguimiento distribuido de W3C

Estamos realizando una transición al [formato de seguimiento distribuido de W3C](https://w3c.github.io/trace-context/). Define:
- `traceparent`: lleva el identificador único global de la operación y un identificador único de la llamada.
- `tracestate`: lleva contexto específico del sistema de seguimiento.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-classic-apps"></a>Habilitar la compatibilidad con el seguimiento distribuido de W3C para aplicaciones clásicas de ASP.NET

Esta característica está disponible en Microsoft.ApplicationInsights.Web y en Microsoft.ApplicationInsights.DependencyCollecto los paquetes que son de la versión 2.8.0-beta1 o posterior.
De manera predeterminada, está **desactivada**. Para habilitarla, cambie `ApplicationInsights.config`:

* En `RequestTrackingTelemetryModule` agregue el elemento `EnableW3CHeadersExtraction` con el valor establecido en `true`.
* En `DependencyTrackingTelemetryModule` agregue el elemento `EnableW3CHeadersInjection` con el valor establecido en `true`.

#### <a name="enable-w3c-distributed-tracing-support-for-aspnet-core-apps"></a>Habilitar la compatibilidad con el seguimiento distribuido de W3C para aplicaciones de ASP.NET Core

Esta característica está en Microsoft.ApplicationInsights.AspNetCore con la versión 2.5.0-beta1 y en Microsoft.ApplicationInsights.DependencyCollecto la versión 2.8.0-beta1.
De manera predeterminada, está **desactivada**. Para habilitarla, establezca `ApplicationInsightsServiceOptions.RequestCollectionOptions.EnableW3CDistributedTracing` en `true`:

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddApplicationInsightsTelemetry(o => 
        o.RequestCollectionOptions.EnableW3CDistributedTracing = true );
    // ....
}
```

## <a name="open-tracing-and-application-insights"></a>Open Tracing y Application Insights

La [especificación del modelo de datos de Open Tracing](https://opentracing.io/) y los modelos de datos de Application Insights se asignan de la siguiente manera:

| Application Insights                  | Open Tracing                                      |
|------------------------------------   |-------------------------------------------------  |
| `Request`, `PageView`                 | `Span` con `span.kind = server`                  |
| `Dependency`                          | `Span` con `span.kind = client`                  |
| `Id` de `Request` y `Dependency`    | `SpanId`                                          |
| `Operation_Id`                        | `TraceId`                                         |
| `Operation_ParentId`                  | `Reference` de tipo `ChildOf` (el intervalo primario)   |

Para más información sobre el modelo de datos de Application Insights, consulte [modelo de datos](../../azure-monitor/app/data-model.md). 

Consulte [specification](https://github.com/opentracing/specification/blob/master/specification.md) (especificación) y [semantic_conventions](https://github.com/opentracing/specification/blob/master/semantic_conventions.md) (convenciones_semánticas) de Open Tracing para obtener definiciones de los conceptos de Open Tracing.

## <a name="telemetry-correlation-in-net"></a>Correlación de telemetría en .NET

Con el tiempo, .NET ha definido varias formas de poner en correlación la telemetría y registros de diagnósticos. Hay un elemento, `System.Diagnostics.CorrelationManager`, que permite realizar un seguimiento de [LogicalOperationStack y ActivityId](https://msdn.microsoft.com/library/system.diagnostics.correlationmanager.aspx). `System.Diagnostics.Tracing.EventSource` y el seguimiento de eventos para Windows definen el método [SetCurrentThreadActivityId](https://msdn.microsoft.com/library/system.diagnostics.tracing.eventsource.setcurrentthreadactivityid.aspx). `ILogger` usa [ámbitos de registro](https://docs.microsoft.com/aspnet/core/fundamentals/logging#log-scopes). WCF y HTTP conectan la propagación del contexto "actual".

Sin embargo, esos métodos no han habilitado la compatibilidad con el seguimiento distribuido automático. `DiagnosticsSource` es una forma de admitir la correlación entre máquinas. Las bibliotecas de .NET admiten Diagnostics Source (Origen de diagnóstico) y permiten la propagación entre máquinas del contexto de propagación a través del transporte como http.

En la [guía de Activities](https://github.com/dotnet/corefx/blob/master/src/System.Diagnostics.DiagnosticSource/src/ActivityUserGuide.md) (Actividades) de Diagnostics Source (Origen de diagnóstico) se explican los conceptos básicos de Activities (Actividades) de seguimiento. 

ASP.NET Core 2.0 admite la extracción de encabezados HTTP e inicia la nueva Activity (Actividad). 

La versión inicial de `System.Net.HttpClient`, `4.1.0`, admite la inserción automática de los encabezados HTTP de correlación y realiza un seguimiento de la llamada HTTP como Activity (Actividad).

Hay un nuevo módulo HTTP, [Microsoft.AspNet.TelemetryCorrelation](https://www.nuget.org/packages/Microsoft.AspNet.TelemetryCorrelation/) de ASP.NET clásico. En este módulo se implementa la correlación de telemetría mediante DiagnosticsSource (Origen de diagnóstico). Inicia la actividad en función de los encabezados de solicitud entrantes.  También pone en correlación la telemetría de las diversas fases del procesamiento de solicitudes. Incluso en los casos en que se ejecutan todas las fases del procesamiento de IIS en un subproceso administrado distinto.

La versión inicial del SDK de Application Insights, `2.4.0-beta1`, usa DiagnosticsSource (Origen de diagnóstico) y Activity (Actividad) para recolectar la telemetría y asociarla a la actividad actual. 

<a name="java-correlation"></a>
## <a name="telemetry-correlation-in-the-java-sdk"></a>Correlación de telemetría en el SDK de Java
El [SDK de Java para Application Insights](../../azure-monitor/app/java-get-started.md) permite la correlación automática de telemetría desde la versión `2.0.0`. Rellena automáticamente el valor de `operation_id` en todos los elementos de telemetría (seguimientos, excepciones, eventos personalizados, etc.) que se emiten en el ámbito de una solicitud. También se encarga de propagar los encabezados de correlación (descritos anteriormente) de las llamadas de servicio a servicio a través de HTTP si el [agente del SDK de Java](../../azure-monitor/app/java-agent.md) está configurado. Nota: En la característica de correlación, solo se admiten las llamadas realizadas a través del cliente Apache HTTP. Si usa Spring RestTemplate o Feign, los dos pueden utilizarse con el cliente Apache HTTP internamente.

En la actualidad, no se admite la propagación automática del contexto entre tecnologías de mensajería (por ejemplo, Kafka, RabbitMQ y Azure Service Bus). Sin embargo, es posible codificar manualmente este tipo de escenario con `trackDependency` API y `trackRequest` API, donde una telemetría de dependencias representa a un mensaje agregado a una cola por un productor, mientras que la solicitud representa a un mensaje procesado por el consumidor. En este caso, tanto `operation_id` como `operation_parentId` deben propagarse en las propiedades del mensaje.

<a name="java-role-name"></a>
## <a name="role-name"></a>Nombre de rol

Es posible que, en ocasiones, quiera personalizar el modo en que los nombres de los componentes aparecen en el [mapa de aplicación](../../azure-monitor/app/app-map.md). Para ello, puede establecer manualmente el valor de `cloud_RoleName` a través de uno de los siguientes mecanismos:

Si usa Spring Boot con el iniciador de Spring Boot de Application Insights, el único cambio necesario es establecer el nombre personalizado para la aplicación en el archivo application.properties.

`spring.application.name=<name-of-app>`

El iniciador de Spring Boot asignará de forma automática cloudRoleName al valor proporcionado para la propiedad spring.application.name.

Si usa `WebRequestTrackingFilter`, `WebAppNameContextInitializer` establecerá automáticamente el nombre de la aplicación. Agregue lo siguiente al archivo de configuración (ApplicationInsights.xml):
```XML
<ContextInitializers>
  <Add type="com.microsoft.applicationinsights.web.extensibility.initializers.WebAppNameContextInitializer" />
</ContextInitializers>
```

A través de la clase de contexto en la nube:

```Java
telemetryClient.getContext().getCloud().setRole("My Component Name");
```

## <a name="next-steps"></a>Pasos siguientes

- [Escritura de telemetría personalizada](../../azure-monitor/app/api-custom-events-metrics.md)
- [Obtenga más información sobre](../../azure-monitor/app/app-map.md#set-cloudrolename) el establecimiento de cloud_RoleName para otros SDK.
- Incorpore todos los componentes del microservicio en Application Insights. Consulte las [plataformas compatibles](../../azure-monitor/app/platforms.md).
- Consulte [modelo de datos](../../azure-monitor/app/data-model.md) para los tipos y el modelo de datos de Application Insights.
- Obtenga información sobre cómo [ampliar y filtrar la telemetría](../../azure-monitor/app/api-filtering-sampling.md).
- [Referencia de configuración de Application Insights](configuration-with-applicationinsights-config.md)

