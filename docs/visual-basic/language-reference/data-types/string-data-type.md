---
title: Tipo de dados da cadeia de caracteres
ms.date: 07/20/2015
f1_keywords:
- vb.String
helpviewer_keywords:
- strings [Visual Basic], character
- strings [Visual Basic], fixed-length
- string keyword [Visual Basic]
- fixed-length strings [Visual Basic], string data type
- literals [Visual Basic], string
- text [Visual Basic], String data type
- $ identifier type character
- String data type
- fixed-length strings
- string literals [Visual Basic]
- data types [Visual Basic], assigning
- String literals [Visual Basic]
- identifier type characters [Visual Basic], $
ms.assetid: 15ac03f5-cabd-42cc-a754-1df3893c25d9
ms.openlocfilehash: cd4b64c101ae56928e84a04649e49c17b6f4023c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415499"
---
# <a name="string-data-type-visual-basic"></a>Tipo de dados da cadeia de caracteres (Visual Basic)

Retém sequências de pontos de código não assinados de 16 bits (2 bytes) que variam em valor de 0 a 65535. Cada *ponto de código*, ou código de caractere, representa um único caractere Unicode. Uma cadeia de caracteres pode conter de 0 a aproximadamente 2.000.000.000 (2 ^ 31) caracteres Unicode.  
  
## <a name="remarks"></a>Comentários  

 Use o `String` tipo de dados para conter vários caracteres sem a sobrecarga de gerenciamento de matriz de `Char()` , uma matriz de `Char` elementos.  
  
 O valor padrão de `String` é `Nothing` (uma referência nula). Observe que isso não é o mesmo que a cadeia de caracteres vazia (valor `""` ).  
  
## <a name="unicode-characters"></a>Caracteres Unicode  

 Os primeiros 128 pontos de código (0 – 127) de Unicode correspondem às letras e aos símbolos em um teclado americano padrão. Esses primeiros 128 pontos de código são os mesmos que os conjuntos de caracteres ASCII define. Os segundos pontos de código 128 (128 – 255) representam caracteres especiais, como letras do alfabeto, acentos, símbolos de moeda e frações baseados em latim. O Unicode usa os pontos de código restantes (256-65535) para uma grande variedade de símbolos. Isso inclui caracteres de texto em todo o mundo, sinais diacríticos e símbolos técnicos e matemáticos.  
  
 Você pode usar métodos como <xref:System.Char.IsDigit%2A> e <xref:System.Char.IsPunctuation%2A> em um caractere individual em uma `String` variável para determinar sua classificação Unicode.  
  
## <a name="format-requirements"></a>Requisitos de formato  

 Você deve colocar um `String` literal entre aspas ( `" "` ). Se você precisar incluir uma aspa como um dos caracteres na cadeia de caracteres, use duas aspas contíguas ( `""` ). O exemplo a seguir ilustra isto.  
  
```vb  
Dim j As String = "Joe said ""Hello"" to me."  
Dim h As String = "Hello"  
' The following messages all display the same thing:  
' "Joe said "Hello" to me."  
MsgBox(j)  
MsgBox("Joe said " & """" & h & """" & " to me.")  
MsgBox("Joe said """ & h & """ to me.")  
```  
  
 Observe que as aspas contíguas que representam uma aspa na cadeia de caracteres são independentes das aspas que começam e terminam o `String` literal.  
  
## <a name="string-manipulations"></a>Manipulações de cadeia de caracteres  

 Depois de atribuir uma cadeia de caracteres a uma `String` variável, essa cadeia de caracteres é *imutável*, o que significa que você não pode alterar seu comprimento ou conteúdo. Quando você altera uma cadeia de caracteres de qualquer forma, Visual Basic cria uma nova cadeia de caracteres e abandona a anterior. `String`Em seguida, a variável aponta para a nova cadeia de caracteres.  
  
 Você pode manipular o conteúdo de uma `String` variável usando uma variedade de funções de cadeia de caracteres. O exemplo a seguir ilustra a <xref:Microsoft.VisualBasic.Strings.Left%2A> função  
  
```vb  
Dim S As String = "Database"  
' The following statement sets S to a new string containing "Data".  
S = Microsoft.VisualBasic.Left(S, 4)  
```  
  
 Uma cadeia de caracteres criada por outro componente pode ser preenchida com espaços à esquerda ou à direita. Se você receber essa cadeia de caracteres, poderá usar as <xref:Microsoft.VisualBasic.Strings.Trim%2A> <xref:Microsoft.VisualBasic.Strings.LTrim%2A> funções, e <xref:Microsoft.VisualBasic.Strings.RTrim%2A> para remover esses espaços.  
  
 Para obter mais informações sobre manipulações de cadeias de caracteres, consulte [Strings](../../programming-guide/language-features/strings/index.md).  
  
## <a name="programming-tips"></a>Dicas de programação  
  
- **Números negativos.** Lembre-se de que os caracteres mantidos por `String` não são assinados e não podem representar valores negativos. Em qualquer caso, você não deve usar `String` para armazenar valores numéricos.  
  
- **Considerações sobre interoperabilidade.** Se você estiver fazendo a interface com componentes não escritos para o .NET Framework, por exemplo, automação ou objetos COM, lembre-se de que os caracteres da cadeia de caracteres têm uma largura de dados diferente (8 bits) em outros ambientes. Se você estiver passando um argumento de cadeia de caracteres de 8 bits para esse componente, declare-o como `Byte()` uma matriz de `Byte` elementos, em vez de `String` em seu novo código de Visual Basic.  
  
- **Digite os caracteres.** Anexar o caractere de tipo de identificador `$` a qualquer identificador força-o para o `String` tipo de dados. `String`Não tem um caractere de tipo literal. No entanto, o compilador trata literais delimitados entre aspas ( `" "` ) como `String` .  
  
- **Tipo de estrutura.** O tipo correspondente no .NET Framework é a <xref:System.String?displayProperty=nameWithType> classe.  
  
## <a name="see-also"></a>Confira também

- <xref:System.String?displayProperty=nameWithType>
- [Tipos de dados](index.md)
- [Tipo de Dados de Caractere](char-data-type.md)
- [Funções de conversão do tipo](../functions/type-conversion-functions.md)
- [Resumo da Conversão](../keywords/conversion-summary.md)
- [Como: Chamar uma função do Windows que use tipos não assinados](../../programming-guide/com-interop/how-to-call-a-windows-function-that-takes-unsigned-types.md)
- [Uso eficiente de tipos de dados](../../programming-guide/language-features/data-types/efficient-use-of-data-types.md)
