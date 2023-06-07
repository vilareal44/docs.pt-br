---
title: Como acessar caracteres em cadeias de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], accessing characters
- characters [Visual Basic], accessing in strings
ms.assetid: 02c5206c-ffab-494d-b648-3b2ea358dc34
ms.openlocfilehash: fa5920cfd25f61f6e6c7d5438ef7c0e38a48fa1e
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84401947"
---
# <a name="how-to-access-characters-in-strings-in-visual-basic"></a>Como acessar caracteres em cadeias de caracteres no Visual Basic
Este exemplo demonstra como usar a <xref:System.String.Chars%2A> propriedade para acessar o caractere no local especificado em uma cadeia de caracteres.  
  
## <a name="example"></a>Exemplo  
 Às vezes, é útil ter dados sobre os caracteres na cadeia de caracteres e as posições desses caracteres na cadeia de caracteres. Você pode considerar uma cadeia de caracteres como uma matriz de personagens ( `Char` instâncias); você pode recuperar um caractere específico referenciando o índice desse caractere por meio da <xref:System.String.Chars%2A> propriedade.  
  
 [!code-vb[VbVbalrStrings#49](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#49)]  
  
 O `index` parâmetro da <xref:System.String.Chars%2A> propriedade é baseado em zero.  
  
## <a name="robust-programming"></a>Programação robusta  
 A <xref:System.String.Chars%2A> propriedade retorna o caractere na posição especificada. No entanto, alguns caracteres Unicode podem ser representados por mais de um caractere. Para saber mais sobre como trabalhar com caracteres Unicode, confira [como converter uma cadeia de caracteres em uma matriz de caracteres](how-to-convert-a-string-to-an-array-of-characters.md).  
  
 A <xref:System.String.Chars%2A> propriedade gera uma <xref:System.IndexOutOfRangeException> exceção se o `index` parâmetro for maior ou igual ao comprimento da cadeia de caracteres, ou se for menor que zero  
  
## <a name="see-also"></a>Confira também

- <xref:System.String.Chars%2A>
- [Como converter uma cadeia de caracteres em uma matriz de caracteres](how-to-convert-a-string-to-an-array-of-characters.md)
- [Convertendo entre cadeias de caracteres e outros tipos de dados no Visual Basic](converting-between-strings-and-other-data-types.md)
- [Cadeias de caracteres](index.md)
