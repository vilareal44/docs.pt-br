---
ms.openlocfilehash: 7d50962b518c15875a5f1a82f5b89ab87a1db02e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619781"
---
### <a name="etw-eventlisteners-do-not-capture-events-from-providers-with-explicit-keywords-like-the-tpl-provider"></a>EventListeners do ETW não capturam eventos dos provedores com palavras-chave explícitas (como o provedor TPL)

#### <a name="details"></a>Detalhes

EventListeners do ETW com uma máscara de palavra-chave em branco não capturam eventos corretamente dos provedores com palavras-chave explícitas. No .NET Framework 4.5, o provedor TPL começou a fornecer palavras-chave explícitas e disparou esse problema. No .NET Framework 4.6, EventListeners foram atualizados para não apresentarem mais esse problema.

#### <a name="suggestion"></a>Sugestão

Para contornar esse problema, substitua as chamadas para <xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)> com chamadas para a sobrecarga EnableEvents que especifica explicitamente a máscara &quot;quaisquer palavras-chave&quot; a ser usada: <code>EnableEvents(eventSource, level, unchecked((EventKeywords)0xFFFFffffFFFFffff))</code>. Como alternativa, esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Diagnostics.Tracing.EventListener.EnableEvents(System.Diagnostics.Tracing.EventSource,System.Diagnostics.Tracing.EventLevel)?displayProperty=nameWithType></li></ul>|
