---
title: <type1>'<typename>' deve implementar '<methodname>' para interface '<interfacename>'
ms.date: 07/20/2015
f1_keywords:
- vbc30149
- bc30149
helpviewer_keywords:
- BC30149
ms.assetid: 29d1b7f4-dca7-478c-bbe7-c657f342c183
ms.openlocfilehash: 90d2b6d70390bfb732af4a5868c935de61d18f94
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84408491"
---
# <a name="type1typename-must-implement-methodname-for-interface-interfacename"></a>\<type1>'\<typename>' deve implementar '\<methodname>' para interface '\<interfacename>'
Uma classe ou estrutura alega implementar uma interface, mas não implementa um procedimento definido pela interface. Todos os membros da interface devem ser implementados.  
  
 **ID do erro:** BC30149  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Declare um procedimento com o mesmo nome e assinatura, conforme definido na interface. Certifique-se de incluir pelo menos `End Function` a `End Sub` instrução ou.  
  
2. Adicione uma `Implements` cláusula ao final da `Function` `Sub` instrução ou. Por exemplo:  
  
    ```vb  
    Public Sub DoSomething() Implements IBaseInterface.DoSomething  
    ```  
  
## <a name="see-also"></a>Confira também

- [Instrução Implements](../statements/implements-statement.md)
- [Interfaces](../../programming-guide/language-features/interfaces/index.md)
