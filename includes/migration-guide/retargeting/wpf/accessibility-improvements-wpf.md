---
ms.openlocfilehash: 47d3829748deef2c7c3610816b8941bf88da7ec6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614314"
---
### <a name="accessibility-improvements-in-wpf"></a>Melhorias de acessibilidade no WPF

#### <a name="details"></a>Detalhes

**Melhorias de Alto Contraste**
<ul><li>Agora, o foco do controle <xref:System.Windows.Controls.Expander> é visível. Em versões anteriores do .NET Framework, ele não era.
-O texto no <xref:System.Windows.Controls.CheckBox> e os <xref:System.Windows.Controls.RadioButton> controles quando eles são selecionados agora são mais fáceis de ver do que nas versões anteriores do .NET Framework.
-A borda de um desabilitado <xref:System.Windows.Controls.ComboBox> agora é a mesma cor que o texto desabilitado. Em versões anteriores do .NET Framework, ele não era.
-Os botões desabilitados e com foco agora usam a cor de tema correta. Em versões anteriores do .NET Framework, isso não acontecia.
-O botão suspenso agora está visível quando o <xref:System.Windows.Controls.ComboBox> estilo de um controle é definido como <xref:System.Windows.Controls.ToolBar.ComboBoxStyleKey?displayProperty=nameWithType> , nas versões anteriores do .NET Framework, não era.
-A seta de indicador de classificação em um <xref:System.Windows.Controls.DataGrid> controle agora usa cores de tema. Em versões anteriores do .NET Framework, isso não acontecia.
-O estilo de hiperlink padrão agora muda para a cor correta do tema no mouse. Em versões anteriores do .NET Framework, isso não acontecia.
-O foco do teclado em botões de opção agora está visível. Em versões anteriores do .NET Framework, ele não era.
-A <xref:System.Windows.Controls.DataGrid> coluna de caixa de seleção do controle agora usa as cores esperadas para comentários de foco de teclado. Em versões anteriores do .NET Framework, isso não acontecia.
-os visuais de foco de teclado agora são visíveis no <xref:System.Windows.Controls.ComboBox> e nos <xref:System.Windows.Controls.ListBox> controles. Em versões anteriores do .NET Framework, ele não era.</p>
**Melhorias de interação do leitor de tela**
<ul><li>Agora, controles <xref:System.Windows.Controls.Expander> são anunciados corretamente como grupos (expandir/recolher) por leitores de tela.
Agora, controles - <xref:System.Windows.Controls.DataGridCell> são anunciados corretamente como uma célula de grade de dados (localizada) por leitores de tela.
-Os leitores de tela agora anunciarão o nome de um editável <xref:System.Windows.Controls.ComboBox> .
Controles - <xref:System.Windows.Controls.PasswordBox> não são mais anunciados como &quot;Nenhum item no modo de exibição&quot; por leitores de tela.</p>
**Suporte do LiveRegion** Leitores de tela, como o Narrator, ajudam as pessoas a conhecer o conteúdo da interface do usuário de um aplicativo, geralmente descrevendo algo sobre a interface do usuário que está concentrada no momento, porque esse é provavelmente o elemento de interesse do usuário. No entanto, se um elemento da interface do usuário for alterado em alguma parte da tela e não tiver o foco, o usuário poderá não ser informado e poderá perder informações importantes. LiveRegions devem resolver esse problema. O desenvolvedor pode usá-las para informar o leitor de tela ou qualquer outro cliente de [Automação da interface do usuário](~/docs/framework/ui-automation/ui-automation-overview.md) de que foi feita uma alteração importante em um elemento da interface do usuário. Em seguida, o leitor de tela pode decidir como e quando informar o usuário dessa alteração. A propriedade LiveSetting também permite que o leitor de tela saiba o quanto é importante informar o usuário sobre a alteração feita na interface do usuário.

#### <a name="suggestion"></a>Sugestão

**Como aceitar ou recusar essas alterações** Para que o aplicativo se beneficie dessas alterações, ele deve ser executado no .NET Framework 4.7.1 ou posterior. O aplicativo pode se beneficiar dessas alterações de uma das seguintes maneiras:

- Direcionar ao .NET Framework 4.7.1. Essa é a abordagem recomendada. Essas alterações de acessibilidade são habilitadas por padrão em aplicativos do WPF destinados ao .NET Framework 4.7.1 ou posterior.
- Recusar os comportamentos de acessibilidade herdados adicionando a seguinte [Opção de AppContext](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) à seção `<runtime>` do arquivo de configuração de aplicativo e configurando-a como `false`, como mostra o exemplo a seguir.

<pre><code class="lang-xml">&lt;?xml version=&quot;1.0&quot; encoding=&quot;utf-8&quot;?&gt;&#13;&#10;&lt;configuration&gt;&#13;&#10;&lt;startup&gt;&#13;&#10;&lt;supportedRuntime version=&quot;v4.0&quot; sku=&quot;.NETFramework,Version=v4.7&quot;/&gt;&#13;&#10;&lt;/startup&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;!-- AppContextSwitchOverrides value attribute is in the form of &#39;key1=true/false;key2=true/false  --&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

Aplicativos destinados ao .NET Framework 4.7.1 ou posterior que desejam preservar o comportamento de acessibilidade herdado pode aceitar o uso das funcionalidades de acessibilidade herdadas definindo explicitamente essa opção de AppContext como `true`.
Para obter uma visão geral de automação da interface do usuário, confira a [Visão geral de automação da interface do usuário](~/docs/framework/ui-automation/ui-automation-overview.md).

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.7.1       |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveSettingProperty?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationElementIdentifiers.LiveRegionChangedEvent?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationLiveSetting?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.LiveSettingProperty?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.SetLiveSetting(System.Windows.DependencyObject,System.Windows.Automation.AutomationLiveSetting)?displayProperty=nameWithType>
- <xref:System.Windows.Automation.AutomationProperties.GetLiveSetting(System.Windows.DependencyObject)?displayProperty=nameWithType>
- <xref:System.Windows.Automation.Peers.AutomationPeer.GetLiveSettingCore?displayProperty=nameWithType>
