---
title: Criação e implementação de interfaces
ms.date: 07/20/2015
helpviewer_keywords:
- interfaces [Visual Basic], walkthroughs
- interfaces [Visual Basic], testing
- interface implementation [Visual Basic], walkthrough
- interfaces [Visual Basic], creating
ms.assetid: ded82af2-9f52-4232-98ef-fe458180f112
ms.openlocfilehash: 89e8f91a04b25b84bc783d5c4f4b91cf12426f72
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84405061"
---
# <a name="walkthrough-creating-and-implementing-interfaces-visual-basic"></a>Instruções passo a passo: criando e implementando interfaces (Visual Basic)

As interfaces descrevem as características das propriedades, métodos e eventos, mas deixam os detalhes de implementação até estruturas ou classes.  
  
 Este tutorial demonstra como declarar e implementar uma interface.  
  
> [!NOTE]
> Este tutorial não fornece informações sobre como criar uma interface do usuário.  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-define-an-interface"></a>Para definir uma interface
  
1. Abra um novo projeto de aplicativo do Windows Visual Basic.  
  
2. Adicione um novo módulo ao projeto clicando em **Adicionar módulo** no menu **projeto** .  
  
3. Nomeie o novo módulo `Module1.vb` e clique em **Adicionar**. O código para o novo módulo é exibido.  
  
4. Defina uma interface chamada `TestInterface` no `Module1` digitando `Interface TestInterface` entre as `Module` instruções e e `End Module` pressione Enter. O **Editor de código** recua a `Interface` palavra-chave e adiciona uma `End Interface` instrução para formar um bloco de código.  
  
5. Defina uma propriedade, um método e um evento para a interface colocando o seguinte código entre as `Interface` `End Interface` instruções e:  
  
     [!code-vb[VbVbalrOOP#98](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#98)]
  
## <a name="implementation"></a>Implementação

 Você pode observar que a sintaxe usada para declarar membros de interface é diferente da sintaxe usada para declarar membros de classe. Essa diferença reflete o fato de que as interfaces não podem conter código de implementação.  
  
### <a name="to-implement-the-interface"></a>Para implementar a interface
  
1. Adicione uma classe chamada `ImplementationClass` adicionando a seguinte instrução a `Module1` , após a `End Interface` instrução, mas antes da `End Module` instrução, e pressione ENTER:  
  
     [!code-vb[VbVbalrOOP#99](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#99)]
  
     Se você estiver trabalhando dentro do ambiente de desenvolvimento integrado, o **Editor de código** fornecerá uma instrução correspondente `End Class` quando você pressionar Enter.  
  
2. Adicione a seguinte `Implements` instrução a `ImplementationClass` , que nomeia a interface que a classe implementa:  
  
     [!code-vb[VbVbalrOOP#100](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#100)]
  
     Quando listado separadamente de outros itens na parte superior de uma classe ou estrutura, a `Implements` instrução indica que a classe ou estrutura implementa uma interface.  
  
     Se você estiver trabalhando no ambiente de desenvolvimento integrado, o **Editor de código** implementará os membros de classe necessários ao `TestInterface` pressionar Enter, e você poderá ignorar a próxima etapa.  
  
3. Se você não estiver trabalhando no ambiente de desenvolvimento integrado, deverá implementar todos os membros da interface `MyInterface` . Adicione o seguinte código ao `ImplementationClass` para implementar `Event1` , `Method1` e `Prop1` :  
  
     [!code-vb[VbVbalrOOP#101](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#101)]
  
     A `Implements` instrução nomeia a interface e o membro de interface que está sendo implementado.  
  
4. Conclua a definição de `Prop1` adicionando um campo particular à classe que armazenou o valor da propriedade:  
  
     [!code-vb[VbVbalrOOP#102](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#102)]
  
     Retornar o valor do `pval` do acessador get da propriedade.  
  
     [!code-vb[VbVbalrOOP#103](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#103)]
  
     Defina o valor de `pval` no acessador do conjunto de propriedades.  
  
     [!code-vb[VbVbalrOOP#104](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#104)]
  
5. Conclua a definição de `Method1` adicionando o código a seguir.  
  
     [!code-vb[VbVbalrOOP#105](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#105)]
  
### <a name="to-test-the-implementation-of-the-interface"></a>Para testar a implementação da interface
  
1. Clique com o botão direito do mouse no formulário de inicialização do seu projeto no **Gerenciador de soluções**e clique em **Exibir código**. O editor exibe a classe para o formulário de inicialização. Por padrão, o formulário de inicialização é chamado `Form1` .  
  
2. Adicione o seguinte `testInstance` campo à `Form1` classe:  
  
     [!code-vb[VbVbalrOOP#120](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#120)]
  
     Ao declarar `testInstance` como `WithEvents` , a `Form1` classe pode manipular seus eventos.  
  
3. Adicione o seguinte manipulador de eventos à `Form1` classe para manipular eventos gerados por `testInstance` :  
  
     [!code-vb[VbVbalrOOP#106](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#106)]
  
4. Adicione uma sub-rotina nomeada `Test` à `Form1` classe para testar a classe de implementação:  
  
     [!code-vb[VbVbalrOOP#107](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#107)]
  
     O `Test` procedimento cria uma instância da classe que implementa `MyInterface` , atribui essa instância ao `testInstance` campo, define uma propriedade e executa um método por meio da interface.  
  
5. Adicione o código para chamar o `Test` procedimento a partir do `Form1 Load` procedimento de seu formulário de inicialização:  
  
     [!code-vb[VbVbalrOOP#108](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#108)]
  
6. Execute o `Test` procedimento pressionando F5. A mensagem "Prop1 foi definido como 9" é exibida. Depois de clicar em OK, a mensagem "o parâmetro X para Method1 é 5" é exibido. Clique em OK e a mensagem "o manipulador de eventos capturou o evento" é exibida.  
  
## <a name="see-also"></a>Confira também

- [Instrução Implements](../../../language-reference/statements/implements-statement.md)
- [Interfaces](index.md)
- [Instrução Interface](../../../language-reference/statements/interface-statement.md)
- [Instrução Event](../../../language-reference/statements/event-statement.md)
