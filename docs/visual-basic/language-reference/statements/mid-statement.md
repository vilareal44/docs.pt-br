---
title: Instrução Mid
ms.date: 07/20/2015
f1_keywords:
- vb.MidB
- vb.Mid
helpviewer_keywords:
- substrings [Visual Basic], Mid statement
- strings [Visual Basic], substrings
- Mid statement [Visual Basic]
- strings [Visual Basic], replacing
ms.assetid: 2b82d7a8-9646-4cb0-bec5-80abc98297bf
ms.openlocfilehash: 90408fd8a8cfc9b74c8422d0571d61f8534403f3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404442"
---
# <a name="mid-statement"></a>Instrução Mid
Substitui um número especificado de caracteres em uma `String` variável por caracteres de outra cadeia de caractere.  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Mid( _  
   ByRef Target As String, _  
   ByVal Start As Integer, _  
   Optional ByVal Length As Integer _  
) = StringExpression  
```  
  
## <a name="parts"></a>Partes  
 `Target`  
 Obrigatórios. Nome da `String` variável a ser modificada.  
  
 `Start`  
 Obrigatórios. Expressão `Integer`. Posição do caractere em `Target` que começa a substituição do texto. `Start`usa um índice baseado em um.  
  
 `Length`  
 Opcional. Expressão `Integer`. Número de caracteres a serem substituídos. Se omitido, todos `String` serão usados.  
  
 `StringExpression`  
 Obrigatórios. `String`expressão que substitui parte de `Target` .  
  
## <a name="exceptions"></a>Exceções  
  
|Tipo de exceção|Condição|  
|--------------------|---------------|  
|<xref:System.ArgumentException>|`Start` <= 0 ou `Length` < 0.|  
  
## <a name="remarks"></a>Comentários  
 O número de caracteres substituídos é sempre menor ou igual ao número de caracteres em `Target` .  
  
 Visual Basic tem uma <xref:Microsoft.VisualBasic.Strings.Mid%2A> função e uma `Mid` instrução. Esses elementos operam em um número especificado de caracteres em uma cadeia de caracteres, mas a `Mid` função retorna os caracteres enquanto a `Mid` instrução substitui os caracteres. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Strings.Mid%2A>.  
  
> [!NOTE]
> A `MidB` instrução das versões anteriores do Visual Basic substitui uma Subcadeia em bytes, em vez de caracteres. Ele é usado principalmente para converter cadeias de caracteres em aplicativos DBCS. Todas as cadeias de caracteres Visual Basic estão em Unicode e `MidB` não têm mais suporte.  
  
## <a name="example"></a>Exemplo  
 Este exemplo usa a `Mid` instrução para substituir um número especificado de caracteres em uma variável String por caracteres de outra cadeia de caracteres.  
  
 [!code-vb[VbVbalrStrings#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStrings/VB/Class1.vb#5)]  
  
## <a name="requirements"></a>Requisitos  
 **Namespace:** [Microsoft. VisualBasic](../runtime-library-members.md)  
  
 **Módulo:**`Strings`  
  
 **Assembly:** Visual Basic a biblioteca de tempo de execução (em Microsoft. VisualBasic. dll)  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Strings.Mid%2A>
- [Cadeias de caracteres](../../programming-guide/language-features/strings/index.md)
- [Introdução a cadeias de caracteres no Visual Basic](../../programming-guide/language-features/strings/introduction-to-strings.md)
