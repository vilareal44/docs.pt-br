---
ms.openlocfilehash: 8b804d23247b95381f3ff83bc12c32215a420fb6
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614414"
---
### <a name="wpf-appdomain-shutdown-handling-may-now-call-dispatcherinvoke-in-cleanup-of-weak-events"></a>O tratamento de desligamento de AppDomain do WPF agora pode chamar Dispatcher. Invoke na limpeza de eventos fracos

#### <a name="details"></a>Detalhes

No .NET Framework 4.7.1 e em versões anteriores, o WPF potencialmente cria um <xref:System.Windows.Threading.Dispatcher?displayProperty=nameWithType> no thread do finalizador do .net durante o desligamento do AppDomain.  Isso foi corrigido em .NET Framework 4.7.2 e versões posteriores, tornando a limpeza de eventos fracos com reconhecimento de thread.  Devido a isso, o WPF pode chamar <xref:System.Windows.Threading.Dispatcher.Invoke%2A?displayProperty=nameWithType> para concluir o processo de limpeza. Em determinados aplicativos, essa alteração no tempo do finalizador pode potencialmente causar exceções durante o desligamento do AppDomain ou do processo.  Isso geralmente é visto em aplicativos que não desligam corretamente os expedidores em execução em threads de trabalho antes do encerramento do processo ou do AppDomain.  Esses aplicativos devem tomar cuidado para gerenciar corretamente o tempo de vida de expedirias.

#### <a name="suggestion"></a>Sugestão

No .NET Framework 4.7.2 e versões posteriores, os desenvolvedores podem desabilitar essa correção para ajudar a aliviar (mas não eliminar) problemas de tempo que podem ocorrer devido à alteração da limpeza. Para desabilitar a alteração na limpeza, use o seguinte sinalizador AppContext.<pre><code class="lang-xml">&lt;configuration&gt;&#13;&#10;&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.MS.Internal.DoNotInvokeInWeakEventTableShutdownListener=true&quot;/&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;&lt;/configuration&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.7.2       |
| Type    | Redirecionando |
