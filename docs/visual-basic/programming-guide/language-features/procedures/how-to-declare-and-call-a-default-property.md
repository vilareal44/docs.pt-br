---
title: Como declarar e chamar uma propriedade padrão
ms.date: 07/20/2015
helpviewer_keywords:
- defaults [Visual Basic], properties
- properties [Visual Basic], default
- procedures [Visual Basic], defining
- default properties [Visual Basic], in Visual Basic
- Visual Basic code, procedures
- Visual Basic code, properties
- default properties
ms.assetid: 68b4026e-09ef-4613-808e-f6287494ff63
ms.openlocfilehash: 4de5d94a94e764d1fc543ffae41b00a9bb729c94
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388148"
---
# <a name="how-to-declare-and-call-a-default-property-in-visual-basic"></a>Como declarar e chamar uma propriedade padrão no Visual Basic
Uma *propriedade padrão* é uma classe ou propriedade de estrutura que seu código pode acessar sem especificá-la. Ao chamar o código, uma classe ou estrutura, mas não uma propriedade, e o contexto permite acesso a uma propriedade, Visual Basic resolve o acesso a essa classe ou propriedade padrão da estrutura, se houver.  
  
 Uma classe ou estrutura pode ter no máximo uma propriedade padrão. No entanto, você pode sobrecarregar uma propriedade padrão e ter mais de uma versão dela.  
  
 Para obter mais informações, consulte [padrão](../../../language-reference/modifiers/default.md).  
  
### <a name="to-declare-a-default-property"></a>Para declarar uma propriedade padrão  
  
1. Declare a propriedade da maneira normal. Não especifique a `Shared` `Private` palavra-chave ou.  
  
2. Inclua a `Default` palavra-chave na declaração de propriedade.  
  
3. Especifique pelo menos um parâmetro para a propriedade. Você não pode definir uma propriedade padrão que não leve pelo menos um argumento.  
  
     [!code-vb[VbVbcnProcedures#17](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#17)]  
  
### <a name="to-call-a-default-property"></a>Para chamar uma propriedade padrão  
  
1. Declare uma variável da classe ou do tipo de estrutura que o contém.  
  
     [!code-vb[VbVbcnProcedures#16](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#16)]  
  
2. Use o nome da variável sozinha em uma expressão em que você normalmente incluiria o nome da propriedade.  
  
     [!code-vb[VbVbcnProcedures#21](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#21)]  
  
3. Siga o nome da variável com uma lista de argumentos entre parênteses. Uma propriedade padrão deve receber pelo menos um argumento.  
  
     [!code-vb[VbVbcnProcedures#20](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#20)]  
  
4. Para recuperar o valor da propriedade padrão, use o nome da variável, com uma lista de argumentos, em uma expressão ou após o sinal de igual ( `=` ) em uma instrução de atribuição.  
  
     [!code-vb[VbVbcnProcedures#15](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#15)]  
  
5. Para definir o valor da propriedade padrão, use o nome da variável, com uma lista de argumentos no lado esquerdo de uma instrução de atribuição.  
  
     [!code-vb[VbVbcnProcedures#14](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#14)]  
  
6. Você sempre pode especificar o nome da propriedade padrão junto com o nome da variável, assim como faria para acessar qualquer outra propriedade.  
  
     [!code-vb[VbVbcnProcedures#19](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#19)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir declara uma propriedade padrão em uma classe.  
  
 [!code-vb[VbVbcnProcedures#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#12)]  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como chamar a propriedade default `myProperty` na classe `class1` . As três instruções de atribuição armazenam valores em `myProperty` e a <xref:Microsoft.VisualBasic.Interaction.MsgBox%2A> chamada lê os valores.  
  
 [!code-vb[VbVbcnProcedures#13](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#13)]  
  
 O uso mais comum de uma propriedade padrão é a <xref:Microsoft.VisualBasic.Collection.Item%2A> propriedade em várias classes de coleção.  
  
## <a name="robust-programming"></a>Programação robusta  
 As propriedades padrão podem resultar em uma pequena redução em caracteres de código-fonte, mas podem tornar seu código mais difícil de ler. Se o código de chamada não estiver familiarizado com sua classe ou estrutura, quando ele fizer uma referência ao nome da classe ou da estrutura, ele não poderá ter certeza se essa referência acessa a classe ou estrutura em si ou uma propriedade padrão. Isso pode levar a erros de compilador ou erros de lógica de tempo de execução sutis.  
  
 Você pode, de certa forma, reduzir a chance de erros de propriedade padrão sempre usando a [instrução Option Strict](../../../language-reference/statements/option-strict-statement.md) para definir a verificação de tipo de compilador como `On` .  
  
 Se você estiver planejando usar uma classe ou estrutura predefinida em seu código, você deve determinar se ela tem uma propriedade padrão e, em caso afirmativo, qual é seu nome.  
  
 Devido a essas desvantagens, você deve considerar não definir propriedades padrão. Para legibilidade de código, você também deve considerar sempre referir-se a todas as propriedades explicitamente, até mesmo propriedades padrão.  
  
## <a name="see-also"></a>Confira também

- [Procedimentos de propriedade](./property-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Instrução Property](../../../language-reference/statements/property-statement.md)
- [Default](../../../language-reference/modifiers/default.md)
- [Diferenças entre propriedades e variáveis no Visual Basic](./differences-between-properties-and-variables.md)
- [Como criar uma propriedade](./how-to-create-a-property.md)
- [Como declarar uma propriedade com níveis de acesso mistos](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Como chamar um procedimento de propriedade](./how-to-call-a-property-procedure.md)
- [Como inserir um valor em uma propriedade](./how-to-put-a-value-in-a-property.md)
- [Como obter um valor a partir de uma propriedade](./how-to-get-a-value-from-a-property.md)
