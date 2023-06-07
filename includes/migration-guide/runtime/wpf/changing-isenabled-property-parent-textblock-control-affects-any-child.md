---
ms.openlocfilehash: 395463225e3c1f1d168dd019ea75966ad54e5a8a
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621025"
---
### <a name="changing-the-isenabled-property-of-the-parent-of-a-textblock-control-affects-any-child-controls"></a>Alterar a propriedade IsEnabled do pai de um controle TextBlock afeta os controles filho

#### <a name="details"></a>Detalhes

A partir do .NET Framework 4.6.2, alterar a propriedade <xref:System.Windows.UIElement.IsEnabled?displayProperty=fullName> do pai de um controle <xref:System.Windows.Controls.TextBlock?displayProperty=fullName> afetará os controles filho (como hiperlinks e botões) do controle <xref:System.Windows.Controls.TextBlock?displayProperty=fullName>. No .NET Framework 4.6.1 e versões anteriores, os controles dentro de <xref:System.Windows.Controls.TextBlock?displayProperty=fullName> nem sempre refletiam o estado da propriedade <xref:System.Windows.UIElement.IsEnabled?displayProperty=fullName> do pai <xref:System.Windows.Controls.TextBlock?displayProperty=fullName>.

#### <a name="suggestion"></a>Sugestão

Nenhum. Essa alteração está em conformidade com o comportamento esperado para controles dentro de um controle <xref:System.Windows.Controls.TextBlock?displayProperty=fullName>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Secundária|
|Versão|4.6.2|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.UIElement.IsEnabled?displayProperty=nameWithType></li></ul>|
