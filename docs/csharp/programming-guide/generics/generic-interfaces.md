---
title: Interfaces genéricas – Guia de Programação em C#
description: Saiba mais sobre como usar interfaces genéricas em C#. Consulte exemplos de código e exiba recursos adicionais disponíveis.
ms.date: 07/20/2015
helpviewer_keywords:
- C# language, generic interfaces
- generics [C#], interfaces
ms.assetid: a8fa49a1-6e78-4a09-87e5-84a0b9f5ffbe
ms.openlocfilehash: 43817a236e95b3ab8fd0ba94da98457eeec2396c
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301886"
---
# <a name="generic-interfaces-c-programming-guide"></a>Interfaces genéricas (Guia de Programação em C#)
Muitas vezes, é útil definir interfaces para classes de coleção genéricas ou para as classe genéricas que representam itens na coleção. Para classes genéricas, prefere-se usar interfaces genéricas, como <xref:System.IComparable%601> em vez de <xref:System.IComparable>, a fim de evitar operações de conversão boxing e unboxing em tipos de valor. A biblioteca de classes .NET Framework define várias interfaces genéricas para uso com as classes de coleção no namespace <xref:System.Collections.Generic>.  
  
 Quando uma interface é especificada como uma restrição em um parâmetro de tipo, somente os tipos que implementam a interface podem ser usados. O exemplo de código a seguir mostra uma classe `SortedList<T>` que deriva da classe `GenericList<T>`. Para obter mais informações, consulte [Introdução aos Genéricos](./index.md). `SortedList<T>` adiciona a restrição `where T : IComparable<T>`. Isso habilita o método `BubbleSort` em `SortedList<T>` a usar o método genérico <xref:System.IComparable%601.CompareTo%2A> em elementos de lista. Neste exemplo, os elementos de lista são uma classe simples, `Person`, que implementa `IComparable<Person>`.  
  
 [!code-csharp[csProgGuideGenerics#29](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics2.cs#29)]  
  
 Várias interfaces podem ser especificadas como restrições em um único tipo, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#30](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#30)]  
  
 Uma interface pode definir mais de um parâmetro de tipo, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#31](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#31)]  
  
 As regras de herança que se aplicam às classes também se aplicam às interfaces:  
  
 [!code-csharp[csProgGuideGenerics#32](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#32)]  
  
 Interfaces genéricas poderão herdar de interfaces não genéricas se a interface genérica for contravariante, o que significa que ela usa apenas seu parâmetro de tipo como um valor retornado. Na biblioteca de classes do .NET Framework, <xref:System.Collections.Generic.IEnumerable%601> herda de <xref:System.Collections.IEnumerable> porque <xref:System.Collections.Generic.IEnumerable%601> usa apenas `T` no valor retornado de <xref:System.Collections.Generic.IEnumerable%601.GetEnumerator%2A> e no getter de propriedade <xref:System.Collections.Generic.IEnumerator%601.Current%2A>.  
  
 Classes concretas podem implementar interfaces construídas fechadas, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#33](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#33)]  
  
 Classes genéricas podem implementar interfaces genéricas ou interfaces construídas fechadas, contanto que a lista de parâmetros de classe forneça todos os argumentos exigidos pela interface, da seguinte maneira:  
  
 [!code-csharp[csProgGuideGenerics#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/csProgGuideGenerics/CS/Generics.cs#34)]  
  
 As regras que controlam a sobrecarga de método são as mesmas para métodos em classes genéricas, structs genéricos ou interfaces genéricas. Para obter mais informações, consulte [Métodos Genéricos](./generic-methods.md).  
  
## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Introdução aos genéricos](./index.md)
- [interface](../../language-reference/keywords/interface.md)
- [Genéricos](../../../standard/generics/index.md)
