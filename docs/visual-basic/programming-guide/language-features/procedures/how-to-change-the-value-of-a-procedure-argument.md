---
title: Como alterar o valor de um argumento de procedimento
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
- arguments [Visual Basic], ByRef
- arguments [Visual Basic], changing value
ms.assetid: 6fad2368-5da7-4c07-8bf8-0f4e65a1be67
ms.openlocfilehash: 46cf9062d01e248b6e90882a923a48210780f7f4
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388498"
---
# <a name="how-to-change-the-value-of-a-procedure-argument-visual-basic"></a>Como alterar o valor de um argumento de procedimento (Visual Basic)
Quando você chama um procedimento, cada argumento fornecido corresponde a um dos parâmetros definidos no procedimento. Em alguns casos, o código do procedimento pode alterar o valor subjacente a um argumento no código de chamada. Em outros casos, o procedimento pode alterar apenas sua cópia local de um argumento.  
  
 Quando você chama o procedimento, Visual Basic faz uma cópia local de cada argumento passado por [ByVal](../../../language-reference/modifiers/byval.md). Para cada argumento passado por [ByRef](../../../language-reference/modifiers/byref.md), Visual Basic dá ao código do procedimento uma referência direta para o elemento de programação subjacente ao argumento no código de chamada.  
  
 Se o elemento subjacente no código de chamada for um elemento modificável e o argumento for passado `ByRef` , o código do procedimento poderá usar a referência direta para alterar o valor do elemento no código de chamada.  
  
## <a name="changing-the-underlying-value"></a>Alterando o valor subjacente  
  
#### <a name="to-change-the-underlying-value-of-a-procedure-argument-in-the-calling-code"></a>Para alterar o valor subjacente de um argumento de procedimento no código de chamada  
  
1. Na declaração de procedimento, especifique [ByRef](../../../language-reference/modifiers/byref.md) para o parâmetro correspondente ao argumento.  
  
2. No código de chamada, passe um elemento de programação modificável como o argumento.  
  
3. No código de chamada, não coloque o argumento entre parênteses na lista de argumentos.  
  
4. No código do procedimento, use o nome do parâmetro para atribuir um valor ao elemento subjacente no código de chamada.  
  
 Veja o exemplo mais abaixo para ver uma demonstração.  
  
## <a name="changing-local-copies"></a>Alterando cópias locais  
 Se o elemento subjacente no código de chamada for um elemento não modificável ou se o argumento for passado `ByVal` , o procedimento não poderá alterar seu valor no código de chamada. No entanto, o procedimento pode alterar sua cópia local desse argumento.  
  
#### <a name="to-change-the-copy-of-a-procedure-argument-in-the-procedure-code"></a>Para alterar a cópia de um argumento de procedimento no código do procedimento  
  
1. Na declaração de procedimento, especifique [ByVal](../../../language-reference/modifiers/byval.md) para o parâmetro correspondente ao argumento.  
  
     -ou-  
  
     No código de chamada, coloque o argumento entre parênteses na lista de argumentos. Isso força Visual Basic a passar o argumento por valor, mesmo se o parâmetro correspondente especificar `ByRef` .  
  
2. No código do procedimento, use o nome do parâmetro para atribuir um valor à cópia local do argumento. O valor subjacente no código de chamada não é alterado.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra dois procedimentos que usam uma variável de matriz e operam em seus elementos. O `increase` procedimento simplesmente adiciona um a cada elemento. O `replace` procedimento atribui uma nova matriz ao parâmetro `a()` e, em seguida, adiciona uma a cada elemento.  
  
 [!code-vb[VbVbcnProcedures#35](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#35)]  
  
 [!code-vb[VbVbcnProcedures#36](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#36)]  
  
 [!code-vb[VbVbcnProcedures#37](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#37)]  
  
 A primeira `MsgBox` chamada exibe "After aumente (n): 11, 21, 31, 41". Como a matriz `n` é um tipo de referência, o `replace` pode alterar seus membros, embora o mecanismo de passagem seja `ByVal` .  
  
 A segunda `MsgBox` chamada exibe "após Replace (n): 101, 201, 301". Como `n` é passado `ByRef` , `replace` o pode modificar a variável `n` no código de chamada e atribuir uma nova matriz a ela. Como `n` é um tipo de referência, `replace` também pode alterar seus membros.  
  
 Você pode impedir que o procedimento modifique a variável em si no código de chamada. Consulte [como: proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md).  
  
## <a name="compile-the-code"></a>Compilar o código  
 Ao passar uma variável por referência, você deve usar a `ByRef` palavra-chave para especificar esse mecanismo.  
  
 O padrão no Visual Basic é passar argumentos por valor. No entanto, é uma boa prática de programação incluir a palavra-chave [ByVal](../../../language-reference/modifiers/byval.md) ou [ByRef](../../../language-reference/modifiers/byref.md) com cada parâmetro declarado. Isso torna seu código mais fácil de ler.  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Sempre há um risco potencial em permitir que um procedimento altere o valor subjacente a um argumento no código de chamada. Certifique-se de que você espera que esse valor seja alterado e esteja preparado para verificá-lo quanto à validade antes de usá-lo.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Diferenças entre argumentos modificáveis e não modificáveis](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [Diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [Como proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [Como forçar um argumento a ser passado por Valor](./how-to-force-an-argument-to-be-passed-by-value.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
