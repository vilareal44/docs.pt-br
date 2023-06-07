---
title: Registros
description: 'Saiba como os registros F # representam agregações simples de valores nomeados, opcionalmente com membros.'
ms.date: 08/15/2020
ms.openlocfilehash: 182b2e83c3940c866197052af102787a96e49c54
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559044"
---
# <a name="records"></a>Registros

Registros representam agregações simples de valores nomeados, opcionalmente com membros. Eles podem ser structs ou tipos de referência.  Eles são tipos de referência por padrão.

## <a name="syntax"></a>Sintaxe

```fsharp
[ attributes ]
type [accessibility-modifier] typename =
    { [ mutable ] label1 : type1;
      [ mutable ] label2 : type2;
      ... }
    [ member-list ]
```

## <a name="remarks"></a>Comentários

Na sintaxe anterior, *TypeName* é o nome do tipo de registro, *Label1* e *Label2* são nomes de valores, chamados de *Labels*e *type1* e *type2* são os tipos desses valores. *lista* de membros é a lista opcional de membros para o tipo.  Você pode usar o `[<Struct>]` atributo para criar um registro de struct em vez de um registro que é um tipo de referência.

Estes são alguns exemplos:

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1901.fs)]

Quando cada rótulo está em uma linha separada, o ponto e vírgula é opcional.

Você pode definir valores em expressões conhecidas como *expressões de registro*. O compilador infere o tipo dos rótulos usados (se os rótulos forem suficientemente distintos dos outros tipos de registro). As chaves ({}) incluem a expressão de registro. O código a seguir mostra uma expressão de registro que inicializa um registro com três elementos float com rótulos `x` `y` e `z` .

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1907.fs)]

Não use a forma reduzida se houver outro tipo que também tenha os mesmos rótulos.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1903.fs)]

Os rótulos do tipo declarado mais recentemente têm precedência sobre aqueles do tipo declarado anteriormente, portanto, no exemplo anterior, `mypoint3D` é inferido como `Point3D` . Você pode especificar explicitamente o tipo de registro, como no código a seguir.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1908.fs)]

Os métodos podem ser definidos para tipos de registro, assim como para tipos de classe.

## <a name="creating-records-by-using-record-expressions"></a>Criando registros usando expressões de registro

Você pode inicializar registros usando os rótulos que são definidos no registro. Uma expressão que faz isso é chamada de expressão de *registro*. Use chaves para colocar a expressão de registro e usar o ponto e vírgula como um delimitador.

O exemplo a seguir mostra como criar um registro.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1904.fs)]

Os pontos-e-vírgulas após o último campo na expressão de registro e na definição de tipo são opcionais, independentemente de os campos estarem todos em uma linha.

Ao criar um registro, você deve fornecer valores para cada campo. Você não pode fazer referência aos valores de outros campos na expressão de inicialização para qualquer campo.

No código a seguir, o tipo de `myRecord2` é inferido a partir dos nomes dos campos. Opcionalmente, você pode especificar o nome do tipo explicitamente.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1905.fs)]

Outra forma de construção de registros pode ser útil quando você precisa copiar um registro existente e, possivelmente, alterar alguns dos valores de campo. A linha de código a seguir ilustra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1906.fs)]

Essa forma da expressão de registro é chamada de *expressão de registro de cópia e atualização*.

Os registros são imutáveis por padrão; no entanto, você pode facilmente criar registros modificados usando uma expressão de cópia e atualização. Você também pode especificar explicitamente um campo mutável.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1909.fs)]

Não use o atributo DefaultValue com campos de registro. Uma abordagem melhor é definir as instâncias padrão de registros com campos que são inicializados para valores padrão e, em seguida, usar uma expressão de registro de cópia e atualização para definir quaisquer campos que sejam diferentes dos valores padrão.

