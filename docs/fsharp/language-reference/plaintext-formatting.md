---
title: Formatação de texto sem formatação
description: 'Saiba como usar printf e outra formatação de texto sem formatação em aplicativos e scripts em F #.'
ms.date: 07/22/2020
ms.openlocfilehash: 90a861736dae69dfbc199a19e24f587c42404737
ms.sourcegitcommit: 7476c20d2f911a834a00b8a7f5e8926bae6804d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2020
ms.locfileid: "88063777"
---
# <a name="plain-text-formatting"></a>Formatação de texto sem formatação

O F # dá suporte à formatação verificada por tipo de texto sem formatação usando as `printf` `printfn` funções relacionadas,, e `sprintf` .
Por exemplo,

```console
dotnet fsi

> printfn "Hello %s, %d + %d is %d" "world" 2 2 (2+2);;
```

fornece a saída

```fsharp
Hello world, 2 + 2 is 4
```

O F # também permite que valores estruturados sejam formatados como texto sem formatação.
Por exemplo, considere o exemplo a seguir que formata a saída como uma exibição matricial de tuplas.

```console
dotnet fsi

> printfn "%A" [ for i in 1 .. 5 -> [ for j in 1 .. 5 -> (i, j) ] ];;

[[(1, 1); (1, 2); (1, 3); (1, 4); (1, 5)];
 [(2, 1); (2, 2); (2, 3); (2, 4); (2, 5)];
 [(3, 1); (3, 2); (3, 3); (3, 4); (3, 5)];
 [(4, 1); (4, 2); (4, 3); (4, 4); (4, 5)];
 [(5, 1); (5, 2); (5, 3); (5, 4); (5, 5)]]
```

A formatação de texto simples estruturado é ativada quando você usa o `%A` formato em `printf` cadeias de caracteres de formatação.
Ele também é ativado ao Formatar a saída de valores em F # Interactive, em que a saída inclui informações extras e também é personalizável.
A formatação de texto sem formatação também é observável por meio de todas as chamadas para `x.ToString()` valores de União e registro em F #, incluindo aquelas que ocorrem implicitamente na depuração, no registro em log e em outras ferramentas.

## <a name="checking-of-printf-format-strings"></a>Verificação de `printf` cadeias de caracteres de formato

Um erro de tempo de compilação será relatado se uma `printf` função de formatação for usada com um argumento que não corresponda aos especificadores de formato printf na cadeia de caracteres de formato.  Por exemplo,

```fsharp
sprintf "Hello %s" (2+2)
```

fornece a saída

```console
  sprintf "Hello %s" (2+2)
  ----------------------^

stdin(3,25): error FS0001: The type 'string' does not match the type 'int'
```

Falando tecnicamente, ao usar `printf` e outras funções relacionadas, uma regra especial no compilador F # verifica o literal de cadeia de caracteres passado como a cadeia de caracteres de formato, garantindo que os argumentos subsequentes aplicados sejam do tipo correto para corresponder aos especificadores de formato usados.

## <a name="format-specifiers-for-printf"></a>Especificadores de formato para`printf`

As especificações de formato para `printf` formatos são cadeias de caracteres com `%` marcadores que indicam o formato. Espaços reservados de formato consistem `%[flags][width][.precision][type]` em onde o tipo é interpretado da seguinte maneira:

