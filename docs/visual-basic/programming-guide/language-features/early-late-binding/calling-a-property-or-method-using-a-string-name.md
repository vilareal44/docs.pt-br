---
title: Chamando uma propriedade ou um método usando o nome de uma cadeia de caracteres
ms.date: 07/20/2015
helpviewer_keywords:
- passing operators [Visual Basic]
- strings [Visual Basic], passing new operators as
- objects [Visual Basic], setting properties
- setting properties, object properties at run time
- method calls [Visual Basic], strings
- methods [Visual Basic], calling with string names
- calling methods [Visual Basic], string names
- properties [Visual Basic], setting at run time
- CallByName function
ms.assetid: 79a7b8b4-b8c7-4ad8-aca8-12a9a2b32f03
ms.openlocfilehash: 29072479db36f9f8a81ffd7f3f5b10208ebaa984
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410651"
---
# <a name="calling-a-property-or-method-using-a-string-name-visual-basic"></a>Chamando uma propriedade ou um método usando o nome de uma cadeia de caracteres (Visual Basic)
Na maioria dos casos, você pode descobrir as propriedades e os métodos de um objeto em tempo de design e escrever código para tratá-los. No entanto, em alguns casos, talvez você não saiba sobre as propriedades e os métodos de um objeto com antecedência, ou talvez queira apenas a flexibilidade de permitir que um usuário final especifique propriedades ou execute métodos em tempo de execução.  
  
## <a name="callbyname-function"></a>Função CallByName  
 Considere, por exemplo, um aplicativo cliente que avalia as expressões inseridas pelo usuário, passando um operador para um componente COM. Suponha que você esteja constantemente adicionando novas funções ao componente que exigem novos operadores. Ao usar técnicas de acesso de objeto padrão, você deve recompilar e redistribuir o aplicativo cliente antes de poder usar os novos operadores. Para evitar isso, você pode usar a `CallByName` função para passar os novos operadores como cadeias de caracteres, sem alterar o aplicativo.  
  
 A `CallByName` função permite que você use uma cadeia de caracteres para especificar uma propriedade ou um método em tempo de execução. A assinatura da `CallByName` função é parecida com esta:  
  
 *Result*  =  Resultado `CallByName` (*Object*, *ProcedureName*, *CallType*, *argumentos*())  
  
 O primeiro argumento, *Object*, usa o nome do objeto no qual você deseja agir. O argumento *ProcedureName* usa uma cadeia de caracteres que contém o nome do método ou procedimento de propriedade a ser invocado. O argumento *CallType* usa uma constante que representa o tipo de procedimento a ser invocado: um método ( `Microsoft.VisualBasic.CallType.Method` ), uma propriedade leitura ( `Microsoft.VisualBasic.CallType.Get` ) ou um conjunto de Propriedades ( `Microsoft.VisualBasic.CallType.Set` ). O argumento *arguments* , que é opcional, usa uma matriz do tipo `Object` que contém argumentos para o procedimento.  
  
 Você pode usar `CallByName` com classes em sua solução atual, mas ela é usada com mais frequência para acessar objetos com ou objetos de .NET Framework assemblies.  
  
 Suponha que você adicione uma referência a um assembly que contém uma classe chamada `MathClass` , que tem uma nova função chamada `SquareRoot` , conforme mostrado no código a seguir:  
  
 [!code-vb[VbVbalrOOP#53](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#53)]  
  
 Seu aplicativo pode usar controles de caixa de texto para controlar qual método será chamado e seus argumentos. Por exemplo, se `TextBox1` contiver a expressão a ser avaliada e `TextBox2` for usado para inserir o nome da função, você poderá usar o seguinte código para invocar a `SquareRoot` função na expressão em `TextBox1` :  
  
 [!code-vb[VbVbalrOOP#54](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#54)]  
  
 Se você inserir "64" em `TextBox1` , "SquareRoot" em `TextBox2` e, em seguida, chamar o `CallMath` procedimento, a raiz quadrada do número em `TextBox1` será avaliada. O código no exemplo invoca a `SquareRoot` função (que usa uma cadeia de caracteres que contém a expressão a ser avaliada como um argumento obrigatório) e retorna "8" na `TextBox1` (a raiz quadrada de 64). É claro que, se o usuário inserir uma cadeia de caracteres inválida no `TextBox2` , se a cadeia de caracteres contiver o nome de uma propriedade em vez de um método, ou se o método tiver um argumento adicional necessário, ocorrerá um erro em tempo de execução. Você precisa adicionar um código de tratamento de erros robusto ao usar o `CallByName` para antecipar esses ou quaisquer outros erros.  
  
> [!NOTE]
> Embora a `CallByName` função possa ser útil em alguns casos, você deve avaliar sua utilidade em relação às implicações de desempenho — usar `CallByName` para invocar um procedimento é um pouco mais lento do que uma chamada de associação tardia. Se você estiver invocando uma função que é chamada repetidamente, como dentro de um loop, o `CallByName` pode ter um efeito grave no desempenho.  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Interaction.CallByName%2A>
- [Determinar o Tipo de Objeto](determining-object-type.md)
