---
title: 'Definir tipos e seus membros-um tour de C #'
description: Os blocos de construção de programas são tipos. Saiba como criar classes, estruturas, interfaces e muito mais em C#.
ms.date: 08/06/2020
ms.openlocfilehash: efd353fe8c1e6a57952bcb2586a05ad38ecd52b9
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559109"
---
# <a name="types-and-members"></a>Tipos e membros

## <a name="classes-and-objects"></a>Classes e objetos

As *classes* são as mais fundamentais para os tipos do C#. Uma classe é uma estrutura de dados que combina ações (métodos e outros membros da função) e estado (campos) em uma única unidade. Uma classe fornece uma definição para *instâncias* da classe, também conhecida como *objetos*. As classes dão suporte à *herança* e *polimorfismo*, mecanismos nos quais *classes derivadas* podem estender e especializar *classes base*.

Novas classes são criadas usando declarações de classe. Uma declaração de classe começa com um cabeçalho. O cabeçalho especifica:

- Os atributos e modificadores da classe
- O nome da classe
- A classe base (ao herdar de uma [classe base](#base-classes))
- As interfaces implementadas pela classe.

O cabeçalho é seguido pelo corpo da classe, que consiste em uma lista de declarações de membro escrita entre os delimitadores `{` e `}`.

O código a seguir mostra uma declaração de uma classe simples chamada `Point` :

:::code language="csharp" source="./snippets/shared/Types.cs" ID="PointClass":::

Instâncias de classes são criadas usando o operador `new`, que aloca memória para uma nova instância, chama um construtor para inicializar a instância e retorna uma referência à instância. As instruções a seguir criam dois `Point` objetos e armazenam referências a esses objetos em duas variáveis:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="CreatePoints":::

A memória ocupada por um objeto é recuperada automaticamente quando o objeto não está mais acessível. Não é necessário nem possível desalocar explicitamente objetos em C#.

### <a name="type-parameters"></a>Parâmetros de tipo

Classes genéricas definem [***parâmetros de tipo***](../programming-guide/generics/index.md). Parâmetros de tipo são uma lista de nomes de parâmetro de tipo entre colchetes angulares. Os parâmetros de tipo seguem o nome da classe. Em seguida, os parâmetros de tipo podem ser usados no corpo das declarações de classe para definir os membros da classe. No exemplo a seguir, os parâmetros de tipo de `Pair` são `TFirst` e `TSecond`:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DefinePairClass":::

Um tipo de classe que é declarado para pegar parâmetros de tipo é chamado de *tipo de classe genérica*. Os tipos struct, interface e delegate também podem ser genéricos.
Quando a classe genérica é usada, os argumentos de tipo devem ser fornecidos para cada um dos parâmetros de tipo:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="CreatePairObject":::

Um tipo genérico com argumentos de tipo fornecidos, como `Pair<int,string>` acima, é chamado de *tipo construído*.

### <a name="base-classes"></a>Classes base

Uma declaração de classe pode especificar uma classe base. Siga o nome da classe e os parâmetros de tipo com dois-pontos e o nome da classe base. Omitir uma especificação de classe base é o mesmo que derivar do `object` de tipo. No exemplo a seguir, a classe base de `Point3D` é `Point` . No primeiro exemplo, a classe base de `Point` é `object` :

:::code language="csharp" source="./snippets/shared/Types.cs" ID="Create3DPoint":::

Uma classe herda os membros de sua classe base. Herança significa que uma classe contém implicitamente quase todos os membros de sua classe base. Uma classe não herda a instância e construtores estáticos e o finalizador. Uma classe derivada pode adicionar novos membros a esses membros que ele herda, mas não pode remover a definição de um membro herdado. No exemplo anterior, `Point3D` herda os `X` Membros e `Y` de `Point` , e cada `Point3D` instância contém três propriedades, `X` , `Y` e `Z` .

Existe uma conversão implícita de um tipo de classe para qualquer um de seus tipos de classe base. Uma variável de um tipo de classe pode referenciar uma instância dessa classe ou uma instância de qualquer classe derivada. Por exemplo, dadas as declarações de classe anteriores, uma variável do tipo `Point` podem referenciar um `Point` ou um `Point3D`:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="ImplicitCastToBase":::

## <a name="structs"></a>Estruturas

Classes definem tipos que dão suporte a herança e polimorfismo. Eles permitem que você crie comportamentos sofisticados com base em hierarquias de classes derivadas. Por outro lado, os tipos de [***struct***](../language-reference/builtin-types/struct.md) são tipos mais simples cujo objetivo principal é armazenar valores de dados. Structs não podem declarar um tipo base; Eles derivam implicitamente de <xref:System.ValueType?displayProperty=nameWithType> . Você não pode derivar outros `struct` tipos de um `struct` tipo. Eles são lacrados implicitamente.

:::code language="csharp" source="./snippets/shared/Types.cs" ID="PointStruct":::

## <a name="interfaces"></a>Interfaces

Uma [***interface***](../programming-guide/interfaces/index.md) define um contrato que pode ser implementado por classes e estruturas. Uma interface pode conter métodos, propriedades, eventos e indexadores. Uma interface normalmente não fornece implementações dos membros que ele define — ela simplesmente especifica os membros que devem ser fornecidos por classes ou estruturas que implementam a interface.

As interfaces podem empregar a ***herança múltipla***. No exemplo a seguir, a interface `IComboBox` herda de `ITextBox` e `IListBox`.

:::code language="csharp" source="./snippets/shared/Types.cs" ID="FirstInterfaces":::

Classes e structs podem implementar várias interfaces. No exemplo a seguir, a classe `EditBox` implementa `IControl` e `IDataBound`.

:::code language="csharp" source="./snippets/shared/Types.cs" ID="ImplementInterfaces":::

Quando uma classe ou struct implementa uma interface específica, as instâncias dessa classe ou struct podem ser convertidas implicitamente para esse tipo de interface. Por exemplo

:::code language="csharp" source="./snippets/shared/Types.cs" ID="UseInterfaces":::

## <a name="enums"></a>Enumerações

Um tipo de [***Enumeração***](../language-reference/builtin-types/enum.md) define um conjunto de valores constantes. A seguir, as seguintes `enum` constantes declarações que definem um ou diferente de raiz:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="EnumDeclaration":::

Você também pode definir um `enum` para ser usado em combinação como sinalizadores. A declaração a seguir declara um conjunto de sinalizadores para as quatro estações. Qualquer combinação das estações pode ser aplicada, incluindo um `All` valor que inclui todas as estações:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="FlagsEnumDeclaration":::

O exemplo a seguir mostra declarações de ambas as enumerações anteriores:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="UsingEnums":::

## <a name="nullable-types"></a>Tipos anuláveis

Variáveis de qualquer tipo podem ser declaradas como ***não anuláveis*** ou ***anuláveis***. Uma variável anulável pode conter um `null` valor adicional, indicando que não há valor. Tipos de valores anuláveis (structs ou enums) são representados por <xref:System.Nullable%601?displayProperty=nameWithType> . Os tipos de referência não anuláveis e anuláveis são representados pelo tipo de referência subjacente. A distinção é representada por metadados lidos pelo compilador e por algumas bibliotecas. O compilador fornece avisos quando referências anuláveis são desreferenciadas sem primeiro verificar seu valor `null` . O compilador também fornece avisos quando referências não anuláveis são atribuídas a um valor que pode ser `null` . O exemplo a seguir declara um ***int anulável***, inicializando-o para `null` . Em seguida, ele define o valor como `5` . Ele demonstra o mesmo conceito com uma ***cadeia de caracteres anulável***. Para obter mais informações, consulte [tipos de valor anulável](../language-reference/builtin-types/nullable-value-types.md) e [tipos de referência anuláveis](../nullable-references.md).

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DeclareNullable":::

## <a name="tuples"></a>Tuplas

O C# oferece suporte a [***tuplas***](../language-reference/builtin-types/value-tuples.md), que fornece sintaxe concisa para agrupar vários elementos de dados em uma estrutura de dados leve. Você cria uma instância de uma tupla declarando os tipos e os nomes dos membros entre `(` e `)` , conforme mostrado no exemplo a seguir:

:::code language="csharp" source="./snippets/shared/Types.cs" ID="DeclareTuples":::

As tuplas fornecem uma alternativa para a estrutura de dados com vários membros, sem usar os blocos de construção descritos no próximo artigo.

>[!div class="step-by-step"]
>[Anterior](index.md) 
> [Avançar](program-building-blocks.md)
