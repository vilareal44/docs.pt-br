---
ms.openlocfilehash: d0de1a262d57c67dd4dfb258d5ac013af5d8783d
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617112"
---
### <a name="wpf-textboxtext-can-be-out-of-sync-with-databinding"></a>O TextBox.Text do WPF pode estar fora de sincronia com a associação de dados

#### <a name="details"></a>Detalhes

Em alguns casos, a propriedade <xref:System.Windows.Controls.TextBox.Text> reflete um valor anterior do valor da propriedade vinculada se a propriedade é modificada durante uma operação de escrita de vinculação de dados.

#### <a name="suggestion"></a>Sugestão

Isso não deve ter nenhum impacto negativo. No entanto, é possível restaurar o comportamento anterior ao definir a propriedade <xref:System.Windows.FrameworkCompatibilityPreferences.KeepTextBoxDisplaySynchronizedWithTextProperty> como `false`.

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Microsoft Edge        |
| Versão | 4.5         |
|Type|Redirecionando

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Controls.TextBox.Text?displayProperty=nameWithType>
