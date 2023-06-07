---
title: Como proteger um argumento de procedimento contra alterações de valor
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- arguments [Visual Basic], passing by reference
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- procedures [Visual Basic], calling
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: d2b7c766-ce16-4d2c-8d79-3fc0e7ba2227
ms.openlocfilehash: b0504d9f7a8862274d5d71dc6a9c1570551629a5
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84414357"
---
# <a name="how-to-protect-a-procedure-argument-against-value-changes-visual-basic"></a>Como proteger um argumento de procedimento contra alterações de valor (Visual Basic)
Se um procedimento declarar um parâmetro como [ByRef](../../../language-reference/modifiers/byref.md), Visual Basic fornece ao código de procedimento uma referência direta para o elemento de programação subjacente ao argumento no código de chamada. Isso permite que o procedimento altere o valor subjacente ao argumento no código de chamada. Em alguns casos, o código de chamada pode querer proteger contra tal alteração.  
  
 Você sempre pode proteger um argumento da alteração declarando o parâmetro correspondente [ByVal](../../../language-reference/modifiers/byval.md) no procedimento. Se você quiser ser capaz de alterar um determinado argumento em alguns casos, mas não em outros, você pode declará-lo `ByRef` e permitir que o código de chamada determine o mecanismo de passagem em cada chamada. Ele faz isso colocando o argumento correspondente entre parênteses para passá-lo por valor ou não colocá-lo entre parênteses para passá-lo por referência. Para obter mais informações, consulte [como: forçar um argumento a ser passado por valor](./how-to-force-an-argument-to-be-passed-by-value.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra dois procedimentos que usam uma variável de matriz e operam em seus elementos. O `increase` procedimento simplesmente adiciona um a cada elemento. O `replace` procedimento atribui uma nova matriz ao parâmetro `a()` e, em seguida, adiciona uma a cada elemento. No entanto, a reatribuição não afeta a variável de matriz subjacente no código de chamada.  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#38](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#38)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 A primeira `MsgBox` chamada exibe "After aumente (n): 11, 21, 31, 41". Como a matriz `n` é um tipo de referência, o `increase` pode alterar seus membros, embora o mecanismo de passagem seja `ByVal` .  
  
 A segunda `MsgBox` chamada exibe "após Replace (n): 11, 21, 31, 41". Como `n` é passado `ByVal` , `replace` não é possível modificar a variável `n` no código de chamada atribuindo uma nova matriz a ela. Quando `replace` o cria a nova instância de matriz `k` e a atribui à variável local `a` , ela perde a referência a `n` passada pelo código de chamada. Quando ele altera os membros de `a` , apenas a matriz local `k` é afetada. Portanto, `replace` o não incrementa os valores de matriz `n` no código de chamada.  
  
## <a name="compile-the-code"></a>Compilar o código  
 O padrão no Visual Basic é passar argumentos por valor. No entanto, é uma boa prática de programação incluir a palavra-chave [ByVal](../../../language-reference/modifiers/byval.md) ou [ByRef](../../../language-reference/modifiers/byref.md) com cada parâmetro declarado. Isso torna seu código mais fácil de ler.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Diferenças entre argumentos modificáveis e não modificáveis](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [Diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [Como alterar o valor de um argumento de procedimento](./how-to-change-the-value-of-a-procedure-argument.md)
- [Como forçar um argumento a ser passado por Valor](./how-to-force-an-argument-to-be-passed-by-value.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
