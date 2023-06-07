---
title: Procedimentos de função
ms.date: 07/20/2015
helpviewer_keywords:
- Function procedures
- return values [Visual Basic], function procedures
- Visual Basic code, procedures
- procedures [Visual Basic], calling
- procedures [Visual Basic], Function procedures
- syntax [Visual Basic], function procedures
ms.assetid: 1b9f632c-553b-4cb6-920a-ded117ead8c0
ms.openlocfilehash: b0ba96a875fd8785e45eee565beefe4b961ffc9d
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388745"
---
# <a name="function-procedures-visual-basic"></a>Procedimentos de função (Visual Basic)

Um `Function` procedimento é uma série de instruções Visual Basic delimitadas `Function` pelas `End Function` instruções e. O `Function` procedimento executa uma tarefa e retorna o controle para o código de chamada. Quando ele retorna o controle, ele também retorna um valor para o código de chamada.

Cada vez que o procedimento é chamado, suas instruções são executadas, começando pela primeira instrução executável após a `Function` instrução e terminando com a primeira `End Function` `Exit Function` instrução,, ou `Return` encontrada.

Você pode definir um `Function` procedimento em um módulo, classe ou estrutura. Ele é `Public` por padrão, o que significa que você pode chamá-lo de qualquer lugar em seu aplicativo que tenha acesso ao módulo, à classe ou à estrutura na qual você o definiu.

Um `Function` procedimento pode receber argumentos, como constantes, variáveis ou expressões, que são passados para ele pelo código de chamada.

## <a name="declaration-syntax"></a>Sintaxe de declaração

A sintaxe para declarar um `Function` procedimento é a seguinte:

```vb
[Modifiers] Function FunctionName [(ParameterList)] As ReturnType
    [Statements]
End Function
```

Os *modificadores* podem especificar o nível de acesso e as informações sobre sobrecarga, substituição, compartilhamento e sombreamento. Para obter mais informações, consulte [instrução de função](../../../language-reference/statements/function-statement.md).

Você declara cada parâmetro da mesma maneira que faz para [procedimentos sub](./sub-procedures.md).

### <a name="data-type"></a>Tipo de dados

Cada `Function` procedimento tem um tipo de dados, da mesma forma que cada variável. Esse tipo de dados é especificado pela `As` cláusula na `Function` instrução e determina o tipo de dados do valor que a função retorna para o código de chamada. As declarações de exemplo a seguir ilustram isso.

```vb
Function Yesterday() As Date
End Function

Function FindSqrt(radicand As Single) As Single
End Function
```

Para obter mais informações, consulte "partes" na [instrução function](../../../language-reference/statements/function-statement.md).

### <a name="returning-values"></a>Retornando valores

O valor `Function` que um procedimento envia de volta para o código de chamada é chamado de valor de retorno. O procedimento retorna esse valor de uma das duas maneiras:

- Ele usa a `Return` instrução para especificar o valor de retorno e retorna o controle imediatamente para o programa de chamada. O exemplo a seguir ilustra isto.

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement immediately transfers control back
      ' to the calling code and returns the value of Expression.
      Return Expression
  End Function
  ```

- Ele atribui um valor ao seu próprio nome de função em uma ou mais instruções do procedimento. O controle não retorna ao programa de chamada até que `Exit Function` uma `End Function` instrução ou seja executada. O exemplo a seguir ilustra isto.

  ```vb
  Function FunctionName [(ParameterList)] As ReturnType
      ' The following statement does not transfer control back to the calling code.
      FunctionName = Expression
      ' When control returns to the calling code, Expression is the return value.
  End Function
  ```

A vantagem de atribuir o valor de retorno ao nome da função é que o controle não retorna do procedimento até encontrar uma `Exit Function` `End Function` instrução or. Isso permite que você atribua um valor preliminar e ajuste-o posteriormente, se necessário.

Para obter mais informações sobre valores de retorno, consulte [instrução de função](../../../language-reference/statements/function-statement.md). Para obter informações sobre como retornar matrizes, consulte [matrizes](../arrays/index.md).

## <a name="calling-syntax"></a>Sintaxe de chamada

Você invoca um `Function` procedimento incluindo seu nome e argumentos no lado direito de uma instrução de atribuição ou em uma expressão. Você deve fornecer valores para todos os argumentos que não são opcionais, e você deve colocar a lista de argumentos entre parênteses. Se nenhum argumento for fornecido, você pode opcionalmente omitir os parênteses.

A sintaxe de uma chamada para um `Function` procedimento é a seguinte.

*lvalue* `=` *nomedafunção* `[(` *argumentolist*    `)]`

`If ((`*nomedafunção* `[(` *argumentolist* `)] / 3) <=` *expressão* de  `) Then`

Quando você chama um `Function` procedimento, não precisa usar seu valor de retorno. Se você não fizer isso, todas as ações da função serão executadas, mas o valor de retorno será ignorado. <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A>geralmente é chamado dessa maneira.

### <a name="illustration-of-declaration-and-call"></a>Ilustração de declaração e chamada

O procedimento a seguir `Function` calcula o lado mais longo, ou hipotenusa, de um triângulo à direita, dado os valores para os outros dois lados.

[!code-vb[VbVbcnProcedures#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#1)]

O exemplo a seguir mostra uma chamada típica para `hypotenuse` .

[!code-vb[VbVbcnProcedures#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#6)]

## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Subprocedimentos](./sub-procedures.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Procedimentos do operador](./operator-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Function](../../../language-reference/statements/function-statement.md)
- [Como criar um procedimento que retorne um valor](./how-to-create-a-procedure-that-returns-a-value.md)
- [Como retornar um valor de um procedimento](./how-to-return-a-value-from-a-procedure.md)
- [Como chamar um procedimento que retorna um valor](./how-to-call-a-procedure-that-returns-a-value.md)
