---
ms.openlocfilehash: 519de92ca4201d199941afe6382ddf9fc480fbbd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614293"
---
### <a name="servicebase-doesnt-propagate-onstart-exceptions"></a>ServiceBase não propaga exceções de OnStart

#### <a name="details"></a>Detalhes

No .NET Framework 4.7 e nas versões anteriores, as exceções geradas durante a inicialização do serviço não são propagadas para o chamador de <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType>.<br/>Começando com os aplicativos direcionados ao .NET Framework 4.7.1, o runtime propaga exceções para <xref:System.ServiceProcess.ServiceBase.Run%2A?displayProperty=nameWithType> de serviços que falham ao iniciar.

#### <a name="suggestion"></a>Sugestão

Na inicialização do serviço, se houver uma exceção, essa exceção será propagada. Isso deve ajudar a diagnosticar casos em que os serviços falham ao iniciar. <br/>Se esse comportamento não for desejado, você poderá recusá-lo, adicionando o seguinte elemento &lt;AppContextSwitchOverrides&gt; à seção &lt;runtime&gt; do arquivo de configuração de aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.ServiceProcess.DontThrowExceptionsOnStart=true" />
```

Se seu aplicativo for destinado a uma versão anterior à 4.7.1, mas você desejar ter esse comportamento, adicione o seguinte elemento &lt;AppContextSwitchOverrides&gt; à seção &lt;runtime&gt; do arquivo de configuração de aplicativo:

```xml
<AppContextSwitchOverrides value="Switch.System.ServiceProcess.DontThrowExceptionsOnStart=false" />
```

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase)?displayProperty=nameWithType>
- <xref:System.ServiceProcess.ServiceBase.Run(System.ServiceProcess.ServiceBase[])?displayProperty=nameWithType>
