---
title: Literais
description: 'Saiba mais sobre os tipos literais na linguagem de programação F #.'
ms.date: 08/15/2020
ms.openlocfilehash: 15f73db3c36f7c60ab1eeba96c63a28ebc6d7f01
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88559148"
---
# <a name="literals"></a>Literais

Este artigo fornece uma tabela que mostra como especificar o tipo de um literal em F #.

## <a name="literal-types"></a> Tipos literais

A tabela a seguir mostra os tipos literais em F #. Os caracteres que representam dígitos na notação hexadecimal não diferenciam maiúsculas de minúsculas; os caracteres que identificam o tipo diferenciam maiúsculas de minúsculas.

|Type|Descrição|Sufixo ou prefixo|Exemplos|
|----|-----------|----------------|--------|
|sbyte|inteiro de 8 bits assinado|a|`86y`<br /><br />`0b00000101y`|
|byte|número natural de 8 bits não assinado|uy|`86uy`<br /><br />`0b00000101uy`|
|int16|inteiro de 16 bits assinado|s|`86s`|
|uint16|número natural de 16 bits não assinado|us|`86us`|
|INT<br /><br />int32|inteiro de 32 bits assinado|l ou None|`86`<br /><br />`86l`|
|uint<br /><br />uint32|número natural de 32 bits não assinado|u ou UL|`86u`<br /><br />`86ul`|
|nativeint|ponteiro nativo para um número natural assinado|n|`123n`|
|unativeint|ponteiro nativo como um número natural não assinado|anula|`0x00002D3Fun`|
|int64|inteiro de 64 bits assinado|L|`86L`|
|uint64|número natural de 64 bits não assinado|UL|`86UL`|
|único, float32|número de ponto flutuante de 32 bits|F ou f|`4.14F` ou `4.14f`|
|||alimentação|`0x00000000lf`|
|barra Clique|número de ponto flutuante de 64 bits|nenhum|`4.14` ou `2.3E+32` ou `2.3e+32`|
|||ALIMENTAÇÃO|`0x0000000000000000LF`|
|BIGINT|inteiro não limitado à representação de 64 bits|I|`9999999999999999999999999999I`|
|decimal|número fracionário representado como um ponto fixo ou um número racional|M ou m|`0.7833M` ou `0.7833m`|
|Char|Caractere unicode|nenhum|`'a'` ou `'\u0061'`|
|String|Cadeia de caracteres Unicode|nenhum|`"text\n"`<br /><br />ou<br /><br />`@"c:\filename"`<br /><br />ou<br /><br />`"""<book title="Paradise Lost">"""`<br /><br />ou<br /><br />`"string1" + "string2"`<br /><br />Consulte também [cadeias de caracteres](Strings.md).|
|byte|Caractere ASCII|B|`'a'B`|
|byte[]|Cadeia de caracteres ASCII|B|`"text"B`|
|Cadeia de caracteres ou byte []|Cadeia de caracteres textual|@ prefix|`@"\\server\share"` Unicode<br /><br />`@"\\server\share"B` LOCALIZADOS|

## <a name="named-literals"></a>Literais nomeadas

Os valores que se destinam a serem constantes podem ser marcados com o atributo [literal](https://fsharp.github.io/fsharp-core-docs/reference/fsharp-core-literalattribute.html) . Esse atributo tem o efeito de fazer com que um valor seja compilado como uma constante.

Em expressões de correspondência de padrões, os identificadores que começam com caracteres minúsculos são sempre tratados como variáveis a serem associadas, e não como literais, de modo que você geralmente deve usar maiúsculas iniciais ao definir literais.

```fsharp
[<Literal>]
let SomeJson = """{"numbers":[1,2,3,4,5]}"""

[<Literal>]
let Literal1 = "a" + "b"

[<Literal>]
let FileLocation =   __SOURCE_DIRECTORY__ + "/" + __SOURCE_FILE__

[<Literal>]
let Literal2 = 1 ||| 64

[<Literal>]
let Literal3 = System.IO.FileAccess.Read ||| System.IO.FileAccess.Write
```

## <a name="remarks"></a>Comentários

As cadeias de caracteres Unicode podem conter codificações explícitas que você pode especificar usando `\u` seguido por um código hexadecimal de 16 bits (0000-ffff) ou codificações UTF-32 que você pode especificar usando `\U` seguido por um código hexadecimal de 32 bits que representa qualquer ponto de código Unicode (00000000-0010FFFF).

O uso de outros outros operadores bit a não `|||` é permitido.

## <a name="integers-in-other-bases"></a>Inteiros em outras bases

Os inteiros de 32 bits assinados também podem ser especificados em hexadecimal, octal ou binário usando um `0x` `0o` prefixo, ou `0b` respectivamente.

```fsharp
let numbers = (0x9F, 0o77, 0b1010)
// Result: numbers : int * int * int = (159, 63, 10)
```

## <a name="underscores-in-numeric-literals"></a>Sublinhados em literais numéricos

Você pode separar dígitos com o caractere de sublinhado ( `_` ).

```fsharp
let value = 0xDEAD_BEEF

let valueAsBits = 0b1101_1110_1010_1101_1011_1110_1110_1111

let exampleSSN = 123_456_7890
```
