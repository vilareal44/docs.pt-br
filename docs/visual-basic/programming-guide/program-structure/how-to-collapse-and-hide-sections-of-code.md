---
title: 'Como: Recolher e ocultar seções do código'
ms.date: 07/20/2015
helpviewer_keywords:
- Visual Basic, code collapsing
- Visual Basic, code hiding
- Visual Basic code, collapsing and hiding
ms.assetid: b770e8f5-e07d-491a-ab4b-a977980f9ba2
ms.openlocfilehash: c11affe9c4dd4251ba8ff4b87029d314970b5fcb
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404843"
---
# <a name="how-to-collapse-and-hide-sections-of-code-visual-basic"></a>Como recolher e ocultar seções do código (Visual Basic)

A `#Region` diretiva permite que você recolha e oculte seções de código em arquivos Visual Basic. A `#Region` diretiva permite especificar um bloco de código que você pode expandir ou recolher ao usar o editor de código do Visual Studio. A capacidade de ocultar o código seletivamente torna os arquivos mais gerenciáveis e fáceis de ler. Para obter mais informações, consulte [Estrutura de tópicos](/visualstudio/ide/outlining).

`#Region`as diretivas dão suporte à semântica de bloco de código, como `#If...#End If` . Isso significa que eles não podem começar em um bloco e terminar em outro; o início e o fim devem estar no mesmo bloco. `#Region`Não há suporte para diretivas em funções.

## <a name="to-collapse-and-hide-a-section-of-code"></a>Para recolher e ocultar uma seção de código

Coloque a seção de código entre as `#Region` `#End Region` instruções e, como no exemplo a seguir:

[!code-vb[VbVbalrConditionalComp#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrConditionalComp/VB/Class1.vb#6)]

O `#Region` bloco pode ser usado várias vezes em um arquivo de código; assim, os usuários podem definir seus próprios blocos de procedimentos e classes que podem, por sua vez, ser recolhidos. `#Region`os blocos também podem ser aninhados dentro de outros `#Region` blocos.

> [!NOTE]
> Ocultar o código não impede que ele seja compilado e não afeta as `#If...#End If` instruções.

## <a name="see-also"></a>Confira também

- [Compilação condicional](conditional-compilation.md)
- [#Region diretiva](../../language-reference/directives/region-directive.md)
- [#If... Diretivas then... #Else](../../language-reference/directives/if-then-else-directives.md)
- [Estrutura de tópicos](/visualstudio/ide/outlining)
