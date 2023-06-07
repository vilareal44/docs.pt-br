---
title: Tipos de estrutura-referência C#
ms.date: 04/21/2020
f1_keywords:
- struct_CSharpKeyword
helpviewer_keywords:
- struct keyword [C#]
- struct type [C#]
- structure type [C#]
ms.assetid: ff3dd9b7-dc93-4720-8855-ef5558f65c7c
ms.openlocfilehash: 515b8d9adc1359581625f0d822e254d2c1df3b58
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88062490"
---
# <a name="structure-types-c-reference"></a>Tipos de estrutura (referência C#)

Um *tipo de estrutura* (ou *tipo de struct*) é um tipo de [valor](value-types.md) que pode encapsular dados e funcionalidade relacionada. Você usa a `struct` palavra-chave para definir um tipo de estrutura:

[!code-csharp[struct example](snippets/StructType.cs#StructExample)]

Os tipos de estrutura têm *semântica de valor*. Ou seja, uma variável de um tipo de estrutura contém uma instância do tipo. Por padrão, os valores de variáveis são copiados na atribuição, passando um argumento para um método e retornando um resultado de método. No caso de uma variável de tipo de estrutura, uma instância do tipo é copiada. Para obter mais informações, consulte [tipos de valor](value-types.md).

Normalmente, você usa tipos de estrutura para criar pequenos tipos centrados em dados que fornecem pouco ou nenhum comportamento. Por exemplo, o .NET usa tipos de estrutura para representar um número ( [inteiro](integral-numeric-types.md) e [real](floating-point-numeric-types.md)), um [valor booliano](bool.md), um [caractere Unicode](char.md), uma [instância de tempo](xref:System.DateTime). Se você estiver concentrado no comportamento de um tipo, considere definir uma [classe](../keywords/class.md). Os tipos de classe têm *semânticas de referência*. Ou seja, uma variável de um tipo de classe contém uma referência a uma instância do tipo, não a instância em si.

Como os tipos de estrutura têm semântica de valor, recomendamos que você defina tipos de estrutura *imutáveis* .

## <a name="readonly-struct"></a>`readonly`struct

A partir do C# 7,2, você usa o `readonly` modificador para declarar que um tipo de estrutura é imutável:

[!code-csharp[readonly struct](snippets/StructType.cs#ReadonlyStruct)]

Todos os membros de dados de um `readonly` struct devem ser somente leitura da seguinte maneira:

- Qualquer declaração de campo deve ter o [ `readonly` modificador](../keywords/readonly.md)
- Qualquer propriedade, incluindo as implementadas automaticamente, deve ser somente leitura

Isso garante que nenhum membro de uma `readonly` struct modifique o estado da estrutura.

> [!NOTE]
> Em uma `readonly` struct, um membro de dados de um tipo de referência mutável ainda pode mutar seu próprio estado. Por exemplo, você não pode substituir uma <xref:System.Collections.Generic.List%601> instância, mas pode adicionar novos elementos a ela.

## <a name="readonly-instance-members"></a>`readonly`Membros da instância

A partir do C# 8,0, você também pode usar o `readonly` modificador para declarar que um membro de instância não modifica o estado de um struct. Se você não puder declarar o tipo de estrutura inteira como `readonly` , use o `readonly` modificador para marcar os membros da instância que não modificam o estado da estrutura. Em uma `readonly` struct, cada membro de instância é implicitamente `readonly` .

Dentro de um `readonly` membro de instância, você não pode atribuir a campos de instância da estrutura. No entanto, um `readonly` membro pode chamar um não `readonly` membro. Nesse caso, o compilador cria uma cópia da instância de estrutura e chama o não `readonly` membro nessa cópia. Como resultado, a instância da estrutura original não é modificada.

Normalmente, você aplica o `readonly` modificador aos seguintes tipos de membros de instância:

- maneiras

  [!code-csharp[readonly method](snippets/StructType.cs#ReadonlyMethod)]

  Você também pode aplicar o `readonly` modificador a métodos que substituem os métodos declarados em <xref:System.Object?displayProperty=nameWithType> :

  [!code-csharp[readonly override](snippets/StructType.cs#ReadonlyOverride)]

- Propriedades e indexadores:

  [!code-csharp[readonly property get](snippets/StructType.cs#ReadonlyProperty)]

  Se você precisar aplicar o `readonly` modificador a ambos os acessadores de uma propriedade ou indexador, aplique-o na declaração da propriedade ou do indexador.

  > [!NOTE]
  > O compilador declara um `get` acessador de uma [propriedade implementada automaticamente](../../programming-guide/classes-and-structs/auto-implemented-properties.md) como `readonly` , independentemente da presença do `readonly` modificador em uma declaração de propriedade.

Você não pode aplicar o `readonly` modificador a membros estáticos de um tipo de estrutura.

O compilador pode fazer uso do `readonly` modificador para otimizações de desempenho. Para obter mais informações, consulte [escrever código C# seguro e eficiente](../../write-safe-efficient-code.md).

## <a name="limitations-with-the-design-of-a-structure-type"></a>Limitações com o design de um tipo de estrutura

Ao criar um tipo de estrutura, você tem os mesmos recursos que com um tipo de [classe](../keywords/class.md) , com as seguintes exceções:

- Não é possível declarar um construtor sem parâmetros. Cada tipo de estrutura já fornece um Construtor implícito sem parâmetros que produz o [valor padrão](default-values.md) do tipo.

- Não é possível inicializar um campo ou propriedade de instância em sua declaração. No entanto, você pode inicializar um campo [estático](../keywords/static.md) ou [const](../keywords/const.md) ou uma propriedade estática em sua declaração.

- Um construtor de um tipo de estrutura deve inicializar todos os campos de instância do tipo.

- Um tipo de estrutura não pode herdar de outra classe ou tipo de estrutura e não pode ser a base de uma classe. No entanto, um tipo de estrutura pode implementar [interfaces](../keywords/interface.md).

- Você não pode declarar um [finalizador](../../programming-guide/classes-and-structs/destructors.md) em um tipo de estrutura.

## <a name="instantiation-of-a-structure-type"></a>Instanciação de um tipo de estrutura

Em C#, você deve inicializar uma variável declarada antes que ela possa ser usada. Como uma variável de tipo de estrutura não pode ser `null` (a menos que seja uma variável de um [tipo de valor anulável](nullable-value-types.md)), você deve instanciar uma instância do tipo correspondente. Há várias maneiras de fazer isso.

Normalmente, você cria uma instância de um tipo de estrutura chamando um construtor apropriado com o [`new`](../operators/new-operator.md) operador. Cada tipo de estrutura tem pelo menos um construtor. Esse é um Construtor implícito sem parâmetros, que produz o [valor padrão](default-values.md) do tipo. Você também pode usar uma [expressão de valor padrão](../operators/default.md) para produzir o valor padrão de um tipo.

Se todos os campos de instância de um tipo de estrutura estiverem acessíveis, você também poderá instanciá-lo sem o `new` operador. Nesse caso, você deve inicializar todos os campos de instância antes do primeiro uso da instância. O seguinte exemplo mostra como fazer isso:

[!code-csharp[without new](snippets/StructType.cs#WithoutNew)]

No caso dos tipos de [valor internos](value-types.md#built-in-value-types), use os literais correspondentes para especificar um valor do tipo.

## <a name="passing-structure-type-variables-by-reference"></a>Passando variáveis de tipo de estrutura por referência

Quando você passa uma variável de tipo de estrutura para um método como um argumento ou retorna um valor de tipo de estrutura de um método, toda a instância de um tipo de estrutura é copiada. Isso pode afetar o desempenho do seu código em cenários de alto desempenho que envolvem tipos de estrutura grandes. Você pode evitar a cópia de valor passando uma variável de tipo de estrutura por referência. Use os [`ref`](../keywords/ref.md#passing-an-argument-by-reference) [`out`](../keywords/out-parameter-modifier.md) modificadores de parâmetro de método,, ou [`in`](../keywords/in-parameter-modifier.md) para indicar que um argumento deve ser passado por referência. Use [ref retorna](../../programming-guide/classes-and-structs/ref-returns.md) para retornar um resultado de método por referência. Para obter mais informações, consulte [escrever código C# seguro e eficiente](../../write-safe-efficient-code.md).

## <a name="ref-struct"></a>`ref`struct

A partir do C# 7,2, você pode usar o `ref` modificador na declaração de um tipo de estrutura. As instâncias de um `ref` tipo de struct são alocadas na pilha e não podem sair para o heap gerenciado. Para garantir isso, o compilador limita o uso de `ref` tipos de struct da seguinte maneira:

- Um `ref` struct não pode ser o tipo de elemento de uma matriz.
- Um `ref` struct não pode ser um tipo declarado de um campo de uma classe ou um não `ref` struct.
- Um `ref` struct não pode implementar interfaces.
- Um `ref` struct não pode ser encaixado em <xref:System.ValueType?displayProperty=nameWithType> ou <xref:System.Object?displayProperty=nameWithType> .
- Um `ref` struct não pode ser um argumento de tipo.
- Uma `ref` variável de struct não pode ser capturada por uma [expressão lambda](../operators/lambda-expressions.md) ou uma [função local](../../programming-guide/classes-and-structs/local-functions.md).
- Uma `ref` variável de struct não pode ser usada em um [`async`](../keywords/async.md) método. No entanto, você pode usar `ref` variáveis struct em métodos síncronos, por exemplo, naquelas que retornam <xref:System.Threading.Tasks.Task> ou <xref:System.Threading.Tasks.Task%601> .
- Uma `ref` variável de struct não pode ser usada em [iteradores](../../iterators.md).

Normalmente, você define um `ref` tipo de struct quando precisa de um tipo que também inclui membros de dados de `ref` tipos de struct:

[!code-csharp[ref struct](snippets/StructType.cs#RefStruct)]

Para declarar uma `ref` struct como [`readonly`](#readonly-struct) , combine os `readonly` `ref` modificadores e na declaração de tipo (o `readonly` modificador deve vir antes do `ref` Modificador):

[!code-csharp[readonly ref struct](snippets/StructType.cs#ReadonlyRef)]

No .NET, exemplos de uma `ref` estrutura são <xref:System.Span%601?displayProperty=nameWithType> e <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> .

## <a name="conversions"></a>Conversões

Para qualquer tipo de estrutura (exceto tipos de [ `ref` struct](#ref-struct) ), existem conversões [boxing e unboxing](../../programming-guide/types/boxing-and-unboxing.md) de e para os <xref:System.ValueType?displayProperty=nameWithType> <xref:System.Object?displayProperty=nameWithType> tipos e. Existem também conversões boxing e unboxing entre um tipo de estrutura e qualquer interface que ela implementa.

## <a name="c-language-specification"></a>Especificação da linguagem C#

Para obter mais informações, consulte a seção [structs](~/_csharplang/spec/structs.md) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md).

Para obter mais informações sobre os recursos introduzidos no C# 7,2 e posterior, consulte as seguintes notas de proposta de recurso:

- [Structs ReadOnly](~/_csharplang/proposals/csharp-7.2/readonly-ref.md#readonly-structs)
- [Membros da instância ReadOnly](~/_csharplang/proposals/csharp-8.0/readonly-instance-members.md)
- [Segurança de tempo de compilação para tipos semelhantes a ref](~/_csharplang/proposals/csharp-7.2/span-safety.md)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Diretrizes de design-escolhendo entre classe e estrutura](../../../standard/design-guidelines/choosing-between-class-and-struct.md)
- [Diretrizes de design – design de struct](../../../standard/design-guidelines/struct.md)
- [Classes e structs](../../programming-guide/classes-and-structs/index.md)