| Especificador de formato   | Tipo (s)        | Comentários                      |
|:-------------------|:---------------|:-----------------------------|
| `%b`               | bool      | Formatado como `true` ou`false`                |
| `%s`               | string    | Formatado como seu conteúdo sem escape         |
| `%c`               | char      | Formatado como o literal de caractere  |
| `%d`, `%i`         | um tipo inteiro básico | Formatado como um inteiro decimal, assinado se o tipo de inteiro básico for assinado |
| `%u`               | um tipo inteiro básico | Formatado como um inteiro decimal sem sinal   |
| `%x`, `%X`         | um tipo inteiro básico | Formatado como um número hexadecimal não assinado (a-f ou A-F para dígitos hexadecimais, respectivamente)  |
|  `%o`              | um tipo inteiro básico | Formatado como um número octal não assinado |
| `%e`, `%E`         | um tipo de ponto flutuante básico | Formatado como um valor assinado, tendo o formato `[-]d.dddde[sign]ddd` em que d é um único dígito decimal, dddd é um ou mais dígitos decimais, o DDD tem exatamente três dígitos decimais e o sinal é `+` ou`-` |
| `%f`               | um tipo de ponto flutuante básico | Formatado como um valor assinado com o formulário `[-]dddd.dddd` , em que `dddd` é um ou mais dígitos decimais. O número de dígitos antes do ponto decimal depende da magnitude do número, e o número de dígitos após o ponto decimal depende da precisão solicitada. |
| `%g`, `%G` | um tipo de ponto flutuante básico |  Formatado usando como um valor assinado, impresso em `%f` ou `%e` formato, o que for mais compacto para o valor e a precisão especificados. |
| `%M` | um `System.Decimal` valor  |    Formatado usando o `"G"` especificador de formato para`System.Decimal.ToString(format)` |
| `%O` | qualquer valor  |   Formatado por Boxing o objeto e chamando seu `System.Object.ToString()` método |
| `%A` | qualquer valor  |   Formatado usando a [formatação de texto simples estruturado](plaintext-formatting.md) com as configurações de layout padrão |
| `%a` | qualquer valor  |   Requer dois argumentos: uma função de formatação que aceita um parâmetro de contexto e o valor, e o valor específico a ser impresso |
| `%t` | qualquer valor  |   Requer um argumento: uma função de formatação que aceita um parâmetro de contexto que gera ou retorna o texto apropriado |
| `%%` | (nenhum)  |   Não requer argumentos e imprime um sinal de porcentagem simples:`%` |

Os tipos de inteiro básicos são `byte` ( `System.Byte` ), `sbyte` ( `System.SByte` ), `int16` ( `System.Int16` ), `uint16` ( `System.UInt16` ), `int32` ( `System.Int32` ), `uint32` ( `System.UInt32` ), `int64` () `System.Int64` , `uint64` ( `System.UInt64` ), `nativeint` ( `System.IntPtr` ), e `unativeint` ( `System.UIntPtr` ).
Os tipos de ponto flutuante básicos são `float` ( `System.Double` ) e `float32` ( `System.Single` ).

A largura opcional é um inteiro que indica a largura mínima do resultado. Por exemplo, `%6d` imprime um inteiro, prefixando-o com espaços para preencher pelo menos seis caracteres. Se Width for `*` , será usado um argumento inteiro extra para especificar a largura correspondente.

Os sinalizadores válidos são:

| Sinalizador   | Efeito        | Comentários                      |
|:-------------------|:---------------|:-----------------------------|
| `0`  | Adicionar zeros em vez de espaços para criar a largura necessária |    |
| `-` |  Justificar à esquerda o resultado dentro da largura especificada |   |
| `+`  | Adicionar um `+` caractere se o número for positivo (para corresponder a um `-` sinal para negativos) |   |
| caractere de espaço | Adicione um espaço extra se o número for positivo (para corresponder a um sinal "-" para negativos) |

O `#` sinalizador printf é inválido e um erro de tempo de compilação será relatado se for usado.

Os valores são formatados usando cultura invariável. As configurações de cultura são irrelevantes para a `printf` formatação, exceto quando afetam os resultados `%O` e a `%A` formatação. Para obter mais informações, consulte [formatação de texto sem formatação estruturada](plaintext-formatting.md).

## <a name="a-formatting"></a>`%A`formatação

O `%A` especificador de formato é usado para formatar valores de uma maneira legível por humanos e também pode ser útil para relatar informações de diagnóstico.

### <a name="primitive-values"></a>Valores primitivos

Ao formatar texto sem formatação usando o `%A` especificador, os valores numéricos de F # são formatados com seu sufixo e cultura invariável. Os valores de ponto flutuante são formatados usando 10 locais de precisão de ponto flutuante. Por exemplo,

```fsharp
printfn "%A" (1L, 3n, 5u, 7, 4.03f, 5.000000001, 5.0000000001)
```

grava

```console
(1L, 3n, 5u, 7, 4.03000021f, 5.000000001, 5.0)
```

Ao usar o `%A` especificador, as cadeias de caracteres são formatadas usando aspas. Os códigos de escape não são adicionados e, em vez disso, os caracteres brutos são impressos. Por exemplo,

```fsharp
printfn "%A" ("abc", "a\tb\nc\"d")

```

grava

```console
("abc", "a      b
c"d")
```

### <a name="net-values"></a>Valores do .NET

