---
title: Sobrecarga de operador
description: 'Saiba como sobrecarregar operadores aritméticos em um tipo de classe ou de registro e no nível global em F #.'
ms.date: 08/15/2020
ms.openlocfilehash: fb86ceb95101fcc1f157ec9ba17a9d8145b11a91
ms.sourcegitcommit: 8bfeb5930ca48b2ee6053f16082dcaf24d46d221
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88557575"
---
# <a name="operator-overloading"></a>Sobrecarga de operador

Este tópico descreve como sobrecarregar operadores aritméticos em um tipo de classe ou de registro e no nível global.

## <a name="syntax"></a>Sintaxe

```fsharp
// Overloading an operator as a class or record member.
static member (operator-symbols) (parameter-list) =
    method-body
// Overloading an operator at the global level
let [inline] (operator-symbols) parameter-list = function-body
```

## <a name="remarks"></a>Comentários

Na sintaxe anterior, o *operador-símbolo* é um de `+` , `-` ,,, `*` `/` `=` e assim por diante. A *lista de parâmetros* especifica os operandos na ordem em que aparecem na sintaxe usual para esse operador. O *corpo do método* constrói o valor resultante.

Sobrecargas de operador para operadores devem ser estáticas. Sobrecargas de operador para operadores unários, como `+` e `-` , devem usar um til ( `~` ) no *símbolo de operador* para indicar que o operador é um operador unário e não um operador binário, como mostrado na declaração a seguir.

```fsharp
static member (~-) (v : Vector)
```

O código a seguir ilustra uma classe vector que tem apenas dois operadores, um para menos unário e outro para multiplicação por um escalar. No exemplo, são necessárias duas sobrecargas para a multiplicação escalar, pois o operador deve funcionar independentemente da ordem em que o vetor e o escalar são exibidos.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4001.fs)]

## <a name="creating-new-operators"></a>Criando novos operadores

Você pode sobrecarregar todos os operadores padrão, mas também pode criar novos operadores fora de sequências de determinados caracteres. Os caracteres de operador permitidos são,,,,,,, `!` `%` `&` `*` `+` `-` `.` `/` , `<` ,,,,,, `=` `>` `?` `@` `^` `|` e `~` . O `~` caractere tem o significado especial de tornar um operador unário e não faz parte da sequência de caracteres do operador. Nem todos os operadores podem se tornar unários.

Dependendo da sequência de caracteres exata que você usar, o operador terá uma certa precedência e associação. A associação pode ser da esquerda para a direita ou da direita para a esquerda e é usada sempre que os operadores do mesmo nível de precedência aparecem em sequência, sem parênteses.

O caractere operador não `.` afeta a precedência, de modo que, por exemplo, se você quiser definir sua própria versão de multiplicação que tenha a mesma precedência e associação como multiplicação comum, poderá criar operadores como `.*` .

Somente os operadores `?` e `?<-` podem começar com `?` .

Uma tabela que mostra a precedência de todos os operadores em F # pode ser encontrada em [referência de símbolo e operador](./symbol-and-operator-reference/index.md).

## <a name="overloaded-operator-names"></a>Nomes de operador sobrecarregados

Quando o compilador F # compila uma expressão de operador, ele gera um método que tem um nome gerado pelo compilador para esse operador. Esse é o nome que aparece na MSIL (Microsoft Intermediate Language) para o método e também em reflexão e IntelliSense. Normalmente, você não precisa usar esses nomes no código F #.

A tabela a seguir mostra os operadores padrão e seus nomes gerados correspondentes.

|Operador|Nome gerado|
|--------|--------------|
|`[]`|`op_Nil`|
|`::`|`op_Cons`|
|`+`|`op_Addition`|
|`-`|`op_Subtraction`|
|`*`|`op_Multiply`|
|`/`|`op_Division`|
|`@`|`op_Append`|
|`^`|`op_Concatenate`|
|`%`|`op_Modulus`|
|`&&&`|`op_BitwiseAnd`|
|<code>&#124;&#124;&#124;</code>|`op_BitwiseOr`|
|`^^^`|`op_ExclusiveOr`|
|`<<<`|`op_LeftShift`|
|`~~~`|`op_LogicalNot`|
|`>>>`|`op_RightShift`|
|`~+`|`op_UnaryPlus`|
|`~-`|`op_UnaryNegation`|
|`=`|`op_Equality`|
|`<=`|`op_LessThanOrEqual`|
|`>=`|`op_GreaterThanOrEqual`|
|`<`|`op_LessThan`|
|`>`|`op_GreaterThan`|
|`?`|`op_Dynamic`|
|`?<-`|`op_DynamicAssignment`|
|<code>&#124;></code>|`op_PipeRight`|
|<code><&#124;</code>|`op_PipeLeft`|
|`!`|`op_Dereference`|
|`>>`|`op_ComposeRight`|
|`<<`|`op_ComposeLeft`|
|`<@ @>`|`op_Quotation`|
|`<@@ @@>`|`op_QuotationUntyped`|
|`+=`|`op_AdditionAssignment`|
|`-=`|`op_SubtractionAssignment`|
|`*=`|`op_MultiplyAssignment`|
|`/=`|`op_DivisionAssignment`|
|`..`|`op_Range`|
|`.. ..`|`op_RangeStep`|

