---
title: where (restrição de tipo genérico) – Referência de C#
ms.date: 04/15/2020
f1_keywords:
- whereconstraint
- whereconstraint_CSharpKeyword
helpviewer_keywords:
- where (generic type constraint) [C#]
ms.openlocfilehash: 406c710cd884363c32b98336717732a09b3d1fc1
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401869"
---
# <a name="where-generic-type-constraint-c-reference"></a>where (restrição de tipo genérico) (Referência de C#)

A cláusula `where` em uma definição genérica especifica restrições sobre os tipos que são usados como argumentos para parâmetros de tipo em um tipo genérico, método, delegado ou função local. As restrições podem especificar interfaces, classes base ou exigir que um tipo genérico seja uma referência, um valor ou um tipo não gerenciado. Eles declaram recursos que o argumento de tipo deve ter.

Por exemplo, você pode declarar uma classe genérica, `MyGenericClass`, de modo que o parâmetro de tipo `T` implementa a interface <xref:System.IComparable%601>:

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#1)]

> [!NOTE]
> Para obter mais informações sobre a cláusula where em uma expressão de consulta, consulte [Cláusula where](where-clause.md).

A cláusula `where` também pode incluir uma restrição de classe base. A restrição de classe base declara que um tipo a ser usado como um argumento de tipo para esse tipo genérico tem a classe especificada como uma classe base, ou é essa classe base. Se a restrição de classe base for usada, ela deverá aparecer antes de qualquer outra restrição nesse parâmetro de tipo. Alguns tipos não têm permissão como uma restrição de classe base: <xref:System.Object>, <xref:System.Array> e <xref:System.ValueType>. Antes do C# 7,3, <xref:System.Enum> , <xref:System.Delegate> , e <xref:System.MulticastDelegate> também não foram permitidos como restrições de classe base. O exemplo a seguir mostra os tipos que agora podem ser especificados como classe base:

[!code-csharp[using an interface constraint](snippets/GenericWhereConstraints.cs#2)]

Em um contexto anulável no C# 8,0 e posterior, a nulidade do tipo de classe base é imposta. Se a classe base for não anulável (por exemplo `Base` ,), o argumento de tipo deverá ser não anulável. Se a classe base for anulável (por exemplo `Base?` ), o argumento de tipo poderá ser um tipo de referência anulável ou não anulável. O compilador emitirá um aviso se o argumento de tipo for um tipo de referência anulável quando a classe base for não anulável.

A cláusula `where` pode especificar que o tipo é um `class` ou um `struct`. A restrição `struct` elimina a necessidade de especificar uma restrição de classe base de `System.ValueType`. O tipo `System.ValueType` não pode ser usado como uma restrição de classe base. O exemplo a seguir mostra as restrições `class` e `struct`:

[!code-csharp[using the class and struct constraints](snippets/GenericWhereConstraints.cs#3)]

Em um contexto anulável no C# 8,0 e posterior, a `class` restrição requer que um tipo seja um tipo de referência não anulável. Para permitir tipos de referência anuláveis, use a `class?` restrição, que permite tipos de referência anuláveis e não anuláveis.

A `where` cláusula pode incluir a `notnull` restrição. A `notnull` restrição limita o parâmetro de tipo para tipos não anuláveis. Esse tipo pode ser um tipo de [valor](../builtin-types/value-types.md) ou um tipo de referência não anulável. A `notnull` restrição está disponível a partir do C# 8,0 para o código compilado em um [ `nullable enable` contexto](../../nullable-references.md#nullable-contexts). Ao contrário de outras restrições, se um argumento de tipo violar a `notnull` restrição, o compilador gerará um aviso em vez de um erro. Os avisos são gerados apenas em um `nullable enable` contexto.

> [!IMPORTANT]
> Declarações genéricas que incluem a `notnull` restrição podem ser usadas em um contexto alheios anulável, mas o compilador não impõe a restrição.

[!code-csharp[using the nonnull constraint](snippets/GenericWhereConstraints.cs#NotNull)]

A cláusula `where` também pode incluir uma restrição `unmanaged`. A restrição `unmanaged` limita o parâmetro de tipo a tipos conhecidos como [tipos não gerenciados](../builtin-types/unmanaged-types.md). Usando a restrição `unmanaged`, é mais fácil escrever o código de interoperabilidade de nível baixo em C#. Essa restrição habilita rotinas reutilizáveis em todos os tipos não gerenciados. A restrição `unmanaged` não pode ser combinada à restrição `class` ou `struct`. A restrição `unmanaged` impõe que o tipo deve ser um `struct`:

[!code-csharp[using the unmanaged constraint](snippets/GenericWhereConstraints.cs#4)]

A cláusula `where` também pode incluir uma restrição de construtor, `new()`. Essa restrição torna possível criar uma instância de um parâmetro de tipo usando o operador `new`. A [restrição New ()](new-constraint.md) permite que o compilador saiba que qualquer argumento de tipo fornecido deve ter um construtor sem parâmetros acessível. Por exemplo:

[!code-csharp[using the new constraint](snippets/GenericWhereConstraints.cs#5)]

A restrição `new()` aparece por último na cláusula `where`. A restrição `new()` não pode ser combinada às restrições `struct` ou `unmanaged`. Todos os tipos que satisfazem as restrições devem ter um construtor sem parâmetros acessível, tornando a restrição `new()` redundante.

Com vários parâmetros de tipo, use uma cláusula `where` para cada parâmetro de tipo, por exemplo:

[!code-csharp[using multiple where constraints](snippets/GenericWhereConstraints.cs#6)]

Você também pode anexar restrições a parâmetros de tipo de métodos genéricos, como mostrado no exemplo a seguir:

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#7)]

Observe que a sintaxe para descrever as restrições de parâmetro de tipo em delegados é a mesma que a dos métodos:

[!code-csharp[where constraints with generic methods](snippets/GenericWhereConstraints.cs#8)]

Para obter informações sobre delegados genéricos, consulte [Delegados genéricos](../../programming-guide/generics/generic-delegates.md).

Para obter detalhes sobre a sintaxe e o uso de restrições, consulte [Restrições a parâmetros de tipo](../../programming-guide/generics/constraints-on-type-parameters.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

 [!INCLUDE[CSharplangspec](~/includes/csharplangspec-md.md)]

## <a name="see-also"></a>Confira também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Introdução aos genéricos](../../programming-guide/generics/index.md)
- [nova restrição](./new-constraint.md)
- [Restrições a parâmetros de tipo](../../programming-guide/generics/constraints-on-type-parameters.md)
