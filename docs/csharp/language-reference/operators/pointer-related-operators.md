---
title: Operadores relacionados a ponteiro – referência do C#
description: Saiba mais sobre operadores C# que você pode usar ao trabalhar com indicadores.
ms.date: 05/20/2019
author: pkulikov
f1_keywords:
- ->_CSharpKeyword
helpviewer_keywords:
- pointer related operators [C#]
- address-of operator [C#]
- '& operator [C#]'
- pointer indirection operator [C#]
- dereference operator [C#]
- '* operator [C#]'
- pointer member access operator [C#]
- -> operator [C#]
- pointer element access [C#]
- '[] operator [C#]'
- pointer arithmetic [C#]
- pointer increment [C#]
- pointer decrement [C#]
- pointer comparison [C#]
ms.openlocfilehash: 3728778b31a4b4adc51933e8fdc6287f28e03d83
ms.sourcegitcommit: ef50c99928183a0bba75e07b9f22895cd4c480f8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2020
ms.locfileid: "87916714"
---
# <a name="pointer-related-operators-c-reference"></a>Operadores relacionados a ponteiro (referência do C#)

Você pode usar os operadores a seguir para trabalhar com ponteiros:

- Operador unário [ `&` (address-of)](#address-of-operator-) : para obter o endereço de uma variável
- Operador unário [ `*` (ponteiro indireção)](#pointer-indirection-operator-) : para obter a variável apontada por um ponteiro
- Os operadores [ `->` (acesso de membro)](#pointer-member-access-operator--) e [ `[]` (acesso de elemento)](#pointer-element-access-operator-)
- Operadores aritméticos [ `+` , `-` , `++` e `--` ](#pointer-arithmetic-operators)
- Operadores de comparação,,,, [ `==` `!=` `<` `>` `<=` e `>=` ](#pointer-comparison-operators)

Para obter informações sobre tipos de ponteiros, veja [Tipos de ponteiro](../../programming-guide/unsafe-code-pointers/pointer-types.md).

> [!NOTE]
> Qualquer operação com ponteiros exige um contexto [unsafe](../keywords/unsafe.md). O código que contém blocos não seguros deve ser compilado com a [`-unsafe`](../compiler-options/unsafe-compiler-option.md) opção do compilador.

## <a name="address-of-operator-amp"></a><a name="address-of-operator-"></a>Operador de endereço&amp;

O operador unário `&` retorna o endereço de seu operando:

[!code-csharp[address of local](snippets/shared/PointerOperators.cs#AddressOf)]

O operando do operador `&` deve ser uma variável fixa. Variáveis *fixas* são variáveis que residem em locais de armazenamento não afetados pela operação do [coletor de lixo](../../../standard/garbage-collection/index.md). No exemplo anterior, a variável local `number` é uma variável fixa, pois reside na pilha. Variáveis que residem em locais de armazenamento que podem ser afetados pelo coletor de lixo (por exemplo, realocado) são chamadas de variáveis *móveis*. Campos de objeto e elementos de matriz são exemplos de variáveis móveis. Você pode obter o endereço de uma variável móvel se você "corrigir" ou "fixar", com uma [ `fixed` instrução](../keywords/fixed-statement.md). O endereço obtido é válido somente dentro do bloco de uma `fixed` instrução. O exemplo a seguir mostra como usar uma `fixed` instrução e o `&` operador:

[!code-csharp[address of fixed](snippets/shared/PointerOperators.cs#AddressOfFixed)]

Não é possível obter o endereço de uma constante nem de um valor.

Para obter mais informações sobre variáveis fixas e móveis, veja a seção [Variáveis fixas e móveis](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables) da [Especificação de linguagem C#](~/_csharplang/spec/introduction.md).

O operador binário `&` computa o [AND lógico](boolean-logical-operators.md#logical-and-operator-) de seus operandos Boolianos ou o [AND lógico bit a bit](bitwise-and-shift-operators.md#logical-and-operator-) de seus operandos integrais.

## <a name="pointer-indirection-operator-"></a>Operador de indireção de ponteiro*

O operador unário de indireção de ponteiro `*` obtém a variável para a qual o operando aponta. Também é conhecido como o operador de desreferenciar. O operando do operador `*` deve ser de um tipo de ponteiro.

[!code-csharp[pointer indirection](snippets/shared/PointerOperators.cs#PointerIndirection)]

Não é possível aplicar o operador `*` a uma expressão do tipo `void*`.

O operador binário `*` computa o [produto](arithmetic-operators.md#multiplication-operator-) de seus operandos numéricos.

## <a name="pointer-member-access-operator--"></a>Operador de acesso a membro do ponteiro ->

O operador `->` combina [indireção do ponteiro](#pointer-indirection-operator-) e [acesso de membro](member-access-operators.md#member-access-expression-). Ou seja, se `x` for um ponteiro do tipo `T*` e `y` for um membro acessível do tipo `T` , uma expressão do formulário

```csharp
x->y
```

é equivalente a

```csharp
(*x).y
```

O exemplo a seguir demonstra o uso do operador `->`:

[!code-csharp[pointer member access](snippets/shared/PointerOperators.cs#MemberAccess)]

Não é possível aplicar o operador `->` a uma expressão do tipo `void*`.

## <a name="pointer-element-access-operator-"></a>Operador de acesso a elemento do ponteiro []

Para uma expressão `p` de um tipo de ponteiro, um acesso de elemento de ponteiro da forma `p[n]` é avaliado como `*(p + n)`, em que `n` deve ser do tipo implicitamente conversível em `int`, `uint`, `long` ou `ulong`. Para obter informações sobre o comportamento do operador `+` com ponteiros, veja a seção [Adição ou subtração de um valor integral para ou de um ponteiro](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer).

O exemplo a seguir demonstra como acessar elementos da matriz com um ponteiro e o operador `[]`:

[!code-csharp[pointer element access](snippets/shared/PointerOperators.cs#ElementAccess)]

No exemplo anterior, uma [ `stackalloc` expressão](stackalloc.md) aloca um bloco de memória na pilha.

> [!NOTE]
> O operador de acesso de elemento de ponteiro não verifica se há erros fora dos limites.

Não é possível usar `[]` para acesso de elemento de ponteiro com uma expressão do tipo `void*`.

Você também pode usar o `[]` operador para [acesso de elemento de matriz ou indexador](member-access-operators.md#indexer-operator-).

## <a name="pointer-arithmetic-operators"></a>Operadores aritméticos de ponteiro

Você pode executar as seguintes operações aritméticas com ponteiros:

- Adicionar ou subtrair um valor integral de ou para um ponteiro
- Subtrair dois ponteiros
- Incrementar ou decrementar um ponteiro

Você não pode executar essas operações com ponteiros do tipo `void*`.

Para obter informações sobre operações aritméticas com suporte com tipos numéricos, veja [Operadores aritméticos](arithmetic-operators.md).

### <a name="addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer"></a>Adição ou subtração de um valor integral a ou de um ponteiro

Para um ponteiro `p` do tipo `T*` e uma expressão `n` de um tipo implicitamente conversível em `int`, `uint`, `long` ou `ulong`, adição e subtração são definidas da seguinte maneira:

- As expressões `p + n` e `n + p` produzem um ponteiro do tipo `T*` que resulta da adição de `n * sizeof(T)` ao endereço fornecido pelo `p`.
- A expressão `p - n` produz um ponteiro do tipo `T*` que resulta da subtração de `n * sizeof(T)` ao endereço fornecido pelo `p`.

O [ `sizeof` operador](sizeof.md) Obtém o tamanho de um tipo em bytes.

O exemplo a seguir demonstra o uso do operador `+` com um ponteiro:

[!code-csharp[pointer addition](snippets/shared/PointerOperators.cs#AddNumber)]

### <a name="pointer-subtraction"></a>Subtração de ponteiro

Para dois ponteiros `p1` e `p2` do tipo `T*`, a expressão `p1 - p2` produz a diferença entre os endereços dados por `p1` e `p2` divididos por `sizeof(T)`. O tipo de resultado é `long`. Ou seja, `p1 - p2` é computado como `((long)(p1) - (long)(p2)) / sizeof(T)`.

O exemplo a seguir demonstra a subtração de ponteiro:

[!code-csharp[pointer subtraction](snippets/shared/PointerOperators.cs#SubtractPointers)]

### <a name="pointer-increment-and-decrement"></a>Incrementar e decrementar ponteiros

O operador de incremento `++`[adiciona](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer) 1 ao operando do ponteiro. O operador de decremento `--`[subtrai](#addition-or-subtraction-of-an-integral-value-to-or-from-a-pointer) 1 do operando do ponteiro.

Os dois operadores têm suporte em duas formas: sufixo (`p++` e `p--`) e prefixo (`++p` e `--p`). O resultado de `p++` e `p--` é o valor de `p` *antes* da operação. O resultado de `++p` e `--p` é o valor de `p` *depois* da operação.

O exemplo a seguir demonstra o comportamento dos operadores de incremento de sufixo e prefixo:

[!code-csharp[pointer increment](snippets/shared/PointerOperators.cs#Increment)]

## <a name="pointer-comparison-operators"></a>Operadores de comparação de ponteiro

Você pode usar os operadores `==`, `!=`, `<`, `>`, `<=` e `>=` para comparar os operandos de qualquer tipo de ponteiro, incluindo `void*`. Esses operadores comparam os endereços fornecidos pelos dois operandos como se fossem inteiros sem sinal.

Para obter informações sobre o comportamento desses operadores para operandos de outros tipos, veja os artigos [Operadores de igualdade](equality-operators.md) e [Operadores de comparação](comparison-operators.md).

## <a name="operator-precedence"></a>Precedência do operador

A lista a seguir ordena operadores relacionados a ponteiro começando da precedência mais alta até a mais baixa:

- Operadores de incremento `x++` e decremento `x--` sufixados e os operadores `->` e `[]`
- Operadores de incremento `++x` e decremento `--x` prefixados e os operadores `&` e `*`
- Operadores de adição `+` e `-`
- Operadores de comparação `<`, `>`, `<=` e `>=`
- Operadores de igualdade `==` e `!=`

Use parênteses, `()`, para alterar a ordem de avaliação imposta pela precedência do operador.

Para obter a lista completa de operadores C# ordenados por nível de precedência, consulte a seção [precedência de operador](index.md#operator-precedence) do artigo sobre [operadores do c#](index.md) .

## <a name="operator-overloadability"></a>Capacidade de sobrecarga do operador

Um tipo definido pelo usuário não pode sobrecarregar operadores relacionados a ponteiro `&`, `*`, `->` e `[]`.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, confira as seguintes seções da [especificação da linguagem C#](~/_csharplang/spec/introduction.md):

- [Variáveis fixas e móveis](~/_csharplang/spec/unsafe-code.md#fixed-and-moveable-variables)
- [O operador address-of](~/_csharplang/spec/unsafe-code.md#the-address-of-operator)
- [Indireção de ponteiro](~/_csharplang/spec/unsafe-code.md#pointer-indirection)
- [Acesso de membro do ponteiro](~/_csharplang/spec/unsafe-code.md#pointer-member-access)
- [Acesso de elemento do ponteiro](~/_csharplang/spec/unsafe-code.md#pointer-element-access)
- [Aritmética do ponteiro](~/_csharplang/spec/unsafe-code.md#pointer-arithmetic)
- [Incrementar e decrementar ponteiros](~/_csharplang/spec/unsafe-code.md#pointer-increment-and-decrement)
- [Comparação de ponteiros](~/_csharplang/spec/unsafe-code.md#pointer-comparison)

## <a name="see-also"></a>Consulte também

- [Referência de C#](../index.md)
- [Operadores e expressões C#](index.md)
- [Tipos de ponteiro](../../programming-guide/unsafe-code-pointers/pointer-types.md)
- [Palavra-chave unsafe](../keywords/unsafe.md)
- [palavra-chave Fixed](../keywords/fixed-statement.md)
- [stackalloc](stackalloc.md)
- [Operador sizeof](sizeof.md)