Ao formatar texto sem formatação usando o `%A` especificador, os objetos não F # .NET são formatados com `x.ToString()` o uso das configurações padrão do .NET fornecido pelo `System.Globalization.CultureInfo.CurrentCulture` e pelo `System.Globalization.CultureInfo.CurrentUICulture` .  Por exemplo,

```fsharp
open System.Globalization

let date = System.DateTime(1999, 12, 31)

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("de-DE")
printfn "Culture 1: %A" date

CultureInfo.CurrentCulture <- CultureInfo.GetCultureInfo("en-US")
printfn "Culture 2: %A" date
```

grava

```console
Culture 1: 31.12.1999 00:00:00
Culture 2: 12/31/1999 12:00:00 AM
```

### <a name="structured-values"></a>Valores estruturados

Ao formatar texto sem formatação usando o `%A` especificador, o recuo de bloco é usado para listas e tuplas F #. Isso é mostrado no exemplo anterior.
A estrutura de matrizes também é usada, incluindo matrizes multidimensionais.  Matrizes unidimensionais são mostradas com a `[| ... |]` sintaxe. Por exemplo,

```fsharp
printfn "%A" [| for i in 1 .. 20 -> (i, i*i) |]
```

grava

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25); (6, 36); (7, 49); (8, 64); (9, 81);
  (10, 100); (11, 121); (12, 144); (13, 169); (14, 196); (15, 225); (16, 256);
  (17, 289); (18, 324); (19, 361); (20, 400)|]
```

A largura de impressão padrão é 80.  Essa largura pode ser personalizada usando uma largura de impressão no especificador de formato. Por exemplo,

```fsharp
printfn "%10A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%20A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%50A" [| for i in 1 .. 5 -> (i, i*i) |]
```

grava

```console
[|(1, 1);
  (2, 4);
  (3, 9);
  (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4);
  (3, 9); (4, 16);
  (5, 25)|]
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
```

A especificação de uma largura de impressão de 0 resulta na não utilização da largura de impressão. Uma única linha de texto resultará, exceto onde as cadeias de caracteres inseridas na saída contiverem quebras de linha.  Por exemplo

```fsharp
printfn "%0A" [| for i in 1 .. 5 -> (i, i*i) |]

