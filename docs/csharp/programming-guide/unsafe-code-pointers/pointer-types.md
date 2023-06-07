---
title: Tipos de ponteiro – Guia de Programação em C#
description: Saiba mais sobre tipos de ponteiro. Veja exemplos de ponteiros diferentes, exemplos de código e exibir recursos adicionais disponíveis.
ms.date: 04/20/2018
helpviewer_keywords:
- unsafe code [C#], pointers
- pointers [C#]
ms.openlocfilehash: 9c62a31f9a4a090fe56fb10ac45fe2f93f1b036e
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87382029"
---
# <a name="pointer-types-c-programming-guide"></a>Tipos de ponteiro (Guia de Programação em C#)

Em um contexto inseguro, um tipo pode ser de ponteiro, valor ou referência. Uma declaração de tipo de ponteiro usa uma das seguintes formas:

``` csharp
type* identifier;
void* identifier; //allowed but not recommended
```

O tipo especificado antes do `*` em um tipo de ponteiro é chamado de **tipo referent**. Somente um [tipo não gerenciado](../../language-reference/builtin-types/unmanaged-types.md) pode ser um tipo referent.

Os tipos de ponteiro não são herdados de [objeto](../../language-reference/builtin-types/reference-types.md) e não há nenhuma conversão entre tipos de ponteiro e `object`. Além disso, as conversões boxing e unboxing não oferecem suporte a ponteiros. No entanto, você pode converter entre diferentes tipos de ponteiro e tipos de ponteiro e tipos integrais.

Quando você designa vários ponteiros na mesma declaração, o asterisco (*) é escrito junto apenas com o tipo subjacente; ele não é usado como um prefixo para cada nome de ponteiro. Por exemplo:

```csharp
int* p1, p2, p3;   // Ok
int *p1, *p2, *p3;   // Invalid in C#
```

Um ponteiro não pode apontar para uma referência ou um [struct](../../language-reference/builtin-types/struct.md) que contenha referências, pois uma referência de objeto pode ser coletada como lixo mesmo se um ponteiro estiver apontando para ela. O coletor de lixo não acompanha se um objeto está sendo apontado por qualquer tipo de ponteiro.

O valor da variável de ponteiro do tipo `myType*` é o endereço de uma variável do tipo `myType`. Estes são exemplos de declarações de tipos de ponteiro:

|Exemplo|Descrição|
|-------------|-----------------|
|`int* p`|`p` é um ponteiro para um inteiro.|
|`int** p`|`p` é um ponteiro para um ponteiro para um inteiro.|
|`int*[] p`|`p` é uma matriz unidimensional de ponteiros para inteiros.|
|`char* p`|`p` é um ponteiro para um caractere.|
|`void* p`|`p` é um ponteiro para um tipo desconhecido.|

O operador de indireção de ponteiro * pode ser usado para acessar o conteúdo no local apontado pela variável de ponteiro. Por exemplo, considere a seguinte declaração:

```csharp
int* myVariable;
```

A expressão `*myVariable` denota a variável `int` encontrada no endereço contido em `myVariable`.

Há vários exemplos de ponteiros nos tópicos [Instrução fixed](../../language-reference/keywords/fixed-statement.md) e [Conversões de ponteiro](./pointer-conversions.md). O exemplo a seguir usa a palavra-chave `unsafe` e a instrução `fixed` e mostra como incrementar um ponteiro interior.  Você pode colar esse código na função principal de um aplicativo de console para executá-lo. Estes exemplos precisam ser compilados com o conjunto de opções do compilador [-unsafe](../../language-reference/compiler-options/unsafe-compiler-option.md).

[!code-csharp[Using pointer types](snippets/FixedKeywordExamples.cs#5)]

Você não pode aplicar o operador de indireção para um ponteiro do tipo `void*`. No entanto, você pode usar uma conversão para converter um ponteiro nulo em qualquer outro tipo de ponteiro e vice-versa.

Um ponteiro pode ser `null`. Aplicar o operador de indireção a um ponteiro nulo causa um comportamento definido por implementação.

Passar ponteiros entre métodos pode causar um comportamento indefinido. Considere usar um método que retorne um ponteiro para uma variável local por meio de um parâmetro `in`, `out` ou `ref`, ou como o resultado da função. Se o ponteiro foi definido em um bloco fixo, a variável à qual ele aponta não pode mais ser corrigida.

A tabela a seguir lista os operadores e as instruções que podem operar em ponteiros em um contexto inseguro:

|Operador/Instrução|Uso|
|-------------------------|---------|
|`*`|Executa indireção de ponteiro.|
|`->`|Acessa um membro de um struct através de um ponteiro.|
|`[]`|Indexa um ponteiro.|
|`&`|Obtém o endereço de uma variável.|
|`++` e `--`|Incrementa e decrementa ponteiros.|
|`+` e `-`|Executa aritmética de ponteiros.|
|`==`, `!=`, `<`, `>`, `<=` e `>=`|Compara ponteiros.|
|[`stackalloc`](../../language-reference/operators/stackalloc.md)|Aloca memória na pilha.|
|[`fixed`privacidade](../../language-reference/keywords/fixed-statement.md)|Corrige temporariamente uma variável para que seu endereço possa ser encontrado.|

Para obter mais informações sobre operadores relacionados a ponteiro, veja [Operadores relacionados a ponteiro](../../language-reference/operators/pointer-related-operators.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [Tipos de ponteiro](~/_csharplang/spec/unsafe-code.md#pointer-types) na [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Código e ponteiros inseguros](index.md)
- [Conversões de ponteiro](pointer-conversions.md)
- [Tipos de referência](../../language-reference/keywords/reference-types.md)
- [Tipos de valor](../../language-reference/builtin-types/value-types.md)
- [UNSAFE](../../language-reference/keywords/unsafe.md)
