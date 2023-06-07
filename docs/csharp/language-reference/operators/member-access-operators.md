---
title: Operadores de acesso de membro e expressões-referência C#
description: Aprenda sobre operadores de C# que você pode usar para acessar membros de tipo.
ms.date: 04/17/2020
author: pkulikov
f1_keywords:
- ._CSharpKeyword
- '[]_CSharpKeyword'
- ()_CSharpKeyword
- ^_CSharpKeyword
- .._CSharpKeyword
helpviewer_keywords:
- member access operators [C#]
- member access operator [C#]
- dot operator [C#]
- . operator [C#]
- subscript operator [C#]
- square brackets [] operator [C#]
- indexer operator [C#]
- '[] operator [C#]'
- null-conditional operators [C#]
- Elvis operator [C#]
- ?. operator [C#]
- ?[] operator [C#]
- invocation operator [C#]
- method call [C#]
- method invocation [C#]
- delegate invocation [C#]
- () operator [C#]
- ^ operator [C#]
- index from end operator [C#]
- hat operator [C#]
- .. operator [C#]
- range operator [C#]
ms.openlocfilehash: 688a1fcff84a6e8f2fa31533a2bc459bf8c8717a
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916787"
---
# <a name="member-access-operators-and-expressions-c-reference"></a>Operadores de acesso de membro e expressões (referência C#)

Você pode usar os seguintes operadores e expressões ao acessar um membro de tipo:

- [ `.` (acesso de membro)](#member-access-expression-): para acessar um membro de um namespace ou de um tipo
- [ `[]` (elemento de matriz ou acesso de indexador)](#indexer-operator-): para acessar um elemento de matriz ou um indexador de tipo
- [ `?.` and `?[]` (operadores condicionais nulos)](#null-conditional-operators--and-): para executar uma operação de acesso de elemento ou membro somente se um operando não for nulo
- [ `()` (invocação)](#invocation-expression-): para chamar um método acessado ou invocar um delegado
- [ `^` (índice de fim)](#index-from-end-operator-): para indicar que a posição do elemento é do final de uma sequência
- [ `..` (Range)](#range-operator-): para especificar um intervalo de índices que você pode usar para obter um intervalo de elementos de sequência

## <a name="member-access-expression-"></a>Expressão de acesso de membro.

Use o token `.` para acessar um membro de um namespace ou um tipo, como demonstram os exemplos a seguir:

- Use `.` para acessar um namespace aninhado em um namespace, como mostra o exemplo a seguir de uma [ `using` diretiva](../keywords/using-directive.md) :

  [!code-csharp[nested namespaces](snippets/shared/MemberAccessOperators.cs#NestedNamespace)]

- Use `.` para formar um *nome qualificado* para acessar um tipo dentro de um namespace, como mostra o código a seguir:

  [!code-csharp[qualified name](snippets/shared/MemberAccessOperators.cs#QualifiedName)]

  Use uma [ `using` diretiva](../keywords/using-directive.md) para tornar o uso de nomes qualificados opcional.

- Use `.` para acessar [membros de tipo](../../programming-guide/classes-and-structs/index.md#members), estático e não-estático, como mostra o código a seguir:

  [!code-csharp-interactive[type members](snippets/shared/MemberAccessOperators.cs#TypeMemberAccess)]

Você também pode usar `.` para acessar um [método de extensão](../../programming-guide/classes-and-structs/extension-methods.md).

## <a name="indexer-operator-"></a>Operador de indexador []

Os colchetes, `[]`, normalmente são usados para acesso de elemento de matriz, indexador ou ponteiro.

### <a name="array-access"></a>Acesso de matriz

O exemplo a seguir demonstra como acessar elementos de matriz:

[!code-csharp-interactive[array access](snippets/shared/MemberAccessOperators.cs#Arrays)]

Se um índice de matriz estiver fora dos limites da dimensão correspondente de uma matriz, uma <xref:System.IndexOutOfRangeException> será gerada.

Como mostra o exemplo anterior, você também usar colchetes quando declara um tipo de matriz ou instancia uma instância de matriz.

Para obter mais informações sobre matrizes, confira [Matrizes](../../programming-guide/arrays/index.md).

### <a name="indexer-access"></a>Acesso de indexador

O exemplo a seguir usa o <xref:System.Collections.Generic.Dictionary%602> tipo .net para demonstrar o acesso ao indexador:

[!code-csharp-interactive[indexer access](snippets/shared/MemberAccessOperators.cs#Indexers)]

Os indexadores permitem indexar instâncias de um tipo definido pelo usuário de maneira semelhante à indexação de matriz. Ao contrário dos índices de matriz, que devem ser inteiros, os parâmetros do indexador podem ser declarados como sendo de qualquer tipo.

Para obter mais informações sobre indexadores, confira [Indexadores](../../programming-guide/indexers/index.md).

### <a name="other-usages-of-"></a>Outros usos de []

Para saber mais sobre o acesso a elemento de ponteiro, confira a seção [Operador de acesso a elemento de ponteiro []](pointer-related-operators.md#pointer-element-access-operator-) do artigo [Operadores relacionados a ponteiro](pointer-related-operators.md).

Os colchetes também são usados para especificar [atributos](../../programming-guide/concepts/attributes/index.md):

```csharp
[System.Diagnostics.Conditional("DEBUG")]
void TraceMethod() {}
```

## <a name="null-conditional-operators--and-"></a>Operadores condicionais nulos ?. e ?[]

Disponível no C# 6 e posterior, um operador NULL-Conditional aplica um [acesso de membro](#member-access-expression-), `?.` ou [acesso de elemento](#indexer-operator-), `?[]` , operação para seu operando somente se esse operando for avaliado como não nulo; caso contrário, retornará `null` . Isto é

- Se for `a` avaliada como `null` , o resultado de `a?.x` ou `a?[x]` é `null` .
- Se for `a` avaliada como não nula, o resultado `a?.x` ou `a?[x]` será o mesmo que o resultado de `a.x` ou `a[x]` , respectivamente.

  > [!NOTE]
  > Se `a.x` ou `a[x]` lançar uma exceção, `a?.x` ou `a?[x]` geraria a mesma exceção para não NULL `a` . Por exemplo, se `a` for uma instância de matriz não nula e `x` estiver fora dos limites de `a` , o `a?[x]` geraria um <xref:System.IndexOutOfRangeException> .

Os operadores condicionais nulos estão entrando em curto-circuito. Ou seja, se uma operação em uma cadeia de membro operações condicionais de acesso a membro ou elemento retornar `null`, o restante da cadeia não será executado. No exemplo a seguir, `B` não será avaliado se `A` for avaliado como `null` e `C` não será avaliado se `A` ou `B` for avaliado como `null`:

```csharp
A?.B?.Do(C);
A?.B?[C];
```

O exemplo a seguir demonstra o uso dos operadores `?.` e `?[]`:

[!code-csharp-interactive[null-conditional operators](snippets/shared/MemberAccessOperators.cs#NullConditional)]

O exemplo anterior também usa o [operador `??` de União nulo](null-coalescing-operator.md) para especificar uma expressão alternativa a ser avaliada, caso o resultado de uma operação condicional nula seja `null` .

Se `a.x` ou `a[x]` for de um tipo de valor não anulável `T` `a?.x` ou `a?[x]` for do [tipo de valor anulável](../builtin-types/nullable-value-types.md) correspondente `T?` . Se você precisar de uma expressão do tipo `T` , aplique o operador de União nula `??` a uma expressão condicional nula, como mostra o exemplo a seguir:

[!code-csharp-interactive[null-conditional with null-coalescing](snippets/shared/MemberAccessOperators.cs#NullConditionalWithNullCoalescing)]

No exemplo anterior, se você não usar o `??` operador, o `numbers?.Length < 2` será avaliado como `false` quando `numbers` é `null` .

O operador de acesso do membro condicional nulo `?.` também é conhecido como o operador Elvis.

> [!NOTE]
> No C# 8, o [operador NULL-tolerante](null-forgiving.md) encerra a lista de operações condicionais nulas anteriores. Por exemplo, a expressão `x?.y!.z` é analisada como `(x?.y)!.z` . Devido a essa interpretação, `z` é avaliada mesmo se `x` for `null` , o que pode resultar em um <xref:System.NullReferenceException> .

### <a name="thread-safe-delegate-invocation"></a>Invocação de delegado thread-safe

Use o operador `?.` para verificar se um delegado é não nulo e chame-o de uma forma thread-safe (por exemplo, quando você [aciona um evento](../../../standard/events/how-to-raise-and-consume-events.md)), conforme mostrado no código a seguir:

```csharp
PropertyChanged?.Invoke(…)
```

Esse código é equivalente ao código a seguir que você usaria no C# 5 ou anterior:

```csharp
var handler = this.PropertyChanged;
if (handler != null)
{
    handler(…);
}
```

Essa é uma forma thread-safe de garantir que apenas um não nulo `handler` seja invocado. Como as instâncias de delegado são imutáveis, nenhum thread pode alterar o objeto referenciado pela `handler` variável local. Em particular, se o código executado por outro thread cancelar a assinatura do `PropertyChanged` evento e `PropertyChanged` se tornar `null` antes de `handler` ser invocado, o objeto referenciado por `handler` permanecerá inalterado. O `?.` operador avalia seu operando à esquerda não mais de uma vez, garantindo que ele não possa ser alterado para `null` depois de ser verificado como não nulo.

## <a name="invocation-expression-"></a>Expressão de invocação ()

Use parênteses, `()`, para chamar um [método](../../programming-guide/classes-and-structs/methods.md) ou invocar um [delegado](../../programming-guide/delegates/index.md).

O exemplo a seguir demonstra como chamar um método (com ou sem argumentos) e invocar um delegado:

[!code-csharp-interactive[invocation with ()](snippets/shared/MemberAccessOperators.cs#Invocation)]

Você também pode usar parênteses ao invocar um [construtor](../../programming-guide/classes-and-structs/constructors.md) com o operador [`new`](new-operator.md).

### <a name="other-usages-of-"></a>Outros usos de ()

Você também pode usar parênteses para ajustar a ordem na qual as operações em uma expressão são avaliadas. Para saber mais, confira [Operadores C#](index.md).

[Expressões de conversão](type-testing-and-cast.md#cast-expression), que executam conversões de tipo explícitas, também usam parênteses.

## <a name="index-from-end-operator-"></a>Índice do operador end ^

Disponível em C# 8,0 e posterior, o `^` operador indica a posição do elemento do final de uma sequência. Para uma sequência de comprimento `length` , `^n` aponta para o elemento com offset `length - n` do início de uma sequência. Por exemplo, `^1` aponta para o último elemento de uma sequência e `^length` aponta para o primeiro elemento de uma sequência.

[!code-csharp[index from end](snippets/shared/MemberAccessOperators.cs#IndexFromEnd)]

Como mostra o exemplo anterior, Expression `^e` é do <xref:System.Index?displayProperty=nameWithType> tipo. Na expressão `^e` , o resultado de `e` deve ser implicitamente conversível para `int` .

Você também pode usar o `^` operador com o [operador Range](#range-operator-) para criar um intervalo de índices. Para obter mais informações, consulte [índices e intervalos](../../tutorials/ranges-indexes.md).

## <a name="range-operator-"></a>Operador de intervalo..

Disponível em C# 8,0 e posterior, o `..` operador especifica o início e o término de um intervalo de índices como operandos. O operando à esquerda é um início *inclusivo* de um intervalo. O operando de lado direito é uma extremidade *exclusiva* de um intervalo. Qualquer um dos operandos pode ser um índice do início ou do final de uma sequência, como mostra o exemplo a seguir:

[!code-csharp[range examples](snippets/shared/MemberAccessOperators.cs#Ranges)]

Como mostra o exemplo anterior, Expression `a..b` é do <xref:System.Range?displayProperty=nameWithType> tipo. Na expressão `a..b` , os resultados de `a` e `b` devem ser conversíveis implicitamente para `int` ou <xref:System.Index> .

Você pode omitir qualquer um dos operandos do `..` operador para obter um intervalo aberto:

- `a..` equivale a `a..^0`
- `..b` equivale a `0..b`
- `..` equivale a `0..^0`

[!code-csharp[ranges with omitted operands](snippets/shared/MemberAccessOperators.cs#RangesOptional)]

Para obter mais informações, consulte [índices e intervalos](../../tutorials/ranges-indexes.md).

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Os `.` `()` operadores,, `^` e `..` não podem ser sobrecarregados. O operador `[]` também é considerado um operador não sobrecarregável. Use [indexadores](../../programming-guide/indexers/index.md) para permitir a indexação com tipos definidos pelo usuário.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Acesso de membros](~/_csharplang/spec/expressions.md#member-access)
- [Acesso a elemento](~/_csharplang/spec/expressions.md#element-access)
- [Operador condicional nulo](~/_csharplang/spec/expressions.md#null-conditional-operator)
- [Expressões de invocação](~/_csharplang/spec/expressions.md#invocation-expressions)

Para obter mais informações sobre índices e intervalos, consulte a [Nota de proposta de recurso](~/_csharplang/proposals/csharp-8.0/ranges.md).

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [?? (operador de união nula)](null-coalescing-operator.md)
- [Operador ::](namespace-alias-qualifier.md)
