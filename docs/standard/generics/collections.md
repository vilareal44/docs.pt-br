---
title: Coleções genéricas no .NET
ms.date: 02/15/2018
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- generics [.NET], collections
- generic collections [.NET]
- generic types [.NET]
ms.assetid: 5b646751-6ab7-465c-916c-b1a76aefa9f5
ms.openlocfilehash: 5767bac0bb1e3ae9e586e9a10d8452d421519447
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287565"
---
# <a name="generic-collections-in-net"></a>Coleções genéricas no .NET

 A biblioteca de classes do .NET fornece várias classes de coleção genérica nos namespaces <xref:System.Collections.Generic> e <xref:System.Collections.ObjectModel>. Para obter informações detalhadas sobre essas classes, consulte [Tipos de coleção comumente usados](../collections/commonly-used-collection-types.md).  
  
## <a name="systemcollectionsgeneric"></a>System.Collections.Generic

 Muitos dos tipos de coleção genéricos são diretamente análogos aos tipos não genéricos. <xref:System.Collections.Generic.Dictionary%602> é uma versão genérica de <xref:System.Collections.Hashtable>; ele usa a estrutura genérica <xref:System.Collections.Generic.KeyValuePair%602> para enumeração em vez de <xref:System.Collections.DictionaryEntry>.  
  
 <xref:System.Collections.Generic.List%601> é uma versão genérica de <xref:System.Collections.ArrayList>. Há classes <xref:System.Collections.Generic.Queue%601> e <xref:System.Collections.Generic.Stack%601> genéricas que correspondem às versões não genéricas.  
  
 Há versões genéricas e não genéricas de <xref:System.Collections.Generic.SortedList%602>. As duas versões são híbridas de um dicionário e de uma lista. A classe genérica <xref:System.Collections.Generic.SortedDictionary%602> é um dicionário puro e não tem nenhum equivalente não genérico.  
  
 A classe genérica <xref:System.Collections.Generic.LinkedList%601> é uma verdadeira lista vinculada. Não tem nenhum equivalente não genérico.  
  
## <a name="systemcollectionsobjectmodel"></a>System.Collections.ObjectModel

 A classe genérica <xref:System.Collections.ObjectModel.Collection%601> fornece uma classe base para derivar seus próprios tipos de coleção genérica. A classe <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> fornece uma maneira fácil de produzir uma coleção somente leitura de qualquer tipo que implementa a interface genérica <xref:System.Collections.Generic.IList%601>. A classe genérica <xref:System.Collections.ObjectModel.KeyedCollection%602> fornece uma maneira de armazenar objetos que contêm suas próprias chaves.  
  
## <a name="other-generic-types"></a>Outros tipos genéricos

 A estrutura genérica <xref:System.Nullable%601> permite que você use tipos de valor como se eles pudessem ser atribuídos `null`. Isso pode ser útil ao trabalhar com consultas de banco de dados, nas quais os campos que contêm tipos de valor podem estar ausentes. O parâmetro de tipo genérico pode ser qualquer tipo de valor.  
  
> [!NOTE]
> No C# e Visual Basic não é necessário usar <xref:System.Nullable%601> explicitamente, pois a linguagem tem sintaxe para tipos que permitem valor nulo. Consulte [tipos de valores anuláveis (referência C#)](../../csharp/language-reference/builtin-types/nullable-value-types.md) e [tipos de valores anuláveis (Visual Basic)](../../visual-basic/programming-guide/language-features/data-types/nullable-value-types.md).
  
 A estrutura genérica <xref:System.ArraySegment%601> fornece uma maneira de delimitar um intervalo de elementos dentro de uma matriz unidimensional baseada em zero de qualquer tipo. O parâmetro de tipo genérico é o tipo dos elementos da matriz.  
  
 O delegado genérico <xref:System.EventHandler%601> eliminará a necessidade de declarar um tipo de delegado para manipular eventos, se o evento seguir o padrão de manipulação de eventos usado pelo .NET Framework. Por exemplo, vamos supor que você tenha criado uma classe `MyEventArgs`, derivada de <xref:System.EventArgs>, para manter os dados para o evento. Em seguida, você pode declarar o evento da seguinte maneira:  
  
 [!code-cpp[Conceptual.Generics.Overview#7](../../../samples/snippets/cpp/VS_Snippets_CLR/conceptual.generics.overview/cpp/source2.cpp#7)]
 [!code-csharp[Conceptual.Generics.Overview#7](../../../samples/snippets/csharp/VS_Snippets_CLR/conceptual.generics.overview/cs/source2.cs#7)]
 [!code-vb[Conceptual.Generics.Overview#7](../../../samples/snippets/visualbasic/VS_Snippets_CLR/conceptual.generics.overview/vb/source2.vb#7)]  
  
## <a name="see-also"></a>Veja também

- <xref:System.Collections.Generic?displayProperty=nameWithType>
- <xref:System.Collections.ObjectModel?displayProperty=nameWithType>
- [Genéricos](index.md)
- [Delegados genéricos para manipular matrizes e listas](delegates-for-manipulating-arrays-and-lists.md)
- [Interfaces genéricas](interfaces.md)
