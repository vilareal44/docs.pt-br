---
title: Módulos, manipuladores e middleware
description: Saiba mais sobre como lidar com solicitações HTTP com módulos, manipuladores e middleware.
author: danroth27
ms.author: daroth
no-loc:
- Blazor
ms.date: 10/11/2019
ms.openlocfilehash: 639755dd78892df1b70ea5245a9584e575fbf691
ms.sourcegitcommit: 0100be20fcf23f61dab672deced70059ed71bb2e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2020
ms.locfileid: "88267874"
---
# <a name="modules-handlers-and-middleware"></a>Módulos, manipuladores e middleware

Um aplicativo ASP.NET Core é criado com base em uma série de *middleware*. O middleware é manipuladores que são organizados em um pipeline para lidar com solicitações e respostas. Em um aplicativo Web Forms, manipuladores e módulos HTTP resolvem problemas semelhantes. Em ASP.NET Core, módulos, manipuladores, *global.asax.cs*e o ciclo de vida do aplicativo são substituídos por middleware. Neste capítulo, você aprenderá o que é o middleware no contexto de um Blazor aplicativo.

## <a name="overview"></a>Visão geral

O pipeline de solicitação do ASP.NET Core consiste em uma sequência de delegados de solicitação, chamados um após o outro. O diagrama a seguir demonstra o conceito. O thread de execução segue as setas pretas.

![pipeline](media/middleware/request-delegate-pipeline.png)

O diagrama anterior não tem um conceito de eventos de ciclo de vida. Esse conceito é fundamental para como as solicitações de Web Forms ASP.NET são tratadas. Esse sistema torna mais fácil motivo de qual processo está ocorrendo e permite que o middleware seja inserido em qualquer ponto. O middleware é executado na ordem em que é adicionado ao pipeline de solicitação. Eles também são adicionados no código em vez de arquivos de configuração, geralmente em *Startup.cs*.

## <a name="katana"></a>Katana

Os leitores familiarizados com o Katana se sentirão confortáveis em ASP.NET Core. Na verdade, Katana é uma estrutura da qual ASP.NET Core deriva. Ele apresentou middleware semelhante e padrões de pipeline para ASP.NET 4. x. O middleware projetado para Katana pode ser adaptado para funcionar com o pipeline de ASP.NET Core.

## <a name="common-middleware"></a>Middleware comum

O ASP.NET 4. x inclui muitos módulos. De maneira semelhante, ASP.NET Core também tem muitos componentes de middleware disponíveis. Os módulos do IIS podem ser usados em alguns casos com ASP.NET Core. Em outros casos, o middleware nativo ASP.NET Core pode estar disponível.

A tabela a seguir lista os componentes e middleware de substituição no ASP.NET Core.

|Módulo                 |Módulo ASP.NET 4. x           |Opção ASP.NET Core|
|-----------------------|-----------------------------|-------------------|
|Erros de HTTP            |`CustomErrorModule`          |[Middleware de páginas de código de status](/aspnet/core/fundamentals/error-handling#usestatuscodepages)|
|Documento padrão       |`DefaultDocumentModule`      |[Middleware de arquivos padrão](/aspnet/core/fundamentals/static-files#serve-a-default-document)|
|Pesquisa no diretório     |`DirectoryListingModule`     |[Middleware de navegação no diretório](/aspnet/core/fundamentals/static-files#enable-directory-browsing)|
|Compactação dinâmica    |`DynamicCompressionModule`   |[Middleware de compactação de resposta](/aspnet/core/performance/response-compression)|
|Rastreamento de solicitações com falha|`FailedRequestsTracingModule`|[Log de ASP.NET Core](/aspnet/core/fundamentals/logging/index#tracesource-provider)|
|Cache de arquivos           |`FileCacheModule`            |[Middleware de cache de resposta](/aspnet/core/performance/caching/middleware)|
|Cache HTTP           |`HttpCacheModule`            |[Middleware de cache de resposta](/aspnet/core/performance/caching/middleware)|
|Log de FTP           |`HttpLoggingModule`          |[Log de ASP.NET Core](/aspnet/core/fundamentals/logging/index)|
|Redirecionamento HTTP       |`HttpRedirectionModule`      |[Middleware de regravação de URL](/aspnet/core/fundamentals/url-rewriting)|
|Filtros ISAPI          |`IsapiFilterModule`          |[Middleware](/aspnet/core/fundamentals/middleware/index)|
|ISAPI                  |`IsapiModule`                |[Middleware](/aspnet/core/fundamentals/middleware/index)|
|Filtragem de solicitações      |`RequestFilteringModule`     |[Irule usando de middleware de regravação de URL](/aspnet/core/fundamentals/url-rewriting#irule-based-rule)|
|Regravação de URL&#8224;   |`RewriteModule`              |[Middleware de regravação de URL](/aspnet/core/fundamentals/url-rewriting)|
|Compactação estática     |`StaticCompressionModule`    |[Middleware de compactação de resposta](/aspnet/core/performance/response-compression)|
|Conteúdo estático         |`StaticFileModule`           |[Middleware de arquivos estáticos](/aspnet/core/fundamentals/static-files)|
|Autorização de URL      |`UrlAuthorizationModule`     |[Identidade do ASP.NET Core](/aspnet/core/security/authentication/identity)|

Essa lista não é exaustiva, mas deve dar uma ideia de qual mapeamento existe entre as duas estruturas. Para obter uma lista mais detalhada, consulte [módulos do IIS com ASP.NET Core](/aspnet/core/host-and-deploy/iis/modules).

## <a name="custom-middleware"></a>Middleware personalizado

O middleware interno pode não lidar com todos os cenários necessários para um aplicativo. Nesses casos, faz sentido criar seu próprio middleware. Há várias maneiras de definir o middleware, com o mais simples de ser um simples delegado. Considere o seguinte middleware, que aceita uma solicitação de cultura de uma cadeia de caracteres de consulta:

```csharp
public class Startup
{
    public void Configure(IApplicationBuilder app)
    {
        app.Use(async (context, next) =>
        {
            var cultureQuery = context.Request.Query["culture"];

            if (!string.IsNullOrWhiteSpace(cultureQuery))
            {
                var culture = new CultureInfo(cultureQuery);

                CultureInfo.CurrentCulture = culture;
                CultureInfo.CurrentUICulture = culture;
            }

            // Call the next delegate/middleware in the pipeline
            await next();
        });

        app.Run(async (context) =>
            await context.Response.WriteAsync(
                $"Hello {CultureInfo.CurrentCulture.DisplayName}"));
    }
}
```

O middleware também pode ser definido como uma classe implementando a `IMiddleware` interface ou seguindo a Convenção de middleware. Para obter mais informações, consulte [Write custom ASP.NET Core middleware](/aspnet/core/fundamentals/middleware/write).

>[!div class="step-by-step"]
>[Anterior](data.md) 
> [Avançar](config.md)