```fsharp
// Rather than use [<DefaultValue>], define a default record.
type MyRecord =
    { Field1 : int
      Field2 : int }

let defaultRecord1 = { Field1 = 0; Field2 = 0 }
let defaultRecord2 = { Field1 = 1; Field2 = 25 }

// Use the with keyword to populate only a few chosen fields
// and leave the rest with default values.
let rr3 = { defaultRecord1 with Field2 = 42 }
```

## <a name="creating-mutually-recursive-records"></a>Criando registros recursivos mutuamente

Em algum momento ao criar um registro, talvez você queira fazer com que ele dependa de outro tipo que você gostaria de definir posteriormente. Esse é um erro de compilação, a menos que você defina os tipos de registro a serem recursivos mutuamente.

A definição de registros recursivos mutuamente é feita com a `and` palavra-chave. Isso permite que você vincule 2 ou mais tipos de registro juntos.

Por exemplo, o código a seguir define um `Person` e `Address` tipo como recursivo mutuamente:

```fsharp
// Create a Person type and use the Address type that is not defined
type Person =
  { Name: string
    Age: int
    Address: Address }
// Define the Address type which is used in the Person record
and Address =
  { Line1: string
    Line2: string
    PostCode: string
    Occupant: Person }
```

Se você definiu o exemplo anterior sem a `and` palavra-chave, ele não será compilado. A `and` palavra-chave é necessária para definições mutuamente recursivas.

## <a name="pattern-matching-with-records"></a>Correspondência de padrões com registros

Os registros podem ser usados com correspondência de padrões. Você pode especificar alguns campos explicitamente e fornecer variáveis para outros campos que serão atribuídos quando ocorrer uma correspondência. O exemplo de código a seguir ilustra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1910.fs)]

A saída desse código é a seguinte:

```console
Point is at the origin.
Point is on the x-axis. Value is 100.000000.
Point is at (10.000000, 0.000000, -1.000000).
```

## <a name="records-and-members"></a>Registros e membros

Você pode especificar membros em registros assim como você pode com classes. Não há suporte para campos. Uma abordagem comum é definir um `Default` membro estático para a construção de um registro fácil:

```fsharp
type Person =
  { Name: string
    Age: int
    Address: string }

    static member Default =
        { Name = "Phillip"
          Age = 12
          Address = "123 happy fun street" }

let defaultPerson = Person.Default
```

Se você usar um autoidentifier, esse identificador se referirá à instância do registro cujo membro é chamado:

```fsharp
type Person =
  { Name: string
    Age: int
    Address: string }

    member this.WeirdToString() =
        this.Name + this.Address + string this.Age

let p = { Name = "a"; Age = 12; Address = "abc123 }
let weirdString = p.WeirdToString()
```

## <a name="differences-between-records-and-classes"></a>Diferenças entre registros e classes

Os campos de registro diferem das classes no que são expostos automaticamente como propriedades e são usados na criação e cópia de registros. A construção do registro também difere da construção da classe. Em um tipo de registro, você não pode definir um construtor. Em vez disso, a sintaxe de construção descrita neste tópico se aplica. Classes não têm relação direta entre parâmetros de construtor, campos e propriedades.

Como tipos de União e estrutura, os registros têm semântica de igualdade estrutural. As classes têm semântica de igualdade de referência. O código de exemplo a seguir demonstra isso.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-1/snippet1911.fs)]

A saída desse código é a seguinte:

```console
The records are equal.
```

Se você escrever o mesmo código com classes, os dois objetos de classe seriam desiguais porque os dois valores representariam dois objetos no heap e apenas os endereços seriam comparados (a menos que o tipo de classe substitua o `System.Object.Equals` método).

Se você precisar de igualdade de referência para registros, adicione o atributo `[<ReferenceEquality>]` acima do registro.

## <a name="see-also"></a>Confira também

- [Tipos F#](fsharp-types.md)
- [Classes](classes.md)
- [Referência de linguagem F #](index.md)
- [Referência-igualdade](https://msdn.microsoft.com/visualfsharpdocs/conceptual/core.referenceequalityattribute-class-%5bfsharp%5d)
- [Correspondência de padrões](pattern-matching.md)
