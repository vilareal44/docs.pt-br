---
title: Instrução If...Then...Else
ms.date: 04/16/2018
f1_keywords:
- vb.ElseIf
- vb.Then
- vb.If
- vb.EndIf
helpviewer_keywords:
- ElseIf statement [Visual Basic], If...Then...Else
- Then statement [Visual Basic], If...Then...Else
- control flow [Visual Basic], branching
- execution [Visual Basic], conditional
- TypeOf...Is expression, and If...Then...Else statements
- If...Then...Else statements
- If statement [Visual Basic], If...Then...Else
- If statement [Visual Basic]
- Is operator [Visual Basic], in If...Then...Else statements
- If Operator [Visual Basic]
- branching [Visual Basic], conditional
- If function [Visual Basic], and If...Then...Else statements
- Else statement [Visual Basic]
ms.assetid: 790068a2-1307-4e28-8a72-be5ebda099e9
ms.openlocfilehash: 0884b71c24742286e695e720add9d00dd4bfe52b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404583"
---
# <a name="ifthenelse-statement-visual-basic"></a>Instrução If...Then... (Visual Basic)

Execute um grupo de instruções condicionalmente, dependendo do valor de uma expressão.

## <a name="syntax"></a>Sintaxe

```vb
' Multiline syntax:
If condition [ Then ]
    [ statements ]
[ ElseIf elseifcondition [ Then ]
    [ elseifstatements ] ]
[ Else
    [ elsestatements ] ]
End If

' Single-line syntax:
If condition Then [ statements ] [ Else [ elsestatements ] ]
```

## <a name="quick-links-to-example-code"></a>Links rápidos para código de exemplo

Este artigo inclui vários exemplos que ilustram o uso do `If` .. `Then` . ...`Else` privacidade

- [Exemplo de sintaxe multilinha](#multi-line)
- [Exemplo de sintaxe aninhada](#nested)
- [Exemplo de sintaxe de linha única](#single-line)

## <a name="parts"></a>Partes

`condition` \
Obrigatórios. Expressão. Deve avaliar para `True` ou `False` , ou para um tipo de dados que é implicitamente conversível para `Boolean` .

Se a expressão for uma [Nullable](../../programming-guide/language-features/data-types/nullable-value-types.md) `Boolean` variável anulável que é avaliada como [Nothing](../nothing.md), a condição será tratada como se fosse `False` , e os `ElseIf` blocos serão avaliados, se existirem, ou o `Else` bloco será executado se existir.

`Then` \
Necessário na sintaxe de linha única; opcional na sintaxe de várias linhas.

`statements` \
Opcional. Uma ou mais instruções `If` a seguir... `Then` que são executadas se for `condition` avaliada como `True` .

`elseifcondition` \
Obrigatório se `ElseIf` estiver presente. Expressão. Deve avaliar para `True` ou `False` , ou para um tipo de dados que é implicitamente conversível para `Boolean` .

`elseifstatements` \
Opcional. Uma ou mais instruções `ElseIf` a seguir... `Then` que são executadas se for `elseifcondition` avaliada como `True` .

`elsestatements` \
Opcional. Uma ou mais instruções que são executadas se nenhuma `condition` expressão anterior ou `elseifcondition` for avaliada como `True` .

`End If` \
Finaliza a versão de várias linhas de `If` ... `Then` ...`Else` impeça.

## <a name="remarks"></a>Comentários

### <a name="multiline-syntax"></a>Sintaxe de várias linhas

Quando um `If` ... `Then` ...`Else` foi encontrada uma instrução, `condition` é testada. Se `condition` for `True` , as instruções a seguir `Then` serão executadas. Se `condition` for `False` , cada `ElseIf` instrução (se houver) será avaliada na ordem. Quando um `True` `elseifcondition` é encontrado, as instruções imediatamente após as associadas `ElseIf` são executadas. Se não for `elseifcondition` avaliada como `True` , ou se não houver `ElseIf` instruções, as instruções a seguir `Else` serão executadas. Depois de executar as instruções a seguir `Then` , `ElseIf` ou `Else` , a execução continuará com a instrução a seguir `End If` .

As `ElseIf` `Else` cláusulas e são ambas opcionais. Você pode ter quantas `ElseIf` cláusulas desejar em um `If` .. `Then` . ...`Else` instrução, mas nenhuma `ElseIf` cláusula pode aparecer após uma `Else` cláusula. `If`...`Then` ...`Else` as instruções podem ser aninhadas entre si.

Na sintaxe de várias linhas, a `If` instrução deve ser a única instrução na primeira linha. As `ElseIf` `Else` instruções, e `End If` podem ser precedidas apenas por um rótulo de linha. A `If` ... `Then` ...`Else` o bloco deve terminar com uma `End If` instrução.

> [!TIP]
> A [seleção... A instrução Case](select-case-statement.md) pode ser mais útil quando você avalia uma única expressão que tem vários valores possíveis.

### <a name="single-line-syntax"></a>Sintaxe de linha única

Você pode usar a sintaxe de linha única para uma única condição com o código a ser executado se for verdadeiro. No entanto, a sintaxe de várias linhas fornece mais estrutura e flexibilidade e é mais fácil de ler, manter e depurar.

O que segue a `Then` palavra-chave é examinado para determinar se uma instrução é de linha única `If` . Se algo diferente de um comentário aparecer depois da `Then` mesma linha, a instrução será tratada como uma instrução de linha única `If` . Se `Then` estiver ausente, deverá ser o início de uma linha múltipla `If` .. `Then` . ...`Else`.

Na sintaxe de linha única, você pode ter várias instruções executadas como o resultado de uma `If` decisão... `Then` . Todas as instruções devem estar na mesma linha e separadas por dois-pontos.

## <a name="multiline-syntax-example"></a>Exemplo de sintaxe multilinha

<a name="multi-line"></a>

O exemplo a seguir ilustra o uso da sintaxe de várias linhas do `If` .. `Then` . ...`Else` privacidade.

[!code-vb[VbVbalrStatements#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#101)]

## <a name="nested-syntax-example"></a>Exemplo de sintaxe aninhada

<a name="nested"></a>

O exemplo a seguir contém aninhado `If` ... `Then` ...`Else` instruções.

[!code-vb[VbVbalrStatements#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#102)]

## <a name="single-line-syntax-example"></a>Exemplo de sintaxe de linha única

<a name="single-line"></a>O exemplo a seguir ilustra o uso da sintaxe de linha única.

[!code-vb[VbVbalrStatements#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/class6.vb#103)]

## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.Choose%2A>
- <xref:Microsoft.VisualBasic.Interaction.Switch%2A>
- [#If... Diretivas then... #Else](../directives/if-then-else-directives.md)
- [Instrução Select...Case](select-case-statement.md)
- [Estruturas de Controle Aninhadas](../../programming-guide/language-features/control-flow/nested-control-structures.md)
- [Estruturas de Decisão](../../programming-guide/language-features/control-flow/decision-structures.md)
- [Operadores lógicos e bit a bit no Visual Basic](../../programming-guide/language-features/operators-and-expressions/logical-and-bitwise-operators.md)
- [Operador If](../operators/if-operator.md)
