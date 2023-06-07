---
ms.openlocfilehash: 14585b6de3ce02884f8be789930fc8610f73ba7d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621026"
---
### <a name="horizontal-scrolling-and-virtualization"></a>Rolagem horizontal e virtualização

#### <a name="details"></a>Detalhes

Essa alteração se aplica a um <xref:System.Windows.Controls.ItemsControl?displayProperty=fullName> que faz sua própria virtualização na direção ortogonal para a direção de rolagem principal (o principal exemplo é <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> com EnableColumnVirtualization=&quot;True&quot;).  O resultado de certas operações de rolagem horizontal foi alterado para produzir resultados mais intuitivos e análogos aos resultados de operações verticais comparáveis.<p/>As operações específicas incluem &quot;Rolar Aqui&quot; e &quot;Borda Direita&quot; para usar os nomes dos menus obtidos clicando com o botão direito do mouse em uma barra de rolagem horizontal.  Ambas calculam um deslocamento candidato e chamam <xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)>.<p/>Depois de rolar para o novo deslocamento, a definição de &quot;aqui&quot; ou de &quot;borda direita&quot; poderá mudar, pois um conteúdo com a virtualização removida recentemente alterou o valor de <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName>.<p/>Antes do .NET Framework 4.6.2, a operação de rolagem simplesmente usa o deslocamento candidato, embora ele talvez não seja mais &quot;aqui&quot; ou na &quot;borda direita&quot;.  Isso resulta em efeitos como o &quot;salto&quot; de rolagem, melhor ilustrado pelo exemplo. Suponha que um <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> tenha ExtentWidth = 1000 e Width = 200.  Uma rolagem para &quot;Borda Direita&quot; usa o deslocamento de candidato 1000-200 = 800.  Durante a rolagem até esse deslocamento, a virtualização de novas colunas é eliminada. Vamos supor que elas sejam muito largas, então o <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> muda para 2000.  A rolagem termina com HorizontalOffset=800 e o elevador &quot;retorna&quot; para perto do meio da barra de rolagem, precisamente em 800/2000 = 40%.<p/>A alteração é para recalcular um novo deslocamento candidato quando essa situação ocorre e tentar novamente. (A rolagem vertical já funciona assim.) <p/>A alteração proporciona uma experiência mais previsível e intuitiva para o usuário final, mas também pode afetar qualquer aplicativo que dependa do valor exato de <xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=fullName> após uma rolagem horizontal, seja ela invocada pelo usuário final ou por uma chamada explícita para <xref:System.Windows.Controls.Primitives.IScrollInfo.SetHorizontalOffset(System.Double)>.

#### <a name="suggestion"></a>Sugestão

Um aplicativo que usa um valor previsto para <xref:System.Windows.Controls.Primitives.IScrollInfo.HorizontalOffset?displayProperty=fullName> deve ser alterado para obter o valor real (e o valor de <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName>) após qualquer rolagem horizontal que poderia alterar <xref:System.Windows.Controls.Primitives.IScrollInfo.ExtentWidth?displayProperty=fullName> devido à eliminação da virtualização.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.Primitives.IScrollInfo?displayProperty=nameWithType></li></ul>|
