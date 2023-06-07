---
ms.openlocfilehash: dfdb62e8dd6db67d4fd12aba93928f4615e3f284
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614318"
---
### <a name="tabcontrol-selectionchanged-event-and-selectedcontent-property"></a>Evento TabControl SelectionChanged e propriedade SelectedContent

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7.1, um <xref:System.Windows.Controls.TabControl> atualiza o valor de sua propriedade <xref:System.Windows.Controls.TabControl.SelectedContent> antes de acionar o evento <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged>, quando sua seleção é alterada. No .NET Framework 4.7 e em versões anteriores, a atualização de SelectedContent acontecia depois do evento.

#### <a name="suggestion"></a>Sugestão

Aplicativos destinados ao .NET Framework 4.7.1 ou posteriores podem recusar essa alteração e usar o comportamento herdado adicionando o seguinte à seção `<runtime>` do arquivo de configuração de aplicativo:

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

Aplicativos destinados ao .NET Framework 4.7 ou anteriores, mas que são executados no .NET Framework 4.7.1 ou posteriores podem habilitar o novo comportamento adicionando a seguinte linha à seção `<runtime>` do arquivo de configuração de aplicativo:

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Controls.TabControl.SelectedContent?displayProperty=nameWithType>
- <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged?displayProperty=nameWithType>
