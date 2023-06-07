---
ms.openlocfilehash: 86169b5c9a20678647153c951550e590a5bce588
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622221"
---
### <a name="resizing-a-grid-can-hang"></a>Redimensionar uma grade pode causar travamento

#### <a name="details"></a>Detalhes

Um loop infinito pode ocorrer durante o layout de um <code>T:System.Windows.Controls.Grid</code> nas seguintes circunstâncias:<ul><li>As definições de linha contêm duas \* linhas, declarando um MinHeight e um MaxHeight.</li><li>O conteúdo das \* -Rows não excede o MaxHeight correspondente</li><li>A altura disponível da grade é ultrapassada pelo primeiro MinHeight (além de qualquer outra linha fixa ou automática)</li><li>O aplicativo é direcionado ao .NET Framework 4.7 ou aceita o algoritmo de alocação do 4.7 definindo <code>Switch.System.Windows.Controls.Grid.StarDefinitionsCanExceedAvailableSpace=false</code></li></ul>O loop também aconteceria com mais de duas linhas, ou no caso análogo para colunas. O problema foi corrigido no .NET Framework 4.7.1.

#### <a name="suggestion"></a>Sugestão

Atualizar para o .NET Framework 4.7.1.  Como alternativa, se não precisar do algoritmo de alocação do 4.7, você poderá usar a seguinte configuração:<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.Grid.StarDefinitionsCanExceedAvailableSpace=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.7|
|Type|Runtime|
