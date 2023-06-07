---
ms.openlocfilehash: 0ef39d67cd4335658658f5772b5bce49d827cad0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614316"
---
### <a name="selector-selectionchanged-event-and-selectedvalue-property"></a>Evento SelectionChanged e propriedade SelectedValue do seletor

#### <a name="details"></a>Detalhes

Começando com o .NET Framework 4.7.1, um <xref:System.Windows.Controls.Primitives.Selector> sempre atualiza o valor de sua propriedade <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> antes de acionar o evento <xref:System.Windows.Controls.Primitives.Selector.SelectionChanged>, quando sua seleção é alterada. Isso torna a propriedade SelectedValue consistente com as outras propriedades de seleção (<xref:System.Windows.Controls.Primitives.Selector.SelectedItem%2A> e <xref:System.Windows.Controls.Primitives.Selector.SelectedIndex%2A>), que são atualizadas antes que o evento seja acionado.<p/>No .NET Framework 4.7 e nas versões anteriores, a atualização de SelectedValue ocorria antes do evento na maioria dos casos, mas ocorria depois do evento quando a alteração da seleção era causada pela alteração da propriedade <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A>.

#### <a name="suggestion"></a>Sugestão

Aplicativos destinados ao .NET Framework 4.7.1 ou posteriores podem recusar essa alteração e usar o comportamento herdado adicionando o seguinte à seção `<runtime>` do arquivo de configuração de aplicativo:

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides&#13;&#10;value=&quot;Switch.System.Windows.Controls.TabControl.SelectionPropertiesCanLagBehindSelectionChangedEvent=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

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