Observe que o `not` operador em F # não é emitido `op_Inequality` porque não é um operador simbólico. É uma função que emite IL que nega uma expressão booliana.

Outras combinações de caracteres de operador que não estão listadas aqui podem ser usadas como operadores e têm nomes que são compostos por meio da concatenação de nomes para os caracteres individuais da tabela a seguir. Por exemplo, +! se torna `op_PlusBang` .

|Caractere de operador|Nome|
|------------------|----|
|`>`|`Greater`|
|`<`|`Less`|
|`+`|`Plus`|
|`-`|`Minus`|
|`*`|`Multiply`|
|`/`|`Divide`|
|`=`|`Equals`|
|`~`|`Twiddle`|
|`%`|`Percent`|
|`.`|`Dot`|
|`&`|`Amp`|
|<code>&#124;</code>|`Bar`|
|`@`|`At`|
|`^`|`Hat`|
|`!`|`Bang`|
|`?`|`Qmark`|
|`(`|`LParen`|
|`,`|`Comma`|
|`)`|`RParen`|
|`[`|`LBrack`|
|`]`|`RBrack`|

## <a name="prefix-and-infix-operators"></a>Operadores de prefixo e infixo

Espera-se que os operadores de *prefixo* sejam colocados na frente de um operando ou operandos, de forma muito semelhante a uma função. Espera-se que os operadores de *infixos* sejam colocados entre os dois operandos.

Somente determinados operadores podem ser usados como operadores de prefixo. Alguns operadores são sempre operadores de prefixo, outros podem ser infixos ou prefixo e o restante são sempre operadores de infixos. Os operadores que começam com `!` , exceto `!=` e o operador `~` , ou sequências repetidas de `~` , são sempre operadores de prefixo. Os operadores,,,,,, `+` `-` `+.` `-.` `&` `&&` `%` e `%%` podem ser operadores de prefixo ou operadores de infixos. Você distingue a versão de prefixo desses operadores da versão de infixo adicionando um `~` no início de um operador de prefixo quando ele é definido. O `~` não é usado quando você usa o operador, somente quando ele é definido.

## <a name="example"></a>Exemplo

O código a seguir ilustra o uso de sobrecarga de operador para implementar um tipo de fração. Uma fração é representada por um numerador e um denominador. A função `hcf` é usada para determinar o fator comum mais alto, que é usado para reduzir frações.

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4002.fs)]

**Saída:**

```console
3/4 + 1/2 = 5/4
3/4 - 1/2 = 1/4
3/4 * 1/2 = 3/8
3/4 / 1/2 = 3/2
3/4 + 1 = 7/4
```

## <a name="operators-at-the-global-level"></a>Operadores no nível global

Você também pode definir operadores no nível global. O código a seguir define um operador `+?` .

[!code-fsharp[Main](~/samples/snippets/fsharp/lang-ref-2/snippet4003.fs)]

A saída do código acima é `12` .

Você pode redefinir os operadores aritméticos regulares dessa maneira porque as regras de escopo para F # ditam que os operadores recentemente definidos têm precedência sobre os operadores internos.

A palavra-chave `inline` é frequentemente usada com operadores globais, que geralmente são funções pequenas que são mais bem integradas ao código de chamada. A criação de funções de operador embutidas também permite que elas trabalhem com parâmetros de tipo resolvidos estaticamente para produzir código genérico resolvido estaticamente. Para obter mais informações, consulte [funções embutidas](./functions/inline-functions.md) e [parâmetros de tipo estaticamente resolvidos](./generics/statically-resolved-type-parameters.md).

## <a name="see-also"></a>Confira também

- [Membros](./members/index.md)
