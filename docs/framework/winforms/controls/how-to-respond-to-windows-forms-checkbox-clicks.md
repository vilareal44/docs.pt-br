---
title: responder a cliques em caixa de seleção
description: Saiba como programar seu aplicativo Windows Forms para executar alguma ação, dependendo do estado da caixa de seleção.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Click event [Windows Forms], CheckBox control
- CheckBox control [Windows Forms], Click events
- events [Windows Forms], Click events
- double-clicks
- check boxes [Windows Forms], responding to events
ms.assetid: c39f901e-8899-43b6-aa31-939cbf7089fb
ms.openlocfilehash: 58944bc421f990343b6c58484aaab3d79c8bda5e
ms.sourcegitcommit: cb27c01a8b0b4630148374638aff4e2221f90b22
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86174491"
---
# <a name="how-to-respond-to-windows-forms-checkbox-clicks"></a>Como responder a cliques CheckBox dos Windows Forms
Sempre que um usuário clicar em um controle de Windows Forms <xref:System.Windows.Forms.CheckBox> , o <xref:System.Windows.Forms.Control.Click> evento ocorrerá. Você pode programar seu aplicativo para realizar alguma ação dependendo do estado da caixa de seleção.  
  
### <a name="to-respond-to-checkbox-clicks"></a>Para responder a cliques na caixa de seleção  
  
1. No <xref:System.Windows.Forms.Control.Click> manipulador de eventos, use a <xref:System.Windows.Forms.CheckBox.Checked%2A> propriedade para determinar o estado do controle e execute qualquer ação necessária.  
  
    ```vb  
    Private Sub CheckBox1_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles CheckBox1.Click  
       ' The CheckBox control's Text property is changed each time the
       ' control is clicked, indicating a checked or unchecked state.  
       If CheckBox1.Checked = True Then  
          CheckBox1.Text = "Checked"  
       Else  
          CheckBox1.Text = "Unchecked"  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void checkBox1_Click(object sender, System.EventArgs e)  
    {  
       // The CheckBox control's Text property is changed each time the
       // control is clicked, indicating a checked or unchecked state.  
       if (checkBox1.Checked)  
       {  
          checkBox1.Text = "Checked";  
       }  
       else  
       {  
          checkBox1.Text = "Unchecked";  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void checkBox1_CheckedChanged(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          if (checkBox1->Checked)  
          {  
             checkBox1->Text = "Checked";  
          }  
          else  
          {  
             checkBox1->Text = "Unchecked";  
          }  
       }  
    ```  
  
    > [!NOTE]
    > Se o usuário tentar clicar duas vezes no <xref:System.Windows.Forms.CheckBox> controle, cada clique será processado separadamente; ou seja, o controle não <xref:System.Windows.Forms.CheckBox> oferece suporte ao evento de clique duplo.  
  
    > [!NOTE]
    > Quando a <xref:System.Windows.Forms.CheckBox.AutoCheck%2A> propriedade é `true` (o padrão), <xref:System.Windows.Forms.CheckBox> é selecionada ou desmarcada automaticamente quando clicada. Caso contrário, você deve definir manualmente a <xref:System.Windows.Forms.CheckBox.Checked%2A> propriedade quando o <xref:System.Windows.Forms.Control.Click> evento ocorrer.  
  
     Você também pode usar o <xref:System.Windows.Forms.CheckBox> controle para determinar um curso de ação.  
  
### <a name="to-determine-a-course-of-action-when-a-check-box-is-clicked"></a>Para determinar um curso de ação quando uma caixa de seleção é clicada  
  
1. Use uma instrução Case para consultar o valor da <xref:System.Windows.Forms.CheckBox.CheckState%2A> propriedade para determinar um curso de ação. Quando a <xref:System.Windows.Forms.CheckBox.ThreeState%2A> propriedade é definida como `true` , a <xref:System.Windows.Forms.CheckBox.CheckState%2A> propriedade pode retornar três valores possíveis, que representam a caixa que está sendo marcada, a caixa que está sendo desmarcada ou um terceiro estado indeterminado no qual a caixa é exibida com uma aparência esmaecida para indicar que a opção não está disponível.  
  
    ```vb  
    Private Sub CheckBox1_Click(ByVal sender As Object, ByVal e As System.EventArgs) Handles CheckBox1.Click  
       Select Case CheckBox1.CheckState  
          Case CheckState.Checked  
             ' Code for checked state.  
          Case CheckState.Unchecked  
             ' Code for unchecked state.  
          Case CheckState.Indeterminate  
             ' Code for indeterminate state.  
       End Select
    End Sub  
    ```  
  
    ```csharp  
    private void checkBox1_Click(object sender, System.EventArgs e)  
    {  
       switch(checkBox1.CheckState)  
       {  
          case CheckState.Checked:  
             // Code for checked state.  
             break;  
          case CheckState.Unchecked:  
             // Code for unchecked state.  
             break;  
          case CheckState.Indeterminate:  
             // Code for indeterminate state.  
             break;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void checkBox1_CheckedChanged(System::Object ^ sender,  
          System::EventArgs ^ e)  
       {  
          switch(checkBox1->CheckState) {  
             case CheckState::Checked:  
                // Code for checked state.  
                break;  
             case CheckState::Unchecked:  
                // Code for unchecked state.  
                break;  
             case CheckState::Indeterminate:  
                // Code for indeterminate state.  
                break;  
          }  
       }  
    ```  
  
    > [!NOTE]
    > Quando a <xref:System.Windows.Forms.CheckBox.ThreeState%2A> propriedade é definida como `true` , a <xref:System.Windows.Forms.CheckBox.Checked%2A> propriedade retorna `true` para <xref:System.Windows.Forms.CheckState.Checked> e <xref:System.Windows.Forms.CheckState.Indeterminate> .  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.CheckBox>
- [Visão geral do controle CheckBox](checkbox-control-overview-windows-forms.md)
- [Como definir opções com controles CheckBox dos Windows Forms](how-to-set-options-with-windows-forms-checkbox-controls.md)
- [Controle de caixa de seleção](checkbox-control-windows-forms.md)
