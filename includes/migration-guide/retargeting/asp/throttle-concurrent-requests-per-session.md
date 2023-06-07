---
ms.openlocfilehash: b521f4163bf5bf4a369d0eec12dae503703a976e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614271"
---
### <a name="throttle-concurrent-requests-per-session"></a>Limitação de solicitações simultâneas por sessão

#### <a name="details"></a>Detalhes

No .NET Framework 4.6.2 e versões anteriores, o ASP.NET executa solicitações com a mesma Sessionid sequencialmente e sempre emite a Sessionid por meio de um cookie por padrão. Se uma página demorar muito tempo para responder, isso degradará significativamente o desempenho do servidor, pressionando <kbd>F5</kbd> no navegador. Na correção, adicionou-se um contador que acompanha as solicitações enfileiradas e encerra as solicitações quando elas excedem um limite especificado. O valor padrão é 50. Se o limite for alcançado, um aviso será registrado no log de eventos e uma resposta HTTP 500 poderá ser registrada no log do IIS.

#### <a name="suggestion"></a>Sugestão

Para restaurar o comportamento antigo, você pode adicionar a seguinte configuração ao arquivo web.config para recusar o novo comportamento.

```xml
<appSettings>
    <add key="aspnet:RequestQueueLimitPerSession" value="2147483647"/>
</appSettings>
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7         |
| Type    | Redirecionando |
