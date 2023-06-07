---
title: 'Como: Rotular instruções'
ms.date: 07/20/2015
helpviewer_keywords:
- colons (:)
- statements [Visual Basic], labels
- ': separator character'
- Visual Basic code, labeling statements
ms.assetid: 38f1ff43-2054-42cb-963b-1998e60c6ed4
ms.openlocfilehash: 8f04d592c51b6a0630bfe623fd3574555aef9ff8
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403207"
---
# <a name="how-to-label-statements-visual-basic"></a>Como rotular instruções (Visual Basic)

Os blocos de instrução são compostos por linhas de código delimitadas por dois-pontos. As linhas de código precedidas por uma cadeia de caracteres de identificação ou um número inteiro são consideradas *rotuladas*. Os rótulos de instrução são usados para marcar uma linha de código para identificá-lo para uso com instruções como `On Error Goto` .

Os rótulos podem ser identificadores de Visual Basic válidos — como aqueles que identificam elementos de programação — ou literais inteiros. Um rótulo deve aparecer no início de uma linha de código-fonte e deve ser seguido por dois-pontos, independentemente de ser seguido por uma instrução na mesma linha.

O compilador identifica os rótulos verificando se o início da linha corresponde a qualquer identificador já definido. Caso contrário, o compilador pressupõe que ele é um rótulo.

Os rótulos têm seu próprio espaço de declaração e não interferem em outros identificadores. O escopo de um rótulo é o corpo do método. A declaração de rótulo tem precedência em qualquer situação ambígua.

> [!NOTE]
> Os rótulos só podem ser usados em instruções Executáveis dentro de métodos.

## <a name="to-label-a-line-of-code"></a>Para rotular uma linha de código

Coloque um identificador, seguido por dois-pontos, no início da linha de código-fonte.

Por exemplo, as linhas de código a seguir são rotuladas com `Jump` e `120` , respectivamente:

[!code-vb[VbVbalrStatements#708](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#708)]

## <a name="see-also"></a>Confira também

- [Instruções](../language-features/statements.md)
- [Nomes de elementos declarados](../language-features/declared-elements/declared-element-names.md)
- [Estrutura do programa e convenções de código](program-structure-and-code-conventions.md)
