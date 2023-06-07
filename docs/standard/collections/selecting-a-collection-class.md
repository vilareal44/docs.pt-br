---
title: Selecionando uma classe de coleção
description: Saiba como decidir qual classe de coleção no .NET escolher. A utilização do tipo errado pode restringir o uso da coleção.
ms.date: 03/18/2019
ms.technology: dotnet-standard
helpviewer_keywords:
- last-in-first-out collections
- first-in-first-out collections
- collections [.NET Framework], selecting collection class
- indexed collections
- Collections classes
- grouping data in collections, selecting collection class
ms.assetid: ba049f9a-ce87-4cc4-b319-3f75c8ddac8a
ms.openlocfilehash: 52a839661a09d6fa7561d67b82d1c1bf854e3cfd
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600809"
---
# <a name="selecting-a-collection-class"></a>Selecionando uma classe de coleção

Certifique-se de escolher sua classe de coleção com cuidado. A utilização do tipo errado pode restringir o uso da coleção.

> [!IMPORTANT]
> Evite usar os tipos no namespace <xref:System.Collections>. As versões genéricas e simultâneas das coleções são as recomendadas devido à maior segurança de tipos e outras melhorias.

Considere as perguntas a seguir:

- Você precisa de uma lista sequencial em que o elemento normalmente será descartado após seu valor ser recuperado?

  - Em caso afirmativo, considere usar a classe <xref:System.Collections.Queue> ou a classe genérica <xref:System.Collections.Generic.Queue%601> caso precise do comportamento PEPS (primeiro a entrar, primeiro a sair). Considere usar a classe <xref:System.Collections.Stack> ou a classe genérica <xref:System.Collections.Generic.Stack%601> caso precise do comportamento UEPS (último a entrar, primeiro a sair). Para obter acesso seguro de vários threads, use as versões simultâneas <xref:System.Collections.Concurrent.ConcurrentQueue%601> e <xref:System.Collections.Concurrent.ConcurrentStack%601>. Para imutabilidade, considere as versões imutáveis <xref:System.Collections.Immutable.ImmutableQueue%601> e <xref:System.Collections.Immutable.ImmutableStack%601> .

  - Caso contrário, considere usar outras coleções.

- Você precisa acessar os elementos em uma ordem específica, como PEPS, UEPS ou aleatória?

  - A <xref:System.Collections.Queue> classe, bem como as <xref:System.Collections.Generic.Queue%601> <xref:System.Collections.Concurrent.ConcurrentQueue%601> classes genéricas,, e <xref:System.Collections.Immutable.ImmutableQueue%601> todas oferecem acesso PEPS. Para obter mais informações, consulte [Quando usar uma coleção thread-safe](thread-safe/when-to-use-a-thread-safe-collection.md).

  - A <xref:System.Collections.Stack> classe, bem como as <xref:System.Collections.Generic.Stack%601> <xref:System.Collections.Concurrent.ConcurrentStack%601> classes genéricas,, e <xref:System.Collections.Immutable.ImmutableStack%601> todas oferecem acesso de UEPS. Para obter mais informações, consulte [Quando usar uma coleção thread-safe](thread-safe/when-to-use-a-thread-safe-collection.md).

  - A classe genérica <xref:System.Collections.Generic.LinkedList%601> permite o acesso sequencial de ponta a ponta.

- Você precisa acessar cada elemento pelo índice?

  - As classes <xref:System.Collections.ArrayList> e <xref:System.Collections.Specialized.StringCollection> e a classe genérica <xref:System.Collections.Generic.List%601> oferecem acesso aos seus elementos pelo índice baseado em zero do elemento. Para imutabilidade, considere as versões genéricas imutáveis <xref:System.Collections.Immutable.ImmutableArray%601> e <xref:System.Collections.Immutable.ImmutableList%601> .

  - As classes <xref:System.Collections.Hashtable>, <xref:System.Collections.SortedList>, <xref:System.Collections.Specialized.ListDictionary> e <xref:System.Collections.Specialized.StringDictionary>, e as classes genéricas <xref:System.Collections.Generic.Dictionary%602> e <xref:System.Collections.Generic.SortedDictionary%602>, oferecem acesso aos seus elementos pela chave do elemento. Além disso, há versões imutáveis de vários tipos correspondentes: <xref:System.Collections.Immutable.ImmutableHashSet%601> ,, <xref:System.Collections.Immutable.ImmutableDictionary%602> <xref:System.Collections.Immutable.ImmutableSortedSet%601> e <xref:System.Collections.Immutable.ImmutableSortedDictionary%602> .

  - As classes <xref:System.Collections.Specialized.NameObjectCollectionBase> e <xref:System.Collections.Specialized.NameValueCollection>, e as classes genéricas <xref:System.Collections.ObjectModel.KeyedCollection%602> e <xref:System.Collections.Generic.SortedList%602>, oferecem acesso aos seus elementos pelo índice baseado em zero ou pela chave do elemento.

