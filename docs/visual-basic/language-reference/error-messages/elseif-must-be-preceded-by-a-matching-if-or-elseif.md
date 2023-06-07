---
title: "'#ElseIf' deve ser precedido por um '#If' ou '#ElseIf' correspondente"
ms.date: 07/20/2015
f1_keywords:
- vbc30014
- bc30014
helpviewer_keywords:
- BC30014
ms.assetid: 5215585e-2efa-485a-9efe-9833a1cc83a0
ms.openlocfilehash: 808cf35fb05da092cdef560721b2f667778aa78f
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409654"
---
# <a name="elseif-must-be-preceded-by-a-matching-if-or-elseif"></a>'#ElseIf' deve ser precedido por um '#If' ou '#ElseIf' correspondente
`#ElseIf`é uma diretiva de compilação condicional. Uma `#ElseIf` cláusula deve ser precedida por uma `#If` cláusula or correspondente `#ElseIf` .  
  
 **ID do erro:** BC30014  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Verifique se um `#If` ou anterior `#ElseIf` não foi separado dele `#ElseIf` por um bloco de compilação condicional intermediário ou foi colocado incorretamente `#End If` .  
  
2. Se o `#ElseIf` for precedido por uma `#Else` diretiva, remova `#Else` ou altere-o para um `#ElseIf` .  
  
3. Se todo o resto estiver em ordem, adicione uma `#If` diretiva ao início do bloco de compilação condicional.  
  
## <a name="see-also"></a>Confira também

- [#If... Diretivas then... #Else](../directives/if-then-else-directives.md)
