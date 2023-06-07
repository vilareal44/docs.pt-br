---
title: Como associar a um método
description: Siga este exemplo para descobrir como associar ao método de um objeto no Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
helpviewer_keywords:
- data binding [WPF], binding to methods using ObjectDataProvider
- binding [WPF], to methods
- methods [WPF], binding to
ms.assetid: 5f55e71e-2182-42a0-88d1-700cc1427a7a
ms.openlocfilehash: 9501e6357203c21651e85f1d65059be1d70becf2
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619592"
---
# <a name="how-to-bind-to-a-method"></a>Como associar a um método
O exemplo a seguir mostra como associar a um método usando <xref:System.Windows.Data.ObjectDataProvider> .  
  
## <a name="example"></a>Exemplo  
 Neste exemplo, `TemperatureScale` é uma classe que tem um método `ConvertTemp`, que usa dois parâmetros (um do tipo `double` e outro do tipo `enum``TempType)` e converte o valor dado de uma escala de temperatura em outra. No exemplo a seguir, um <xref:System.Windows.Data.ObjectDataProvider> é usado para instanciar o `TemperatureScale` objeto. O método `ConvertTemp` é chamado com dois parâmetros especificados.  
  
 [!code-xaml[BindToMethod#WindowResources](~/samples/snippets/csharp/VS_Snippets_Wpf/BindToMethod/CS/Window1.xaml#windowresources)]  
  
 Agora que o método está disponível como um recurso, é possível associar aos seus resultados. No exemplo a seguir, a <xref:System.Windows.Controls.TextBox.Text%2A> propriedade de <xref:System.Windows.Controls.TextBox> e o <xref:System.Windows.Controls.Primitives.Selector.SelectedValue%2A> de <xref:System.Windows.Controls.ComboBox> está associada aos dois parâmetros do método. Isso permite que os usuários especifiquem a temperatura a ser convertida e a escala de conversão. Observe que <xref:System.Windows.Data.Binding.BindsDirectlyToSource%2A> é definido como `true` porque estamos ligando à <xref:System.Windows.Data.ObjectDataProvider.MethodParameters%2A> propriedade da <xref:System.Windows.Data.ObjectDataProvider> instância e não às propriedades do objeto encapsulado pelo <xref:System.Windows.Data.ObjectDataProvider> (o `TemperatureScale` objeto).  
  
 O <xref:System.Windows.Controls.ContentControl.Content%2A> das últimas <xref:System.Windows.Controls.Label> atualizações quando o usuário modifica o conteúdo da <xref:System.Windows.Controls.TextBox> ou a seleção do <xref:System.Windows.Controls.ComboBox> .  
  
 [!code-xaml[BindToMethod#UI](~/samples/snippets/csharp/VS_Snippets_Wpf/BindToMethod/CS/Window1.xaml#ui)]  
  
 O conversor `DoubleToString` pega um duplo e o transforma em uma cadeia de caracteres na <xref:System.Windows.Data.IValueConverter.Convert%2A> direção (da origem da associação com o destino de associação, que é a <xref:System.Windows.Controls.TextBox.Text%2A> Propriedade) e converte um em `string` um `double` na <xref:System.Windows.Data.IValueConverter.ConvertBack%2A> direção.  
  
 O `InvalidationCharacterRule` é um <xref:System.Windows.Controls.ValidationRule> que verifica se há caracteres inválidos. O modelo de erro padrão, que é uma borda vermelha em torno do <xref:System.Windows.Controls.TextBox> , parece notificar os usuários quando o valor de entrada não é um valor duplo.  
  
## <a name="see-also"></a>Consulte também

- [Tópicos explicativos](data-binding-how-to-topics.md)
- [Associar a uma enumeração](how-to-bind-to-an-enumeration.md)
