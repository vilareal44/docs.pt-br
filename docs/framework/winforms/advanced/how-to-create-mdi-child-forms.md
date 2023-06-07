---
title: Como criar formulários filho MDI
description: Saiba como usar o Visual Studio para criar um formulário filho MDI (interface de vários documentos) que exibe um controle RichTextBox.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- MDI [Windows Forms], creating forms
- child forms
ms.assetid: 164b69bb-2eca-4339-ada3-0679eb2c6dda
ms.openlocfilehash: 7fe5cde342f0f5ee078f888b7492cd4618bea5c4
ms.sourcegitcommit: 3824ff187947572b274b9715b60c11269335c181
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84903175"
---
# <a name="how-to-create-mdi-child-forms"></a>Como criar formulários filho MDI

Os formulários filho MDI são um elemento essencial de [aplicativos de interface de vários documentos (MDI)](multiple-document-interface-mdi-applications.md), pois esses formulários são o centro da interação do usuário.

No procedimento a seguir, você usará o Visual Studio para criar um formulário filho MDI que exibe um <xref:System.Windows.Forms.RichTextBox> controle, semelhante à maioria dos aplicativos de processamento de texto. Ao substituir o <xref:System.Windows.Forms> controle por outros controles, como o <xref:System.Windows.Forms.DataGridView> controle ou uma mistura de controles, você pode criar janelas filho MDI (e, por extensão, aplicativos MDI) com diversas possibilidades.

## <a name="create-mdi-child-forms"></a>Criar formulários filho MDI

1. Crie um novo projeto de aplicativo Windows Forms no Visual Studio. Na janela **Propriedades** do formulário, defina sua <xref:System.Windows.Forms.Form.IsMdiContainer%2A> propriedade como `true` e sua `WindowsState` propriedade como `Maximized` .

   Isso designa o formulário como um recipiente MDI para janelas filho.

2. No `Toolbox` , arraste um <xref:System.Windows.Forms.MenuStrip> controle para o formulário. Defina sua propriedade `Text` para o **Arquivo**.

3. Clique nas reticências (...) ao lado da propriedade **Items** e clique em **Adicionar** para adicionar dois itens de menu da ferramenta filho. Defina a propriedade `Text` para esses itens como **Novo** e **Janela**.

4. No **Gerenciador de Soluções**, clique com o botão direito do mouse no projeto e selecione **Adicionar** > **Novo Item**.

