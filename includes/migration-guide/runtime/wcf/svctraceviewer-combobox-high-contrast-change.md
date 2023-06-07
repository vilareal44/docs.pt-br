---
ms.openlocfilehash: 06f4186e8f233f5c769dfc5e05d2de5eacd9b053
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621912"
---
### <a name="svctraceviewer-combobox-high-contrast-change"></a>Alteração de alto contraste de ComboBox de svcTraceViewer

#### <a name="details"></a>Detalhes

Na [ferramenta Visualizador de Rastreamento de Serviço da Microsoft](~/docs/framework/wcf/service-trace-viewer-tool-svctraceviewer-exe.md), os controles ComboBox não eram exibidos na cor correta em determinados temas de alto contraste. O problema foi corrigido no .NET Framework 4.7.2. No entanto, devido aos requisitos de compatibilidade com versões anteriores do SDK do .NET Framework, a correção não estava visível para clientes por padrão. O .NET 4.8 revela essa alteração adicionando os seguintes [comutadores de configuração do AppContext](~/docs/framework/configure-apps/file-schema/runtime/appcontextswitchoverrides-element.md) ao arquivo svcTraceViewer.exe.config:<pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false&quot; /&gt;&#13;&#10;</code></pre>

#### <a name="suggestion"></a>Sugestão

<ul><li>Como recusar a alteração Se você não quiser ter a alteração de comportamento de alto contraste, poderá desabilitá-la removendo a seguinte seção do arquivo svcTraceViewer.exe.config:</li></ul><pre><code class="lang-xml">&lt;AppContextSwitchOverrides value=&quot;Switch.UseLegacyAccessibilityFeatures=false;Switch.UseLegacyAccessibilityFeatures.2=false&quot; /&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.8|
|Type|Runtime|
