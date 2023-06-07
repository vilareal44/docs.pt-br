---
title: A instrução não é válida dentro de um método/lambda de várias linhas
ms.date: 07/20/2015
f1_keywords:
- vbc30024
- bc30024
helpviewer_keywords:
- BC30024
ms.assetid: 758e7a8f-429b-42c1-9a78-778e5b480e04
ms.openlocfilehash: f3c43d640259d5e1af545e2610088aab5d70453d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84396240"
---
# <a name="statement-is-not-valid-inside-a-methodmultiline-lambda"></a>A instrução não é válida dentro de um método/lambda de várias linhas
A instrução não é válida dentro de `Sub` um `Function` procedimento,, propriedade `Get` ou propriedade `Set` . Algumas instruções podem ser colocadas no nível do módulo ou da classe. Outros, como `Option Strict` , devem estar no nível de namespace e preceder todas as outras declarações.  
  
 **ID do erro:** BC30024  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Remova a instrução do procedimento.  
  
## <a name="see-also"></a>Confira também

- [Instrução Sub](../statements/sub-statement.md)
- [Instrução Function](../statements/function-statement.md)
- [Instrução Get](../statements/get-statement.md)
- [Instrução SET](../statements/set-statement.md)
