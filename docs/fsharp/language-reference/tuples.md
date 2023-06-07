---
title: Tuplas
description: 'Saiba mais sobre a tupla F #, um agrupamento de valores sem nome, mas ordenados, possivelmente de tipos diferentes.'
ms.date: 05/16/2016
ms.openlocfilehash: 5d26fd5d7ec5b4939a895a6d2a6a0d7fc6c6c733
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86173283"
---
# <a name="tuples"></a>Tuplas

Uma *tupla* é um agrupamento de valores não nomeados, mas ordenados, possivelmente de tipos diferentes.  As tuplas podem ser tipos de referência ou estruturas.

## <a name="syntax"></a>Sintaxe

```fsharp
(element, ... , element)
struct(element, ... ,element )
```

## <a name="remarks"></a>Comentários

Cada *elemento* na sintaxe anterior pode ser qualquer expressão F # válida.

## <a name="examples"></a>Exemplos

Exemplos de tuplas incluem pares, corridas e assim por diante, dos mesmos ou de tipos diferentes. Alguns exemplos são ilustrados no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L6-L21)]

## <a name="obtaining-individual-values"></a>Obtendo valores individuais

Você pode usar a correspondência de padrões para acessar e atribuir nomes para elementos de tupla, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L27-L29)]

Você também pode desconstruir uma tupla por meio de correspondência de padrões fora de uma `match` expressão por meio de `let` associação:

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L34-L37)]

Ou você pode padronizar a correspondência em tuplas como entradas para funções:

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L43-L47)]

Se você precisar de apenas um elemento da tupla, o caractere curinga (o sublinhado) poderá ser usado para evitar a criação de um novo nome para um valor que não seja necessário.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L53-L54)]

A cópia de elementos de uma tupla de referência em uma tupla de struct também é simples:

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L62-L66)]

As funções `fst` e `snd` (somente tuplas de referência) retornam o primeiro e o segundo elementos de uma tupla, respectivamente.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L72-L73)]

Não há nenhuma função interna que retorne o terceiro elemento de um triplo, mas você pode facilmente escrever um da seguinte maneira.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L78-L78)]

Em geral, é melhor usar a correspondência de padrões para acessar elementos individuais de tupla.

## <a name="using-tuples"></a>Usando tuplas

As tuplas fornecem uma maneira conveniente de retornar vários valores de uma função, conforme mostrado no exemplo a seguir. Este exemplo executa divisão de inteiros e retorna o resultado arredondado da operação como um primeiro membro de um par de tupla e o restante como um segundo membro do par.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L83-L86)]

As tuplas também podem ser usadas como argumentos de função quando você deseja evitar o currying implícito de argumentos de função que é implícito pela sintaxe de função usual.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L88-L88)]

A sintaxe comum para definir a função `let sum a b = a + b` permite que você defina uma função que é o aplicativo parcial do primeiro argumento da função, conforme mostrado no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/basic-examples.fsx#L90-L94)]

Usar uma tupla como o parâmetro desabilita currying. Para obter mais informações, consulte "aplicativo parcial de argumentos" em [funções](./functions/index.md).

## <a name="names-of-tuple-types"></a>Nomes de tipos de tupla

Quando você escreve o nome de um tipo que é uma tupla, você usa o `*` símbolo para separar elementos. Para uma tupla que consiste em um `int` , um `float` e um `string` , como `(10, 10.0, "ten")` , o tipo seria escrito da seguinte maneira.

```fsharp
int * float * string
```

## <a name="interoperation-with-c-tuples"></a>Interoperação com tuplas do C#

O C# 7,0 introduziu tuplas para o idioma.  Tuplas em C# são structs e são equivalentes a tuplas struct em F #.  Se você precisar interoperar com o C#, deverá usar tuplas struct.

Isso é fácil de fazer.  Por exemplo, imagine que você tenha que passar uma tupla para uma classe C# e, em seguida, consumir seu resultado, que também é uma tupla:

```csharp
namespace CSharpTupleInterop
{
    public static class Example
    {
        public static (int, int) AddOneToXAndY((int x, int y) a) =>
            (a.x + 1, a.y + 1);
    }
}
```

No código F #, você pode passar uma tupla struct como o parâmetro e consumir o resultado como uma tupla de struct.

```fsharp
open TupleInterop

let struct (newX, newY) = Example.AddOneToXAndY(struct (1, 2))
// newX is now 2, and newY is now 3
```

### <a name="converting-between-reference-tuples-and-struct-tuples"></a>Convertendo entre tuplas de referência e tuplas de struct

Como tuplas de referência e tuplas de struct têm uma representação subjacente completamente diferente, elas não são implicitamente conversíveis.  Ou seja, o código como o seguinte não será compilado:

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/interop.fsx#L5-L12)]

Você deve fazer a correspondência de padrão em uma tupla e construir a outra com as partes constituintes.  Por exemplo:

[!code-fsharp[Main](~/samples/snippets/fsharp/tuples/interop.fsx#L18-L22)]

## <a name="compiled-form-of-reference-tuples"></a>Formulário compilado de tuplas de referência

Esta seção explica a forma de tuplas quando elas são compiladas.  As informações aqui não são necessárias para leitura, a menos que você esteja visando .NET Framework 3,5 ou inferior.

As tuplas são compiladas em objetos de um dos vários tipos genéricos, todas nomeadas `System.Tuple` , que são sobrecarregadas na arity ou no número de parâmetros de tipo. Os tipos de tupla aparecem neste formulário quando você os exibe de outro idioma, como C# ou Visual Basic, ou quando você está usando uma ferramenta que não reconhece construções F #. Os `Tuple` tipos foram introduzidos no .NET Framework 4. Se você estiver visando uma versão anterior do .NET Framework, o compilador usará versões do [System. tupla](https://msdn.microsoft.com/library/5ac7953d-acdc-4a58-bfb7-c1f6406c0fa3) da versão 2,0 da biblioteca principal do F #. Os tipos nesta biblioteca são usados somente para aplicativos direcionados às versões 2,0, 3,0 e 3,5 do .NET Framework. O encaminhamento de tipo é usado para garantir a compatibilidade binária entre os componentes .NET Framework 2,0 e .NET Framework 4 F #.

### <a name="compiled-form-of-struct-tuples"></a>Formulário compilado de tuplas de struct

As tuplas de struct (por exemplo, `struct (x, y)` ) são fundamentalmente diferentes das tuplas de referência.  Eles são compilados no <xref:System.ValueTuple> tipo, sobrecarregado por arity ou o número de parâmetros de tipo.  Eles são equivalentes a [tuplas C# 7,0](../../csharp/language-reference/builtin-types/value-tuples.md) e [Visual Basic 2017 tuplas](../../visual-basic/programming-guide/language-features/data-types/tuples.md)e interoperam bidirecionalmente.

## <a name="see-also"></a>Veja também

- [Referência de linguagem F #](index.md)
- [Tipos F#](fsharp-types.md)
