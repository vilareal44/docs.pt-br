---
title: Como implementar notificação de alteração da propriedade
description: Habilite suas propriedades para notificar automaticamente uma fonte de associação quando o valor da propriedade for alterado em Windows Presentation Foundation (WPF).
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- notifications of change [WPF]
- data binding [WPF], property change notifications
- change notifications [WPF]
- properties [WPF], change notifications
ms.assetid: 30b59d9e-8c3a-4349-aa82-4be837e841cf
ms.openlocfilehash: 6c298930b7b1f812e94ea6add8f53c67d3453530
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85620775"
---
# <a name="how-to-implement-property-change-notification"></a>Como implementar notificação de alteração da propriedade
Para oferecer suporte <xref:System.Windows.Data.BindingMode.OneWay> ou <xref:System.Windows.Data.BindingMode.TwoWay> Associação para permitir que suas propriedades de destino de associação reflitam automaticamente as alterações dinâmicas da fonte de associação (por exemplo, para que o painel de visualização seja atualizado automaticamente quando o usuário editar um formulário), sua classe precisa fornecer as notificações de propriedade alteradas adequadas. Este exemplo mostra como criar uma classe que implementa <xref:System.ComponentModel.INotifyPropertyChanged> .  
  
## <a name="example"></a>Exemplo  
 Para implementar <xref:System.ComponentModel.INotifyPropertyChanged> , você precisa declarar o <xref:System.ComponentModel.INotifyPropertyChanged.PropertyChanged> evento e criar o `OnPropertyChanged` método. Em seguida, para cada propriedade para a qual você deseja alterar notificações, chame `OnPropertyChanged` sempre que a propriedade for atualizada.  
  
 [!code-csharp[SimpleBinding#PersonClass](~/samples/snippets/csharp/VS_Snippets_Wpf/SimpleBinding/CSharp/Person.cs#personclass)]
 [!code-vb[SimpleBinding#PersonClass](~/samples/snippets/visualbasic/VS_Snippets_Wpf/SimpleBinding/VisualBasic/Person.vb#personclass)]  
  
 Para ver um exemplo de como a `Person` classe pode ser usada para dar suporte à <xref:System.Windows.Data.BindingMode.TwoWay> associação, consulte [controlar quando o texto da caixa de Text atualiza a origem](how-to-control-when-the-textbox-text-updates-the-source.md).  
  
## <a name="see-also"></a>Consulte também

- [Visão geral das origens da associação](binding-sources-overview.md)
- [Visão geral da ligação de dados](../../../desktop-wpf/data/data-binding-overview.md)
- [Tópicos explicativos](data-binding-how-to-topics.md)
