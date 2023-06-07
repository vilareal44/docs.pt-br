---
title: Tipos de tupla-referência C#
description: 'Saiba mais sobre tuplas C#: estruturas de dados leves que você pode usar para agrupar elementos de dados livremente relacionados'
ms.date: 07/09/2020
helpviewer_keywords:
- value tuples [C#]
ms.openlocfilehash: 3d79ab19117847e2364b154db33a1521416bb3f4
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174984"
---
# <a name="tuple-types-c-reference"></a>Tipos de tupla (referência C#)

Disponível em C# 7,0 e posterior, o recurso de *tuplas* fornece uma sintaxe concisa para agrupar vários elementos de dados em uma estrutura de dados leve. O exemplo a seguir mostra como você pode declarar uma variável de tupla, inicializá-la e acessar seus membros de dados:

[!code-csharp-interactive[tuple intro](snippets/ValueTuples.cs#Introduction)]

Como mostra o exemplo anterior, para definir um tipo de tupla, você especifica os tipos de todos os seus membros de dados e, opcionalmente, os [nomes de campo](#tuple-field-names). Você não pode definir métodos em um tipo de tupla, mas pode usar os métodos fornecidos pelo .NET, como mostra o exemplo a seguir:

[!code-csharp-interactive[tuple methods](snippets/ValueTuples.cs#MethodOnTuples)]

A partir do C# 7,3, os tipos de tupla dão suporte a [operadores de igualdade](../operators/equality-operators.md) `==` e `!=` . Para obter mais informações, consulte a seção de [igualdade de tupla](#tuple-equality) .

Tipos de tupla são [tipos de valor](value-types.md); os elementos de tupla são campos públicos. Isso torna as tuplas tipos de valor *mutável* .

> [!NOTE]
> O recurso de tuplas requer o <xref:System.ValueTuple?displayProperty=nameWithType> tipo e os tipos genéricos relacionados (por exemplo, <xref:System.ValueTuple%602?displayProperty=nameWithType> ), que estão disponíveis no .NET Core e .NET Framework 4,7 e posterior. Para usar tuplas em um projeto que tenha como destino .NET Framework 4.6.2 ou anterior, adicione o pacote NuGet [`System.ValueTuple`](https://www.nuget.org/packages/System.ValueTuple/) ao projeto.

Você pode definir tuplas com um grande número arbitrário de elementos:

[!code-csharp-interactive[large tuple](snippets/ValueTuples.cs#LargeTuple)]

## <a name="use-cases-of-tuples"></a>Casos de uso de tuplas

Um dos casos de uso mais comuns de tuplas é como um tipo de retorno de método. Ou seja, em vez de definir [ `out` parâmetros de método](../keywords/out-parameter-modifier.md), você pode agrupar resultados de método em um tipo de retorno de tupla, como mostra o exemplo a seguir:

[!code-csharp-interactive[multiple method outputs](snippets/ValueTuples.cs#MultipleReturns)]

Como mostra o exemplo anterior, você pode trabalhar com a instância de tupla retornada diretamente ou [desconstruir](#tuple-assignment-and-deconstruction) -la em variáveis separadas.

Você também pode usar tipos de tupla em vez de [tipos anônimos](../../programming-guide/classes-and-structs/anonymous-types.md); por exemplo, em consultas LINQ. Para obter mais informações, consulte [escolhendo entre tipos anônimos e de tupla](../../../standard/base-types/choosing-between-anonymous-and-tuple.md).

Normalmente, você usa tuplas para agrupar elementos de dados livremente relacionados. Isso geralmente é útil em métodos de utilitário privado e interno. No caso da API pública, considere definir uma [classe](../keywords/class.md) ou um tipo de [estrutura](struct.md) .

## <a name="tuple-field-names"></a>Nomes de campo de tupla

Você pode especificar explicitamente os nomes dos campos de tupla em uma expressão de inicialização de tupla ou na definição de um tipo de tupla, como mostra o exemplo a seguir:

[!code-csharp-interactive[explicit field names](snippets/ValueTuples.cs#ExplicitFieldNames)]

A partir do C# 7,1, se você não especificar um nome de campo, ele poderá ser inferido a partir do nome da variável correspondente em uma expressão de inicialização de tupla, como mostra o exemplo a seguir:

[!code-csharp-interactive[inferred field names](snippets/ValueTuples.cs#InferFieldNames)]

Isso é conhecido como inicializadores de projeção de tupla. O nome de uma variável não é projetado para um nome de campo de tupla nos seguintes casos:

- O nome do candidato é um nome de membro de um tipo de tupla, por exemplo,, `Item3` `ToString` ou `Rest` .
- O nome do candidato é uma duplicata de outro nome de campo de tupla, explícita ou implícita.

Nesses casos, você especifica explicitamente o nome de um campo ou acessa um campo por seu nome padrão.

Os nomes padrão dos campos de tupla `Item1` são `Item2` , `Item3` e assim por diante. Você sempre pode usar o nome padrão de um campo, mesmo quando um nome de campo é especificado explicitamente ou inferido, como mostra o exemplo a seguir:

[!code-csharp-interactive[default field names](snippets/ValueTuples.cs#DefaultFieldNames)]

As [comparações de igualdade](#tuple-equality) de tupla e [atribuição de tupla](#tuple-assignment-and-deconstruction) não têm nomes de campo em conta.

No momento da compilação, o compilador substitui nomes de campo não padrão pelos nomes padrão correspondentes. Como resultado, nomes de campo explicitamente especificados ou inferidos não estão disponíveis em tempo de execução.

## <a name="tuple-assignment-and-deconstruction"></a>Atribuição e desconstrução de tupla

O C# dá suporte à atribuição entre tipos de tupla que atendem às duas condições a seguir:

- ambos os tipos de tupla têm o mesmo número de elementos
- para cada posição de tupla, o tipo do elemento de tupla à direita é o mesmo que ou implicitamente conversível para o tipo do elemento de tupla esquerdo correspondente

Os valores de elemento de tupla são atribuídos após a ordem dos elementos de tupla. Os nomes dos campos de tupla são ignorados e não atribuídos, como mostra o exemplo a seguir:

[!code-csharp-interactive[tuple assignment](snippets/ValueTuples.cs#Assignment)]

Você também pode usar o operador de atribuição `=` para *desconstruir* uma instância de tupla em variáveis separadas. Você pode fazer isso de uma das seguintes maneiras:

- Declare explicitamente o tipo de cada variável dentro dos parênteses:

  [!code-csharp-interactive[specify types of variables](snippets/ValueTuples.cs#DeconstructExplicit)]

- Use a `var` palavra-chave fora dos parênteses para declarar variáveis digitadas implicitamente e deixe o compilador inferir seus tipos:

  [!code-csharp-interactive[implicitly typed variables](snippets/ValueTuples.cs#DeconstructVar)]

- Usar variáveis existentes:

  [!code-csharp-interactive[existing variables](snippets/ValueTuples.cs#DeconstructExisting)]

Para obter mais informações sobre a desconstrução de tuplas e outros tipos, consulte [decompondo tuplas e outros tipos](../../deconstruct.md).

## <a name="tuple-equality"></a>Igualdade de tupla

Começando com o C# 7.3, os tipos de tupla oferecem suporte aos operadores `==` e `!=`. Esses operadores comparam membros do operando à esquerda com os membros correspondentes do operando à direita após a ordem dos elementos de tupla.

[!code-csharp-interactive[tuple equality](snippets/ValueTuples.cs#TupleEquality)]

Como mostra o exemplo anterior, as `==` `!=` operações e não levam em conta os nomes de campo de tupla.

Duas tuplas são comparáveis quando as duas condições a seguir são satisfeitas:

- Ambas as tuplas têm o mesmo número de elementos. Por exemplo, `t1 != t2` não compilar se `t1` e `t2` tiver diferentes números de elementos.
- Para cada posição de tupla, os elementos correspondentes dos operandos de tupla à esquerda e à direita são comparáveis com os `==` operadores e `!=` . Por exemplo, `(1, (2, 3)) == ((1, 2), 3)` não compila porque o `1` não é comparável com `(1, 2)` .

Os `==` `!=` operadores e comparam tuplas na forma de curto-circuito. Ou seja, uma operação é interrompida assim que ela atende a um par de elementos não iguais ou atinge as extremidades das tuplas. No entanto, antes de qualquer comparação, *todos os* elementos de tupla são avaliados, como mostra o exemplo a seguir:

[!code-csharp-interactive[tuple element evaluation](snippets/ValueTuples.cs#TupleEvaluationForEquality)]

## <a name="tuples-as-out-parameters"></a>Parâmetros de tuplas como saída

Normalmente, você refatoria um método que tem [ `out` parâmetros](../keywords/out-parameter-modifier.md) em um método que retorna uma tupla. No entanto, há casos em que um `out` parâmetro pode ser de um tipo de tupla. O exemplo a seguir mostra como trabalhar com tuplas como `out` parâmetros:

[!code-csharp-interactive[tuple as out parameter](snippets/ValueTuples.cs#TupleAsOutParameter)]

## <a name="tuples-vs-systemtuple"></a>Tuplas vs`System.Tuple`

As tuplas do C#, que são apoiadas por <xref:System.ValueTuple?displayProperty=nameWithType> tipos, são diferentes das tuplas representadas por <xref:System.Tuple?displayProperty=nameWithType> tipos. As principais diferenças são as seguintes:

- `ValueTuple`os tipos são [tipos de valor](value-types.md). `Tuple`tipos são [tipos de referência](../keywords/reference-types.md).
- `ValueTuple`os tipos são mutáveis. `Tuple`os tipos são imutáveis.
- Os membros de dados dos `ValueTuple` tipos são campos. Os membros de dados dos `Tuple` tipos são propriedades.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte as seguintes notas de proposta de recurso:

- [Inferir nomes de tupla (também conhecido como inicializadores de projeção de tupla)](~/_csharplang/proposals/csharp-7.1/infer-tuple-names.md)
- [Suporte para `==` e `!=` em tipos de tupla](~/_csharplang/proposals/csharp-7.3/tuple-equality.md)

## <a name="see-also"></a>Veja também

- [Referência de C#](../index.md)
- [Tipos de valor](value-types.md)
- [Escolhendo entre tipos anônimos e de tupla](../../../standard/base-types/choosing-between-anonymous-and-tuple.md)
- <xref:System.ValueTuple?displayProperty=nameWithType>
