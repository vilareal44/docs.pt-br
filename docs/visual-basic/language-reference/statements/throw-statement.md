---
title: Instrução Throw
ms.date: 07/20/2015
f1_keywords:
- vb.Throw
helpviewer_keywords:
- structured exception handling, throw statements
- try-catch exception handling, throw statements
- throw statement [Visual Basic], about throw statements
- throwing exceptions, throw statement
- exception handling, throw statement
- On Error statement [Visual Basic], throwing exceptions
- throwing exceptions
- exception handling, unstructured
- throw statement [Visual Basic]
ms.assetid: a6e07406-5c8a-4498-87a2-8339f3651d62
ms.openlocfilehash: 95572b1739490e90f53da6b6ec283bfb532c46d3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404129"
---
# <a name="throw-statement-visual-basic"></a>Instrução Throw (Visual Basic)

Gera uma exceção em um procedimento.

## <a name="syntax"></a>Sintaxe

```vb
Throw [ expression ]
```

## <a name="part"></a>Parte

`expression`\
Fornece informações sobre a exceção a ser gerada. Opcional ao residir em uma `Catch` instrução, caso contrário, é necessário.

## <a name="remarks"></a>Comentários

A `Throw` instrução gera uma exceção que você pode manipular com o código de manipulação de exceção estruturado ( `Try` ... `Catch` ...`Finally`) ou código de tratamento de exceção não estruturado ( `On Error GoTo` ). Você pode usar a `Throw` instrução para interceptar erros em seu código porque Visual Basic move para cima a pilha de chamadas até encontrar o código de tratamento de exceção apropriado.

Uma `Throw` instrução sem expressão só pode ser usada em uma `Catch` instrução; nesse caso, a instrução gera novamente a exceção que está sendo manipulada pela `Catch` instrução.

A `Throw` instrução redefine a pilha de chamadas para a `expression` exceção. Se `expression` não for fornecido, a pilha de chamadas permanecerá inalterada. Você pode acessar a pilha de chamadas para a exceção por meio da <xref:System.Exception.StackTrace%2A> propriedade.

## <a name="example"></a>Exemplo

O código a seguir usa a `Throw` instrução para gerar uma exceção:

[!code-vb[VbVbalrStatements#84](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#84)]

## <a name="see-also"></a>Confira também

- [Instrução Try...Catch...Finally](try-catch-finally-statement.md)
- [Instrução On Error](on-error-statement.md)
