---
title: Quando usar coleções genéricas
ms.date: 04/30/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- collections [.NET Framework], generic
- generic collections [.NET Framework]
ms.assetid: e7b868b1-11fe-4ac5-bed3-de68aca47739
ms.openlocfilehash: c59a125a8df95e3c4fe6e1839956d800bd6ee910
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84290377"
---
# <a name="when-to-use-generic-collections"></a>Quando usar coleções genéricas

O uso de coleções genéricas oferece o benefício automático da segurança de tipo sem a necessidade de derivar de um tipo de coleção base e implementar membros específicos de tipo. Geralmente, os tipos de coleção genéricos também têm melhor desempenho que os tipos de coleção não genéricos correspondentes (e melhores que os tipos derivados de tipos de coleção base não genéricos) quando os elementos de coleção são tipos de valor, porque com os genéricos, não há necessidade de box os elementos.

Para programas direcionados .NET Standard 1,0 ou posterior, use as classes de coleção genéricas no <xref:System.Collections.Concurrent> namespace quando vários threads podem estar adicionando ou removendo itens da coleção simultaneamente. Além disso, quando a imutabilidade é desejada, considere as classes de coleção genéricas no <xref:System.Collections.Immutable> namespace.

Os seguintes tipos genéricos correspondem aos tipos de coleção existentes:

- <xref:System.Collections.Generic.List%601> é a classe genérica que corresponde à <xref:System.Collections.ArrayList>.

- <xref:System.Collections.Generic.Dictionary%602> e <xref:System.Collections.Concurrent.ConcurrentDictionary%602> são as classes genéricas que correspondem à <xref:System.Collections.Hashtable>.

- <xref:System.Collections.ObjectModel.Collection%601> é a classe genérica que corresponde à <xref:System.Collections.CollectionBase>. <xref:System.Collections.ObjectModel.Collection%601>pode ser usado como uma classe base, mas, ao contrário <xref:System.Collections.CollectionBase> de, não é abstrato, o que o torna muito mais fácil de usar.

- <xref:System.Collections.ObjectModel.ReadOnlyCollection%601> é a classe genérica que corresponde à <xref:System.Collections.ReadOnlyCollectionBase>. <xref:System.Collections.ObjectModel.ReadOnlyCollection%601>Não é abstrato e tem um construtor que torna mais fácil expor um existente <xref:System.Collections.Generic.List%601> como uma coleção somente leitura.

- As <xref:System.Collections.Generic.Queue%601> <xref:System.Collections.Concurrent.ConcurrentQueue%601> <xref:System.Collections.Immutable.ImmutableQueue%601> classes genéricas,,,, <xref:System.Collections.Immutable.ImmutableArray%601> <xref:System.Collections.Generic.SortedList%602> e <xref:System.Collections.Immutable.ImmutableSortedSet%601> correspondem às respectivas classes não genéricas com os mesmos nomes.

## <a name="additional-types"></a>Tipos adicionais

Vários tipos de coleção genérica não têm equivalentes não genéricas. Eles incluem o seguinte:

- <xref:System.Collections.Generic.LinkedList%601> é uma lista vinculada de uso geral que fornece operações de inserção e remoção O(1).

- <xref:System.Collections.Generic.SortedDictionary%602> é um dicionário classificado com operações de inserção e recuperação O(log `n`), o que o torna uma alternativa útil para <xref:System.Collections.Generic.SortedList%602>.

- <xref:System.Collections.ObjectModel.KeyedCollection%602> é um híbrido entre uma lista e um dicionário, que fornece uma maneira de armazenar objetos que contenham suas próprias chaves.

- <xref:System.Collections.Concurrent.BlockingCollection%601> implementa uma classe de coleção com funcionalidade de delimitação e bloqueio.

- <xref:System.Collections.Concurrent.ConcurrentBag%601> fornece rápida inserção e remoção de elementos não classificados.

### <a name="immutable-builders"></a>Construtores imutáveis

Quando você deseja a funcionalidade de imutabilidade em seu aplicativo, o <xref:System.Collections.Immutable> namespace oferece tipos de coleção genéricos que você pode usar. Todos os tipos de coleção imutáveis oferecem `Builder` classes que podem otimizar o desempenho quando você estiver realizando várias mutações. A `Builder` classe agrupa operações em um estado mutável. Quando todas as mutações tiverem sido concluídas, chame o `ToImmutable` método para "congelar" todos os nós e crie uma coleção genérica imutável, por exemplo, um <xref:System.Collections.Immutable.ImmutableList%601> .

O `Builder` objeto pode ser criado chamando o método não genérico `CreateBuilder()` . Em uma `Builder` instância do, você pode chamar `ToImmutable()` . Da mesma forma, da `Immutable*` coleção, você pode chamar `ToBuilder()` para criar uma instância de construtor da coleção imutável genérica. A seguir estão os vários `Builder` tipos.

- <xref:System.Collections.Immutable.ImmutableArray%601.Builder>
- <xref:System.Collections.Immutable.ImmutableDictionary%602.Builder>
- <xref:System.Collections.Immutable.ImmutableHashSet%601.Builder>
- <xref:System.Collections.Immutable.ImmutableList%601.Builder>
- <xref:System.Collections.Immutable.ImmutableSortedDictionary%602.Builder>
- <xref:System.Collections.Immutable.ImmutableSortedSet%601.Builder>

## <a name="linq-to-objects"></a>Objetos LINQ to

O recurso LINQ para objetos permite que você use consultas LINQ para acessar objetos na memória, desde que o tipo de objeto implemente a interface <xref:System.Collections.IEnumerable?displayProperty=nameWithType> ou <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType>. Consultas LINQ fornecem um padrão comum para acessar dados; normalmente são mais concisos e legíveis do que `foreach` loops padrão e fornecem recursos de filtragem, classificação e agrupamento. Consultas LINQ também podem melhorar o desempenho. Para obter mais informações, confira [LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md), [LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md) e [PLINQ (Parallel LINQ)](../parallel-programming/introduction-to-plinq.md).

## <a name="additional-functionality"></a>Funcionalidade adicional
Alguns dos tipos genéricos possuem funcionalidades que não se encontram em tipos de coleção genérica. Por exemplo, a classe <xref:System.Collections.Generic.List%601>, que corresponde à classe <xref:System.Collections.ArrayList> não genérica, possui vários métodos que aceitam delegados genéricos, tais como o delegado <xref:System.Predicate%601> que permite que você especifique métodos para pesquisa na lista, o delegado <xref:System.Action%601> que representa métodos que atuam em cada elemento da lista e o delegado <xref:System.Converter%602> que permite definir conversões entre tipos.

A classe <xref:System.Collections.Generic.List%601> permite que você especifique suas próprias implementações de interface genérica <xref:System.Collections.Generic.IComparer%601> para classificação e pesquisa na lista. As classes <xref:System.Collections.Generic.SortedDictionary%602> e <xref:System.Collections.Generic.SortedList%602> também possuem esse recurso. Além disso, essas classes permitem que você especifique comparadores quando a coleção for criada. De maneira semelhante, as classes <xref:System.Collections.Generic.Dictionary%602> e <xref:System.Collections.ObjectModel.KeyedCollection%602> permitem que você especifique seus próprios comparadores de igualdade.

## <a name="see-also"></a>Veja também

- [Coleções e estruturas de dados](index.md)
- [Tipos de coleção comumente usados](commonly-used-collection-types.md)
- [Genéricos](../generics/index.md)
