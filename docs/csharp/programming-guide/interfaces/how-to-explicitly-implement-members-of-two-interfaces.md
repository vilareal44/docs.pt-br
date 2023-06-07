---
title: Como implementar explicitamente membros de duas interfaces – guia de programação C#
description: Saiba como implementar explicitamente duas interfaces que têm os mesmos nomes de membro e dar a cada membro da interface uma implementação separada neste exemplo de C#.
ms.date: 07/20/2015
helpviewer_keywords:
- inheritance [C#], explicitly implementing interface members
- interfaces [C#], explicitly implementing with inheritance
ms.assetid: 8b402ddc-dff9-4869-89cb-d718c764e68e
ms.openlocfilehash: d60ec43f734d5e8bfa7f467874440bd3514877fe
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303056"
---
# <a name="how-to-explicitly-implement-members-of-two-interfaces-c-programming-guide"></a>Como implementar explicitamente membros de duas interfaces (guia de programação C#)
A implementação explícita da [interface](../../language-reference/keywords/interface.md) também permite ao programador implementar duas interfaces que têm os mesmos nomes de membro e implementar separadamente cada membro de interface. Este exemplo exibe as dimensões de uma caixa em unidades inglesas e no sistema métrico. A Caixa [classe](../../language-reference/keywords/class.md) implementa duas interfaces, IEnglishDimensions e IMetricDimensions, que representam os diferentes sistemas de medida. As duas interfaces têm nomes de membro idênticos, Comprimento e Largura.  
  
## <a name="example"></a>Exemplo  
 [!code-csharp[csProgGuideInheritance#9](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#9)]  
  
## <a name="robust-programming"></a>Programação robusta  
 Caso deseje que as medidas padrão estejam unidades inglesas, implemente os métodos Comprimento e Largura normalmente e implemente explicitamente os métodos Comprimento e Largura da interface IMetricDimensions:  
  
 [!code-csharp[csProgGuideInheritance#10](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#10)]  
  
 Nesse caso, é possível acessar as unidades inglesas da instância de classe e acessar as unidades métricas da instância da interface:  
  
 [!code-csharp[csProgGuideInheritance#11](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideInheritance/CS/Inheritance.cs#11)]  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Classes e structs](../classes-and-structs/index.md)
- [Interfaces](./index.md)
- [Como implementar membros de interface explicitamente](./how-to-explicitly-implement-interface-members.md)
