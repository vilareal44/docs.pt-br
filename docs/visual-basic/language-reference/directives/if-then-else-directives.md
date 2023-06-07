---
title: '#Diretivas If...Then...#Else'
ms.date: 04/11/2018
f1_keywords:
- vb.#EndIf
- '#End If'
- '#Then'
- '#ElseIf'
- vb.#ElseIf
- vb.#Else
- vb.#If
helpviewer_keywords:
- Visual Basic code, compiling
- '#If directive [Visual Basic]'
- conditional compilation [Visual Basic], directives
- '#End if directive [Visual Basic]'
- selective compiling
- else directive (#else)
- '#Else directive [Visual Basic]'
ms.assetid: 10bba104-e3fd-451b-b672-faa472530502
ms.openlocfilehash: 7054a6ada4583c5d89e020437eb622a59d3eb17a
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410004"
---
# <a name="ifthenelse-directives"></a>Diretivas #If...Then...#Else

Compila condicionalmente os blocos selecionados de código de Visual Basic.

## <a name="syntax"></a>Sintaxe

```vb
#If expression Then
   statements
[ #ElseIf expression Then
   [ statements ]
...
#ElseIf expression Then
   [ statements ] ]
[ #Else
   [ statements ] ]
#End If
```

## <a name="parts"></a>Partes

`expression`  
Necessário para `#If` `#ElseIf` instruções and, opcional em outro lugar. Qualquer expressão, consistindo exclusivamente em uma ou mais constantes de compilador condicionais, literais e operadores, que é avaliada como `True` ou `False` .

`statements`  
Necessário para `#If` bloco de instrução, opcional em outro lugar. Visual Basic linhas de programa ou diretivas de compilador que são compiladas se a expressão associada for avaliada como `True` .

`#End If`  
Encerra o `#If` bloco de instruções.

## <a name="remarks"></a>Comentários

Na superfície, o comportamento das `#If...Then...#Else` diretivas é exibido da mesma forma que as `If...Then...Else` instruções. No entanto, as `#If...Then...#Else` diretivas avaliam o que é compilado pelo compilador, enquanto as `If...Then...Else` instruções avaliam condições em tempo de execução.

A compilação condicional é normalmente usada para compilar o mesmo programa para diferentes plataformas. Ele também é usado para impedir que o código de depuração apareça em um arquivo executável. O código excluído durante a compilação condicional é completamente omitido do arquivo executável final, portanto, não tem efeito sobre o tamanho ou o desempenho.

Independentemente do resultado de qualquer avaliação, todas as expressões são avaliadas usando `Option Compare Binary` . A `Option Compare` instrução não afeta as expressões nas `#If` `#ElseIf` instruções e.

> [!NOTE]
> Não existe uma forma de linha única `#If` das `#Else` diretivas,, `#ElseIf` e `#End If` . Nenhum outro código pode aparecer na mesma linha que qualquer uma das diretivas.

As instruções em um bloco de compilação condicional devem ser instruções lógicas completas. Por exemplo, você não pode compilar condicionalmente apenas os atributos de uma função, mas pode declarar condicionalmente a função junto com seus atributos:

```vb
#If DEBUG Then
<WebMethod()>
Public Function SomeFunction() As String
#Else
<WebMethod(CacheDuration:=86400)>
Public Function SomeFunction() As String
#End If
```

## <a name="example"></a>Exemplo

Este exemplo usa a `#If...Then...#Else` construção para determinar se deve compilar determinadas instruções.

[!code-vb[VbVbalrConditionalComp#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#1)]

## <a name="see-also"></a>Confira também

- [#Const diretiva](const-directive.md)
- [Instrução If...Then...Else](../statements/if-then-else-statement.md)
- [Compilação condicional](../../programming-guide/program-structure/conditional-compilation.md)
- <xref:System.Diagnostics.ConditionalAttribute?displayProperty=nameWithType>
