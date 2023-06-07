---
ms.openlocfilehash: 7ac0cac53ab2fa7657d0ae58f11d9e777631acc9
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619884"
---
### <a name="accessing-a-wpf-datagrids-selected-items-from-a-handler-of-the-datagrids-unloadingrow-event-can-cause-a-nullreferenceexception"></a>Acessar os itens selecionados de um DataGrid do WPF em um manipulador do evento UnloadingRow do DataGrid pode gerar NullReferenceException

#### <a name="details"></a>Detalhes

Por causa de um bug no .NET Framework 4.5, os manipuladores de eventos <xref:System.Windows.Controls.DataGrid> que envolvem a remoção de uma linha podem gerar <xref:System.NullReferenceException?displayProperty=fullName> se acessarem as propriedades <xref:System.Windows.Controls.Primitives.Selector.SelectedItem?displayProperty=fullName> ou <xref:System.Windows.Controls.Primitives.MultiSelector.SelectedItems?displayProperty=fullName> de <xref:System.Windows.Controls.DataGrid>.

#### <a name="suggestion"></a>Sugestão

Esse problema foi corrigido no .NET Framework 4.6 e pode ser resolvido com o upgrade para essa versão do .NET Framework.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.DataGrid.UnloadingRow?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.DataGrid.UnloadingRowDetails?displayProperty=nameWithType></li></ul>|
