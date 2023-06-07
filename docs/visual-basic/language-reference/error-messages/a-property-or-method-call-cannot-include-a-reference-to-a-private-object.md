---
title: Uma chamada de propriedade ou método não pode incluir uma referência a um objeto particular, como um argumento ou um valor de retorno
ms.date: 07/20/2015
f1_keywords:
- vbrID98
ms.assetid: 059b43e1-202d-4fa2-806b-7bad63c1e7ca
ms.openlocfilehash: 36c71cdb345d0fdc0da2b58865a1f11956bcb944
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409966"
---
# <a name="a-property-or-method-call-cannot-include-a-reference-to-a-private-object-either-as-an-argument-or-as-a-return-value"></a>Uma chamada de propriedade ou método não pode incluir uma referência a um objeto particular, como um argumento ou um valor de retorno

Entre as possíveis causas desse erro estão:  
  
- Um cliente invocou uma propriedade ou um método de um componente fora do processo e tentou passar uma referência a um objeto particular como um dos argumentos.  
  
- Um componente fora do processo chamou um método de retorno de chamada em seu cliente e tentou passar uma referência a um objeto particular.  
  
- Um componente fora do processo tentou passar uma referência a um objeto particular como um argumento de um evento que ele estava gerando.  
  
- Um cliente tentou atribuir uma referência de objeto particular a um `ByRef` argumento de um evento que ele estava manipulando.  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Remova a referência.  
  
## <a name="see-also"></a>Confira também

- [Privada](../modifiers/private.md)