printfn "%0A" [| for i in 1 .. 5 -> "abc\ndef" |]
```

grava

```console
[|(1, 1); (2, 4); (3, 9); (4, 16); (5, 25)|]
[|"abc
def"; "abc
def"; "abc
def"; "abc
def"; "abc
def"|]
```

Um limite de profundidade de 4 é usado para valores de Sequence ( `IEnumerable` ), que são mostrados como `seq { ...}` . Um limite de profundidade de 100 é usado para valores de lista e matriz.
Por exemplo,

```fsharp
printfn "%A" (seq { for i in 1 .. 10 -> (i, i*i) })
```

grava

```console
seq [(1, 1); (2, 4); (3, 9); (4, 16); ...]
```

O recuo de bloco também é usado para a estrutura de valores públicos de registro e União. Por exemplo,

```fsharp
type R = { X : int list; Y : string list }

printfn "%A" { X =  [ 1;2;3 ]; Y = ["one"; "two"; "three"] }
```

grava

```console
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

Se `%+A` for usado, a estrutura privada de registros e uniões também será revelada com o uso de reflexão. Por exemplo

```fsharp
type internal R =
    { X : int list; Y : string list }
    override _.ToString() = "R"

let internal data = { X = [ 1;2;3 ]; Y = ["one"; "two"; "three"] }

printfn "external view:\n%A" data

printfn "internal view:\n%+A" data

```

grava

```console
external view:
R

internal view:
{ X = [1; 2; 3]
  Y = ["one"; "two"; "three"] }
```

### <a name="large-cyclic-or-deeply-nested-values"></a>Valores grandes, cíclicos ou profundamente aninhados

Os valores estruturados grandes são formatados para uma contagem máxima de nós de objeto geral de 10000.
Valores profundamente aninhados são formatados para uma profundidade de 100.  Em ambos os casos, `...` é usado para Elide parte da saída.  Por exemplo,

```fsharp
type Tree =
    | Tip
    | Node of Tree * Tree

let rec make n =
    if n = 0 then
        Tip
    else
        Node(Tip, make (n-1))

printfn "%A" (make 1000)
```

produz uma saída grande com algumas partes omitido:

```console
Node(Tip, Node(Tip, ....Node (..., ...)...))
```

Os ciclos são detectados nos grafos de objeto e `...` são usados em locais onde os ciclos são detectados. Por exemplo

```fsharp
type R = { mutable Links: R list }
let r = { Links = [] }
r.Links <- [r]
printfn "%A" r
```

grava

```console
{ Links = [...] }
```

### <a name="lazy-null-and-function-values"></a>Valores lentos, nulos e de função

Valores lentos são impressos como `Value is not created` ou texto equivalente quando o valor ainda não foi avaliado.

Valores nulos são impressos como `null` a menos que o tipo estático do valor seja determinado para ser um tipo de União em que `null` é uma representação permitida.

Os valores da função F # são impressos como seu nome de fechamento gerado internamente, por exemplo, `<fun:it@43-7>` .

### <a name="customize-plain-text-formatting-with-structuredformatdisplay"></a>Personalizar a formatação de texto sem formatação com`StructuredFormatDisplay`

Ao usar o `%A` especificador, a presença do `StructuredFormatDisplay` atributo em declarações de tipo é respeitada.  Isso pode ser usado para especificar o texto e a propriedade substitutos para exibir um valor. Por exemplo:

```fsharp
[<StructuredFormatDisplay("Counts({Clicks})")>]
type Counts = { Clicks:int list}

printfn "%20A" {Clicks=[0..20]}
```

grava

```console
Counts([0; 1; 2; 3;
        4; 5; 6; 7;
        8; 9; 10; 11;
        12; 13; 14;
        15; 16; 17;
        18; 19; 20])
```

### <a name="customize-plain-text-formatting-by-overriding-tostring"></a>Personalizar a formatação de texto sem formatação substituindo`ToString`

A implementação padrão de `ToString` é observável na programação em F #. Geralmente, os resultados padrão não são adequados para uso em exibição de informações voltadas para o programador ou saída do usuário e, como resultado, é comum substituir a implementação padrão.  

Por padrão, os tipos de registro e União F # substituem a implementação de `ToString` por uma implementação que usa o `sprintf "%+A"` .  Por exemplo,

```fsharp
type Counts = { Clicks:int list }

printfn "%s" ({Clicks=[0..10]}.ToString())
```

grava

```console
{ Clicks = [0; 1; 2; 3; 4; 5; 6; 7; 8; 9; 10] }
```

Para tipos de classe, nenhuma implementação padrão de `ToString` é fornecida e o padrão .net é usado, o que relata o nome do tipo. Por exemplo,

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Default structured print gives this:\n%A" data
printfn "Default ToString gives:\n%s" (data.ToString())
```

grava

```console
Default structured print gives this:
[MyClassType; MyClassType]
Default ToString gives:
[MyClassType; MyClassType]
```

Adicionar uma substituição para `ToString` pode dar melhor formatação.

```fsharp
type MyClassType(clicks: int list) =
   member _.Clicks = clicks
   override _.ToString() = sprintf "MyClassType(%0A)" clicks

let data = [ MyClassType([1..5]); MyClassType([1..5]) ]
printfn "Now structured print gives this:\n%A" data
printfn "Now ToString gives:\n%s" (data.ToString())
```

grava

```console
Now structured print gives this:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
Now ToString gives:
[MyClassType([1; 2; 3; 4; 5]); MyClassType([1; 2; 3; 4; 5])]
```

## <a name="f-interactive-structured-printing"></a>F# Interativo impressão estruturada

F# Interativo ( `dotnet fsi` ) usa uma versão estendida da formatação de texto sem formatação estruturada para relatar valores e permite personalização adicional. Para obter mais informações, consulte [F# interativo](fsharp-interactive-options.md).

## <a name="customize-debug-displays"></a>Personalizar exibições de depuração

Os depuradores para .NET respeitam o uso de atributos como `DebuggerDisplay` e `DebuggerTypeProxy` , e eles afetam a exibição estruturada de objetos nas janelas de inspeção do depurador.
O compilador F # gerou automaticamente esses atributos para tipos de registro e União discriminados, mas não tipos de classe, interface ou struct.

Esses atributos são ignorados na formatação de texto sem formatação F #, mas pode ser útil implementar esses métodos para melhorar as exibições ao depurar tipos F #.

## <a name="see-also"></a>Confira também

- [Cadeias de caracteres](strings.md)
- [Registros](records.md)
- [Uniões discriminadas](discriminated-unions.md)
- [F# Interativo](fsharp-interactive-options.md)
