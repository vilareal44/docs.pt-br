---
title: Instrução switch do C#
ms.date: 04/09/2019
f1_keywords:
- switch_CSharpKeyword
- switch
- case
- case_CSharpKeyword
helpviewer_keywords:
- switch statement [C#]
- switch keyword [C#]
- case statement [C#]
- default keyword [C#]
ms.assetid: 44bae8b8-8841-4d85-826b-8a94277daecb
ms.openlocfilehash: 9335399be2d4909a02fecbf2959c6f5608664732
ms.sourcegitcommit: da21fc5a8cce1e028575acf31974681a1bc5aeed
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/08/2020
ms.locfileid: "84493663"
---
# <a name="switch-c-reference"></a>switch (Referência em C#)

Este artigo aborda a `switch` instrução. Para obter informações sobre a `switch` expressão (introduzida no C# 8,0), consulte o artigo sobre [ `switch` expressões](../operators/switch-expression.md) na seção [expressões e operadores](../operators/index.md) .

`switch` é uma instrução de seleção que escolhe uma única *seção switch* para ser executada de uma lista de candidatas com base em uma correspondência de padrão com a *expressão de correspondência*.

[!code-csharp[switch#1](~/samples/snippets/csharp/language-reference/keywords/switch/switch1.cs#1)]

A instrução `switch` geralmente é usada como uma alternativa para um constructo [if-else](if-else.md) se uma única expressão é testada com três ou mais condições. Por exemplo, a instrução `switch` a seguir determina se uma variável do tipo `Color` tem um dos três valores:

[!code-csharp[switch#3](~/samples/snippets/csharp/language-reference/keywords/switch/switch3.cs#1)]

Ela é equivalente ao exemplo a seguir que usa um constructo `if`-`else`.

[!code-csharp[switch#3a](~/samples/snippets/csharp/language-reference/keywords/switch/switch3a.cs#1)]

## <a name="the-match-expression"></a>A expressão de correspondência

A expressão de correspondência fornece o valor para corresponder aos padrões nos rótulos `case`. Sua sintaxe é:

```csharp
   switch (expr)
```

No C# 6 e versões anteriores, a expressão de correspondência deve ser uma expressão que retorna um valor dos seguintes tipos:

- um [char](../builtin-types/char.md).
- uma [cadeia de caracteres](../builtin-types/reference-types.md).
- um [bool](../builtin-types/bool.md).
- um valor [integral](../builtin-types/integral-numeric-types.md) , como um `int` ou um `long` .
- um valor de [Enumeração](../builtin-types/enum.md) .

Começando com o C# 7.0, a expressão de correspondência pode ser qualquer expressão não nula.

## <a name="the-switch-section"></a>A seção switch

Uma instrução `switch` inclui uma ou mais seções do comutador. Cada seção switch contém um ou mais *rótulos case* (em um rótulo case ou padrão) seguidos por uma ou mais instruções. A instrução `switch` pode incluir no máximo um rótulo padrão colocado em qualquer seção switch. O exemplo a seguir mostra uma instrução `switch` simples que tem três seções switch, cada uma contendo duas instruções. A segunda seção switch contém os rótulos `case 2:` e `case 3:`.

Uma instrução `switch` pode incluir qualquer número de seções switch e cada seção pode ter um ou mais rótulos case, conforme mostrado no exemplo a seguir. No entanto, dois rótulos case não podem conter a mesma expressão.

[!code-csharp[switch#2](~/samples/snippets/csharp/language-reference/keywords/switch/switch2.cs#1)]

Apenas uma seção switch em uma instrução switch é executada. O C# não permite que a execução continue de uma seção switch para a próxima. Por isso, o código a seguir gera um erro do compilador, CS0163: "O controle não pode passar de um rótulo case (\<case label>) para outro".

```csharp
switch (caseSwitch)
{
    // The following switch section causes an error.
    case 1:
        Console.WriteLine("Case 1...");
        // Add a break or other jump statement here.
    case 2:
        Console.WriteLine("... and/or Case 2");
        break;
}
```

Esse requisito é atendido normalmente saindo explicitamente da seção switch usando uma instrução [break](break.md), [goto](goto.md) ou [return](return.md). No entanto, o código a seguir também é válido porque garante que o controle do programa não pode passar para a seção switch `default`.

[!code-csharp[switch#4](~/samples/snippets/csharp/language-reference/keywords/switch/switch4.cs#1)]

A execução da lista de instruções na seção switch com um rótulo case que corresponde à expressão de correspondência começa com a primeira instrução e continua pela lista de instruções, normalmente até uma instrução de salto como `break`, `goto case`, `goto label`, `return` ou `throw` ser atingida. Nesse ponto, o controle é transferido para fora da instrução `switch` ou para outro rótulo case. Se uma instrução `goto` for usada, ela deverá transferir controle para um rótulo de constante. Essa restrição é necessária, já que tentar transferir o controle para um rótulo não constante pode ter efeitos colaterais indesejáveis, tais como transferência do controle para um local não intencional no código ou criação de um loop infinito.

## <a name="case-labels"></a>Rótulos case

Cada rótulo case especifica um padrão a ser comparado com a expressão de correspondência (a variável `caseSwitch` nos exemplos anteriores). Se eles corresponderem, o controle será transferido para a seção switch que contém o **primeiro** rótulo case correspondente. Se nenhum padrão de rótulo de caso corresponder à expressão de correspondência, o controle será transferido para a seção com o rótulo de caso `default`, se existir algum. Se não houver nenhum case `default`, nenhuma declaração em qualquer seção switch será executada e o controle será transferido para fora da instrução `switch`.

Para obter informações sobre a instrução `switch` e a correspondência de padrões, consulte a seção [Correspondência de padrões com a instrução `switch`](#pattern-matching with-the-switch-statement).

Como o C# 6 dá suporte apenas ao padrão de constante e não permite a repetição de valores de constantes, os rótulos case definem valores mutuamente exclusivos e apenas um padrão pode corresponder à expressão de correspondência. Como resultado, a ordem na qual as instruções `case` aparecem não é importante.

No entanto, no C# 7.0, como há suporte para outros padrões, os rótulos case não precisam definir valores mutuamente exclusivos e vários padrões podem corresponder à expressão de correspondência. Como são executadas apenas as instruções na primeira seção switch que contém o primeiro padrão de correspondência, a ordem na qual as instruções `case` aparecem agora é importante. Se o C# detecta uma seção switch cujas instruções case são equivalentes ou são subconjuntos de instruções anteriores, ele gera um erro do compilador, CS8120, “O switch case já foi tratado por um case anterior”.

O exemplo a seguir ilustra uma instrução `switch` que usa uma variedade de padrões não mutuamente exclusivos. Se você mover a seção switch `case 0:` para que ela não seja mais a primeira seção na instrução `switch`, o C# gera um erro do compilador porque um inteiro cujo valor é zero é um subconjunto de todos os inteiros, que é o padrão definido pela instrução `case int val`.

[!code-csharp[switch#5](~/samples/snippets/csharp/language-reference/keywords/switch/switch5.cs#1)]

Você pode corrigir esse problema e eliminar o aviso do compilador em uma das duas maneiras:

- Alterando a ordem das seções switch.

- Usando uma [cláusula where](#the-case-statement-and-the-when-clause) no rótulo `case`.

## <a name="the-default-case"></a>O case `default`

O case `default` especifica a seção switch a ser executada se a expressão de correspondência não corresponder a nenhum outro rótulo `case`. Se um case `default` não estiver presente e a expressão de correspondência não corresponder a nenhum outro rótulo `case`, o fluxo do programa passará pela instrução `switch`.

O case `default` pode aparecer em qualquer ordem na instrução `switch`. Independentemente de sua ordem no código-fonte, ele é sempre avaliado por último, afinal os rótulos `case` foram avaliados.

## <a name="pattern-matching-with-the-switch-statement"></a> Correspondência de padrões com a instrução `switch`

Cada instrução `case` define um padrão que, se corresponde à expressão de correspondência, faz com que sua seção switch recipiente seja executada. Todas as versões do C# dão suporte ao padrão de constante. Começando com o C# 7.0, há suporte para os padrões restantes.

### <a name="constant-pattern"></a>Padrão de constante

O padrão de constante testa se a expressão de correspondência é igual a uma constante especificada. Sua sintaxe é:

```csharp
   case constant:
```

em que *constant* é o valor para testar. *constant* pode ser qualquer uma das expressões de constante a seguir:

- Um literal [bool](../builtin-types/bool.md) : `true` ou `false` .
- Qualquer constante [integral](../builtin-types/integral-numeric-types.md) , como um `int` , um `long` ou um `byte` .
- O nome de uma variável `const` declarada.
- Uma constante de enumeração.
- Um literal [char](../builtin-types/char.md).
- Um literal [string](../builtin-types/reference-types.md).

A expressão de constante é avaliada da seguinte forma:

- Se *expr* e *constant* forem tipos integrais, o operador de igualdade de C# determinará se a expressão retorna `true` (ou seja, se `expr == constant`).

- Caso contrário, o valor da expressão será determinado por uma chamada ao método estático [Object.Equals(expr, constant)](xref:System.Object.Equals(System.Object,System.Object)).

O exemplo a seguir usa o padrão de constante para determinar se uma data específica é um final de semana, o primeiro dia, o último dia ou o meio da semana de trabalho. Ele avalia a propriedade <xref:System.DateTime.DayOfWeek?displayProperty=nameWithType> do dia atual em relação aos membros da enumeração <xref:System.DayOfWeek>.

[!code-csharp[switch#7](~/samples/snippets/csharp/language-reference/keywords/switch/const-pattern.cs#1)]

O exemplo a seguir usa o padrão de constante para manipular a entrada do usuário em um aplicativo de console que simula uma máquina de café automática.

[!code-csharp[switch#6](~/samples/snippets/csharp/language-reference/keywords/switch/switch6.cs)]

### <a name="type-pattern"></a>Padrão de tipo

O padrão de tipo permite a conversão e a avaliação do tipo concisa. Quando usado com a instrução `switch` para realizar a correspondência de padrões, testa se uma expressão pode ser convertida para um tipo especificado e, se puder, o converte em uma variável desse tipo. Sua sintaxe é:

```csharp
   case type varname
```

em que *type* é o nome do tipo no qual o resultado de *expr* deverá ser convertido e *varname* é o objeto para o qual o resultado de *expr* é convertido se a correspondência é bem-sucedida. O tipo de tempo de compilação *expr* pode ser um parâmetro de tipo genérico, começando com C# 7.1.

A expressão `case` será `true` se qualquer uma das condições seguintes for verdadeira:

- *expr* for uma instância do mesmo tipo que *type*.

- *expr* for uma instância de um tipo derivado de *type*. Em outras palavras, o resultado de *expr* pode sofrer upcast para uma instância de *type*.

- *expr* tem um tipo de tempo de compilação que é uma classe base de *type*, e *expr* tem um tipo de runtime que é *type* ou é derivado de *type*. O *tipo do tempo de compilação* de uma variável é o tipo da variável conforme definido em sua declaração de tipo. O *tipo de runtime* de uma variável é o tipo da instância atribuída a essa variável.

- *expr* é uma instância de um tipo que implementa a interface de *type*.

Se a expressão case for true, *varname* será definitivamente atribuído e terá o escopo local dentro da seção switch apenas.

Observe que `null` não corresponde a um tipo. Para corresponder um `null`, use o seguinte rótulo `case`:

```csharp
case null:
```

O exemplo a seguir usa o padrão de tipo para fornecer informações sobre vários tipos de coleção.

[!code-csharp[type-pattern#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern.cs#1)]

Em vez de `object`, você poderia empregar um método genérico, usando o tipo da coleção como o parâmetro de tipo, conforme mostrado no código a seguir:

[!code-csharp[type-pattern#3](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern3.cs#1)]

A versão genérica é diferente da primeira amostra de duas maneiras. Primeiro, você não pode usar o case `null`. Você não pode usar nenhum case constante porque o compilador não pode converter qualquer tipo arbitrário de `T` para qualquer tipo diferente de `object`. O que tinha sido o case `default` agora testa para um `object` não nulo. Isso significa que o case `default` testa apenas para `null`.

Sem a correspondência de padrões, esse código pode ser escrito da seguinte maneira. O uso da correspondência de padrões de tipo produz um código mais compacto e legível eliminando a necessidade de testar se o resultado de uma conversão é um `null` ou executar conversões repetidas.

[!code-csharp[type-pattern2#1](~/samples/snippets/csharp/language-reference/keywords/switch/type-pattern2.cs#1)]

## <a name="the-case-statement-and-the-when-clause"></a>A instrução `case` e a cláusula `when`

A partir do C# 7.0, como as instruções case não precisam ser mutuamente exclusivas, você pode usar a adição de uma cláusula `when` para especificar uma condição adicional que precisa ser atendida para que a instrução case seja avaliada como true. A cláusula `when` pode ser qualquer expressão que retorna um valor booliano.

O exemplo a seguir define uma classe `Shape` base, uma classe `Rectangle` que deriva de `Shape` e uma classe `Square` que deriva de `Rectangle`. Ele usa a cláusula `when` para garantir que o `ShowShapeInfo` trata um objeto `Rectangle` que recebeu comprimentos e larguras como um `Square` mesmo se ele não tiver sido instanciado como um objeto `Square`. O método não tenta exibir informações sobre um objeto que é `null` ou uma forma cuja área é zero.

[!code-csharp[when-clause#1](~/samples/snippets/csharp/language-reference/keywords/switch/when-clause.cs#1)]

Observe que a cláusula `when` no exemplo que tenta testar se um objeto `Shape` é `null` não é executada. O padrão de tipo correto para testar um `null` é `case null:`.

## <a name="c-language-specification"></a>especificação da linguagem C#

Para obter mais informações, consulte [A instrução switch](~/_csharplang/spec/statements.md#the-switch-statement) na [Especificação da linguagem C#](/dotnet/csharp/language-reference/language-specification/introduction). A especificação da linguagem é a fonte definitiva para a sintaxe e o uso de C#.

## <a name="see-also"></a>Confira também

- [Referência do C#](../index.md)
- [Guia de programação C#](../../programming-guide/index.md)
- [Palavras-chave do C#](index.md)
- [if-else](if-else.md)
- [Correspondência padrão](../../pattern-matching.md)
