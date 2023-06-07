---
title: Instrução Return
ms.date: 07/20/2015
f1_keywords:
- vb.Return
helpviewer_keywords:
- Return statement [Visual Basic], syntax
- control flow [Visual Basic], returning control to expressions
- Return statement [Visual Basic]
- expressions [Visual Basic], returning control to
ms.assetid: ac86e7f0-5a67-42c3-9834-0e0381efa3ec
ms.openlocfilehash: cdb58e32c30c8e6c1662fb698ac5576c3f71258c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84404194"
---
# <a name="return-statement-visual-basic"></a>Instrução Return (Visual Basic)
Retorna o controle para o código que chamou `Function` um `Sub` procedimento,, `Get` , `Set` ou `Operator` .  
  
## <a name="syntax"></a>Sintaxe  
  
```vb  
Return  
' -or-  
Return expression  
```  
  
## <a name="part"></a>Parte  
 `expression`  
 Necessário em um `Function` `Get` procedimento, ou `Operator` . Expressão que representa o valor a ser retornado para o código de chamada.  
  
## <a name="remarks"></a>Comentários  
 Em um `Sub` `Set` procedimento ou, a `Return` instrução é equivalente a uma `Exit Sub` `Exit Property` instrução or e `expression` não deve ser fornecida.  
  
 Em um `Function` `Get` procedimento,, ou `Operator` , a `Return` instrução deve incluir `expression` e `expression` deve ser avaliada como um tipo de dados que é conversível para o tipo de retorno do procedimento. Em um `Function` `Get` procedimento ou, você também tem a alternativa de atribuir uma expressão ao nome do procedimento para servir como o valor de retorno e, em seguida, executar `Exit Function` uma `Exit Property` instrução or. Em um `Operator` procedimento, você deve usar `Return expression` .  
  
 Você pode incluir quantas `Return` instruções forem apropriadas no mesmo procedimento.  
  
> [!NOTE]
> O código em um `Finally` bloco é executado depois `Return` que uma instrução em um `Try` `Catch` bloco ou é encontrada, mas antes que essa `Return` instrução seja executada. Uma `Return` instrução não pode ser incluída em um `Finally` bloco.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir usa a `Return` instrução várias vezes para retornar ao código de chamada quando o procedimento não precisa fazer mais nada.  
  
 [!code-vb[VbVbalrStatements#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#53)]  
  
## <a name="see-also"></a>Confira também

- [Instrução Function](function-statement.md)
- [Instrução Sub](sub-statement.md)
- [Instrução Get](get-statement.md)
- [Instrução SET](set-statement.md)
- [Instrução Operator](operator-statement.md)
- [Instrução Property](property-statement.md)
- [Instrução Exit](exit-statement.md)
- [Instrução Try...Catch...Finally](try-catch-finally-statement.md)