- Cada elemento conterá um valor, uma combinação de uma chave e um valor ou uma combinação de uma chave e diversos valores?

  - Um valor: use qualquer uma das coleções baseadas na interface do <xref:System.Collections.IList> ou na interface genérica do <xref:System.Collections.Generic.IList%601>. Para uma opção imutável, considere a <xref:System.Collections.Immutable.IImmutableList%601> interface genérica.

  - Uma chave e um valor: use qualquer uma das coleções baseadas na interface do <xref:System.Collections.IDictionary> ou na interface genérica do <xref:System.Collections.Generic.IDictionary%602>. Para uma opção imutável, considere as <xref:System.Collections.Immutable.IImmutableSet%601> <xref:System.Collections.Immutable.IImmutableDictionary%602> interfaces genéricas ou.

  - Um valor com chave incorporada: use a classe genérica <xref:System.Collections.ObjectModel.KeyedCollection%602>.

  - Uma chave e vários valores: use a classe <xref:System.Collections.Specialized.NameValueCollection>.

- Você precisa classificar os elementos de forma diferente de como foram inseridos?

  - O <xref:System.Collections.Hashtable> classifica os elementos dele pelos próprios códigos hash.

  - A classe <xref:System.Collections.SortedList> e as classes genéricas <xref:System.Collections.Generic.SortedList%602> e <xref:System.Collections.Generic.SortedDictionary%602> classificam seus elementos pela chave. A ordem de classificação se baseia na implementação da interface <xref:System.Collections.IComparer> da classe <xref:System.Collections.SortedList> e na implementação da interface genérica <xref:System.Collections.Generic.IComparer%601> das classes genéricas <xref:System.Collections.Generic.SortedList%602> e <xref:System.Collections.Generic.SortedDictionary%602>. Dos dois tipos genéricos, <xref:System.Collections.Generic.SortedDictionary%602> oferece um melhor desempenho do que <xref:System.Collections.Generic.SortedList%602>, enquanto <xref:System.Collections.Generic.SortedList%602> consome menos memória.

  - O <xref:System.Collections.ArrayList> oferece um método <xref:System.Collections.ArrayList.Sort%2A> que usa uma implementação <xref:System.Collections.IComparer> como parâmetro. Seu equivalente genérico, a classe genérica <xref:System.Collections.Generic.List%601>, fornece um método <xref:System.Collections.Generic.List%601.Sort%2A> que usa uma implementação da interface genérica <xref:System.Collections.Generic.IComparer%601> como parâmetro.

- Você precisa de rapidez para pesquisas e recuperação de informações?

  - <xref:System.Collections.Specialized.ListDictionary> é mais rápido do que o <xref:System.Collections.Hashtable> para pequenas coleções (10 itens ou menos). A classe genérica <xref:System.Collections.Generic.Dictionary%602> fornece pesquisa mais rápida do que a classe genérica <xref:System.Collections.Generic.SortedDictionary%602>. A implementação com multithread é <xref:System.Collections.Concurrent.ConcurrentDictionary%602>. O <xref:System.Collections.Concurrent.ConcurrentBag%601> fornece uma inserção com multithread rápida para dados não ordenados. Para obter mais informações sobre os dois tipos de multi-threaded, consulte [Quando usar uma coleção thread-safe](thread-safe/when-to-use-a-thread-safe-collection.md).

- Você precisa de coleções que aceitem apenas cadeias de caracteres?

  - <xref:System.Collections.Specialized.StringCollection> (com base no <xref:System.Collections.IList>) e <xref:System.Collections.Specialized.StringDictionary> (com base no <xref:System.Collections.IDictionary>) estão no namespace <xref:System.Collections.Specialized>.

  - Além disso, é possível usar qualquer uma das classes genéricas de coleção no namespace <xref:System.Collections.Generic> como coleções de cadeias de caracteres fortemente tipadas especificando a classe <xref:System.String> para seus argumentos de tipo genérico. Por exemplo, você pode declarar uma variável para ser do tipo [lista \<String> ](xref:System.Collections.Generic.List%601) ou [dicionário<cadeia ](xref:System.Collections.Generic.Dictionary%602)de caracteres>.

## <a name="linq-to-objects-and-plinq"></a>LINQ to Objects e PLINQ

O LINQ to Objects permite que os desenvolvedores usem consultas LINQ para acessar objetos na memória, desde que o tipo de objeto implemente <xref:System.Collections.IEnumerable> ou <xref:System.Collections.Generic.IEnumerable%601>. As consultas LINQ fornecem um padrão comum para acessar dados, são geralmente mais concisas e legíveis que os loops padrão `foreach` e fornecem capacidades de filtragem, ordenação e agrupamento. Para obter mais informações, confira [LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md) e [LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md).

PLINQ fornece uma implementação paralela de LINQ to Objects que pode oferecer uma execução de consulta mais rápida em muitos cenários, por meio do uso mais eficiente dos computadores de vários núcleos. Para obter mais informações, consulte [PLINQ (Parallel LINQ)](../parallel-programming/introduction-to-plinq.md).

## <a name="see-also"></a>Consulte também

- <xref:System.Collections>
- <xref:System.Collections.Specialized>
- <xref:System.Collections.Generic>
- [Coleções com segurança de thread](thread-safe/index.md)
