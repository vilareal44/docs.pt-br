---
title: Operador IsTrue
ms.date: 07/20/2015
f1_keywords:
- vb.istrue
helpviewer_keywords:
- IsTrue operator [Visual Basic]
- OrElse operator [Visual Basic]
ms.assetid: b6cec0f2-61b1-4331-a7f0-4d07ee3179d6
ms.openlocfilehash: bc129d3a3aec76285d5ea8fb727fc72c3064c9cf
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84370642"
---
# <a name="istrue-operator-visual-basic"></a>Operador IsTrue (Visual Basic)
Determina se uma expressão é `True` .  
  
 Você não pode chamar `IsTrue` explicitamente em seu código, mas o compilador Visual Basic pode usá-lo para gerar código de `OrElse` cláusulas. Se você definir uma classe ou estrutura e, em seguida, usar uma variável desse tipo em uma `OrElse` cláusula, deverá definir `IsTrue` nessa classe ou estrutura.  
  
 O compilador considera os `IsTrue` `IsFalse` operadores e como um *par correspondente*. Isso significa que, se você definir um deles, também deverá definir o outro.  
  
## <a name="compiler-use-of-istrue"></a>Uso do compilador de IsTrue  
 Quando você tiver definido uma classe ou estrutura, poderá usar uma variável desse tipo em uma `For` instrução,, `If` `Else If` ou `While` , ou em uma `When` cláusula. Se você fizer isso, o compilador exigirá um operador que converte o tipo em um `Boolean` valor para que ele possa testar uma condição. Ele procura um operador adequado na seguinte ordem:  
  
1. Um operador de conversão de ampliação de sua classe ou estrutura para `Boolean` .  
  
2. Um operador de conversão de ampliação de sua classe ou estrutura para `Boolean?` .  
  
3. O `IsTrue` operador em sua classe ou estrutura.  
  
4. Uma conversão de restrição para `Boolean?` que não envolve uma conversão de `Boolean` para `Boolean?` .  
  
5. Um operador de conversão de restrição de sua classe ou estrutura para `Boolean` .  
  
 Se você não tiver definido nenhuma conversão para `Boolean` ou um `IsTrue` operador, o compilador sinalizará um erro.  
  
> [!NOTE]
> O `IsTrue` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando seu operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir define o contorno de uma estrutura que inclui definições para os `IsFalse` `IsTrue` operadores e.  
  
 [!code-vb[VbVbalrOperators#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>Confira também

- [Operador IsFalse](isfalse-operator.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Operador OrElse](orelse-operator.md)
