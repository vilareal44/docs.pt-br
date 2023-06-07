---
title: Como converter uma cadeia de caracteres em uma matriz de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- character arrays [Visual Basic], converting strings
- arrays [Visual Basic], converting strings to
- examples [Visual Basic], string conversion
- strings [Visual Basic], converting to arrays
- string conversion [Visual Basic], arrays
ms.assetid: 1b54b686-ab29-413b-adce-6bd5422376eb
ms.openlocfilehash: eca8cd7be8da1f6149dadf1e9edeab5e5225ab9f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84360664"
---
# <a name="how-to-convert-a-string-to-an-array-of-characters-in-visual-basic"></a>Como converter uma cadeia de caracteres em uma matriz de caracteres no Visual Basic
Às vezes, é útil ter dados sobre os caracteres na cadeia de caracteres e as posições desses caracteres em sua cadeia de caracteres, como quando você está analisando uma cadeia de caracteres. Este exemplo mostra como você pode obter uma matriz dos caracteres em uma cadeia de caracteres chamando o método da cadeia de caracteres <xref:System.String.ToCharArray%2A> .  
  
## <a name="example"></a>Exemplo  
 Este exemplo demonstra como dividir uma cadeia de caracteres em uma `Char` matriz, e como dividir uma cadeia de caracteres em uma `String` matriz de seu caractere de texto Unicode. O motivo para essa distinção é que os caracteres de texto Unicode podem ser compostos de dois ou mais `Char` caracteres (como um par alternativo ou uma sequência de caracteres de combinação). Para obter mais informações, consulte <xref:System.Globalization.TextElementEnumerator> e [o padrão Unicode](https://www.unicode.org/standard/standard.html).  
  
 [!code-vb[VbVbalrStrings#75](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class4.vb#75)]  
  
## <a name="example"></a>Exemplo  
 É mais difícil dividir uma String em seus caracteres de texto Unicode, mas isso é necessário se você precisar de informações sobre a representação visual de uma cadeia de caracteres. Este exemplo usa o <xref:System.Globalization.StringInfo.SubstringByTextElements%2A> método para obter informações sobre os caracteres de texto Unicode que compõem uma cadeia de caracteres.  
  
 [!code-vb[VbVbalrStrings#76](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class4.vb#76)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.String.Chars%2A>
- <xref:System.Globalization.StringInfo?displayProperty=nameWithType>
- [Como acessar caracteres em cadeias de caracteres](how-to-access-characters-in-strings.md)
- [Convertendo entre cadeias de caracteres e outros tipos de dados no Visual Basic](converting-between-strings-and-other-data-types.md)
- [Cadeias de caracteres](index.md)
