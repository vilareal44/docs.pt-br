---
title: Coleções e estruturas de dados
description: Saiba como usar coleções e estruturas de dados no .NET. Use coleções genéricas e não genéricas em operações thread-safe.
ms.date: 04/30/2020
ms.technology: dotnet-standard
helpviewer_keywords:
- grouping data in collections
- objects [.NET Framework], grouping in collections
- Array class, grouping data in collections
- threading [.NET Framework], safety
- Collections classes
- collections [.NET Framework]
ms.assetid: 60cc581f-1db5-445b-ba04-a173396bf872
ms.openlocfilehash: 3d5b16dccdd9867293a52c74a2d379c807fd93e7
ms.sourcegitcommit: 7137e12f54c4e83a94ae43ec320f8cf59c1772ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2020
ms.locfileid: "84662739"
---
# <a name="collections-and-data-structures"></a>Coleções e estruturas de dados

Dados semelhantes podem normalmente ser tratados com mais eficiência quando armazenados e manipulados como uma coleção. Você pode usar a <xref:System.Array?displayProperty=nameWithType> classe ou as classes nos <xref:System.Collections> <xref:System.Collections.Generic> <xref:System.Collections.Concurrent> namespaces,, e <xref:System.Collections.Immutable> para adicionar, remover e modificar elementos individuais ou um intervalo de elementos em uma coleção.

Há dois tipos principais de coleções; coleções genéricas e coleções não genéricas. Coleções genéricas foram adicionadas ao .NET Framework 2.0 e fornecem coleções que são fortemente tipadas no tempo de compilação. Por isso, coleções genéricas normalmente oferecem melhor desempenho. Coleções genéricas aceitam um parâmetro de tipo quando são criadas e não exigem que você converta de e para o tipo <xref:System.Object> ao adicionar ou remover itens da coleção.  Além disso, há suporte para a maioria das coleções genéricas em aplicativos da Windows Store. As coleções não genéricas armazenam itens como <xref:System.Object> , exigem a conversão e a maioria não tem suporte para o desenvolvimento de aplicativos da Windows Store. No entanto, você pode ver as coleções não genéricas no código mais antigo.

