---
title: Como implementar validação de associação
description: Saiba como usar a validação de associação para fornecer comentários visuais ao usuário quando um valor inválido é inserido no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- validation of binding [WPF]
- data binding [WPF], validation of binding
- binding [WPF], validation of
ms.assetid: eb98b33d-9866-49ae-b981-bc5ff20d607a
ms.openlocfilehash: 0813be9f79a83a9dae26db1dadb58b5e973339fd
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85617876"
---
# <a name="how-to-implement-binding-validation"></a>Como implementar validação de associação

Este exemplo mostra como usar um <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> e um gatilho de estilo para fornecer comentários visuais para informar o usuário quando um valor inválido é inserido, com base em uma regra de validação personalizada.

## <a name="example"></a>Exemplo

O conteúdo de texto do <xref:System.Windows.Controls.TextBox> no exemplo a seguir é associado à `Age` Propriedade (do tipo int) de um objeto de origem de associação chamado `ods` . A associação é configurada para usar uma regra de validação chamada `AgeRangeRule` para que, se o usuário inserir caracteres não numéricos ou um valor menor que 21 ou maior que 130, um ponto de exclamação vermelho apareça ao lado da caixa de texto e uma dica de ferramenta com a mensagem de erro seja exibida quando o usuário move o mouse sobre a caixa de texto.

[!code-xaml[BindValidation#2](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#2)]

O exemplo a seguir mostra a implementação de `AgeRangeRule` , que herda de <xref:System.Windows.Controls.ValidationRule> e substitui o <xref:System.Windows.Controls.ValidationRule.Validate%2A> método. O `Int32.Parse` método é chamado no valor para garantir que ele não contenha caracteres inválidos. O <xref:System.Windows.Controls.ValidationRule.Validate%2A> método retorna um <xref:System.Windows.Controls.ValidationResult> que indica se o valor é válido com base em se uma exceção é capturada durante a análise e se o valor de idade está fora dos limites inferior e superior.

[!code-csharp[BindValidation#3](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/AgeRangeRule.cs#3)]
[!code-vb[BindValidation#3](~/samples/snippets/visualbasic/VS_Snippets_Wpf/BindValidation/VisualBasic/AgeRangeRule.vb#3)]

O exemplo a seguir mostra o personalizado <xref:System.Windows.Controls.ControlTemplate> `validationTemplate` que cria um ponto de exclamação vermelho para notificar o usuário sobre um erro de validação. Modelos de controle são usados para redefinir a aparência de um controle.

[!code-xaml[BindValidation#4](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#4)]

Conforme mostrado no exemplo a seguir, o <xref:System.Windows.Controls.ToolTip> que mostra a mensagem de erro é criado usando o estilo chamado `textBoxInError` . Se o valor de <xref:System.Windows.Controls.Validation.HasError%2A> for `true` , o gatilho definirá a dica de ferramenta do atual <xref:System.Windows.Controls.TextBox> como seu primeiro erro de validação. O <xref:System.Windows.Data.Binding.RelativeSource%2A> é definido como <xref:System.Windows.Data.RelativeSourceMode.Self> , referindo-se ao elemento atual.

[!code-xaml[BindValidation#5](~/samples/snippets/csharp/VS_Snippets_Wpf/BindValidation/CSharp/Window1.xaml#5)]

Para obter o exemplo completo, consulte [exemplo de validação de associação](https://github.com/Microsoft/WPF-Samples/tree/master/Data%20Binding/BindValidation).
  
Observe que, se você não fornecer um <xref:System.Windows.Controls.Validation.ErrorTemplate%2A> modelo de erro padrão, será exibido para fornecer comentários visuais ao usuário quando houver um erro de validação. Consulte “Validação de dados” na [Visão geral de associação de dados](../../../desktop-wpf/data/data-binding-overview.md) para obter mais informações. Além disso, [!INCLUDE[TLA2#tla_winclient](../../../../includes/tla2sharptla-winclient-md.md)] fornece uma regra de validação interna que captura exceções que são geradas durante a atualização da propriedade de origem de associação. Para obter mais informações, consulte <xref:System.Windows.Controls.ExceptionValidationRule>.

## <a name="see-also"></a>Consulte também

- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
