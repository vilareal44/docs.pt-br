---
title: expressão stackalloc-referência C#
ms.date: 03/13/2020
f1_keywords:
- stackalloc_CSharpKeyword
helpviewer_keywords:
- stackalloc expression [C#]
ms.openlocfilehash: 4f20f3262b77cc2fe16480e53d13960e68d230b5
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916677"
---
# <a name="stackalloc-expression-c-reference"></a>expressão stackalloc (referência C#)

Uma `stackalloc` expressão aloca um bloco de memória na pilha. Um bloco de memória alocado na pilha criado durante a execução do método é descartado automaticamente quando esse método é retornado. Não é possível liberar explicitamente a memória alocada com `stackalloc` . Um bloco de memória alocado de pilha não está sujeito à [coleta de lixo](../../../standard/garbage-collection/index.md) e não precisa ser fixado com uma [ `fixed` instrução](../keywords/fixed-statement.md).

Você pode atribuir o resultado de uma `stackalloc` expressão a uma variável de um dos seguintes tipos:

- A partir do C# 7,2 <xref:System.Span%601?displayProperty=nameWithType> ou <xref:System.ReadOnlySpan%601?displayProperty=nameWithType> , como mostra o exemplo a seguir:

  [!code-csharp[stackalloc span](snippets/shared/StackallocOperator.cs#AssignToSpan)]

  Você não precisa usar um contexto [unsafe](../keywords/unsafe.md) quando atribui um bloco de memória alocado na pilha a uma variável <xref:System.Span%601> ou <xref:System.ReadOnlySpan%601>.

  Ao trabalhar com esses tipos, você pode usar uma expressão `stackalloc` em [condicional](conditional-operator.md) ou expressões de atribuição, como mostra o seguinte exemplo:

  [!code-csharp[stackalloc expression](snippets/shared/StackallocOperator.cs#AsExpression)]

  A partir do C# 8,0, você pode usar uma `stackalloc` expressão dentro de outras expressões sempre que uma <xref:System.Span%601> <xref:System.ReadOnlySpan%601> variável ou é permitida, como mostra o exemplo a seguir:

  [!code-csharp[stackalloc in nested expressions](snippets/shared/StackallocOperator.cs#Nested)]

  > [!NOTE]
  > É recomendável usar os tipos <xref:System.Span%601> ou <xref:System.ReadOnlySpan%601> sempre que possível para trabalhar com memória alocada na pilha.

- Um [tipo de ponteiro](../../programming-guide/unsafe-code-pointers/pointer-types.md), como mostra o seguinte exemplo:

  [!code-csharp[stackalloc pointer](snippets/shared/StackallocOperator.cs#AssignToPointer)]

  Como mostra o exemplo anterior, você precisa usar um contexto `unsafe` ao trabalhar com tipos de ponteiro.

  No caso de tipos de ponteiro, você pode usar uma `stackalloc` expressão somente em uma declaração de variável local para inicializar a variável.

A quantidade de memória disponível na pilha é limitada. Se você alocar muita memória na pilha, um <xref:System.StackOverflowException> será lançado. Para evitar isso, siga as regras abaixo:

- Limite a quantidade de memória que você aloca com `stackalloc` :

  [!code-csharp[limit stackalloc](snippets/shared/StackallocOperator.cs#LimitStackalloc)]

  Como a quantidade de memória disponível na pilha depende do ambiente no qual o código é executado, seja conservador quando você define o valor de limite real.

- Evite usar `stackalloc` loops internos. Aloque o bloco de memória fora de um loop e reutilize-o dentro do loop.

O conteúdo da memória recém-alocada é indefinido. Você deve inicializá-lo antes do uso. Por exemplo, você pode usar o <xref:System.Span%601.Clear%2A?displayProperty=nameWithType> método que define todos os itens com o valor padrão do tipo `T` .

A partir do C# 7,3, você pode usar a sintaxe do inicializador de matriz para definir o conteúdo da memória alocada recentemente. O seguinte exemplo demonstra várias maneiras de fazer isso:

[!code-csharp[stackalloc initialization](snippets/shared/StackallocOperator.cs#StackallocInit)]

Em Expression `stackalloc T[E]` , `T` deve ser um [tipo não gerenciado](../builtin-types/unmanaged-types.md) e `E` deve ser avaliado como um valor [int](../builtin-types/integral-numeric-types.md) não negativo.

## <a name="security"></a>Segurança

O uso de `stackalloc` habilita automaticamente os recursos de detecção de estouro de buffer no CLR (Common Language Runtime). Se for detectada uma estouro de buffer, o processo será encerrado assim que possível para minimizar a chance de o código mal-intencionado ser executado.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte a seção de [alocação de pilha](~/_csharplang/spec/unsafe-code.md#stack-allocation) da [especificação da linguagem C#](~/_csharplang/spec/introduction.md) e a nota de proposta de recurso [permitir `stackalloc` em contextos aninhados](~/_csharplang/proposals/csharp-8.0/nested-stackalloc.md) .

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Operadores relacionados a ponteiro](pointer-related-operators.md)
- [Tipos de ponteiro](../../programming-guide/unsafe-code-pointers/pointer-types.md)
- [Tipos relacionados a memória e extensão](../../../standard/memory-and-spans/index.md)
- [Dos e não stackalloc](https://vcsjones.dev/2020/02/24/stackalloc/)
