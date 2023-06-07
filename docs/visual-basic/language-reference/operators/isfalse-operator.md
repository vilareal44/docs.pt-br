---
title: Operador IsFalse
ms.date: 07/20/2015
f1_keywords:
- vb.isfalse
helpviewer_keywords:
- AndAlso operator [Visual Basic]
- IsFalse operator [Visual Basic]
ms.assetid: 37fc9dbf-e5cc-4570-b93f-7213447974df
ms.openlocfilehash: 7c5ad5fa0b72370eeb19fbaced88807570467552
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84370668"
---
# <a name="isfalse-operator-visual-basic"></a>Operador IsFalse (Visual Basic)
Determina se uma expressão é `False` .  
  
 Você não pode chamar `IsFalse` explicitamente em seu código, mas o compilador Visual Basic pode usá-lo para gerar código de `AndAlso` cláusulas. Se você definir uma classe ou estrutura e, em seguida, usar uma variável desse tipo em uma `AndAlso` cláusula, deverá definir `IsFalse` nessa classe ou estrutura.  
  
 O compilador considera os `IsFalse` `IsTrue` operadores e como um *par correspondente*. Isso significa que, se você definir um deles, também deverá definir o outro.  
  
> [!NOTE]
> O `IsFalse` operador pode ser *sobrecarregado*, o que significa que uma classe ou estrutura pode redefinir seu comportamento quando seu operando tem o tipo dessa classe ou estrutura. Se o seu código usar esse operador em uma classe ou estrutura desse tipo, certifique-se de entender seu comportamento redefinido. Para obter mais informações, consulte [procedimentos de operador](../../programming-guide/language-features/procedures/operator-procedures.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir define o contorno de uma estrutura que inclui definições para os `IsFalse` `IsTrue` operadores e.  
  
 [!code-vb[VbVbalrOperators#28](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOperators/VB/Class1.vb#28)]  
  
## <a name="see-also"></a>Confira também

- [Operador IsTrue](istrue-operator.md)
- [Como definir um operador](../../programming-guide/language-features/procedures/how-to-define-an-operator.md)
- [Operador AndAlso](andalso-operator.md)
