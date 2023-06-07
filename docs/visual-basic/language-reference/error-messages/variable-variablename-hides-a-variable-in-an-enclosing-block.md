---
title: A variável '<variablename>' oculta uma variável em bloco delimitador
ms.date: 07/20/2015
f1_keywords:
- vbc30616
- bc30616
helpviewer_keywords:
- BC30616
ms.assetid: e7658ebc-da45-451b-a409-a0f8915f0beb
ms.openlocfilehash: 474a920c9cfdfba7a8157320d9c88b8677958425
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84406515"
---
# <a name="variable-variablename-hides-a-variable-in-an-enclosing-block"></a>A variável '\<variablename>' oculta uma variável em bloco delimitador
Uma variável colocada em um bloco tem o mesmo nome que outra variável local.  
  
 **ID do erro:** BC30616  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Renomeie a variável no bloco incluído para que ela não seja a mesma que qualquer outra variável local. Por exemplo:  
  
    ```vb  
    Dim a, b, x As Integer  
    If a = b Then  
       Dim y As Integer = 20 ' Uniquely named block variable.  
    End If  
    ```  
  
- Uma causa comum para esse erro é o uso de `Catch e As Exception` dentro de um manipulador de eventos. Se esse for o caso, nomeie a `Catch` variável de bloco `ex` em vez de `e` .  
  
- Outra fonte comum desse erro é uma tentativa de acessar uma variável local declarada dentro de um `Try` bloco em um `Catch` bloco separado. Para corrigir isso, declare a variável fora da `Try...Catch...Finally` estrutura.  
  
## <a name="see-also"></a>Confira também

- [Instrução Try...Catch...Finally](../statements/try-catch-finally-statement.md)
- [Declaração de Variável](../../programming-guide/language-features/variables/variable-declaration.md)
