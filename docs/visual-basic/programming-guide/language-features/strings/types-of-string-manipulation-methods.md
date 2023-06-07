---
title: Tipos de métodos de manipulação de cadeia de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- strings [Visual Basic], manipulating [Visual Basic]
- string manipulation
ms.assetid: 905055cd-7f50-48fb-9eed-b0995af1dc1f
ms.openlocfilehash: aba9af9c699cf8d07862c5d2967902bec1623500
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84363753"
---
# <a name="types-of-string-manipulation-methods-in-visual-basic"></a>Tipos de métodos de manipulação da cadeia de caracteres no Visual Basic
Há várias maneiras diferentes de analisar e manipular suas cadeias de caracteres. Alguns dos métodos fazem parte da linguagem de Visual Basic e outros são inerentes à `String` classe.  
  
## <a name="visual-basic-language-and-the-net-framework"></a>Visual Basic idioma e o .NET Framework  
 Visual Basic métodos são usados como funções inerentes do idioma. Eles podem ser usados sem qualificação em seu código. O exemplo a seguir mostra o uso típico de um comando de manipulação de cadeia de caracteres Visual Basic:  
  
 [!code-vb[VbVbalrStrings#44](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#44)]  
  
 Neste exemplo, a `Mid` função executa uma operação direta no `aString` e atribui o valor a `bString` .  
  
 Para obter uma lista de métodos de manipulação de cadeia de caracteres Visual Basic, consulte [Resumo de manipulação de cadeia de caracteres](../../../language-reference/keywords/string-manipulation-summary.md).  
  
### <a name="shared-methods-and-instance-methods"></a>Métodos compartilhados e métodos de instância  
 Você também pode manipular cadeias de caracteres com os métodos da `String` classe. Há dois tipos de métodos em `String` : métodos *compartilhados* e métodos de *instância* .  
  
#### <a name="shared-methods"></a>Métodos compartilhados  
 Um método compartilhado é um método que deriva da `String` própria classe e não requer uma instância dessa classe para funcionar. Esses métodos podem ser qualificados com o nome da classe ( `String` ) em vez de com uma instância da `String` classe. Por exemplo:  
  
 [!code-vb[VbVbalrStrings#45](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#45)]  
  
 No exemplo anterior, o <xref:System.String.Copy%2A?displayProperty=nameWithType> método é um método estático, que age em uma expressão que é fornecida e atribui o valor resultante a `bString` .  
  
#### <a name="instance-methods"></a>Métodos de instância  
 Os métodos de instância, por outro lado, são provenientes de uma determinada instância do `String` e devem ser qualificados com o nome da instância. Por exemplo:  
  
 [!code-vb[VbVbalrStrings#46](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class2.vb#46)]  
  
 Neste exemplo, o <xref:System.String.Substring%2A?displayProperty=nameWithType> método é um método da instância do `String` (ou seja, `aString` ). Ele executa uma operação em `aString` e atribui esse valor a `bString` .  
  
 Para obter mais informações, consulte a documentação da <xref:System.String> classe.  
  
## <a name="see-also"></a>Confira também

- [Introdução a cadeias de caracteres no Visual Basic](introduction-to-strings.md)
