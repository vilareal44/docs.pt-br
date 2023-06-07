---
title: Como implementar uma propriedade de dependência
description: Defina uma propriedade de dependência em Windows Presentation Foundation, fazendo backup de uma propriedade Common Language Runtime com um campo DependencyProperty.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- dependency properties [WPF], backing properties with
- properties [WPF], backing with dependency properties
ms.assetid: 855fd6d7-19ac-493c-bf5e-2f40b57cdc92
ms.openlocfilehash: 673f653a9b02627efcccdfe08c4812ca0834217c
ms.sourcegitcommit: 87cfeb69226fef01acb17c56c86f978f4f4a13db
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87165094"
---
# <a name="how-to-implement-a-dependency-property"></a>Como implementar uma propriedade de dependência
Este exemplo mostra como fazer uma propriedade de Common Language Runtime (CLR) com um <xref:System.Windows.DependencyProperty> campo, definindo, portanto, uma propriedade de dependência. Ao definir suas próprias propriedades, se deseja dar suporte a muitos aspectos de funcionalidades do [!INCLUDE[TLA#tla_winclient](../../../../includes/tlasharptla-winclient-md.md)], incluindo estilos, vinculação de dados, herança, animação e valores padrão, você deve implementá-las como uma propriedade de dependência.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir registra primeiro uma propriedade de dependência chamando o <xref:System.Windows.DependencyProperty.Register%2A> método. O nome do campo de identificador que você usa para armazenar o nome e as características da propriedade de dependência deve ser o <xref:System.Windows.DependencyProperty.Name%2A> que você escolheu para a propriedade de dependência como parte da <xref:System.Windows.DependencyProperty.Register%2A> chamada, acrescentada pela cadeia de caracteres literal `Property` . Por exemplo, se você registrar uma propriedade de dependência com um <xref:System.Windows.DependencyProperty.Name%2A> de `Location` , o campo identificador que você definir para a propriedade de dependência deverá ser `LocationProperty` nomeado.  
  
 Neste exemplo, o nome da propriedade de dependência e seu acessador CLR é `State` ; o campo identificador é `StateProperty` ; o tipo da propriedade é <xref:System.Boolean> ; e o tipo que registra a propriedade de dependência é `MyStateControl` .  
  
 Se você não conseguir seguir esse padrão de nomenclatura, os designers poderão não relatar sua propriedade corretamente e determinados aspectos do aplicativo de estilo do sistema de propriedade poderão não se comportar conforme o esperado.  
  
 Também é possível especificar metadados padrão para uma propriedade de dependência. Esse exemplo registra o valor padrão da propriedade de dependência `State` como `false`.  
  
 [!code-csharp[PropertySystemEsoterics#MyStateControl](~/samples/snippets/csharp/VS_Snippets_Wpf/PropertySystemEsoterics/CSharp/SDKSampleLibrary/class1.cs#mystatecontrol)]
 [!code-vb[PropertySystemEsoterics#MyStateControl](~/samples/snippets/visualbasic/VS_Snippets_Wpf/PropertySystemEsoterics/visualbasic/sdksamplelibrary/class1.vb#mystatecontrol)]  
  
 Para obter mais informações sobre como e por que implementar uma propriedade de dependência, em vez de apenas fazer backup de uma propriedade CLR com um campo privado, consulte [visão geral das propriedades de dependência](dependency-properties-overview.md).  
  
## <a name="see-also"></a>Confira também

- [Visão geral das propriedades de dependência](dependency-properties-overview.md)
- [Tópicos explicativos](properties-how-to-topics.md)
