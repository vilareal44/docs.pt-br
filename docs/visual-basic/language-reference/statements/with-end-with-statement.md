---
title: Instrução With...End With
ms.date: 07/20/2015
f1_keywords:
- vb.With
helpviewer_keywords:
- With keyword [Visual Basic]
- loop structures [Visual Basic], and With...End With statements
- execution [Visual Basic], on object
- instructions, repeating
- With...End With statements [Visual Basic]
- With statement [Visual Basic]
- With statement [Visual Basic], nesting
- objects [Visual Basic], accessing
- With block
- End keyword [Visual Basic], With...End With statements
ms.assetid: 340d5fbb-4f43-48ec-a024-80843c137817
ms.openlocfilehash: 50f3bd0c6e96254274b429794901e2e4ac719ad0
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401375"
---
# <a name="withend-with-statement-visual-basic"></a>Instrução With...End With (Visual Basic)

Executa uma série de instruções que referenciam repetidamente um único objeto ou estrutura de modo que as instruções possam usar uma sintaxe simplificada para acessar membros do objeto ou estrutura.  Ao usar uma estrutura, você só poderá ler os valores dos membros ou invocar métodos, e obterá um erro se tentar atribuir valores aos membros de uma estrutura usada em uma instrução `With...End With`.

## <a name="syntax"></a>Sintaxe

```vb
With objectExpression
    [ statements ]
End With
```

## <a name="parts"></a>Partes

|Termo|Definição|
|---|---|
|`objectExpression`|Obrigatórios. Uma expressão que avalia como um objeto. A expressão pode ser arbitrariamente complexa e é avaliada apenas uma vez. A expressão pode ser avaliada como qualquer tipo de dados, inclusive tipos elementares.|
|`statements`|Opcional. Uma ou mais instruções entre `With` e `End With` que podem fazer referência a membros de um objeto que é gerado pela avaliação de `objectExpression`.|
|`End With`|Obrigatórios. Finaliza a definição do bloco `With`.|

## <a name="remarks"></a>Comentários

Usando `With...End With`, você pode executar uma série de instruções em um objeto especificado sem especificar o nome do objeto várias vezes. Dentro de um bloco de instrução `With`, você pode especificar um membro do objeto que começa com um ponto, como se o objeto de instrução `With` o precedesse.

Por exemplo, para alterar várias propriedades diferentes em um único objeto, coloque as instruções de atribuição de propriedade dentro do bloco `With...End With`, fazendo referência ao objeto apenas uma vez em vez de uma vez para cada atribuição de propriedade.

Se seu código acessar o mesmo objeto em várias instruções, você ganhará os seguintes benefícios usando a instrução `With`:

- Você não precisa avaliar várias vezes uma expressão complexa ou atribuir o resultado a uma variável temporária para fazer referência a seus membros várias vezes.

- Você torna seu código mais legível eliminando expressões aplicáveis repetitivas.

O tipo de dados de `objectExpression` pode ser qualquer tipo de classe ou estrutura, ou até mesmo um tipo elementar do Visual Basic, como `Integer`.  Se `objectExpression` resultar em algo diferente de um objeto, você só poderá ler os valores de seus membros ou invocar métodos, e obterá um erro se tentar atribuir valores aos membros de uma estrutura usada em uma instrução `With...End With`.  Esse é o mesmo erro que você obteria se invocasse um método que retornasse uma estrutura e imediatamente acessasse e atribuísse um valor a um membro do resultado da função, como `GetAPoint().x = 1`.  O problema em ambos os casos é que a estrutura só existe na pilha de chamadas, e não há maneira de um membro da estrutura alterada nessas situações gravar em um local de modo que qualquer outro código no programa possa observar a alteração.

`objectExpression` é avaliada uma vez, ao entrar no bloco. Você não pode reatribuir `objectExpression` de dentro do bloco `With`.

Dentro de um bloco `With`, você pode acessar os métodos e as propriedades de apenas o objeto especificado sem qualificá-los. Você pode usar métodos e propriedades de outros objetos, mas deverá qualificá-los com seus nomes de objeto.

Você pode colocar uma instrução `With...End With` dentro de outra. As instruções `With...End With` aninhadas podem ser confusas se os objetos que estão sendo referidos não são claros em termos de contexto. Você deve fornecer uma referência totalmente qualificada a um objeto que esteja em um bloco `With` externo quando o objeto é referenciado dentro de um bloco `With` interno.

Você não pode ramificar em uma instrução `With` de fora do bloco.

A menos que o bloco contenha um loop, as instruções são executadas somente uma vez. Você pode aninhar diferentes tipos de estruturas de controle. Para obter mais informações, consulte [estruturas de controle aninhado](../../programming-guide/language-features/control-flow/nested-control-structures.md).

> [!NOTE]
> Você também pode usar a palavra-chave `With` em inicializadores de objetos. Para obter mais informações e exemplos, consulte [inicializadores de objeto: tipos nomeados e anônimos](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md) e [tipos anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md).
>
> Se você estiver usando um bloco `With` somente para inicializar as propriedades ou os campos de um objeto que acabou de instanciar, considere usar um inicializador do objeto como alternativa.

## <a name="example"></a>Exemplo

No exemplo a seguir, cada bloco `With` executa uma série de instruções em um único objeto.

[!code-vb[VbVbalrWithStatement#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrwithstatement/vb/mainwindow.xaml.vb#2)]

## <a name="example"></a>Exemplo

O exemplo a seguir aninha instruções `With…End With`. Na instrução `With` aninhada, a sintaxe faz referência ao objeto interno.

[!code-vb[VbVbalrWithStatement#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/vbvbalrwithstatement/vb/mainwindow.xaml.vb#1)]

## <a name="see-also"></a>Confira também

- <xref:System.Collections.Generic.List%601>
- [Estruturas de Controle Aninhadas](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Inicializadores de objeto: tipos nomeados e anônimos](../../programming-guide/language-features/objects-and-classes/object-initializers-named-and-anonymous-types.md)
- [Tipos anônimos](../../programming-guide/language-features/objects-and-classes/anonymous-types.md)
