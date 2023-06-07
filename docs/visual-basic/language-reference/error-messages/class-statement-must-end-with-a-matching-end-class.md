---
title: Instrução 'Class' deve finalizar com 'End Class' correspondente
ms.date: 07/20/2015
f1_keywords:
- vbc30481
- bc30481
helpviewer_keywords:
- BC30481
ms.assetid: 583f3029-bc3a-4e06-866f-92dbecc46f19
ms.openlocfilehash: 01c231f577d21028e9ef92f37c7ac5f7f1fe2aa3
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84415382"
---
# <a name="class-statement-must-end-with-a-matching-end-class"></a>Instrução 'Class' deve finalizar com 'End Class' correspondente
`Class`é usado para iniciar um `Class` bloco; portanto, ele só pode aparecer no início do bloco, com uma instrução correspondente que `End Class` termina o bloco. Você tem uma instrução redundante `Class` ou não terminou seu `Class` bloco com `End Class` .  
  
 **ID do erro:** BC30481  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Localize e remova a instrução desnecessária `Class` .  
  
- Conclua o `Class` bloco com uma correspondência `End Class` .  
  
## <a name="see-also"></a>Confira também

- [\<keyword>Instrução End](../statements/end-keyword-statement.md)
- [Instrução Class](../statements/class-statement.md)
