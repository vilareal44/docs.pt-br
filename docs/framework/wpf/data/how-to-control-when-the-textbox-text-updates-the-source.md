---
title: Como controlar quando o texto de TextBox atualiza a origem
description: Controle o tempo de atualizações de origem de associação usando a propriedade UpdateSourceTrigger no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- source updates [WPF], timing of
- data binding [WPF], timing of source updates
- timing of source updates [WPF]
ms.assetid: ffb7b96a-351d-4c68-81e7-054033781c64
ms.openlocfilehash: 8f6eb5beb0d14a951f6e6cf6eb81e130f5a3e194
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85622777"
---
# <a name="how-to-control-when-the-textbox-text-updates-the-source"></a>Como controlar quando o texto de TextBox atualiza a origem
Este tópico descreve como usar a <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> propriedade para controlar a temporização de atualizações de origem de associação. O tópico usa o <xref:System.Windows.Controls.TextBox> controle como um exemplo.

## <a name="example"></a>Exemplo
 A <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType> propriedade tem um <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor padrão de <xref:System.Windows.Data.UpdateSourceTrigger.LostFocus> . Isso significa que se um aplicativo tiver um <xref:System.Windows.Controls.TextBox> com uma propriedade associada a dados <xref:System.Windows.Controls.TextBox.Text%2A?displayProperty=nameWithType> , o texto que você digitar no <xref:System.Windows.Controls.TextBox> não atualizará a origem até que o <xref:System.Windows.Controls.TextBox> perca o foco (por exemplo, quando você clicar fora do <xref:System.Windows.Controls.TextBox> ).

 Se você quiser que a origem seja atualizada enquanto digita, defina o <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> da associação como <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged> . No exemplo a seguir, as linhas de código realçadas mostram que as `Text` Propriedades de <xref:System.Windows.Controls.TextBox> e <xref:System.Windows.Controls.TextBlock> estão associadas à mesma propriedade Source. A <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> propriedade da <xref:System.Windows.Controls.TextBox> associação é definida como <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged> .

 [!code-xaml[SimpleBinding#USTHowTo](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Page1.xaml?highlight=33-39,41-42)]

 Como resultado, o <xref:System.Windows.Controls.TextBlock> mostra o mesmo texto (porque a origem é alterada) à medida que o usuário insere texto no <xref:System.Windows.Controls.TextBox> , conforme ilustrado pela seguinte captura de tela do exemplo:

 ![Captura de tela que mostra vinculação de dados simples.](./media/how-to-control-when-the-textbox-text-updates-the-source/data-binding-simple-binding-sample.png)

 Se você tiver uma caixa de diálogo ou um formulário editável do usuário e desejar adiar as atualizações de origem até que o usuário termine de editar os campos e clique em "OK", você poderá definir o <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor de suas associações <xref:System.Windows.Data.UpdateSourceTrigger.Explicit> como, como no exemplo a seguir:

 [!code-xaml[UpdateSource#2](~/samples/snippets/csharp/VS_Snippets_Wpf/UpdateSource/CSharp/Window1.xaml#2)]

 Quando você define o <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor como <xref:System.Windows.Data.UpdateSourceTrigger.Explicit> , o valor de origem só é alterado quando o aplicativo chama o <xref:System.Windows.Data.BindingExpression.UpdateSource%2A> método. O exemplo a seguir mostra como chamar <xref:System.Windows.Data.BindingExpression.UpdateSource%2A> para `itemNameTextBox` :

 [!code-csharp[UpdateSource#1](~/samples/snippets/csharp/VS_Snippets_Wpf/UpdateSource/CSharp/Window1.xaml.cs#1)]
 [!code-vb[UpdateSource#1](~/samples/snippets/visualbasic/VS_Snippets_Wpf/UpdateSource/VisualBasic/Window1.xaml.vb#1)]

> [!NOTE]
> Você pode usar a mesma técnica para propriedades de outros controles, mas tenha em mente que a maioria das outras propriedades tem um <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> valor padrão de <xref:System.Windows.Data.UpdateSourceTrigger.PropertyChanged> . Para obter mais informações, consulte a <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> página de propriedades.

> [!NOTE]
> A <xref:System.Windows.Data.Binding.UpdateSourceTrigger%2A> Propriedade lida com as atualizações de origem e, portanto, é relevante apenas para <xref:System.Windows.Data.BindingMode.TwoWay> <xref:System.Windows.Data.BindingMode.OneWayToSource> associações ou. Para <xref:System.Windows.Data.BindingMode.TwoWay> <xref:System.Windows.Data.BindingMode.OneWayToSource> que as associações funcionem, o objeto de origem precisa fornecer notificações de alteração de propriedade. Você pode consultar os exemplos citados neste tópico para obter mais informações. Além disso, você pode examinar [Implementar notificação de alteração de propriedade](how-to-implement-property-change-notification.md).

## <a name="see-also"></a>Consulte também

- [Tópicos explicativos](data-binding-how-to-topics.md)
