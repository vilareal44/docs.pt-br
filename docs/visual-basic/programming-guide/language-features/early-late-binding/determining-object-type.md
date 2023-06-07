---
title: Determinar o Tipo de Objeto
ms.date: 07/20/2015
helpviewer_keywords:
- classes [Visual Basic], discovering which an object belongs to
- types [Visual Basic], determining Visual Basic object types
- object variables [Visual Basic], testing values
- TypeOf...Is expression, object type at run time
- TypeName function
- objects [Visual Basic], type determining
ms.assetid: d95e7ad1-cd63-41d6-9a28-d7a1380d49c1
ms.openlocfilehash: 3b1c4ad0ab4fd8d2897aff6ad9097cdc81272455
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410638"
---
# <a name="determining-object-type-visual-basic"></a>Determinando o tipo de objeto (Visual Basic)
Variáveis de objeto genérico (ou seja, variáveis declaradas como `Object` ) podem conter objetos de qualquer classe. Ao usar variáveis do tipo `Object` , talvez seja necessário executar ações diferentes com base na classe do objeto; por exemplo, alguns objetos podem não dar suporte a uma propriedade ou um método específico. Visual Basic fornece dois meios de determinar qual tipo de objeto é armazenado em uma variável de objeto: a `TypeName` função e o `TypeOf...Is` operador.  
  
## <a name="typename-and-typeofis"></a>TypeName e TypeOf... For  
 A `TypeName` função retorna uma cadeia de caracteres e é a melhor opção quando você precisa armazenar ou exibir o nome de classe de um objeto, conforme mostrado no fragmento de código a seguir:  
  
 [!code-vb[VbVbalrOOP#92](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#92)]  
  
 O `TypeOf...Is` operador é a melhor opção para testar o tipo de um objeto, pois é muito mais rápido do que uma comparação de cadeia de caracteres equivalente usando `TypeName` . O fragmento de código a seguir usa `TypeOf...Is` em uma `If...Then...Else` instrução:  
  
 [!code-vb[VbVbalrOOP#93](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#93)]  
  
 Uma palavra de atenção é devida aqui. O `TypeOf...Is` operador retornará `True` se um objeto for de um tipo específico ou for derivado de um tipo específico. Quase tudo o que você faz com Visual Basic envolve objetos, que incluem alguns elementos que normalmente não são considerados objetos como, por exemplo, cadeias de caracteres e inteiros. Esses objetos são derivados de e herdam métodos do <xref:System.Object> . Quando passado um `Integer` e avaliado com `Object` , o `TypeOf...Is` operador retorna `True` . O exemplo a seguir relata que o parâmetro `InParam` é um `Object` e um `Integer` :  
  
 [!code-vb[VbVbalrOOP#94](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#94)]  
  
 O exemplo a seguir usa `TypeOf...Is` e `TypeName` para determinar o tipo de objeto passado para ele no `Ctrl` argumento. O `TestObject` procedimento chama `ShowType` três tipos diferentes de controles.  
  
#### <a name="to-run-the-example"></a>Para executar o exemplo  
  
1. Crie um novo projeto de aplicativo do Windows e adicione um <xref:System.Windows.Forms.Button> controle, um <xref:System.Windows.Forms.CheckBox> controle e um <xref:System.Windows.Forms.RadioButton> controle ao formulário.  
  
2. No botão do formulário, chame o `TestObject` procedimento.  
  
3. Adicione o seguinte código ao seu formulário:  
  
     [!code-vb[VbVbalrOOP#95](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#95)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Information.TypeName%2A>
- [Chamando uma propriedade ou um método usando o nome de uma cadeia de caracteres](calling-a-property-or-method-using-a-string-name.md)
- [Tipo de dados Object](../../../language-reference/data-types/object-data-type.md)
- [Instrução If...Then...Else](../../../language-reference/statements/if-then-else-statement.md)
- [Tipo de dados da cadeia de caracteres](../../../language-reference/data-types/string-data-type.md)
- [Tipo de Dados Integer](../../../language-reference/data-types/integer-data-type.md)
