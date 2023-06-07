---
title: Restrições a parâmetros de tipo – Guia de Programação em C#
description: Saiba mais sobre restrições em parâmetros de tipo. Restrições informam ao compilador quais recursos um argumento de tipo deve ter.
ms.date: 04/12/2018
helpviewer_keywords:
- generics [C#], type constraints
- type constraints [C#]
- type parameters [C#], constraints
- unbound type parameter [C#]
ms.openlocfilehash: 91807fa05ce49b8507ee6913ff2620452fcbfab5
ms.sourcegitcommit: 6f58a5f75ceeb936f8ee5b786e9adb81a9a3bee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87301938"
---
# <a name="constraints-on-type-parameters-c-programming-guide"></a>Restrições a parâmetros de tipo (Guia de Programação em C#)

Restrições informam o compilador sobre as funcionalidades que um argumento de tipo deve ter. Sem nenhuma restrição, o argumento de tipo poderia ser qualquer tipo. O compilador pode assumir somente os membros de <xref:System.Object?displayProperty=nameWithType>, que é a classe base definitiva para qualquer tipo .NET. Para obter mais informações, consulte [Por que usar restrições](#why-use-constraints). Se o código do cliente usar um tipo que não atenda a uma restrição, o compilador emitirá um erro. Restrições são especificadas usando a palavra-chave contextual `where`. A tabela a seguir lista os sete tipos de restrições:

|Constraint|Descrição|
|----------------|-----------------|
|`where T : struct`|O argumento de tipo deve ser um tipo de valor não anulável. Para obter informações sobre tipos de valor anulável, consulte [tipos de valor anulável](../../language-reference/builtin-types/nullable-value-types.md). Como todos os tipos de valor têm um construtor acessível sem parâmetros, a `struct` restrição implica a `new()` restrição e não pode ser combinada com a `new()` restrição. Não é possível combinar a `struct` restrição com a `unmanaged` restrição.|
|`where T : class`|O argumento de tipo deve ser um tipo de referência. Essa restrição se aplica também a qualquer classe, interface, delegado ou tipo de matriz. Em um contexto anulável no C# 8,0 ou posterior, `T` deve ser um tipo de referência não anulável. |
|`where T : class?`|O argumento de tipo deve ser um tipo de referência, anulável ou não anulável. Essa restrição se aplica também a qualquer classe, interface, delegado ou tipo de matriz.|
|`where T : notnull`|O argumento de tipo deve ser um tipo não anulável. O argumento pode ser um tipo de referência não anulável em C# 8,0 ou posterior, ou um tipo de valor não anulável. |
|`where T : unmanaged`|O argumento de tipo deve ser um tipo não- [gerenciado](../../language-reference/builtin-types/unmanaged-types.md)não anulável. A `unmanaged` restrição implica a `struct` restrição e não pode ser combinada com as `struct` `new()` restrições ou.|
|`where T : new()`|O argumento de tipo deve ter um construtor público sem parâmetros. Quando usado em conjunto com outras restrições, a restrição `new()` deve ser a última a ser especificada. A `new()` restrição não pode ser combinada com `struct` as `unmanaged` restrições e.|
|`where T :` *\<base class name>*|O argumento de tipo deve ser ou derivar da classe base especificada. Em um contexto anulável no C# 8,0 e posterior, `T` deve ser um tipo de referência não anulável derivado da classe base especificada. |
|`where T :` *\<base class name>?*|O argumento de tipo deve ser ou derivar da classe base especificada. Em um contexto anulável no C# 8,0 e posterior, `T` pode ser um tipo anulável ou não anulável derivado da classe base especificada. |
|`where T :` *\<interface name>*|O argumento de tipo deve ser ou implementar a interface especificada. Várias restrições de interface podem ser especificadas. A interface de restrição também pode ser genérica. Em um contexto anulável no C# 8,0 e posterior, `T` deve ser um tipo não anulável que implementa a interface especificada.|
|`where T :` *\<interface name>?*|O argumento de tipo deve ser ou implementar a interface especificada. Várias restrições de interface podem ser especificadas. A interface de restrição também pode ser genérica. Em um contexto anulável no C# 8,0, `T` pode ser um tipo de referência anulável, um tipo de referência não anulável ou um tipo de valor. `T`Não pode ser um tipo de valor anulável.|
|`where T : U`|O argumento de tipo fornecido para `T` deve ser ou derivado do argumento fornecido para `U` . Em um contexto anulável, se `U` for um tipo de referência não anulável, `T` deverá ser um tipo de referência não anulável. Se `U` for um tipo de referência anulável, `T` poderá ser anulável ou não anulável. |

## <a name="why-use-constraints"></a>Por que usar restrições

As restrições especificam os recursos e as expectativas de um parâmetro de tipo. Declarar essas restrições significa que você pode usar as operações e chamadas de método do tipo restrito. Se sua classe ou método genérico usar qualquer operação nos membros genéricos além da atribuição simples ou chamar quaisquer métodos sem suporte pelo <xref:System.Object?displayProperty=nameWithType> , você precisará aplicar restrições ao parâmetro de tipo. Por exemplo, a restrição de classe base informa ao compilador que somente os objetos desse tipo ou derivados desse tipo serão usados como argumentos de tipo. Uma vez que o compilador tiver essa garantia, ele poderá permitir que métodos desse tipo sejam chamados na classe genérica. O exemplo de código a seguir demonstra a funcionalidade que pode ser adicionada à classe `GenericList<T>` (em [Introdução aos Genéricos](../../../standard/generics/index.md)) ao aplicar uma restrição de classe base.

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#9)]

A restrição permite que a classe genérica use a propriedade `Employee.Name`. A restrição especifica que todos os itens do tipo `T` são um objeto `Employee` ou um objeto que herda de `Employee`.

Várias restrições podem ser aplicadas ao mesmo parâmetro de tipo e as restrições em si podem ser tipos genéricos, da seguinte maneira:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#10)]

Ao aplicar a restrição `where T : class`, evite os operadores `==` e `!=` no parâmetro de tipo, pois esses operadores testarão somente a identidade de referência e não a igualdade de valor. Esse comportamento ocorrerá mesmo se esses operadores forem sobrecarregados em um tipo usado como argumento. O código a seguir ilustra esse ponto; a saída é false, muito embora a classe <xref:System.String> sobrecarregue o operador `==`.

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#11)]

O compilador só sabe que `T` é um tipo de referência em tempo de compilação e deve usar os operadores padrão que são válidos para todos os tipos de referência. Caso seja necessário testar a igualdade de valor, a maneira recomendada é também aplicar a restrição `where T : IEquatable<T>` ou `where T : IComparable<T>` e implementar a interface em qualquer classe que seja usada para construir a classe genérica.

## <a name="constraining-multiple-parameters"></a>Restringindo vários parâmetros

É possível aplicar restrições a vários parâmetros e várias restrições a um único parâmetro, conforme mostrado no exemplo a seguir:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#12)]

## <a name="unbounded-type-parameters"></a>Parâmetros de tipo não associado

 Os parâmetros de tipo que não têm restrições, como o T na classe pública `SampleClass<T>{}`, são denominados “parâmetros de tipo não associado”. Os parâmetros de tipo não associado têm as seguintes regras:

- Os `!=` `==` operadores e não podem ser usados porque não há garantia de que o argumento de tipo concreto dará suporte a esses operadores.
- Eles podem ser convertidos para e de `System.Object` ou explicitamente convertidos para qualquer tipo de interface.
- Você pode compará-los com [nulo](../../language-reference/keywords/null.md). Se um parâmetro não associado for comparado a `null`, a comparação sempre retornará false se o argumento de tipo for um tipo de valor.

## <a name="type-parameters-as-constraints"></a>Parâmetros de tipo como restrições

O uso de um parâmetro de tipo genérico como uma restrição será útil quando uma função membro com parâmetro de tipo próprio tiver que restringir esse parâmetro para o parâmetro de tipo do tipo recipiente, conforme mostrado no exemplo a seguir:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#13)]

No exemplo anterior, `T` é uma restrição de tipo no contexto do método `Add` e um parâmetro de tipo não associado no contexto da classe `List`.

Parâmetros de tipo também podem ser usados como restrições em definições de classe genérica. O parâmetro de tipo deve ser declarado entre colchetes angulares junto com quaisquer outros parâmetros de tipo:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#14)]

A utilidade dos parâmetros de tipo como restrições com classes genéricas é limitada, pois o compilador não pode presumir nada sobre o parâmetro de tipo, exceto que ele deriva de `System.Object`. Use parâmetros de tipo como restrições em classes genéricas em cenários nos quais deseja impor uma relação de herança entre dois parâmetros de tipo.

## <a name="notnull-constraint"></a>Restrição não nula

A partir do C# 8,0 em um contexto anulável, você pode usar a `notnull` restrição para especificar que o argumento de tipo deve ser um tipo de valor não anulável ou um tipo de referência não anulável. A `notnull` restrição só pode ser usada em um `nullable enable` contexto. O compilador gerará um aviso se você adicionar a `notnull` restrição em um contexto alheios anulável.

Ao contrário de outras restrições, quando um argumento de tipo viola a `notnull` restrição, o compilador gera um aviso quando esse código é compilado em um `nullable enable` contexto. Se o código for compilado em um contexto alheios anulável, o compilador não gerará nenhum aviso ou erro.

A partir do C# 8,0 em um contexto anulável, a `class` restrição especifica que o argumento de tipo deve ser um tipo de referência não anulável. Em um contexto anulável, quando um parâmetro de tipo é um tipo de referência anulável, o compilador gera um aviso.

## <a name="unmanaged-constraint"></a>Restrição não gerenciada

A partir do C# 7,3, você pode usar a `unmanaged` restrição para especificar que o parâmetro de tipo deve ser um tipo não- [gerenciado](../../language-reference/builtin-types/unmanaged-types.md)não anulável. A restrição `unmanaged` permite que você escreva rotinas reutilizáveis para trabalhar com tipos que podem ser manipulados como blocos de memória, conforme mostrado no exemplo a seguir:

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#15)]

O método anterior deve ser compilado em um contexto `unsafe` porque ele usa o operador `sizeof` em um tipo não conhecido como um tipo interno. Sem a restrição `unmanaged`, o operador `sizeof` não está disponível.

A `unmanaged` restrição implica a `struct` restrição e não pode ser combinada com ela. Como a `struct` restrição implica a `new()` restrição, a `unmanaged` restrição também não pode ser combinada com a `new()` restrição.

## <a name="delegate-constraints"></a>Restrições de delegado

Também começando com o C# 7.3, você pode usar <xref:System.Delegate?displayProperty=nameWithType> ou <xref:System.MulticastDelegate?displayProperty=nameWithType> como uma restrição de classe base. O CLR sempre permitia essa restrição, mas a linguagem C# não a permite. A restrição `System.Delegate` permite que você escreva código que funcione com delegados de uma maneira fortemente tipada. O código a seguir define um método de extensão que combina dois delegados, desde que eles sejam do mesmo tipo:

[!code-csharp[using the delegate constraint](snippets/GenericWhereConstraints.cs#16)]

Você pode usar o método acima para combinar delegados que são do mesmo tipo:

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#17)]

Se você remover a marca de comentário na última linha, ela não será compilada. Ambos `first` e `test` são tipos delegados, mas são tipos delegados diferentes.

## <a name="enum-constraints"></a>Restrições de enum

Começando com o C# 7.3, você também pode especificar o tipo <xref:System.Enum?displayProperty=nameWithType> como uma restrição de classe base. O CLR sempre permitia essa restrição, mas a linguagem C# não a permite. Genéricos usando `System.Enum` fornecem programação fortemente tipada para armazenar em cache os resultados do uso de métodos estáticos em `System.Enum`. O exemplo a seguir localiza todos os valores válidos para um tipo enum e, em seguida, cria um dicionário que mapeia esses valores para sua representação de cadeia de caracteres.

[!code-csharp[using the enum constraint](snippets/GenericWhereConstraints.cs#18)]

`Enum.GetValues`e `Enum.GetName` usar reflexão, que tem implicações de desempenho. Você pode chamar `EnumNamedValues` para criar uma coleção que é armazenada em cache e reutilizada em vez de repetir as chamadas que exigem reflexão.

Você pode usá-lo conforme mostrado no exemplo a seguir para criar uma enum e compilar um dicionário de seus nomes e valores:

[!code-csharp[enum definition](snippets/GenericWhereConstraints.cs#19)]

[!code-csharp[using the enum constrained method](snippets/GenericWhereConstraints.cs#20)]

## <a name="see-also"></a>Veja também

- <xref:System.Collections.Generic>
- [Guia de programação C#](../index.md)
- [Introdução aos genéricos](./index.md)
- [Classes genéricas](./generic-classes.md)
- [nova restrição](../../language-reference/keywords/new-constraint.md)
