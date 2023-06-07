---
ms.openlocfilehash: 2b88ab7cfdd7a0a0a770b305ecd0200afd9a9b4b
ms.sourcegitcommit: 2543a78be6e246aa010a01decf58889de53d1636
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2020
ms.locfileid: "86441538"
---
### <a name="authorization-resource-in-endpoint-routing-is-httpcontext"></a>Autorização: o recurso no roteamento de ponto de extremidade é HttpContext

Ao usar o roteamento de ponto de extremidade no ASP.NET Core 3,1, o recurso usado para autorização é o ponto de extremidade. Essa abordagem não era suficiente para obter acesso aos dados da rota ( <xref:Microsoft.AspNetCore.Routing.RouteData> ). Anteriormente no MVC, um <xref:Microsoft.AspNetCore.Http.HttpContext> recurso foi passado, o que permite o acesso ao ponto de extremidade ( <xref:Microsoft.AspNetCore.Http.Endpoint> ) e aos dados de rota. Essa alteração garante que o recurso passado para autorização seja sempre o `HttpContext` .

#### <a name="version-introduced"></a>Versão introduzida

5,0 Preview 7

#### <a name="old-behavior"></a>Comportamento antigo

Ao usar o roteamento de ponto de extremidade e os atributos de middleware de autorização ( <xref:Microsoft.AspNetCore.Authorization.AuthorizationMiddleware> ) ou [[autorizar]](xref:Microsoft.AspNetCore.Authorization.AuthorizeAttribute) , o recurso passado para autorização é o ponto de extremidade correspondente.

#### <a name="new-behavior"></a>Novo comportamento

O roteamento do ponto de extremidade passa a `HttpContext` para a autorização.

#### <a name="reason-for-change"></a>Motivo da alteração

Você pode acessar o ponto de extremidade do `HttpContext` . No entanto, não havia como ir do ponto de extremidade para coisas como os dados de rota. Houve uma perda na funcionalidade do roteamento sem ponto de extremidade.

#### <a name="recommended-action"></a>Ação recomendada

Se seu aplicativo usar o recurso de ponto de extremidade, chame <xref:Microsoft.AspNetCore.Http.EndpointHttpContextExtensions.GetEndpoint%2A> no `HttpContext` para continuar acessando o ponto de extremidade.

No ASP.NET Core 5,0 Preview 8 e posterior, você pode reverter para o comportamento antigo com o <xref:System.AppContext.SetSwitch%2A> . Por exemplo:

```csharp
AppContext.SetSwitch(
    "Microsoft.AspNetCore.Authorization.SuppressUseHttpContextAsAuthorizationResource",
    isEnabled: true);
```

#### <a name="category"></a>Categoria

ASP.NET Core

#### <a name="affected-apis"></a>APIs afetadas

Nenhum

<!--

#### Affected APIs

Not detectable via API analysis

-->
