---
ms.openlocfilehash: 4303b177ffe01328965c909a5a3809414fcead91
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614315"
---
### <a name="the-default-hash-algorithm-for-wpfs-markup-compiler-is-now-sha256"></a>O algoritmo de hash padrão do compilador de marcação do WPF agora é o SHA256

#### <a name="details"></a>Detalhes

O MarkupCompiler do WPF fornece serviços de compilação para arquivos de marcação XAML.  No .NET Framework 4.7.1 e nas versões anteriores, o algoritmo de hash padrão usado para as somas de verificação era o SHA1. Devido a questões de segurança recentes com o SHA1, esse padrão foi alterado para o SHA256 começando com o .NET Framework 4.7.2.  Essa alteração afeta toda a geração de soma de verificação para arquivos de marcação durante a compilação.

#### <a name="suggestion"></a>Sugestão

O desenvolvedor que direcionar ao .NET Framework 4.7.2 ou superior e desejar reverter para o comportamento de hash do SHA1 precisará definir o sinalizador de AppContext a seguir.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Markup.DoNotUseSha256ForMarkupCompilerChecksumAlgorithm=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

O desenvolvedor que desejar utilizar o hash SHA256 ao direcionar a uma versão do framework abaixo do .NET 4.7.2 precisará definir o sinalizador de AppContext abaixo.  Observe que a versão instalada do .NET Framework precisará ser a 4.7.2 ou superior.

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Markup.DoNotUseSha256ForMarkupCompilerChecksumAlgorithm=false&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Transparente |
| Versão | 4.7.2       |
| Type    | Redirecionando |