5. Na caixa de diálogo **Adicionar novo item** , selecione **Windows Form** (no Visual Basic ou no Visual C#) ou **Windows Forms aplicativo (.net)** (em Visual C++) no painel **modelos** . Na caixa **Nome**, dê o nome **Form2** ao formulário. Selecione **abrir** para adicionar o formulário ao projeto.

    > [!NOTE]
    > O formulário MDI filho criado nesta etapa é um formulário padrão do Windows. Assim, ele tem uma <xref:System.Windows.Forms.Form.Opacity%2A> propriedade, que permite controlar a transparência do formulário. No entanto, a <xref:System.Windows.Forms.Form.Opacity%2A> propriedade foi projetada para janelas de nível superior. Não use-a com formulários filho MDI, pois podem ocorrer problemas de pintura.

     Este formulário será o modelo para seus formulários filho MDI.

     O **Designer de Formulários do Windows** é aberto, exibindo **Form2**.

6. Na **Caixa de Ferramentas**, arraste um controle **RichTextBox** para o formulário.

7. Na janela **Propriedades**, defina a propriedade `Anchor` como **Superior, Esquerda** e a propriedade `Dock` como **Preenchimento**.

   Isso faz com que o <xref:System.Windows.Forms.RichTextBox> controle preencha completamente a área do formulário filho MDI, mesmo quando o formulário é redimensionado.

8. Clique duas vezes no **novo** item de menu para criar um <xref:System.Windows.Forms.Control.Click> manipulador de eventos para ele.

9. Insira um código semelhante ao seguinte para criar um novo formulário filho MDI quando o usuário clicar no item de menu **Novo**.

   > [!NOTE]
   > No exemplo a seguir, o manipulador de eventos manipula o <xref:System.Windows.Forms.Control.Click> evento para `MenuItem2` . Lembre-se de que, dependendo das especificidades da arquitetura de seu aplicativo, seu item de menu **Novo** não poderá ser `MenuItem2`.

    ```vb
    Protected Sub MDIChildNew_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles MenuItem2.Click
       Dim NewMDIChild As New Form2()
       'Set the Parent Form of the Child window.
       NewMDIChild.MdiParent = Me
       'Display the new form.
       NewMDIChild.Show()
    End Sub
    ```

    ```csharp
    protected void MDIChildNew_Click(object sender, System.EventArgs e){
       Form2 newMDIChild = new Form2();
       // Set the Parent Form of the Child window.
       newMDIChild.MdiParent = this;
       // Display the new form.
       newMDIChild.Show();
    }
    ```

    ```cpp
    private:
       void menuItem2_Click(System::Object ^ sender,
          System::EventArgs ^ e)
       {
          Form2^ newMDIChild = gcnew Form2();
          // Set the Parent Form of the Child window.
          newMDIChild->MdiParent = this;
          // Display the new form.
          newMDIChild->Show();
       }
    ```

   Em C++, adicione a seguinte `#include` diretiva na parte superior de Form1. h:

   ```cpp
   #include "Form2.h"
   ```

10. Na lista suspensa na parte superior da janela **Propriedades** , selecione a faixa de menu que corresponde à faixa de menu **arquivo** e defina a <xref:System.Windows.Forms.MenuStrip.MdiWindowListItem%2A> propriedade para a janela <xref:System.Windows.Forms.ToolStripMenuItem> .

    Isso permite que o menu **janela** mantenha uma lista de janelas filho MDI abertas com uma marca de seleção ao lado da janela filho ativa.

11. Pressione **F5** para executar o aplicativo. Ao selecionar **Novo** do menu **Arquivo**, você pode criar novos formulários filho MDI, que são mantidos no item de menu **Janela**.

    > [!NOTE]
    > Quando um formulário filho MDI tem um <xref:System.Windows.Forms.MainMenu> componente (com, geralmente, uma estrutura de menu de itens de menu) e é aberto em um formulário pai MDI que tem um <xref:System.Windows.Forms.MainMenu> componente (com, normalmente, uma estrutura de menu de itens de menu), os itens de menu serão mesclados automaticamente se você tiver definido a <xref:System.Windows.Forms.MenuItem.MergeType%2A> Propriedade (e, opcionalmente, a <xref:System.Windows.Forms.MenuItem.MergeOrder%2A> Propriedade). Defina a <xref:System.Windows.Forms.MenuItem.MergeType%2A> propriedade de ambos os <xref:System.Windows.Forms.MainMenu> componentes e todos os itens de menu do formulário filho como <xref:System.Windows.Forms.MenuMerge.MergeItems> . Além disso, defina a <xref:System.Windows.Forms.MenuItem.MergeOrder%2A> propriedade para que os itens de menu de ambos os menus apareçam na ordem desejada. Além disso, tenha em mente que quando você fecha um formulário pai MDI, cada um dos formulários filho MDI gera um <xref:System.Windows.Forms.Form.Closing> evento antes que o <xref:System.Windows.Forms.Form.Closing> evento para o pai MDI seja gerado. Cancelar o evento de um filho MDI <xref:System.Windows.Forms.Form.Closing> não impedirá que o evento do pai MDI <xref:System.Windows.Forms.Form.Closing> seja gerado; no entanto, o <xref:System.ComponentModel.CancelEventArgs> argumento para o evento do pai MDI <xref:System.Windows.Forms.Form.Closing> agora será definido como `true` . Você pode forçar o fechamento do pai MDI e todos os formulários filho MDI ao definir o <xref:System.ComponentModel.CancelEventArgs> argumento como `false` .

## <a name="see-also"></a>Veja também

- [Aplicativos da interface MDI (Interface de Vários Documentos)](multiple-document-interface-mdi-applications.md)
- [Como criar formulários pai MDI](how-to-create-mdi-parent-forms.md)
- [Como determinar o filho MDI ativo](how-to-determine-the-active-mdi-child.md)
- [Como enviar dados para o filhos MDI ativos](how-to-send-data-to-the-active-mdi-child.md)
- [Como Organizar Formulários Filho MDI](how-to-arrange-mdi-child-forms.md)
