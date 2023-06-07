---
title: Operadores anuláveis
description: 'Saiba mais sobre os operadores anuláveis que estão disponíveis na linguagem de programação F #.'
ms.date: 05/16/2016
ms.openlocfilehash: 951692ba22781f7f9e759c55bc708fc24f7a5014
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559135"
---
# <a name="nullable-operators"></a>Operadores anuláveis

Operadores anuláveis são operadores aritméticos ou de comparação que funcionam com tipos aritméticos anuláveis em um ou ambos os lados. Os tipos anuláveis surgem com frequência quando você trabalha com dados de fontes como bancos de dado que permitem nulos em vez de valores reais. Operadores anuláveis são usados frequentemente em expressões de consulta. Além dos operadores anuláveis para aritmética e comparação, os operadores de conversão podem ser usados para converter entre tipos anuláveis. Também há versões anuláveis de certos operadores de consulta.

## <a name="table-of-nullable-operators"></a>Tabela de operadores anuláveis

A tabela a seguir lista os operadores anuláveis com suporte na linguagem F #.

|Permite valor nulo à esquerda|Nullable à direita|Ambos os lados anuláveis|
|---|---|---|
|[? >=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E=%20))|[>=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3E=?%20))|[? >=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E=?%20))|
|[? >](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E%20))|[>?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3E?%20))|[? >?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3E?%20))|
|[? <=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C=%20))|[<=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C=?%20))|[? <=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C=?%20))|
|[? <](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%20))|[<?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C?%20))|[? <?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C?%20))|
|[?=](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?=%20))|[=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20=?%20))|[?=?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?=?%20))|
|[? <>](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%3E%20))|[<>?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%3C%3E?%20))|[? <>?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%3C%3E?%20))|
|[?+](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?+%20))|[+?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20+?%20))|[?+?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?+?%20))|
|[?-](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?-%20))|[-?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20-?%20))|[?-?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?-?%20))|
|[?*](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?*%20))|[*?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20*?%20))|[?*?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?*?%20))|
|[?/](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?/%20))|[/?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20/?%20))|[?/?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?/?%20))|
|[?%](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%%20))|[%?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20%?%20))|[?%?](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html#(%20?%?%20))|

## <a name="remarks"></a>Comentários

Os operadores anuláveis são incluídos no módulo [NullableOperators](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullableoperators.html) no namespace [FSharp. Linq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq.html). O tipo de dados anuláveis é `System.Nullable<'T>` .

Em expressões de consulta, os tipos anuláveis surgem durante a seleção de dados de uma fonte de dados que permite valores nulos em vez de Values. Em um banco de dados SQL Server, cada coluna de data em uma tabela tem um atributo que indica se são permitidos nulos. Se forem permitidos nulos, os dados retornados do banco de dado poderão conter nulos que não podem ser representados por um tipo de dados primitivo, como `int` , `float` e assim por diante. Portanto, os dados são retornados como um `System.Nullable<int>` em vez de `int` e `System.Nullable<float>` em vez de `float` . O valor real pode ser obtido de um `System.Nullable<'T>` objeto usando a `Value` propriedade, e você pode determinar se um `System.Nullable<'T>` objeto tem um valor chamando o `HasValue` método. Outro método útil é o `System.Nullable<'T>.GetValueOrDefault` método, que permite que você obtenha o valor ou um valor padrão do tipo apropriado. O valor padrão é alguma forma de valor "zero", como 0, 0,0 ou `false` .

Tipos anuláveis podem ser convertidos em tipos primitivos não anuláveis usando os operadores de conversão usuais, como `int` ou `float` . Também é possível converter de um tipo anulável para outro tipo anulável usando os operadores de conversão para tipos anuláveis. Os operadores de conversão apropriados têm o mesmo nome que os padrão, mas estão em um módulo separado, o módulo [anulável](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq-nullablemodule.html) no namespace [FSharp. Linq](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-linq.html) . Normalmente, você abre esse namespace ao trabalhar com expressões de consulta. Nesse caso, você pode usar os operadores de conversão anulável adicionando o prefixo `Nullable.` ao operador de conversão apropriado, conforme mostrado no código a seguir.

```fsharp
open Microsoft.FSharp.Linq

let nullableInt = new System.Nullable<int>(10)

// Use the Nullable.float conversion operator to convert from one nullable type to another nullable type.
let nullableFloat = Nullable.float nullableInt

// Use the regular non-nullable float operator to convert to a non-nullable float.
printfn "%f" (float nullableFloat)
```

A saída é `10.000000`.

Os operadores de consulta em campos de dados anuláveis, como `sumByNullable` , também existem para uso em expressões de consulta. Os operadores de consulta para tipos não anuláveis não são de tipo compatível com tipos anuláveis, portanto, você deve usar a versão anulável do operador de consulta apropriada quando estiver trabalhando com valores de dados anuláveis. Para obter mais informações, consulte [expressões de consulta](../query-expressions.md).

O exemplo a seguir mostra o uso de operadores anuláveis em uma expressão de consulta F #. A primeira consulta mostra como você escreveria uma consulta sem um operador anulável; a segunda consulta mostra uma consulta equivalente que usa um operador anulável. Para o contexto completo, incluindo como configurar o banco de dados para usar este código de exemplo, consulte [passo a passos: acessando um banco de dados SQL usando provedores de tipos](../../tutorials/type-providers/index.md).

```fsharp
open System
open System.Data
open System.Data.Linq
open Microsoft.FSharp.Data.TypeProviders
open Microsoft.FSharp.Linq

[<Generate>]
type dbSchema = SqlDataConnection<"Data Source=MYSERVER\INSTANCE;Initial Catalog=MyDatabase;Integrated Security=SSPI;">

let db = dbSchema.GetDataContext()

query {
    for row in db.Table2 do
    where (row.TestData1.HasValue && row.TestData1.Value > 2)
    select row
} |> Seq.iter (fun row -> printfn "%d %s" row.TestData1.Value row.Name)

query {
    for row in db.Table2 do
    // Use a nullable operator ?>
    where (row.TestData1 ?> 2)
    select row
} |> Seq.iter (fun row -> printfn "%d %s" (row.TestData1.GetValueOrDefault()) row.Name)
```

## <a name="see-also"></a>Confira também

- [Provedores de tipos](../../tutorials/type-providers/index.md)
- [Expressões de consulta](../query-expressions.md)
