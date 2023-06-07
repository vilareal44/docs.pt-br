---
ms.openlocfilehash: 9e70bcd95393a7ff24de43577d26869ce674067b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614415"
---
### <a name="wpf-focusvisual-for-radiobutton-and-checkbox-now-displays-correctly-when-the-controls-have-no-content"></a>O FocusVisual do WPF para RadioButton e CheckBox agora é exibido corretamente quando os controles não têm nenhum conteúdo

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.1 e nas versões anteriores, o <xref:System.Windows.Controls.CheckBox?displayProperty=nameWithType> e o <xref:System.Windows.Controls.RadioButton?displayProperty=nameWithType> do WPF têm visuais de foco inconsistentes e, nos temas Clássico e de Alto Contraste, eles têm visuais de foco incorretos.  Esses problemas ocorrem quando os controles não têm nenhum conjunto de conteúdo.  Isso pode dificultar a transição entre os temas e deixar o visual de foco confuso. No .NET Framework 4.7.2, agora esses visuais são mais consistentes entre os temas e mais facilmente visíveis nos temas Clássico e de Alto Contraste.

#### <a name="suggestion"></a>Sugestão

O desenvolvedor que direcionar ao .NET Framework 4.7.2 e desejar reverter para o comportamento no .NET 4.7.1 precisará definir o sinalizador de AppContext a seguir.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures.2=true;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

O desenvolvedor que desejar utilizar essa alteração ao direcionar a uma versão do framework abaixo do .NET 4.7.2 precisará definir os sinalizadores de AppContext a seguir. Observe que todos os sinalizadores precisam ser definidos corretamente e a versão instalada do .NET Framework precisa ser a 4.7.2 ou superior. Os aplicativos WPF precisam aceitar todas as melhorias de acessibilidade anteriores para obter as mais recentes. Para fazer isso, verifique se as opções AppContext 'Switch.UseLegacyAccessibilityFeatures' e 'Switch.UseLegacyAccessibilityFeatures.2' estão definidas como false.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false;&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |
