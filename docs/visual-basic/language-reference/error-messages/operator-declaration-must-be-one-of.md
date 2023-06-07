---
title: 'A declaração do operador deve ser uma das: +,-, *,-,-, ^, &amp; , like, mod, and ou, Xor, not,  <<,  >>, =,  <>, <, <=, >, re>=, CType, IsTrue, IsFalse'
ms.date: 07/20/2015
f1_keywords:
- bc33000
- vbc33000
helpviewer_keywords:
- BC33000
ms.assetid: 15c5d8eb-3a8c-4141-8f41-33151afabf97
ms.openlocfilehash: 3fb2cf392611e5ca83818e3bf173513be031085d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84409312"
---
# <a name="operator-declaration-must-be-one-of----amp-like-mod-and-or-xor-not--"></a>A declaração do operador deve ser um destes: +,-, *, \, /, ^, &amp; , like, mod, and ou, Xor, not, \<\<, >>...
Você pode declarar apenas um operador que seja elegível para sobrecarga. A tabela a seguir lista os operadores que você pode declarar.  
  
|Tipo|Operadores|  
|----------|---------------|  
|Unário|`+`, `-`, `IsFalse`, `IsTrue`, `Not`|  
|Binário|`+`, `-`, `*`, `/`, `\`, `&`, `^`, `>>`, `<<`, `=`, `<>`, `>`, `>=`, `<`, `<=`, `And`, `Like`, `Mod`, `Or`, `Xor`|  
|Conversão (unário)|`CType`|  
  
 Observe que o operador `=` na lista binária é o operador de comparação, não o operador de atribuição.  
  
 **ID do erro:** BC33000  
  
## <a name="to-correct-this-error"></a>Para corrigir este erro  
  
1. Selecione um operador do conjunto de operadores sobrecarregados.  
  
2. Se você precisar da funcionalidade de sobrecarregar um operador que você não pode sobrecarregar diretamente, crie um `Function` procedimento que aceite os parâmetros apropriados e retorne o valor apropriado.  
  
## <a name="see-also"></a>Confira também

- [Instrução Operator](../statements/operator-statement.md)
- [Procedimentos do operador](../../programming-guide/language-features/procedures/operator-procedures.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Como definir um operador de conversão](../../programming-guide/language-features/procedures/how-to-define-a-conversion-operator.md)
- [Instrução Function](../statements/function-statement.md)
