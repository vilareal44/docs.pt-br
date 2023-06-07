---
title: Coerções e conversões de tipo – Guia de Programação em C#
description: Saiba mais sobre conversão e conversões de tipo, como conversão implícita, explícita (conversões) e definidas pelo usuário.
ms.date: 07/06/2020
helpviewer_keywords:
- type conversion [C#]
- data type conversion [C#]
- numeric conversions [C#]
- conversions [C#], type
- casting [C#]
- converting types [C#]
ms.assetid: 568df58a-d292-4b55-93ba-601578722878
ms.openlocfilehash: 040b5679b1e6666a7f0308e5990781a2ef86c530
ms.sourcegitcommit: 552b4b60c094559db9d8178fa74f5bafaece0caf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87381951"
---
# <a name="casting-and-type-conversions-c-programming-guide"></a>Coerções e conversões de tipo (Guia de Programação em C#)

Como o C# é tipado estaticamente no tempo de compilação, depois que uma variável é declarada, ela não pode ser declarada novamente ou atribuída a um valor de outro tipo, a menos que esse tipo possa ser convertido implicitamente no tipo da variável. Por exemplo, a `string` não pode ser convertida implicitamente em `int`. Portanto, depois de declarar `i` como um `int`, não é possível atribuir a cadeia de caracteres "Hello" a ele, como mostra o código a seguir:

```csharp
int i;

// error CS0029: Cannot implicitly convert type 'string' to 'int'
i = "Hello";
```

No entanto, às vezes é necessário copiar um valor para uma variável ou um parâmetro de método de outro tipo. Por exemplo, você pode ter que passar uma variável de inteiro para um método cujo parâmetro é digitado como `double`. Ou talvez precise atribuir uma variável de classe a uma variável de um tipo de interface. Esses tipos de operações são chamados de *conversões de tipo*. No C#, você pode realizar os seguintes tipos de conversões:

- **Conversões implícitas**: nenhuma sintaxe especial é necessária porque a conversão sempre é bem sucedido e nenhum dado será perdido. Exemplos incluem conversões de tipos integrais menores para maiores e conversões de classes derivadas para classes base.

- **Conversões explícitas (conversão)**: conversões explícitas exigem uma [expressão de conversão](../../language-reference/operators/type-testing-and-cast.md#cast-expression). A conversão é necessária quando as informações podem ser perdidas na conversão ou quando a conversão pode não funcionar por outros motivos. Exemplos típicos incluem a conversão numérica para um tipo que tem menos precisão ou um intervalo menor e a conversão de uma instância de classe base para uma classe derivada.

- **Conversões definidas pelo usuário**: as conversões definidas pelo usuário são realizadas por métodos especiais que podem ser definidos para habilitar conversões explícitas e implícitas entre tipos personalizados que não têm uma relação de classe base/classe derivada. Para saber mais, confira [Operadores de conversão definidos pelo usuário](../../language-reference/operators/user-defined-conversion-operators.md).

- **Conversões com classes auxiliares**: para converter entre tipos não compatíveis, assim como inteiros e objetos <xref:System.DateTime?displayProperty=nameWithType>, ou cadeias de caracteres hexadecimais e matrizes de bytes, você pode usar a classe <xref:System.BitConverter?displayProperty=nameWithType>, a classe <xref:System.Convert?displayProperty=nameWithType> e os métodos `Parse` dos tipos numéricos internos, tais como <xref:System.Int32.Parse%2A?displayProperty=nameWithType>. Para obter mais informações, consulte [como converter uma matriz de bytes em um int](./how-to-convert-a-byte-array-to-an-int.md), [como converter uma cadeia de caracteres em um número](./how-to-convert-a-string-to-a-number.md)e [como converter entre cadeias de caracteres hexadecimais e tipos numéricos](./how-to-convert-between-hexadecimal-strings-and-numeric-types.md).

## <a name="implicit-conversions"></a>Conversões implícitas

Para tipos numéricos internos, uma conversão implícita poderá ser feita quando o valor a ser armazenado puder se ajustar à variável sem ser truncado ou arredondado. Para tipos integrais, isso significa que o intervalo do tipo de origem é um subconjunto apropriado do intervalo para o tipo de destino. Por exemplo, uma variável do tipo [long](../../language-reference/builtin-types/integral-numeric-types.md) (inteiro de 64 bits) pode armazenar qualquer valor que um [int](../../language-reference/builtin-types/integral-numeric-types.md) (inteiro de 32 bits) pode armazenar. No exemplo a seguir, o compilador converte implicitamente o valor de `num` à direita em um tipo `long` antes de atribuí-lo a `bigNum`.

[!code-csharp[csProgGuideTypes#34](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#34)]

Para obter uma lista completa de todas as conversões numéricas implícitas, consulte a seção [conversões numéricas implícitas](../../language-reference/builtin-types/numeric-conversions.md#implicit-numeric-conversions) do artigo sobre [conversões numéricas internas](../../language-reference/builtin-types/numeric-conversions.md) .

Para tipos de referência, uma conversão implícita sempre existe de uma classe para qualquer uma das suas interfaces ou classes base diretas ou indiretas. Nenhuma sintaxe especial é necessária porque uma classe derivada sempre contém todos os membros de uma classe base.

```csharp
Derived d = new Derived();

// Always OK.
Base b = d;
```

## <a name="explicit-conversions"></a>Conversões explícitas

No entanto, se uma conversão não puder ser realizada sem o risco de perda de informações, o compilador exigirá que você execute uma conversão explícita, que é chamada de *cast*. Uma conversão é uma maneira de informar explicitamente ao compilador que você pretende fazer a conversão e que você está ciente de que a perda de dados pode ocorrer, ou a conversão pode falhar em tempo de execução. Para executar uma conversão, especifique entre parênteses o tipo para o qual você está convertendo, na frente do valor ou da variável a ser convertida. O programa a seguir converte um [Double](../../language-reference/builtin-types/floating-point-numeric-types.md) em um [int](../../language-reference/builtin-types/integral-numeric-types.md). O programa não será compilado sem a conversão.

[!code-csharp[csProgGuideTypes#2](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#2)]

Para obter uma lista completa de conversões numéricas explícitas com suporte, consulte a seção de [conversões numéricas explícitas](../../language-reference/builtin-types/numeric-conversions.md#explicit-numeric-conversions) do artigo [embutir conversões numéricas](../../language-reference/builtin-types/numeric-conversions.md) .

Para tipos de referência, uma conversão explícita será necessária se você precisar converter de um tipo base para um tipo derivado:

```csharp
// Create a new derived type.
Giraffe g = new Giraffe();

// Implicit conversion to base type is safe.
Animal a = g;

// Explicit conversion is required to cast back
// to derived type. Note: This will compile but will
// throw an exception at run time if the right-side
// object is not in fact a Giraffe.
Giraffe g2 = (Giraffe)a;
```

Uma operação de conversão entre tipos de referência não altera o tipo de tempo de execução do objeto subjacente. Ela apenas altera o tipo do valor que está sendo usado como uma referência a esse objeto. Para obter mais informações, consulte [Polimorfismo](../classes-and-structs/polymorphism.md).

## <a name="type-conversion-exceptions-at-run-time"></a>Exceções de conversão de tipo em tempo de execução

Em algumas conversões de tipo de referência, o compilador não poderá determinar se uma conversão será válida. É possível que uma operação de conversão que é compilada corretamente falhe em tempo de execução. Conforme mostrado no exemplo a seguir, um tipo de conversão que falha em tempo de execução fará com que uma <xref:System.InvalidCastException> seja lançada.

[!code-csharp[csProgGuideTypes#41](~/samples/snippets/csharp/VS_Snippets_VBCSharp/CsProgGuideTypes/CS/Class1.cs#41)]

O `Test` método tem um `Animal` parâmetro, portanto, converter explicitamente o argumento `a` para um `Reptile` faz uma suposição perigosa. É mais seguro não fazer suposições, mas sim verificar o tipo. O C# fornece o operador [is](../../language-reference/operators/type-testing-and-cast.md#is-operator) para habilitar o teste de compatibilidade antes de realmente executar uma conversão. Para obter mais informações, consulte [como converter com segurança usando correspondência de padrões e os operadores as e is](../../how-to/safely-cast-using-pattern-matching-is-and-as-operators.md).

## <a name="c-language-specification"></a>especificação da linguagem C#

Para saber mais, confira a seção [Conversões](~/_csharplang/spec/conversions.md) da [Especificação da linguagem C#](~/_csharplang/spec/introduction.md).

## <a name="see-also"></a>Veja também

- [Guia de programação C#](../index.md)
- [Types](./index.md)
- [Expressão de conversão](../../language-reference/operators/type-testing-and-cast.md#cast-expression)
- [Operadores de conversões definidas pelo usuário](../../language-reference/operators/user-defined-conversion-operators.md)
- [Conversão de tipos generalizados](https://docs.microsoft.com/previous-versions/visualstudio/visual-studio-2013/yy580hbd(v=vs.120))
- [Como converter uma cadeia de caracteres em um número](./how-to-convert-a-string-to-a-number.md)