Começando com o .NET Framework 4, as coleções no namespace <xref:System.Collections.Concurrent> fornecem operações thread-safe eficientes para acessar itens da coleção de vários threads. As classes de coleção imutáveis no <xref:System.Collections.Immutable> namespace ([pacote NuGet](https://www.nuget.org/packages/System.Collections.Immutable)) são inerentemente seguras ao thread porque as operações são executadas em uma cópia da coleção original e a coleção original não pode ser modificada.

<a name="BKMK_Commoncollectionfeatures"></a>
## <a name="common-collection-features"></a>Recursos comuns de coleção

Todas as coleções fornecem métodos para adicionar, remover ou localizar itens na coleção. Além disso, todas as coleções que direta ou indiretamente implementam a interface <xref:System.Collections.ICollection> ou a interface <xref:System.Collections.Generic.ICollection%601> compartilham estes recursos:

- **A capacidade de enumerar a coleção**

    Coleções do .NET Framework implementam <xref:System.Collections.IEnumerable?displayProperty=nameWithType> ou <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> para permitir a iteração da coleção por meio dela. Um enumerador pode ser considerado um ponteiro móvel para qualquer elemento da coleção. A instrução [foreach, in](../../csharp/language-reference/keywords/foreach-in.md) e a [Instrução For Each...Next](../../visual-basic/language-reference/statements/for-each-next-statement.md) usam o enumerador exposto pelo método <xref:System.Collections.IEnumerable.GetEnumerator%2A> e ocultam a complexidade de manipulação do enumerador. Além disso, qualquer coleção que implementa <xref:System.Collections.Generic.IEnumerable%601?displayProperty=nameWithType> é considerada um *tipo passível de consulta* e pode ser consultada com LINQ. Consultas LINQ fornecem um padrão comum para o acesso de dados. Elas são geralmente mais concisas e legíveis que loops `foreach` padrão e fornecem filtragem, classificação e agrupamento de recursos. Consultas LINQ também podem melhorar o desempenho. Para obter mais informações, consulte [LINQ to Objects (C#)](../../csharp/programming-guide/concepts/linq/linq-to-objects.md), [LINQ to Objects (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/linq-to-objects.md), [Parallel LINQ (PLINQ)](../parallel-programming/introduction-to-plinq.md), [Introdução a consultas LINQ (C#)](../../csharp/programming-guide/concepts/linq/introduction-to-linq-queries.md) e [Operações básicas de consulta (Visual Basic)](../../visual-basic/programming-guide/concepts/linq/basic-query-operations.md).

- **A capacidade de copiar o conteúdo da coleção para uma matriz**

    Todas as coleções podem ser copiadas para uma matriz usando o método **CopyTo**; no entanto, a ordem dos elementos na nova matriz se baseia na sequência na qual o enumerador os retorna. A matriz resultante é sempre unidimensional com um limite inferior de zero.

Além disso, muitas classes de coleção contêm os seguintes recursos:

- **Propriedades de capacidade e contagem**

    A capacidade de uma coleção é o número de elementos que ela pode conter. A contagem de uma coleção é o número de elementos que ela realmente contém. Algumas coleções ocultam a capacidade ou a contagem ou ambas.

    A maioria das coleções se expande automaticamente em capacidade quando a capacidade atual é atingida. A memória é realocada e os elementos são copiados da coleção antiga para a nova. Isso reduz o código necessário para usar a coleção; no entanto, o desempenho da coleção pode ser afetado de forma negativa. Por exemplo, para <xref:System.Collections.Generic.List%601> , se <xref:System.Collections.Generic.List%601.Count%2A> for menor que <xref:System.Collections.Generic.List%601.Capacity%2A> , a adição de um item será uma operação O (1). Se a capacidade precisar ser aumentada para acomodar o novo elemento, adicionar um item se tornará uma `n` operação O (), em que `n` é <xref:System.Collections.Generic.List%601.Count%2A> . A melhor maneira de evitar um desempenho ruim causado por várias realocações é definir a capacidade inicial para que seja o tamanho estimado da coleção.

    Uma <xref:System.Collections.BitArray> é um caso especial; sua capacidade é a mesma que seu comprimento, que é o mesmo de sua contagem.

- **Um limite inferior consistente**

    O limite inferior de uma coleção é o índice do seu primeiro elemento. Todas as coleções indexadas nos namespaces <xref:System.Collections> têm um limite inferior de zero, indicando que são indexados em 0. <xref:System.Array>tem um limite inferior de zero por padrão, mas um limite inferior diferente pode ser definido ao criar uma instância da classe de **matriz** usando <xref:System.Array.CreateInstance%2A?displayProperty=nameWithType> .

- **Sincronização para acesso de vários threads** ( <xref:System.Collections> somente classes).

    Tipos de coleção não genérica no namespace <xref:System.Collections> oferecem algum acesso thread-safe com sincronização; geralmente exposto por meio dos membros <xref:System.Collections.ICollection.SyncRoot%2A> e <xref:System.Collections.ICollection.IsSynchronized%2A>. Essas coleções são não thread-safe por padrão. Se você precisar de acesso com multithread escalável e eficiente para uma coleção, use uma das classes no namespace <xref:System.Collections.Concurrent> ou considere usar uma coleção imutável. Para obter mais informações, veja [Coleções thread-safe](thread-safe/index.md).

<a name="BKMK_Choosingacollection"></a>
## <a name="choose-a-collection"></a>Escolher uma coleção

Em geral, você deve usar coleções genéricas. A tabela a seguir descreve alguns cenários comuns de coleção e as classes de coleção que você pode usar para esses cenários. Se você for inexperiente com coleções genéricas, esta tabela o ajudará a escolher a coleção genérica adequada para a tarefa.

|Eu quero…|Opções de coleção genérica|Opções de coleção não genérica|Opções de coleção thread-safe ou imutável|
|-|-|-|-|
|Armazenar itens como pares chave/valor para consulta rápida por chave|<xref:System.Collections.Generic.Dictionary%602>|<xref:System.Collections.Hashtable><br /><br /> (Um conjunto de pares chave/valor que são organizados com base no código hash da chave.)|<xref:System.Collections.Concurrent.ConcurrentDictionary%602><br /><br /> <xref:System.Collections.ObjectModel.ReadOnlyDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableDictionary%602>|
|Itens de acesso por índice|<xref:System.Collections.Generic.List%601>|<xref:System.Array><br /><br /> <xref:System.Collections.ArrayList>|<xref:System.Collections.Immutable.ImmutableList%601><br /><br /> <xref:System.Collections.Immutable.ImmutableArray>|
|Usar itens primeiro a entrar, primeiro a sair (PEPS)|<xref:System.Collections.Generic.Queue%601>|<xref:System.Collections.Queue>|<xref:System.Collections.Concurrent.ConcurrentQueue%601><br /><br /> <xref:System.Collections.Immutable.ImmutableQueue%601>|
|Usar dados último a entrar, primeiro a sair (UEPS)|<xref:System.Collections.Generic.Stack%601>|<xref:System.Collections.Stack>|<xref:System.Collections.Concurrent.ConcurrentStack%601><br /><br /> <xref:System.Collections.Immutable.ImmutableStack%601>|
|Acessar itens em sequência|<xref:System.Collections.Generic.LinkedList%601>|Nenhuma recomendação|Nenhuma recomendação|
|Receba notificações quando itens forem removidos da coleção ou adicionados a ela. (implementa <xref:System.ComponentModel.INotifyPropertyChanged> e <xref:System.Collections.Specialized.INotifyCollectionChanged>)|<xref:System.Collections.ObjectModel.ObservableCollection%601>|Nenhuma recomendação|Nenhuma recomendação|
|Uma coleção classificada|<xref:System.Collections.Generic.SortedList%602>|<xref:System.Collections.SortedList>|<xref:System.Collections.Immutable.ImmutableSortedDictionary%602><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|
|Um conjunto de funções matemáticas|<xref:System.Collections.Generic.HashSet%601><br /><br /> <xref:System.Collections.Generic.SortedSet%601>|Nenhuma recomendação|<xref:System.Collections.Immutable.ImmutableHashSet%601><br /><br /> <xref:System.Collections.Immutable.ImmutableSortedSet%601>|

### <a name="algorithmic-complexity-of-collections"></a>Complexidade do Algorithm de coleções

Ao escolher uma [classe de coleção](selecting-a-collection-class.md), vale a pena considerar possíveis compensações no desempenho. Use a tabela a seguir para referenciar como vários tipos de coleção mutáveis são comparados na complexidade do algoritmo para suas contrapartes imutáveis correspondentes. Frequentemente, os tipos de coleção imutáveis são menos econômicos, mas fornecem imutabilidade, que geralmente é um benefício comparativa válido.

| Mutável                   | Amortizado  | Pior caso                | Imutável                          | Complexidade |
|---------------------------|------------|---------------------------|------------------------------------|------------|
| `Stack<T>.Push`           | O (1)       | O ( `n` )                    | `ImmutableStack<T>.Push`           | O (1)       |
| `Queue<T>.Enqueue`        | O (1)       | O ( `n` )                    | `ImmutableQueue<T>.Enqueue`        | O (1)       |
| `List<T>.Add`             | O (1)       | O ( `n` )                    | `ImmutableList<T>.Add`             | O (log `n` ) |
| `List<T>.Item[Int32]`     | O (1)       | O (1)                      | `ImmutableList<T>.Item[Int32]`     | O (log `n` ) |
| `List<T>.Enumerator`      | O ( `n` )     | O ( `n` )                    | `ImmutableList<T>.Enumerator`      | O ( `n` )     |
| `HashSet<T>.Add`, pesquisa  | O (1)       | O ( `n` )                    | `ImmutableHashSet<T>.Add`          | O (log `n` ) |
| `SortedSet<T>.Add`        | O (log `n` ) | O ( `n` )                    | `ImmutableSortedSet<T>.Add`        | O (log `n` ) |
| `Dictionary<T>.Add`       | O (1)       | O ( `n` )                    | `ImmutableDictionary<T>.Add`       | O (log `n` ) |
| `Dictionary<T>`busca    | O (1)       | O (1) – ou estritamente O ( `n` ) | `ImmutableDictionary<T>`busca    | O (log `n` ) |
| `SortedDictionary<T>.Add` | O (log `n` ) | O ( `n` log `n` )            | `ImmutableSortedDictionary<T>.Add` | O (log `n` ) |

Um `List<T>` pode ser enumerado com eficiência usando um `for` loop ou um `foreach` loop. `ImmutableList<T>`No entanto, um trabalho insatisfatório dentro de um `for` loop ocorre devido ao tempo (de log `n` ) do seu indexador. A enumeração de um `ImmutableList<T>` usando um `foreach` loop é eficiente porque o `ImmutableList<T>` usa uma árvore binária para armazenar seus dados em vez de uma matriz simples, como o `List<T>` uso. Uma matriz pode ser rapidamente indexada em, enquanto uma árvore binária deve ser percorrido até que o nó com o índice desejado seja encontrado.

Além disso, `SortedSet<T>` o tem a mesma complexidade que `ImmutableSortedSet<T>` . Isso porque ambos usam árvores binárias. É claro que a diferença significativa é que `ImmutableSortedSet<T>` usa uma árvore binária imutável. Como `ImmutableSortedSet<T>` o também oferece uma <xref:System.Collections.Immutable.ImmutableSortedSet%601.Builder?displayProperty=nameWithType> classe que permite a mutação, você pode ter a imutabilidade e o desempenho.

<a name="BKMK_RelatedTopics"></a>
## <a name="related-topics"></a>Tópicos relacionados

|Title|Descrição|
|-----------|-----------------|
|[Selecionando uma classe de coleção](selecting-a-collection-class.md)|Descreve as diferentes coleções e ajuda a selecionar uma para o seu cenário.|
|[Tipos de coleção comumente usados](commonly-used-collection-types.md)|Descreve os tipos de coleção genérica e não genérica normalmente usadas, tais como <xref:System.Array?displayProperty=nameWithType>, <xref:System.Collections.Generic.List%601?displayProperty=nameWithType>, e <xref:System.Collections.Generic.Dictionary%602?displayProperty=nameWithType>.|
|[Quando usar coleções genéricas](when-to-use-generic-collections.md)|Descreve o uso de tipos de coleção genérica.|
|[Comparações e classificações dentro de coleções](comparisons-and-sorts-within-collections.md)|Discute o uso de comparações de igualdade e comparações de classificação em coleções.|
|[Tipos de coleção classificados](sorted-collection-types.md)|Descreve as características e o desempenho de coleções classificadas|
|[Tipos de Coleção de Tabela de Hash e Dicionário](hashtable-and-dictionary-collection-types.md)|Descreve os recursos de tipos de dicionário baseado em hash genérico e não genérico.|
|[Coleções com segurança de thread](thread-safe/index.md)|Descreve os tipos de coleção, tais como <xref:System.Collections.Concurrent.BlockingCollection%601?displayProperty=nameWithType> e <xref:System.Collections.Concurrent.ConcurrentBag%601?displayProperty=nameWithType> que dão suporte a acesso simultâneo seguro e eficiente de vários threads.|
|System.Collections.Immutable|Apresenta as coleções imutáveis e fornece links para os tipos de coleção.|

<a name="BKMK_Reference"></a>
## <a name="reference"></a>Referência
<xref:System.Array?displayProperty=nameWithType>
<xref:System.Collections?displayProperty=nameWithType>
<xref:System.Collections.Concurrent?displayProperty=nameWithType>
<xref:System.Collections.Generic?displayProperty=nameWithType>
<xref:System.Collections.Specialized?displayProperty=nameWithType>
<xref:System.Linq?displayProperty=nameWithType>
<xref:System.Collections.Immutable>
