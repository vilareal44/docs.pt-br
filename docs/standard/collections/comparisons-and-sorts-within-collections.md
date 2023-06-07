---
title: Comparações e classificações dentro de coleções
description: As comparações & classificações usando as classes System. Collections no .NET, que ajudam a localizar um elemento para remover ou retornar o valor de um par de chave e valor.
ms.date: 04/30/2020
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- sorting data, collections
- IComparable.CompareTo method
- Collections classes
- Equals method
- collections [.NET Framework], comparisons
ms.assetid: 5e4d3b45-97f0-423c-a65f-c492ed40e73b
ms.openlocfilehash: aa001e8469947a532d77059bd52024c6b47b508e
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769191"
---
# <a name="comparisons-and-sorts-within-collections"></a>Comparações e classificações dentro de coleções

As classes <xref:System.Collections> executam comparações em quase todos os processos envolvidos no gerenciamento de coleções, seja procura pelo elemento para remoção ou retorno do valor de um par de chaves e valores.

As coleções normalmente usam um comparador de igualdade e/ou um comparador de ordenação. Dois constructos são usados para comparações.

<a name="BKMK_Checkingforequality"></a>
## <a name="check-for-equality"></a>Verificar igualdade

Métodos como `Contains`, <xref:System.Collections.IList.IndexOf%2A>, <xref:System.Collections.Generic.List%601.LastIndexOf%2A> e `Remove` usam um comparador de igualdade para os elementos da coleção. Se a coleção for genérica, os itens serão comparados para igualdade de acordo com as seguintes diretrizes:

- Se o tipo T implementa a interface genérica <xref:System.IEquatable%601>, então o comparador de igualdade será o método <xref:System.IEquatable%601.Equals%2A> dessa interface.

- Se o tipo T não implementar <xref:System.IEquatable%601>, <xref:System.Object.Equals%2A?displayProperty=nameWithType> será usado.

Além disso, algumas sobrecargas de construtores para coleções de dicionários aceitam uma implementação <xref:System.Collections.Generic.IEqualityComparer%601>, que é usada para comparar chaves com relação à igualdade. Para ver um exemplo, consulte o construtor <xref:System.Collections.Generic.Dictionary%602.%23ctor%2A>.

<a name="BKMK_Determiningsortorder"></a>
## <a name="determine-sort-order"></a>Determinar ordem de classificação

Métodos como `BinarySearch` e `Sort` usam um comparador de ordenação para os elementos da coleção. As comparações podem ser entre elementos da coleção ou entre um elemento e um valor especificado. Para comparar objetos, há o conceito de um `default comparer` e um `explicit comparer`.

O comparador padrão baseia-se em pelo menos um dos objetos que estão sendo comparados para implementar a interface **IComparable**. É uma boa prática implementar **IComparable** em todas as classes usadas como valores em uma coleção de lista ou como chaves em uma coleção de dicionário. Para uma coleção genérica, a comparação de igualdade é determinada de acordo com o seguinte:

- Se o tipo T implementa a interface genérica <xref:System.IComparable%601?displayProperty=nameWithType>, então o comparador padrão será o método <xref:System.IComparable%601.CompareTo%28%600%29?displayProperty=nameWithType> dessa interface.

- Se o tipo T implementa a interface não genérica <xref:System.IComparable?displayProperty=nameWithType>, então o comparador padrão será o método <xref:System.IComparable.CompareTo%28System.Object%29?displayProperty=nameWithType> dessa interface.

- Se o tipo T não implementa a interface, não há nenhum comparador padrão, e um representante de comparação ou comparador deve ser fornecido explicitamente.

Para fornecer comparações explícitas, alguns métodos aceitam uma implementação de **IComparer** como um parâmetro. Por exemplo, o método <xref:System.Collections.Generic.List%601.Sort%2A?displayProperty=nameWithType> aceita uma implementação <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType>.

A configuração de cultura atual do sistema pode afetar as comparações e as classificações dentro de uma coleção. Por padrão, as comparações e classificações nas classes **Collections** levam em conta a cultura. Para ignorar a configuração de cultura e assim obter comparação consistente e classificar os resultados, use o <xref:System.Globalization.CultureInfo.InvariantCulture%2A> com sobrecargas de membros que aceitam uma <xref:System.Globalization.CultureInfo>. Para obter mais informações, consulte [Executando operações de cadeia de caracteres que não levam em conta a cultura em coleções](../globalization-localization/performing-culture-insensitive-string-operations-in-collections.md) e [Executando operações de cadeia de caracteres que não levam em conta a cultura em matrizes](../globalization-localization/performing-culture-insensitive-string-operations-in-arrays.md).

<a name="BKMK_Equalityandsortexample"></a>
## <a name="equality-and-sort-example"></a>Exemplo de igualdade e classificação

O código a seguir demonstra uma implementação de <xref:System.IEquatable%601> e <xref:System.IComparable%601> em um objeto de negócios simples. Além disso, quando o objeto é armazenado em uma lista e classificado, você verá que chamar o método <xref:System.Collections.Generic.List%601.Sort> resulta no uso do comparador padrão para o tipo `Part` e o método <xref:System.Collections.Generic.List%601.Sort%28System.Comparison%7B%600%7D%29> implementado usando um método anônimo.

[!code-csharp[System.Collections.Generic.List.Sort#1](../../../samples/snippets/csharp/VS_Snippets_CLR_System/system.collections.generic.list.sort/cs/program.cs#1)]
[!code-vb[System.Collections.Generic.List.Sort#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR_System/system.collections.generic.list.sort/vb/module1.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Collections.IComparer>
- <xref:System.IEquatable%601>
- <xref:System.Collections.Generic.IComparer%601>
- <xref:System.IComparable>
- <xref:System.IComparable%601>
