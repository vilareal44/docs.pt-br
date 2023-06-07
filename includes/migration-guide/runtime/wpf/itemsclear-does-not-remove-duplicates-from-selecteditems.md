---
ms.openlocfilehash: 75f176133697056bab9349ba1d18d7a0e1aa7da2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619762"
---
### <a name="itemsclear-does-not-remove-duplicates-from-selecteditems"></a>Items.Clear não remove duplicatas de SelectedItems

#### <a name="details"></a>Detalhes

Suponha que um Seletor (com seleção múltipla habilitada) tenha duplicatas em sua coleção <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName> – o mesmo item é exibido mais de uma vez.  A remoção desses itens da fonte de dados (por exemplo, chamando Items.Clear) falha ao removê-los de <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName>; somente a primeira instância é removida. Além disso, o uso subsequente de <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName> (por exemplo, SelectedItems.Clear()) pode encontrar problemas, como <xref:System.ArgumentException?displayProperty=fullName>, pois <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName> contém itens que não estão mais na fonte de dados.

#### <a name="suggestion"></a>Sugestão

Atualize se possível para o .NET Framework 4.6.2.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=nameWithType></li></ul>|
