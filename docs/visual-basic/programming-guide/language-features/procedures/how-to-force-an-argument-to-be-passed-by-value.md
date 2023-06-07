---
title: Como forçar um argumento a ser passado por Valor
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], arguments
- procedures [Visual Basic], parameters
- procedure arguments
- Visual Basic code, procedures
- arguments [Visual Basic], ByVal
- arguments [Visual Basic], passing by value
- procedure parameters
- procedures [Visual Basic], calling
- arguments [Visual Basic], in parentheses
- procedure arguments [Visual Basic], in parentheses
- arguments [Visual Basic], changing value
ms.assetid: 77b4f2d2-1055-4c2f-a521-874d1db86946
ms.openlocfilehash: 48f7d7afebd76916cba11459532d89d71871c79b
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84387955"
---
# <a name="how-to-force-an-argument-to-be-passed-by-value-visual-basic"></a>Como forçar um argumento a ser passado por valor (Visual Basic)
A declaração de procedimento determina o mecanismo de passagem. Se um parâmetro for declarado como [ByRef](../../../language-reference/modifiers/byref.md), Visual Basic esperará passar o argumento correspondente por referência. Isso permite que o procedimento altere o valor do elemento de programação subjacente ao argumento no código de chamada. Se você quiser proteger o elemento subjacente contra essa alteração, poderá substituir o mecanismo de `ByRef` passagem na chamada de procedimento ao colocar o nome do argumento entre parênteses. Esses parênteses são além dos parênteses que envolvem a lista de argumentos na chamada.  
  
 O código de chamada não pode substituir um mecanismo [ByVal](../../../language-reference/modifiers/byval.md) .  
  
### <a name="to-force-an-argument-to-be-passed-by-value"></a>Para forçar um argumento a ser passado por valor  
  
- Se o parâmetro correspondente for declarado `ByVal` no procedimento, você não precisará executar etapas adicionais. Visual Basic já espera passar o argumento por valor.  
  
- Se o parâmetro correspondente for declarado `ByRef` no procedimento, coloque o argumento entre parênteses na chamada de procedimento.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir substitui uma `ByRef` declaração de parâmetro. Na chamada que força `ByVal` , observe os dois níveis de parênteses.  
  
 [!code-vb[VbVbcnProcedures#39](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#39)]  
  
 [!code-vb[VbVbcnProcedures#40](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#40)]  
  
 Quando `str` é colocado entre parênteses extras dentro da lista de argumentos, o `setNewString` procedimento não pode alterar seu valor no código de chamada e `MsgBox` exibe "não pode ser substituído se passado por ByVal". Quando `str` não está entre parênteses extras, o procedimento pode alterá-lo e `MsgBox` exibe "Este é um novo valor para o argumento inString".  
  
## <a name="compile-the-code"></a>Compilar o código  
 Ao passar uma variável por referência, você deve usar a `ByRef` palavra-chave para especificar esse mecanismo.  
  
 O padrão no Visual Basic é passar argumentos por valor. No entanto, é uma boa prática de programação incluir a palavra-chave [ByVal](../../../language-reference/modifiers/byval.md) ou [ByRef](../../../language-reference/modifiers/byref.md) com cada parâmetro declarado. Isso torna seu código mais fácil de ler.  
  
## <a name="robust-programming"></a>Programação robusta  
 Se um procedimento declarar um parâmetro [ByRef](../../../language-reference/modifiers/byref.md), a execução correta do código poderá depender da possibilidade de alterar o elemento subjacente no código de chamada. Se o código de chamada substituir esse mecanismo de chamada ao colocar o argumento entre parênteses ou se ele passar um argumento não modificável, o procedimento não poderá alterar o elemento subjacente. Isso pode produzir resultados inesperados no código de chamada.  
  
## <a name="net-framework-security"></a>Segurança do .NET Framework  
 Sempre há um risco potencial em permitir que um procedimento altere o valor subjacente a um argumento no código de chamada. Certifique-se de que você espera que esse valor seja alterado e esteja preparado para verificá-lo quanto à validade antes de usá-lo.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Como passar argumentos para um procedimento](./how-to-pass-arguments-to-a-procedure.md)
- [Passar argumentos por valor e por referência](./passing-arguments-by-value-and-by-reference.md)
- [Diferenças entre argumentos modificáveis e não modificáveis](./differences-between-modifiable-and-nonmodifiable-arguments.md)
- [Diferenças entre passar um argumento por valor e por referência](./differences-between-passing-an-argument-by-value-and-by-reference.md)
- [Como alterar o valor de um argumento de procedimento](./how-to-change-the-value-of-a-procedure-argument.md)
- [Como proteger um argumento de procedimento contra alterações de valor](./how-to-protect-a-procedure-argument-against-value-changes.md)
- [Passando argumentos por posição e nome](./passing-arguments-by-position-and-by-name.md)
- [Tipos de valor e referência](../data-types/value-types-and-reference-types.md)
