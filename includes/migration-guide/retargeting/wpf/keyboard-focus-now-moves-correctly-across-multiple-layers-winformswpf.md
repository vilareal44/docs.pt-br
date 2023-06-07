---
ms.openlocfilehash: a82b720fd4e771481ea1142a88a095443afa0d5b
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614317"
---
### <a name="keyboard-focus-now-moves-correctly-across-multiple-layers-of-winformswpf-hosting"></a>O foco do teclado agora é movido corretamente em várias camadas de hospedagem do WinForms/WPF

#### <a name="details"></a>Detalhes

Considere um aplicativo WPF que hospede um controle do WinForms que, por sua vez, hospede controles do WPF. É possível que os usuários não consigam sair com Tab da camada do WinForms se o primeiro ou o último controle nessa camada for o `System.Windows.Forms.Integration.ElementHost` do WPF. Essa alteração corrige esse problema e os usuários agora conseguem sair com Tab da camada do WinForms. Os aplicativos automatizados baseados em foco que nunca saem da camada do WinForms podem não funcionar mais conforme o esperado.

#### <a name="suggestion"></a>Sugestão

O desenvolvedor que desejar utilizar esta alteração ao direcionar a uma versão do framework abaixo do .NET 4.7.2 poderá definir o seguinte conjunto de sinalizadores de AppContext como false para que a alteração seja habilitada.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

Os aplicativos WPF precisam aceitar todas as melhorias de acessibilidade iniciais para obter as melhorias mais recentes. Em outras palavras, ambas as opções `Switch.UseLegacyAccessibilityFeatures` e `Switch.UseLegacyAccessibilityFeatures.2` precisam ser definidas. O desenvolvedor que precisar da funcionalidade anterior ao direcionar ao .NET 4.7.2 ou a versões posteriores poderá definir o seguinte sinalizador de AppContext como true para que a alteração seja desabilitada.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures.2=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |
