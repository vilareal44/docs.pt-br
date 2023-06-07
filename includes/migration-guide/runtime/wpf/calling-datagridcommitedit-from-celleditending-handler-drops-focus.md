---
ms.openlocfilehash: c3c3ed44cf53625c246dfe0408bb861750ecf336
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621919"
---
### <a name="calling-datagridcommitedit-from-a-celleditending-handler-drops-focus"></a>Chamar DataGrid.CommitEdit de um manipulador CellEditEnding descarta o foco

#### <a name="details"></a>Detalhes

Chamar <xref:System.Windows.Controls.DataGrid.CommitEdit> de um dos manipuladores de eventos <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> do <xref:System.Windows.Controls.DataGrid.CellEditEnding?displayProperty=fullName> faz com que <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> perca o foco.

#### <a name="suggestion"></a>Sugestão

Esse bug foi corrigido no .NET Framework 4.5.2, portanto, ele pode ser evitado com a atualização do .NET Framework. Como alternativa, ele pode ser evitado com a nova seleção explícita de <xref:System.Windows.Controls.DataGrid?displayProperty=fullName> após chamada de <xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=fullName>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.DataGrid.CommitEdit?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.DataGrid.CommitEdit(System.Windows.Controls.DataGridEditingUnit,System.Boolean)?displayProperty=nameWithType></li></ul>|
