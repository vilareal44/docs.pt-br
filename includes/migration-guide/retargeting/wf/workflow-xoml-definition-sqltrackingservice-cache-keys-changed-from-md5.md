---
ms.openlocfilehash: 7a7ecf052276fe8e62463498b83230d466630c79
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617779"
---
### <a name="workflow-xoml-definition-and-sqltrackingservice-cache-keys-changed-from-md5-to-sha256"></a>Alteração da definição XOML do fluxo de trabalho e das chaves de cache SqlTrackingService de MD5 para SHA256

#### <a name="details"></a>Detalhes

O runtime de fluxo de trabalho mantém um cache de definições de fluxo de trabalho definido em XOML. O SqlTrackingService também mantém um cache inserido por cadeias de caracteres. Esses caches são inseridos por valores que incluem o valor de hash da soma de verificação. No .NET Framework 4.7.2 e versões anteriores, o hash de soma de verificação usava o algoritmo MD5, o que causou problemas em sistemas habilitados para FIPS. A partir do .NET Framework 4,8, o algoritmo usado é SHA256. Não deve haver um problema de compatibilidade com essa alteração porque os valores são recalculados toda vez que o tempo de execução do fluxo de trabalho e o SqlTrackingService são iniciados. No entanto, fornecemos quirks para permitir que os clientes revertam para o uso do algoritmo de hash herdado, se necessário.

#### <a name="suggestion"></a>Sugestão

Se essa alteração apresentar um problema ao executar fluxos de trabalho, tente definir um ou ambos os comutadores `AppContext`:

- &quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey&quot; como true.
- &quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKey&quot; como true.
No código:

<pre><code class="lang-csharp">System.AppContext.SetSwitch(&quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey&quot;, true);&#13;&#10;System.AppContext.SetSwitch(&quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKey&quot;, true);&#13;&#10;</code></pre>

Ou, no arquivo de configuração (isso precisa estar no arquivo de configuração para o aplicativo que está criando o objeto <xref:System.Workflow.Runtime.WorkflowRuntime>):

<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Workflow.Runtime.UseLegacyHashForWorkflowDefinitionDispenserCacheKey=true&quot; /&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Workflow.Runtime.UseLegacyHashForSqlTrackingCacheKeytrue&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.8         |
| Type    | Redirecionando |
