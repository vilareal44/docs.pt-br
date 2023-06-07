---
title: Como criar uma expressão lambda
ms.date: 07/20/2015
helpviewer_keywords:
- lambda expressions [Visual Basic]
- expressions [Visual Basic], lambda
ms.assetid: 3279bd5c-80f7-410a-a7ba-f7085ed36aa5
ms.openlocfilehash: 7affc84fa501ba98bdfa93835f0b0e381580b9bd
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388381"
---
# <a name="how-to-create-a-lambda-expression-visual-basic"></a>Como criar uma expressão lambda (Visual Basic)
Uma *expressão lambda* é uma função ou sub-rotina que não tem um nome. Uma expressão lambda pode ser usada sempre que um tipo delegado é válido.  
  
### <a name="to-create-a-single-line-lambda-expression-function"></a>Para criar uma função de expressão lambda de linha única  
  
1. Em qualquer situação em que um tipo delegado possa ser usado, digite a palavra-chave `Function` , como no exemplo a seguir:  
  
     `Dim add1 =`   `Function`  
  
2. Entre parênteses, diretamente depois `Function` , digite os parâmetros da função. Observe que você não especifica um nome após `Function` .  
  
     `Dim add1 = Function`   `(num As Integer)`  
  
3. Após a lista de parâmetros, digite uma única expressão como o corpo da função. O valor que a expressão avalia é o valor retornado pela função. Você não usa uma `As` cláusula para especificar o tipo de retorno.  
  
     [!code-vb[VbVbalrLambdas#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#1)]  
  
     Você chama a expressão lambda passando um argumento inteiro.  
  
     [!code-vb[VbVbalrLambdas#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#2)]  
  
4. Como alternativa, o mesmo resultado é realizado pelo exemplo a seguir:  
  
     [!code-vb[VbVbalrLambdas#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#3)]  
  
### <a name="to-create-a-single-line-lambda-expression-subroutine"></a>Para criar uma sub-rotina de expressão lambda de linha única  
  
1. Em qualquer situação em que um tipo delegado possa ser usado, digite a palavra-chave `Sub` , conforme mostrado no exemplo a seguir.  
  
     `Dim add1 =`   `Sub`  
  
2. Entre parênteses, diretamente depois `Sub` , digite os parâmetros da sub-rotina. Observe que você não especifica um nome após `Sub` .  
  
     `Dim add1 = Sub`   `(msg As String)`  
  
3. Após a lista de parâmetros, digite uma única instrução como o corpo da sub-rotina.  
  
     [!code-vb[VbVbalrLambdas#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#17)]  
  
     Você chama a expressão lambda passando um argumento de cadeia de caracteres.  
  
     [!code-vb[VbVbalrLambdas#18](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#18)]  
  
### <a name="to-create-a-multiline-lambda-expression-function"></a>Para criar uma função de expressão lambda de várias linhas  
  
1. Em qualquer situação em que um tipo delegado possa ser usado, digite a palavra-chave `Function` , conforme mostrado no exemplo a seguir.  
  
     `Dim add1 =`   `Function`  
  
2. Entre parênteses, diretamente depois `Function` , digite os parâmetros da função. Observe que você não especifica um nome após `Function` .  
  
     `Dim add1 = Function`   `(index As Integer)`  
  
3. Pressione ENTER. A `End Function` instrução é adicionada automaticamente.  
  
4. No corpo da função, adicione o código a seguir para criar uma expressão e retornar o valor. Você não usa uma `As` cláusula para especificar o tipo de retorno.  
  
     [!code-vb[VbVbalrLambdas#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#19)]  
  
     Você chama a expressão lambda passando um argumento inteiro.  
  
     [!code-vb[VbVbalrLambdas#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#20)]  
  
### <a name="to-create-a-multiline-lambda-expression-subroutine"></a>Para criar uma sub-rotina de expressão lambda de várias linhas  
  
1. Em qualquer situação em que um tipo delegado possa ser usado, digite a palavra-chave `Sub` , conforme mostrado no exemplo a seguir:  
  
     `Dim add1 =`   `Sub`  
  
2. Entre parênteses, diretamente depois `Sub` , digite os parâmetros da sub-rotina. Observe que você não especifica um nome após `Sub` .  
  
     `Dim add1 = Sub`  `(msg As String)`  
  
3. Pressione ENTER. A `End Sub` instrução é adicionada automaticamente.  
  
4. No corpo da função, adicione o seguinte código a ser executado quando a sub-rotina for invocada.  
  
     [!code-vb[VbVbalrLambdas#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#21)]  
  
     Você chama a expressão lambda passando um argumento de cadeia de caracteres.  
  
     [!code-vb[VbVbalrLambdas#22](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class1.vb#22)]  
  
## <a name="example"></a>Exemplo  
 Um uso comum de expressões lambda é definir uma função que possa ser passada como o argumento para um parâmetro cujo tipo é `Delegate` . No exemplo a seguir, o <xref:System.Diagnostics.Process.GetProcesses%2A> método retorna uma matriz dos processos em execução no computador local. O <xref:System.Linq.Enumerable.Where%2A> método da <xref:System.Linq.Enumerable> classe requer um `Boolean` delegado como argumento. A expressão lambda no exemplo é usada para essa finalidade. Ele retorna `True` para cada processo que tem apenas um thread, e eles são selecionados em `filteredList` .  
  
 [!code-vb[VbVbalrLambdas#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class4.vb#10)]  
  
 O exemplo anterior é equivalente ao código a seguir, que é escrito na sintaxe de LINQ (consulta integrada à linguagem):  
  
 [!code-vb[VbVbalrLambdas#11](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrLambdas/VB/Class5.vb#11)]  
  
## <a name="see-also"></a>Confira também

- <xref:System.Linq.Enumerable>
- [Expressões lambda](./lambda-expressions.md)
- [Instrução Function](../../../language-reference/statements/function-statement.md)
- [Instrução Sub](../../../language-reference/statements/sub-statement.md)
- [Delegados](../delegates/index.md)
- [Como passar procedimentos para outro procedimento no Visual Basic](../delegates/how-to-pass-procedures-to-another-procedure.md)
- [Instrução Delegate](../../../language-reference/statements/delegate-statement.md)
- [Introdução a LINQ no Visual Basic](../linq/introduction-to-linq.md)
