---
title: responder a cliques em botão
description: Saiba como responder a Windows Forms cliques de botão. O uso mais básico de um controle de botão Windows Forms é executar algum código quando o botão é clicado.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- buttons [Windows Forms], responding to Click events
- events [Windows Forms], Click events
- Click event [Windows Forms], Button control
- MouseDown event
- Button control [Windows Forms], click response
- double-clicks
- examples [Windows Forms], controls
- Click event [Windows Forms], responding to
ms.assetid: 7a4951bd-369c-4662-b246-28ad83eda484
ms.openlocfilehash: 5c458d56dbd6f1cab8e88bdbb86ede958367e5c4
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619722"
---
# <a name="how-to-respond-to-windows-forms-button-clicks"></a>Como responder a cliques no botão dos Windows Forms
O uso mais básico de um controle de Windows Forms <xref:System.Windows.Forms.Button> é executar algum código quando o botão é clicado.  
  
 Clicar em um <xref:System.Windows.Forms.Button> controle também gera vários outros eventos, como os <xref:System.Windows.Forms.Control.MouseEnter> <xref:System.Windows.Forms.Control.MouseDown> eventos, e <xref:System.Windows.Forms.Control.MouseUp> . Se você pretende anexar manipuladores de eventos para esses eventos relacionados, verifique se suas ações não entrem em conflito. Por exemplo, se clicar no botão limpa as informações que o usuário digitou na caixa de texto, pausar o ponteiro do mouse sobre o botão não deverá exibir uma dica de ferramenta com essas informações agora inexistentes.  
  
 Se o usuário tentar clicar duas vezes no <xref:System.Windows.Forms.Button> controle, cada clique será processado separadamente; ou seja, o controle não oferece suporte ao evento de clique duplo.  
  
### <a name="to-respond-to-a-button-click"></a>Para responder a um clique de botão  
  
- No botão, `Click` <xref:System.EventHandler> escreva o código para executar. `Button1_Click` deve ser associado ao controle. Para saber mais, veja [Como criar manipuladores de eventos em tempo de execução para formulários dos Windows Forms](../how-to-create-event-handlers-at-run-time-for-windows-forms.md).  
  
    ```vb  
    Private Sub Button1_Click(ByVal sender As System.Object, ByVal e As System.EventArgs) Handles Button1.Click  
       MessageBox.Show("Button1 was clicked")  
    End Sub  
    ```  
  
    ```csharp  
    private void button1_Click(object sender, System.EventArgs e)  
    {  
       MessageBox.Show("button1 was clicked");  
    }  
    ```  
  
    ```cpp  
    private:  
       void button1_Click(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          MessageBox::Show("button1 was clicked");  
       }  
    ```  
  
## <a name="see-also"></a>Consulte também

- [Visão geral do controle Button](button-control-overview-windows-forms.md)
- [Forma de selecionar um controle de botão dos Windows Forms](ways-to-select-a-windows-forms-button-control.md)
- [Controle de botão](button-control-windows-forms.md)
