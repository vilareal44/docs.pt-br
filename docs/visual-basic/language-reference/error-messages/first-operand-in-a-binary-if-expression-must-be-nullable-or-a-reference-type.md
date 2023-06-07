---
title: O primeiro operando em uma expressão 'If' binária deve ser um tipo que permite valor nulo ou um de referência
ms.date: 07/20/2015
f1_keywords:
- bc33107
- vbc33107
helpviewer_keywords:
- BC33107
ms.assetid: 493c8899-3f6b-4471-8eb6-9284e8492768
ms.openlocfilehash: ca16c6604ee071668a5c65d7e9052b233e2313c7
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84403012"
---
# <a name="first-operand-in-a-binary-if-expression-must-be-nullable-or-a-reference-type"></a>O primeiro operando em uma expressão 'If' binária deve ser um tipo que permite valor nulo ou um de referência
Uma `If` expressão pode ter dois ou três argumentos. Quando você envia apenas dois argumentos, o primeiro argumento deve ser um tipo de referência ou um tipo de valor anulável. Se o primeiro argumento for avaliado como algo diferente de `Nothing` , seu valor será retornado. Se o primeiro argumento for avaliado como `Nothing` , o segundo argumento será avaliado e retornado.  
  
 Por exemplo, o código a seguir contém duas `If` expressões, uma com três argumentos e outra com dois argumentos. As expressões calculam e retornam o mesmo valor.  
  
```vb  
' firstChoice is a nullable value type.  
Dim firstChoice? As Integer = Nothing  
Dim secondChoice As Integer = 1128  
' If expression with three arguments.  
Console.WriteLine(If(firstChoice IsNot Nothing, firstChoice, secondChoice))  
' If expression with two arguments.  
Console.WriteLine(If(firstChoice, secondChoice))  
```  
  
 As expressões a seguir causam esse erro:  
  
```vb  
Dim choice1 = 4  
Dim choice2 = 5  
Dim booleanVar = True  
  
' Not valid.  
'Console.WriteLine(If(choice1 < choice2, 1))  
' Not valid.  
'Console.WriteLine(If(booleanVar, "Test returns True."))  
```  
  
 **ID do erro:** BC33107  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
- Se você não puder alterar o código para que o primeiro argumento seja um tipo de valor anulável ou tipo de referência, considere converter para uma expressão de três argumentos `If` ou para uma `If...Then...Else` instrução.  
  
```vb  
Console.WriteLine(If(choice1 < choice2, 1, 2))  
Console.WriteLine(If(booleanVar, "Test returns True.", "Test returns False."))  
```  
  
## <a name="see-also"></a>Confira também

- [Operador If](../operators/if-operator.md)
- [Instrução If...Then...Else](../statements/if-then-else-statement.md)
- [Tipos de valor anulável](../../programming-guide/language-features/data-types/nullable-value-types.md)
