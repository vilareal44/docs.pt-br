---
ms.openlocfilehash: 2aa424ff5e3308b730c22cb865993d4100f193cc
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617780"
---
### <a name="workflow-xoml-file-checksums-changed-from-md5-to-sha256"></a>Somas de verificação do arquivo XOML de fluxo de trabalho alteradas de MD5 para SHA256

#### <a name="details"></a>Detalhes

Para oferecer suporte à depuração de fluxos de trabalho baseados em XOML com o Visual Studio, quando projetos de fluxo de trabalho que contêm arquivos XOML são criados, uma soma de verificação do conteúdo do arquivo XOML é incluída no código gerado como um valor <xref:System.Workflow.ComponentModel.Compiler.WorkflowMarkupSourceAttribute.MD5Digest?displayProperty=nameWithType>. No .NET Framework 4.7.2 e versões anteriores, o hash de soma de verificação usava o algoritmo MD5, o que causou problemas em sistemas habilitados para FIPS. A partir do .NET Framework 4.8, o algoritmo usado será o SHA256. Para ser compatível com WorkflowMarkupSourceAttribute.MD5Digest, somente os primeiros 16 bytes da soma de verificação gerada são usados. Isso pode causar problemas durante a depuração. Você precisará compilar novamente seu projeto.

#### <a name="suggestion"></a>Sugestão

Se compilar novamente o projeto não resolver o problema, tente definir a `AppContext` opção &quot;Switch.System.Workflow.ComponentModel.UseLegacyHashForXomlFileChecksum&quot; como true. no código:

<pre><code class="lang-csharp">System.AppContext.SetSwitch(&quot;Switch.System.Workflow.ComponentModel.UseLegacyHashForXomlFileChecksum&quot;, true);&#13;&#10;</code></pre>

Ou em um arquivo de configuração (isso precisa estar no MSBuild.exe.config para o MSBuild.exe que você está usando):

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Workflow.ComponentModel.UseLegacyHashForXomlFileChecksum=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.8         |
| Type    | Redirecionando |
