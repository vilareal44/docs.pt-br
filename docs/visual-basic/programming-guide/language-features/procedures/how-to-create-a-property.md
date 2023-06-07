---
title: Como criar uma propriedade
ms.date: 07/20/2015
helpviewer_keywords:
- procedures [Visual Basic], defining
- Visual Basic code, procedures
- Visual Basic code, properties
- properties [Visual Basic]
ms.assetid: 4d229712-6be8-4c5c-bac5-06995ce9185a
ms.openlocfilehash: fa220998d12206e620c242b9b39df3dc1b639d29
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84388252"
---
# <a name="how-to-create-a-property-visual-basic"></a>Como criar uma propriedade (Visual Basic)
Você coloca uma definição de propriedade entre uma `Property` instrução e uma `End Property` instrução. Nessa definição, você define um `Get` procedimento, um `Set` procedimento ou ambos. Todo o código da propriedade está dentro desses procedimentos.  
  
 O `Get` procedimento recupera o valor da propriedade e o `Set` procedimento armazena um valor. Se você quiser que a propriedade tenha acesso de leitura/gravação, você deve definir os dois procedimentos. Para uma propriedade somente leitura, você define apenas `Get` e para uma propriedade somente gravação, você define somente `Set` .  
  
### <a name="to-create-a-property"></a>Para criar uma propriedade  
  
1. Fora de qualquer propriedade ou procedimento, use uma [instrução de propriedade](../../../language-reference/statements/property-statement.md), seguida por uma `End Property` instrução.  
  
2. Se a propriedade usar parâmetros, siga a `Property` palavra-chave com o nome do procedimento e, em seguida, a lista de parâmetros entre parênteses.  
  
3. Siga os parênteses com uma `As` cláusula para especificar o tipo de dados do valor da propriedade. Você deve especificar o tipo de dados até mesmo para uma propriedade somente gravação.  
  
4. Adicione `Get` `Set` procedimentos e, conforme apropriado. Consulte as instruções a seguir.  
  
### <a name="to-create-a-get-procedure-that-retrieves-a-property-value"></a>Para criar um procedimento Get que recupera um valor de propriedade  
  
1. Entre as `Property` `End Property` instruções e, escreva uma [instrução Get](../../../language-reference/statements/get-statement.md), seguida por uma `End Get` instrução. Você não precisa definir nenhum parâmetro para o `Get` procedimento.  
  
2. Coloque as instruções de código para recuperar o valor da propriedade entre `Get` as `End Get` instruções e. Esse código pode incluir outros cálculos e manipulações de dados, além de gerar e retornar o valor da propriedade.  
  
3. Use uma `Return` instrução para retornar o valor da propriedade para o código de chamada.  
  
 Você deve escrever um `Get` procedimento para uma propriedade de leitura/gravação e para uma propriedade somente leitura. Você não deve definir um `Get` procedimento para uma propriedade somente gravação.  
  
### <a name="to-create-a-set-procedure-that-writes-a-propertys-value"></a>Para criar um procedimento Set que grava o valor de uma propriedade  
  
1. Entre as `Property` `End Property` instruções e, escreva uma [instrução SET](../../../language-reference/statements/set-statement.md), seguida por uma `End Set` instrução.  
  
2. Na `Set` instrução, siga a `Set` palavra-chave com uma lista de parâmetros entre parênteses. Essa lista de parâmetros deve incluir pelo menos um parâmetro de valor para o valor passado pelo código de chamada. O nome padrão para esse parâmetro de valor é `Value` , mas você pode usar um nome diferente, se apropriado. O parâmetro de valor deve ter o mesmo tipo de dados que a própria propriedade.  
  
3. Coloque as instruções de código para armazenar um valor na propriedade entre as `Set` `End Set` instruções e. Esse código pode incluir outros cálculos e manipulações de dados, além de validar e armazenar o valor da propriedade.  
  
4. Use o parâmetro value para aceitar o valor fornecido pelo código de chamada. Você pode armazenar esse valor diretamente em uma instrução de atribuição ou usá-lo em uma expressão para calcular o valor interno a ser armazenado.  
  
 Você deve escrever um `Set` procedimento para uma propriedade de leitura/gravação e para uma propriedade somente gravação. Você não deve definir um `Set` procedimento para uma propriedade somente leitura.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir cria uma propriedade de leitura/gravação que armazena um nome completo como dois nomes de constituintes, o primeiro nome e o sobrenome. Quando o código de chamada lê `fullName` , o `Get` procedimento combina os dois nomes constituintes e retorna o nome completo. Quando o código de chamada atribui um novo nome completo, o `Set` procedimento tenta dividi-lo em dois nomes constituintes. Se não encontrar um espaço, ele o armazenará como o primeiro nome.  
  
 [!code-vb[VbVbcnProcedures#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#8)]  
  
 O exemplo a seguir mostra as chamadas típicas para os procedimentos de Propriedade do `fullName` . A primeira chamada define o valor da propriedade e a segunda chamada a recupera.  
  
 [!code-vb[VbVbcnProcedures#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbcnProcedures/VB/Class1.vb#9)]  
  
## <a name="see-also"></a>Confira também

- [Procedimentos](./index.md)
- [Procedimentos de propriedade](./property-procedures.md)
- [Parâmetros e Argumentos de Procedimento](./procedure-parameters-and-arguments.md)
- [Diferenças entre propriedades e variáveis no Visual Basic](./differences-between-properties-and-variables.md)
- [Como declarar uma propriedade com níveis de acesso mistos](./how-to-declare-a-property-with-mixed-access-levels.md)
- [Como chamar um procedimento de propriedade](./how-to-call-a-property-procedure.md)
- [Como declarar e chamar uma propriedade padrão no Visual Basic](./how-to-declare-and-call-a-default-property.md)
- [Como inserir um valor em uma propriedade](./how-to-put-a-value-in-a-property.md)
- [Como obter um valor a partir de uma propriedade](./how-to-get-a-value-from-a-property.md)
