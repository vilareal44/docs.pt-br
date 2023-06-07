---
ms.openlocfilehash: 710d1517397f423fa40cc0c4a26c3499aac6179e
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619879"
---
### <a name="calling-itemsrefresh-on-a-wpf-listbox-listview-or-datagrid-with-items-selected-can-cause-duplicate-items-to-appear-in-the-element"></a>Chamar Items.Refresh em uma ListBox, ListView ou DataGrid do WPF com itens selecionados pode exibir itens duplicados no elemento

#### <a name="details"></a>Detalhes

No .NET Framework 4.5, chamar ListBox.Items.Refresh do código enquanto itens estiverem selecionados em uma <xref:System.Windows.Controls.ListBox?displayProperty=fullName> pode fazer com que os itens selecionados sejam duplicados na lista. Ocorre um problema semelhante com <xref:System.Windows.Controls.ListView?displayProperty=fullName> e <xref:System.Windows.Controls.DataGrid?displayProperty=fullName>. Isso foi corrigido no .NET Framework 4.6.

#### <a name="suggestion"></a>Sugestão

Esse problema pode ser contornado de forma programática cancelando a seleção dos itens antes de chamar <xref:System.Windows.Data.CollectionView.Refresh?displayProperty=fullName> e selecionando-os novamente após a conclusão da chamada. Como alternativa, esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Data.CollectionView.Refresh?displayProperty=nameWithType></li></ul>|
