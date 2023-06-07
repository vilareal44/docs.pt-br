---
ms.openlocfilehash: fbf3c0c8f1d11f9f5997a4d1027242c4710c7107
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621766"
---
### <a name="item-scrolling-a-flat-list-with-items-of-different-pixel-height"></a>Rolagem de itens em uma lista simples de itens com diferentes alturas em pixels

#### <a name="details"></a>Detalhes

Quando um <xref:System.Windows.Controls.ItemsControl?displayProperty=fullName> exibe uma coleção usando virtualização (<code>IsVirtualizing=true</code>) e rolagem de itens (<code>ScrollUnit=Item</code>) e quando o controle rola para exibir um item cuja altura em pixels é diferente de seus vizinhos, o <xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=fullName> itera em todos os itens na coleção. A interface do usuário não está respondendo durante essa iteração; se a coleção for grande, isso poderá ser percebido como uma parada. A iteração ocorre em outras circunstâncias, mesmo nas versões anteriores do .NET Framework. Por exemplo, isso ocorre na rolagem de pixels (<code>ScrollUnit=Pixel</code>) ao encontrar um item com uma altura de pixels diferente e na rolagem de itens de dados hierárquicos (como em um <xref:System.Windows.Controls.TreeView?displayProperty=fullName> ou um <xref:System.Windows.Controls.ItemsControl?displayProperty=fullName> com o agrupamento habilitado) ao encontrar um item com um número de itens descendentes diferentes de seus vizinhos. Para o caso em que há rolagem de itens e diferentes alturas em pixels, a iteração foi introduzida no .NET Framework 4.6.1 para corrigir bugs no layout de dados hierárquicos.  Ela não será necessária se os dados forem simples (sem hierarquia) e, nesse caso, ela não ocorre no .NET Framework 4.6.2.

#### <a name="suggestion"></a>Sugestão

Se a iteração ocorrer no .NET Framework 4.6.1, mas não nas versões anteriores, ou seja, se o <xref:System.Windows.Controls.ItemsControl?displayProperty=fullName> for uma lista plana de rolagem de item com itens de alturas em pixels diferentes, haverá duas soluções:<ol><li>Instalar o .NET Framework 4.6.2.</li><li>Instalar o hotfix HR 1605 para o .NET Framework 4.6.1.</li></ol>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.1|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.VirtualizingStackPanel?displayProperty=nameWithType></li></ul>|
