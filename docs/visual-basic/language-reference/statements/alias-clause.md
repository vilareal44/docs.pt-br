---
title: Cláusula Alias
ms.date: 07/20/2015
f1_keywords:
- vb.Alias
helpviewer_keywords:
- Alias keyword [Visual Basic]
ms.assetid: 58c06b11-465d-4d87-906a-73200a3d7f19
ms.openlocfilehash: c28e931a376b20b2058a7187551405cd9523d4fe
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408465"
---
# <a name="alias-clause-visual-basic"></a>Cláusula Alias (Visual Basic)
Indica que um procedimento externo tem outro nome em sua DLL.  
  
## <a name="remarks"></a>Comentários  
 A `Alias` palavra-chave pode ser usada neste contexto:  
  
 [Instrução Declare](declare-statement.md)  
  
 No exemplo a seguir, a `Alias` palavra-chave é usada para fornecer o nome da função em advapi32. dll, `GetUserNameA` que `getUserName` é usada no lugar deste exemplo. A função `getUserName` é chamada em sub `getUser` , que exibe o nome do usuário atual.  
  
 [!code-vb[VbVbalrStatements#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrStatements/VB/Class1.vb#15)]  
  
## <a name="see-also"></a>Confira também

- [Palavras-chave](../keywords/index.md)
