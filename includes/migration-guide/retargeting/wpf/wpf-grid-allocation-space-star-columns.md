---
ms.openlocfilehash: 3709b9e694011666cebcb0ae09fbc838f65967af
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85614306"
---
### <a name="wpf-grid-allocation-of-space-to-star-columns"></a>Alocação de espaço para colunas com estrela em Grades do WPF

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.7, o WPF substitui o algoritmo usado pelo <xref:System.Windows.Controls.Grid> para alocar espaço para colunas com \*. Isso altera a largura real atribuída a colunas com \* em diversos casos:

- Quando uma ou mais \* colunas também têm uma largura mínima ou máxima que substitui a alocação proporcional para essa coluna. (A largura mínima pode ser derivada de uma declaração MinWidth explícita ou de um mínimo implícito obtido do conteúdo da coluna. A largura máxima só pode ser definida explicitamente de uma declaração MaxWidth.)
- Quando uma ou mais colunas de \* declaram um peso de \* extremamente grande, maior que 10^298.
- Quando os pesos de \* são diferentes o suficiente para encontrar instabilidade de ponto flutuante (estouro, estouro negativo, perda de precisão).
- Quando o arredondamento de layout está habilitado e o DPI de exibição efetivo é suficientemente alto.
Nos dois primeiros casos, as larguras produzidas pelo novo algoritmo podem ser significativamente diferentes das produzidas pelo algoritmo antigo. No último caso, a diferença será no máximo um ou dois pixels.<p/>O novo algoritmo corrige vários bugs presentes no algoritmo antigo:

- A alocação total de colunas pode exceder a largura da grade. Isso pode ocorrer ao alocar espaço para uma coluna cujo compartilhamento proporcional seja menor que seu tamanho mínimo. O algoritmo aloca o tamanho mínimo, o que reduz o espaço disponível para outras colunas. Se não houver nenhuma coluna de \* restante para alocar, a alocação total será muito grande.
- A alocação total pode ser menor que a largura da grade. Esse é o problema duplo do n° 1, que surge ao alocar para uma coluna cujo compartilhamento proporcional é maior do que seu tamanho máximo, sem nenhuma coluna de \* restante para concluir o processo.
- Duas colunas com \* podem receber alocações que não sejam proporcionais a seus pesos \*. Esta é uma versão atenuada da n° 1 e da n° 2, que surge ao alocar para colunas de \* A, B e C (nessa ordem), quando o compartilhamento proporcional de B viola sua restrição mínima (ou máxima). Como acima, isso altera o espaço disponível para a coluna C, que obtém menos (ou mais) alocação proporcional do que A.
- As colunas com pesos muito grandes (&gt; 10^298) são tratadas como se tivessem o peso 10^298. Não são consideradas as diferenças proporcionais entre elas (e entre colunas com pesos um pouco menores).
- Colunas com pesos infinitos não são tratadas corretamente. (Na verdade, você não pode definir um peso para infinito, mas essa é uma restrição artificial. O código de alocação tentou lidar com isso, mas não teve muito sucesso.)
- Vários problemas menores, evitando estouro, estouro negativo, perda de precisão e problemas de ponto flutuante semelhantes.
- Os ajustes de arredondamento de layout são incorretos em um DPI suficientemente alto.
O novo algoritmo produz resultados que atendam aos seguintes critérios:<p/>a. A largura real atribuída a uma coluna de * nunca é menor que sua largura mínima nem maior que sua largura máxima.<br/>B. Cada <em>coluna que não é atribuída à sua largura mínima ou máxima recebe uma largura proporcional ao <em>peso. Para ser preciso, se duas colunas forem declaradas com a largura x</em> e y</em> , respectivamente, e se nenhuma coluna receber sua largura mínima ou máxima, as larguras reais v e w atribuídas às colunas estarão na mesma proporção: v/w = = x/y.<br/>C. A largura total alocada para &quot; colunas proporcionais &quot; \* é igual ao espaço disponível após a alocação para as colunas restritas (fixo, automático e \* -colunas que são alocadas na largura mínima ou máxima). Isso pode ser zero, por exemplo se a soma das larguras mínimas excede a largura disponível da grade.<br/>D. Todas essas instruções devem ser interpretadas em relação ao layout &quot;ideal&quot;. Quando o arredondamento de layout está em vigor, as larguras reais podem diferir das larguras ideais em até um pixel.<br/>O algoritmo antigo respeitou (A), mas não respeitou os outros critérios nos casos descritos acima.<p/>Tudo que foi dito sobre colunas e larguras neste tópico se aplica também a linhas e alturas.

#### <a name="suggestion"></a>Sugestão

Por padrão, os aplicativos direcionados a versões do .NET Framework começando com o .NET Framework 4.7 usarão o novo algoritmo, enquanto os aplicativos direcionados ao .NET Framework 4.6.2 ou a versões anteriores usarão o algoritmo antigo.<p/>Para substituir o padrão, use a seguinte definição de configuração:

<pre><code class="lang-xml">&lt;runtime&gt;&#13;&#10;&lt;AppContextSwitchOverrides value=&quot;Switch.System.Windows.Controls.Grid.StarDefinitionsCanExceedAvailableSpace=true&quot; /&gt;&#13;&#10;&lt;/runtime&gt;&#13;&#10;</code></pre>

O valor `true` seleciona o algoritmo antigo, `false` seleciona o novo algoritmo.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Secundária       |
| Versão | 4.7         |
| Type    | Redirecionando |
