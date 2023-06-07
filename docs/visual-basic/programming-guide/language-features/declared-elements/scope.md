---
title: Escopo
ms.date: 07/20/2015
helpviewer_keywords:
- module scope [Visual Basic]
- scope [Visual Basic], levels
- module level
- procedures [Visual Basic], scope
- declared elements [Visual Basic], scope
- namespaces [Visual Basic], scope
- scope [Visual Basic], declared elements
- scope [Visual Basic], about scope
- levels of scope [Visual Basic]
- block scope [Visual Basic]
- scope [Visual Basic], Visual Basic
- procedure scope [Visual Basic]
ms.assetid: 208106fe-79c9-4eec-93c6-55f08548895f
ms.openlocfilehash: 1bee904996257474b7457b2aefb1f17d250933cb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410728"
---
# <a name="scope-in-visual-basic"></a>Escopo no Visual Basic

O *escopo* de um elemento declarado é o conjunto de todo o código que pode fazer referência a ele sem qualificar seu nome ou disponibilizá-lo por meio de uma [instrução Imports (namespace e tipo do .net)](../../../language-reference/statements/imports-statement-net-namespace-and-type.md). Um elemento pode ter um escopo em um dos seguintes níveis:

|Nível|Descrição|
|-----------|-----------------|
|Escopo de bloco|Disponível somente dentro do bloco de código no qual ele é declarado|
|Escopo do procedimento|Disponível para todo o código dentro do procedimento no qual ele é declarado|
|Escopo do módulo|Disponível para todo o código dentro do módulo, da classe ou da estrutura na qual ele é declarado|
|Escopo do namespace|Disponível para todo o código no namespace no qual ele é declarado|

Esses níveis de progresso do escopo do mais estreito (bloco) para o mais largo (namespace), em que o *escopo mais estreito* significa o menor conjunto de código que pode se referir ao elemento sem qualificação. Para obter mais informações, consulte "níveis de escopo" nesta página.

## <a name="specifying-scope-and-defining-variables"></a>Especificando o escopo e Definindo variáveis

Você especifica o escopo de um elemento ao declará-lo. O escopo pode depender dos seguintes fatores:

- A região (bloco, procedimento, módulo, classe ou estrutura) na qual você declara o elemento

- O namespace que contém a declaração do elemento

- O nível de acesso que você declara para o elemento

Tenha cuidado ao definir variáveis com o mesmo nome, mas com escopo diferente, porque isso pode levar a resultados inesperados. Para obter mais informações, consulte [referências a elementos declarados](references-to-declared-elements.md).

## <a name="levels-of-scope"></a>Níveis de escopo

Um elemento de programação está disponível em toda a região em que você o declara. Todo o código na mesma região pode se referir ao elemento sem qualificar seu nome.

### <a name="block-scope"></a>Escopo do bloco

Um bloco é um conjunto de instruções incluídas em instruções de declaração de início e término, como a seguinte:

- `Do` e `Loop`

- `For`[ `Each` ] e`Next`

- `If` e `End If`

- `Select` e `End Select`

- `SyncLock` e `End SyncLock`

- `Try` e `End Try`

- `While` e `End While`

- `With` e `End With`

Se você declarar uma variável em um bloco, poderá usá-la somente dentro desse bloco. No exemplo a seguir, o escopo da variável de inteiro `cube` é o bloco entre `If` e `End If` , e você não pode mais se referir `cube` quando a execução passa para fora do bloco.

```vb
If n < 1291 Then
    Dim cube As Integer
    cube = n ^ 3
End If
```

> [!NOTE]
> Mesmo que o escopo de uma variável seja limitado a um bloco, seu tempo de vida ainda é o de todo o procedimento. Se você inserir o bloco mais de uma vez durante o procedimento, cada variável de bloco manterá seu valor anterior. Para evitar resultados inesperados nesse caso, é recomendável inicializar variáveis de bloco no início do bloco.

### <a name="procedure-scope"></a>Escopo do procedimento

Um elemento declarado em um procedimento não está disponível fora desse procedimento. Somente o procedimento que contém a declaração pode usá-lo. As variáveis nesse nível também são conhecidas como *variáveis locais*. Você os declara com a [instrução Dim](../../../language-reference/statements/dim-statement.md), com ou sem a palavra-chave [static](../../../language-reference/modifiers/static.md) .

O procedimento e o escopo do bloco estão fortemente relacionados. Se você declarar uma variável dentro de um procedimento, mas fora de qualquer bloco dentro desse procedimento, você pode considerar a variável como tendo o escopo de bloco, onde o bloco é o procedimento inteiro.

> [!NOTE]
> Todos os elementos locais, mesmo que sejam `Static` variáveis, são privados para o procedimento no qual aparecem. Você não pode declarar nenhum elemento usando a palavra-chave [Public](../../../language-reference/modifiers/public.md) em um procedimento.

### <a name="module-scope"></a>Escopo do módulo

