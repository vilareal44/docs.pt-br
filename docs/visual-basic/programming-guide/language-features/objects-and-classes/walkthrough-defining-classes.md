---
title: definir classes
ms.date: 07/20/2015
helpviewer_keywords:
- execution [Visual Basic], ending
- objects [Visual Basic], initializing
- Initialize event [Visual Basic]
- files [Visual Basic], closing
- programs [Visual Basic], quitting
- code, exiting
- objects [Visual Basic], creating
- program termination
- classes [Visual Basic], walkthroughs
- class modules, walkthroughs
- Terminate event [Visual Basic]
- execution [Visual Basic], stopping
ms.assetid: 07018828-2d49-4cf5-a44b-19fb15d9efea
ms.openlocfilehash: 646c6ce307131f3edba194af19920eade9c1753c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84389108"
---
# <a name="walkthrough-defining-classes-visual-basic"></a>Instruções passo a passo: definindo classes (Visual Basic)

Este tutorial demonstra como definir classes, que você pode usar para criar objetos. Ele também mostra como adicionar propriedades e métodos à nova classe e demonstra como inicializar um objeto.  
  
[!INCLUDE[note_settings_general](~/includes/note-settings-general-md.md)]  
  
## <a name="to-define-a-class"></a>Para definir uma classe
  
1. Crie um projeto clicando em **novo projeto** no menu **arquivo** . A caixa de diálogo **Novo Projeto** aparecerá.  
  
2. Selecione aplicativo do Windows na lista de modelos de projeto Visual Basic para exibir o novo projeto.  
  
3. Adicione uma nova classe ao projeto clicando em **Adicionar classe** no menu **projeto** . A caixa de diálogo **Adicionar Novo Item** aparecerá.  
  
4. Selecione o modelo de **classe** .  
  
5. Nomeie a nova classe `UserNameInfo.vb` e clique em **Adicionar** para exibir o código da nova classe.  
  
     [!code-vb[VbVbalrOOP#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#5)]
  
    > [!NOTE]
    > Você pode usar o **Editor de código** Visual Basic para adicionar uma classe ao formulário de inicialização digitando a `Class` palavra-chave seguida pelo nome da nova classe. O **Editor de código** fornece uma `End Class` instrução correspondente para você.  
  
6. Defina um campo particular para a classe adicionando o seguinte código entre as `Class` instruções e `End Class` :  
  
     [!code-vb[VbVbalrOOP#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#7)]
  
     Declarar o campo como `Private` significa que ele pode ser usado somente dentro da classe. Você pode disponibilizar os campos de fora de uma classe usando modificadores de acesso, como o `Public` que fornecem mais acesso. Para obter mais informações, consulte [níveis de acesso em Visual Basic](../declared-elements/access-levels.md).  
  
7. Defina uma propriedade para a classe adicionando o seguinte código:  
  
     [!code-vb[VbVbalrOOP#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#8)]
  
8. Defina um método para a classe adicionando o seguinte código:  
  
     [!code-vb[VbVbalrOOP#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#9)]
  
9. Defina um construtor com parâmetros para a nova classe adicionando um procedimento chamado `Sub New` :  
  
     [!code-vb[VbVbalrOOP#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#10)]
  
     O `Sub New` Construtor é chamado automaticamente quando um objeto baseado nessa classe é criado. Esse construtor define o valor do campo que contém o nome de usuário.  
  
## <a name="to-create-a-button-to-test-the-class"></a>Para criar um botão para testar a classe
  
1. Altere o formulário de inicialização para o modo de design clicando com o botão direito do mouse em seu nome em **Gerenciador de soluções** e clicando em **Designer de exibição**. Por padrão, o formulário de inicialização para projetos de aplicativos do Windows é chamado de Form1. vb. O formulário principal será exibido.  
  
2. Adicione um botão ao formulário principal e clique duas vezes nele para exibir o código do manipulador de `Button1_Click` eventos. Adicione o seguinte código para chamar o procedimento de teste:  
  
     [!code-vb[VbVbalrOOP#12](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrOOP/VB/OOP.vb#12)]
  
## <a name="to-run-your-application"></a>Executar seu aplicativo
  
1. Execute o aplicativo pressionando F5. Clique no botão no formulário para chamar o procedimento de teste. Ele exibe uma mensagem informando que o original `UserName` é "Moore, Bobby", pois o procedimento chamou o `Capitalize` método do objeto.  
  
2. Clique em **OK** para descartar a caixa de mensagem. O `Button1 Click` procedimento altera o valor da `UserName` propriedade e exibe uma mensagem informando que o novo valor de `UserName` é "Worden, Joe".  
  
## <a name="see-also"></a>Confira também

- [Programação orientada a objeto (Visual Basic)](../../concepts/object-oriented-programming.md)
- [Objetos e classes](index.md)
