---
title: Selecionar texto no controle de caixa de texto
description: Saiba como selecionar texto programaticamente no controle de caixa de texto Windows Forms. Além disso, saiba como alertar visualmente o leitor da posição da cadeia de caracteres encontrada.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- TextBox control [Windows Forms], selecting text programmatically
- text boxes [Windows Forms], selecting text programmatically
- text [Windows Forms], selecting in text boxes programmatically
ms.assetid: 8c591546-6a01-45c7-8e03-f78431f903b1
ms.openlocfilehash: b8fdaff76461c4d6766dfc6afaef5e814d982834
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85621555"
---
# <a name="how-to-select-text-in-the-windows-forms-textbox-control"></a>Como selecionar texto no controle TextBox dos Windows Forms
Você pode selecionar texto programaticamente no <xref:System.Windows.Forms.TextBox> controle de Windows Forms. Por exemplo, se você criar uma função que pesquisa de texto de uma determinada cadeia de caracteres, poderá selecionar o texto para alertar visualmente o leitor da posição da cadeia de caracteres encontrada.  
  
### <a name="to-select-text-programmatically"></a>Selecionar texto com programação  
  
1. Defina a <xref:System.Windows.Forms.TextBoxBase.SelectionStart%2A> propriedade para o início do texto que você deseja selecionar.  
  
     A <xref:System.Windows.Forms.TextBoxBase.SelectionStart%2A> propriedade é um número que indica o ponto de inserção dentro da cadeia de texto, sendo que 0 é a posição mais à esquerda. Se a <xref:System.Windows.Forms.TextBoxBase.SelectionStart%2A> propriedade for definida como um valor igual ou maior que o número de caracteres na caixa de texto, o ponto de inserção será colocado após o último caractere.  
  
2. Defina a <xref:System.Windows.Forms.TextBoxBase.SelectionLength%2A> propriedade como o comprimento do texto que você deseja selecionar.  
  
     A <xref:System.Windows.Forms.TextBoxBase.SelectionLength%2A> propriedade é um valor numérico que define a largura do ponto de inserção. A definição de <xref:System.Windows.Forms.TextBoxBase.SelectionLength%2A> para um número maior que 0 faz com que esse número de caracteres seja selecionado, começando do ponto de inserção atual.  
  
3. Adicional Acesse o texto selecionado por meio da <xref:System.Windows.Forms.TextBoxBase.SelectedText%2A> propriedade.  
  
     O código abaixo seleciona o conteúdo de uma caixa de texto quando o evento do controle <xref:System.Windows.Forms.Control.Enter> ocorre. Este exemplo verifica se a caixa de texto tem um valor para a <xref:System.Windows.Forms.TextBox.Text%2A> propriedade que não é `null` ou uma cadeia de caracteres vazia. Quando a caixa de texto recebe o foco, o texto presente nela é selecionado. O manipulador de eventos `TextBox1_Enter` deve ser associado ao controle; para obter mais informações, consulte [Como criar manipuladores de eventos no tempo de execução para Windows Forms](../how-to-create-event-handlers-at-run-time-for-windows-forms.md).  
  
     Para testar esse exemplo, pressione a tecla Tab até que a caixa de texto receber o foco. Se você clicar na caixa de texto, a seleção dele será cancelada.  
  
    ```vb  
    Private Sub TextBox1_Enter(ByVal sender As Object, ByVal e As System.EventArgs) Handles TextBox1.Enter  
       If (Not String.IsNullOrEmpty(TextBox1.Text)) Then  
          TextBox1.SelectionStart = 0  
          TextBox1.SelectionLength = TextBox1.Text.Length  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_Enter(object sender, System.EventArgs e){  
       if (!String.IsNullOrEmpty(textBox1.Text))  
       {  
          textBox1.SelectionStart = 0;  
          textBox1.SelectionLength = textBox1.Text.Length;  
       }  
    }  
    ```  
  
    ```cpp  
    private:  
       void textBox1_Enter(System::Object ^ sender,  
          System::EventArgs ^ e) {  
       if (!System::String::IsNullOrEmpty(textBox1->Text))  
       {  
          textBox1->SelectionStart = 0;  
          textBox1->SelectionLength = textBox1->Text->Length;  
       }  
    }  
    ```  
  
## <a name="see-also"></a>Consulte também

- <xref:System.Windows.Forms.TextBox>
- [Visão geral do controle TextBox](textbox-control-overview-windows-forms.md)
- [Como controlar o ponto de inserção em um controle TextBox dos Windows Forms](how-to-control-the-insertion-point-in-a-windows-forms-textbox-control.md)
- [Como criar uma caixa de texto de senha com o controle TextBox dos Windows Forms](how-to-create-a-password-text-box-with-the-windows-forms-textbox-control.md)
- [Como criar uma caixa de texto somente leitura](how-to-create-a-read-only-text-box-windows-forms.md)
- [Como inserir aspas em uma cadeia de caracteres](how-to-put-quotation-marks-in-a-string-windows-forms.md)
- [Como exibir várias linhas no controle TextBox dos Windows Forms](how-to-view-multiple-lines-in-the-windows-forms-textbox-control.md)
- [Controle TextBox](textbox-control-windows-forms.md)
