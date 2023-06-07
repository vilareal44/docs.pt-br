---
ms.openlocfilehash: d78d083b16ac034c6c393dbc0f6094ee4c6c63c0
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622337"
---
### <a name="datagridcellspanelbringindexintoview-throws-argumentoutofrangeexception"></a>DataGridCellsPanel.BringIndexIntoView gera ArgumentOutOfRangeException

#### <a name="details"></a>Detalhes

<xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)> será executado assincronamente quando a virtualização de coluna estiver habilitada, mas as larguras das colunas ainda não estiverem determinadas.  Se as colunas forem removidas antes da operação assíncrona, um <xref:System.ArgumentOutOfRangeException?displayProperty=fullName> poderá ocorrer.

#### <a name="suggestion"></a>Sugestão

Siga um destes procedimentos:<ol><li>Atualize para o .NET Framework 4.7.</li><li>Instale o patch de manutenção mais recente para o .NET Framework 4.6.2.</li><li>Evitar remover colunas até que a resposta assíncrona ao método <xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)> seja concluída.</li></ol>

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object)?displayProperty=nameWithType></li><li><xref:System.Windows.Controls.DataGrid.ScrollIntoView(System.Object,System.Windows.Controls.DataGridColumn)?displayProperty=nameWithType></li></ul>|
