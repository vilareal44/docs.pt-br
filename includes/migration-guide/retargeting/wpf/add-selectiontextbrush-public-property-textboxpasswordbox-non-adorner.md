---
ms.openlocfilehash: aade03c29ff16053a97683499cf719a98ae33f3f
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617782"
---
### <a name="add-selectiontextbrush-public-property-to-textboxpasswordbox-non-adorner-selection"></a>Adicionar a propriedade pública SelectionTextBrush à seleção não baseada em adorno TextBox/PasswordBox

#### <a name="details"></a>Detalhes

Em aplicativos do WPF que usam a [seleção de texto não baseada em adorno](https://github.com/Microsoft/dotnet/blob/master/Documentation/compatibility/wpf-TextBox-PasswordBox-text-selection-does-not-follow-system-colors.md) para <xref:System.Windows.Controls.TextBox> e <xref:System.Windows.Controls.PasswordBox>, os desenvolvedores podem agora definir a propriedade SelectionTextBrush recém-adicionada para alterar a renderização do texto selecionado.  Por padrão, essa cor é alterada com <xref:System.Windows.SystemColors.HighlightTextBrushKey>.  Se a seleção de texto não baseada em adorno não estiver habilitada, essa propriedade não fará nada.

#### <a name="suggestion"></a>Sugestão

Depois que a seleção de texto não baseada em adorno estiver habilitada, será possível usar a propriedade <xref:System.Windows.Controls.PasswordBox.SelectionTextBrush?displayProperty=nameWithType> e <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrush> para alterar a aparência do texto selecionado. Isso pode ser feito usando XAML:

<pre><code class="lang-xaml">&lt;TextBox SelectionBrush=&quot;Red&quot; SelectionTextBrush=&quot;White&quot;  SelectionOpacity=&quot;0.5&quot;&#13;&#10;Foreground=&quot;Blue&quot; CaretBrush=&quot;Blue&quot;&gt;&#13;&#10;This is some text.&#13;&#10;&lt;/TextBox&gt;&#13;&#10;</code></pre>

| Name    | Valor       |
|:--------|:------------|
| Escopo   | Principal       |
| Versão | 4.8         |
| Type    | Redirecionando |

#### <a name="affected-apis"></a>APIs afetadas

- <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrushProperty?displayProperty=nameWithType>
- <xref:System.Windows.Controls.Primitives.TextBoxBase.SelectionTextBrush?displayProperty=nameWithType>
- [System.Windows.Controls.TextBox](xref:System.Windows.Controls.TextBox)
- [System.Windows.Controls.PasswordBox](xref:System.Windows.Controls.PasswordBox)
