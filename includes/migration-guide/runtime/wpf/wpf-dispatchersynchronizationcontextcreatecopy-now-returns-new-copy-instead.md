---
ms.openlocfilehash: fc6066fd0b23d299158114cb397934041b99ba47
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619892"
---
### <a name="wpf-dispatchersynchronizationcontextcreatecopy-now-returns-a-new-copy-instead-of-the-current-instance"></a>DispatcherSynchronizationContext.CreateCopy do WPF agora retorna uma nova cópia em vez da instância atual

#### <a name="details"></a>Detalhes

No .NET Framework 4, <xref:System.Windows.Threading.DispatcherSynchronizationContext.CreateCopy> retornava uma referência à instância atual, basicamente como uma otimização de desempenho. No .NET Framework 4.5, ele retorna uma nova instância que possibilita, pela primeira vez, concluir que referências iguais indicam que o thread em execução está no contexto de sincronização correto.  É provável que o código que verifica a identidade dessas referências seja afetado, mas, por causa da alteração, o código que chama <xref:System.Windows.Threading.DispatcherSynchronizationContext.CreateCopy> deve ser testado como parte da migração para o .NET Framework 4.5 ou mais recente.

#### <a name="suggestion"></a>Sugestão

Lembre-se de que <xref:System.Windows.Threading.DispatcherSynchronizationContext.CreateCopy> agora retornará um novo objeto <xref:System.Threading.SynchronizationContext?displayProperty=fullName>. Anteriormente, o código que usava a equivalência de referências gerada dessa maneira não estava de fato verificado se ela estava no contexto apropriado, mas isso foi corrigido no .NET Framework 4.5 ou posteriores.  Embora não haja probabilidade de causar problemas, praticar os caminhos de código afetados deve ser suficiente para determinar se isso impõe algum problema.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Threading.DispatcherSynchronizationContext.CreateCopy?displayProperty=nameWithType></li></ul>|