Para sua conveniência, o *nível de módulo* de termo único aplica-se igualmente a módulos, classes e estruturas. Você pode declarar elementos nesse nível colocando a instrução de declaração fora de qualquer procedimento ou bloco, mas dentro do módulo, da classe ou da estrutura.

Quando você faz uma declaração no nível do módulo, o nível de acesso que você escolhe determina o escopo. O namespace que contém o módulo, a classe ou a estrutura também afeta o escopo.

Os elementos para os quais você declara o nível de acesso [privado](../../../language-reference/modifiers/private.md) estão disponíveis para cada procedimento nesse módulo, mas não para qualquer código em um módulo diferente. A `Dim` instrução no nível do módulo será padronizada `Private` se você não usar nenhuma palavra-chave de nível de acesso. No entanto, você pode tornar o nível de escopo e de acesso mais óbvio usando a `Private` palavra-chave na `Dim` instrução.

No exemplo a seguir, todos os procedimentos definidos no módulo podem se referir à variável de cadeia de caracteres `strMsg` . Quando o segundo procedimento é chamado, ele exibe o conteúdo da variável de cadeia de caracteres `strMsg` em uma caixa de diálogo.

```vb
' Put the following declaration at module level (not in any procedure).
Private strMsg As String
' Put the following Sub procedure in the same module.
Sub initializePrivateVariable()
    strMsg = "This variable cannot be used outside this module."
End Sub
' Put the following Sub procedure in the same module.
Sub usePrivateVariable()
    MsgBox(strMsg)
End Sub
```

### <a name="namespace-scope"></a>Escopo do namespace

Se você declarar um elemento no nível do módulo usando a palavra-chave [Friend](../../../language-reference/modifiers/friend.md) ou [Public](../../../language-reference/modifiers/public.md) , ele ficará disponível para todos os procedimentos em todo o namespace no qual o elemento é declarado. Com a seguinte alteração no exemplo anterior, a variável de cadeia de caracteres `strMsg` pode ser referenciada pelo código em qualquer lugar no namespace de sua declaração.

```vb
' Include this declaration at module level (not inside any procedure).
Public strMsg As String
```

O escopo do namespace inclui namespaces aninhados. Um elemento disponível de dentro de um namespace também está disponível de dentro de qualquer namespace aninhado dentro desse namespace.

Se o seu projeto não contiver nenhuma [instrução de namespace](../../../language-reference/statements/namespace-statement.md)s, tudo no projeto estará no mesmo namespace. Nesse caso, o escopo do namespace pode ser considerado como escopo do projeto. `Public`os elementos em um módulo, classe ou estrutura também estão disponíveis para qualquer projeto que faça referência a seu projeto.

## <a name="choice-of-scope"></a>Escolha do escopo

Ao declarar uma variável, você deve ter em mente os seguintes pontos ao escolher seu escopo.

### <a name="advantages-of-local-variables"></a>Vantagens das variáveis locais

As variáveis locais são uma boa opção para qualquer tipo de cálculo temporário, pelos seguintes motivos:

- **Prevenção de conflito de nomes.** Nomes de variáveis locais não são suscetíveis a conflitos. Por exemplo, você pode criar vários procedimentos diferentes contendo uma variável chamada `intTemp` . Desde que cada `intTemp` um seja declarado como uma variável local, cada procedimento reconhece apenas sua própria versão do `intTemp` . Qualquer procedimento pode alterar o valor em seu local `intTemp` sem afetar as `intTemp` variáveis em outros procedimentos.

- **Consumo de memória.** As variáveis locais consomem memória somente enquanto o procedimento está em execução. Sua memória é liberada quando o procedimento retorna ao código de chamada. Por outro lado, variáveis [compartilhadas](../../../language-reference/modifiers/shared.md) e [estáticas](../../../language-reference/modifiers/static.md) consomem recursos de memória até que seu aplicativo pare de funcionar, portanto, use-os somente quando necessário. As *variáveis de instância* consomem memória enquanto sua instância continua a existir, o que as torna menos eficiente do que as variáveis locais, mas potencialmente mais eficientes do que `Shared` ou `Static` variáveis.

### <a name="minimizing-scope"></a>Minimizando o escopo

Em geral, ao declarar qualquer variável ou constante, é uma boa prática de programação tornar o escopo o mais estreito possível (o escopo do bloco é o mais estreito). Isso ajuda a conservar a memória e minimiza as chances de seu código se referir erroneamente à variável errada. Da mesma forma, você deve declarar uma variável para ser [estática](../../../language-reference/modifiers/static.md) somente quando for necessário preservar seu valor entre chamadas de procedimento.

## <a name="see-also"></a>Confira também

- [Características do Elemento Declarado](declared-element-characteristics.md)
- [Como controlar o escopo de uma variável](how-to-control-the-scope-of-a-variable.md)
- [Tempo de vida no Visual Basic](lifetime.md)
- [Níveis de acesso no Visual Basic](access-levels.md)
- [Referências a elementos declarados](references-to-declared-elements.md)
- [Declaração de Variável](../variables/variable-declaration.md)
