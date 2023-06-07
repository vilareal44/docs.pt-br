---
title: Tipos de coleção Sorted
ms.date: 04/30/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- SortedDictionary collection type
- SortedList class, grouping data in collections
- grouping data in collections, SortedList collection type
- SortedList collection type
- collections [.NET Framework], SortedList collection type
ms.assetid: 3db965b2-36a6-4b12-b76e-7f074ff7275a
ms.openlocfilehash: 2d9d3744859eea1a09923980b3b4c57eca6bba97
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84287933"
---
# <a name="sorted-collection-types"></a>Tipos de coleção Sorted

A classe <xref:System.Collections.SortedList?displayProperty=nameWithType>, a classe genérica <xref:System.Collections.Generic.SortedList%602?displayProperty=nameWithType> e a classe genérica <xref:System.Collections.Generic.SortedDictionary%602?displayProperty=nameWithType> são semelhantes à classe <xref:System.Collections.Hashtable> e à classe genérica <xref:System.Collections.Generic.Dictionary%602>, pois elas implementam a interface <xref:System.Collections.IDictionary>, mas mantêm seus elementos em ordem de classificação por chave e não têm a inserção de O(1) nem a característica de recuperação das tabelas de hash. As três classes têm várias funcionalidades em comum:

- Todas as três classes implementam a interface <xref:System.Collections.IDictionary?displayProperty=nameWithType>. As duas classes genéricas também implementam a interface genérica <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>.

- Cada elemento é um par chave/valor para fins de enumeração.

   > [!NOTE]
   > A classe não genérica <xref:System.Collections.SortedList> retorna objetos <xref:System.Collections.DictionaryEntry> quando enumerada, embora os dois tipos genéricos retornem objetos <xref:System.Collections.Generic.KeyValuePair%602>.

- Os elementos são classificados de acordo com uma implementação de <xref:System.Collections.IComparer?displayProperty=nameWithType> (para a <xref:System.Collections.SortedList> não genérica) ou uma implementação de <xref:System.Collections.Generic.IComparer%601?displayProperty=nameWithType> (para as duas classes genéricas).

- Cada classe fornece propriedades que retornam coleções contendo apenas as chaves ou apenas os valores.

A tabela a seguir lista algumas das diferenças entre as duas classes de listas classificadas e a classe <xref:System.Collections.Generic.SortedDictionary%602>.

| <xref:System.Collections.SortedList> classe não genérica e <xref:System.Collections.Generic.SortedList%602> classe genérica | <xref:System.Collections.Generic.SortedDictionary%602> classe genérica |
|--|--|
| As propriedades que retornam chaves e valores são indexadas, permitindo uma recuperação indexada eficiente. | Recuperação não indexada. |
| A recuperação é O(log `n`). | A recuperação é O(log `n`). |
| A inserção e a remoção são geralmente O(`n`). No entanto, a inserção é O(log `n`) para dados que já estão em ordem de classificação, de forma que cada elemento seja adicionado ao final da lista. (Isso pressupõe que um redimensionamento não é necessário.) | A inserção e a remoção são O(log `n`). |
| Usa menos memória do que um <xref:System.Collections.Generic.SortedDictionary%602>. | Usa mais memória do que a classe não genérica <xref:System.Collections.SortedList> e a classe genérica <xref:System.Collections.Generic.SortedList%602>. |

Para listas ou dicionários classificados que precisam estar acessíveis simultaneamente em vários threads, você pode adicionar a lógica de classificação a uma classe derivada de <xref:System.Collections.Concurrent.ConcurrentDictionary%602>. Ao considerar a imutabilidade, os seguintes tipos imutáveis correspondentes seguem semânticas de classificação semelhantes: <xref:System.Collections.Immutable.ImmutableSortedSet%601> e <xref:System.Collections.Immutable.ImmutableSortedDictionary%602> .

> [!NOTE]
> Para valores que contêm suas próprias chaves (por exemplo, registros de funcionários que contêm um número de ID do funcionário), você pode criar uma coleção com chave que tem algumas características de uma lista e algumas características de um dicionário, derivando da classe genérica <xref:System.Collections.ObjectModel.KeyedCollection%602>.

A partir do .NET Framework 4, a classe <xref:System.Collections.Generic.SortedSet%601> fornece uma árvore de balanceamento automático que mantém os dados na ordem classificada após inserções, exclusões e pesquisas. Essa classe e a classe <xref:System.Collections.Generic.HashSet%601> implementam a interface <xref:System.Collections.Generic.ISet%601>.

## <a name="see-also"></a>Veja também

- <xref:System.Collections.IDictionary?displayProperty=nameWithType>
- <xref:System.Collections.Generic.IDictionary%602?displayProperty=nameWithType>
- <xref:System.Collections.Concurrent.ConcurrentDictionary%602>
- [Tipos de coleção comumente usados](commonly-used-collection-types.md)
