---
ms.openlocfilehash: 13d3799aeede86b01aa81ce1cd69b3c4c22311ca
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619899"
---
### <a name="wpf-textbox-defaults-to-undo-limit-of-100"></a>O padrão da Caixa de Texto do WPF é o limite de 100 na ação desfazer

#### <a name="details"></a>Detalhes

No .NET Framework 4.5, o limite padrão de desfazer para uma caixa de texto do WPF é 100 (em vez de ser ilimitado como no .NET Framework 4.0)

#### <a name="suggestion"></a>Sugestão

Se um limite de 100 da ação desfazer for muito baixo, o limite poderá ser definido explicitamente com <xref:System.Windows.Controls.Primitives.TextBoxBase.UndoLimit>.

| Name    | Valor       |
|:--------|:------------|
| Escopo   |Microsoft Edge|
|Versão|4.5|
|Type|Runtime

#### <a name="affected-apis"></a>APIs afetadas

-<xref:System.Windows.Controls.TextBox?displayProperty=nameWithType></li></ul>|
