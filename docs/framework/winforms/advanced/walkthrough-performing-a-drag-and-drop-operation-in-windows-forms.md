---
title: 'Walkthrough: executar uma operação de arrastar e soltar'
description: Saiba como executar uma operação de arrastar e soltar em Windows Forms manipulando uma série de eventos, mais notavelmente os eventos DragEnter, DragLeave e DragDrop.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- Windows Forms, drag and drop operations
- drag and drop [Windows Forms], Windows Forms
ms.assetid: eb66f6bf-4a7d-4c2d-b276-40fefb2d3b6c
ms.openlocfilehash: 83bfda875e2fdec3981bbcb8f8f7be00db342440
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325820"
---
# <a name="walkthrough-performing-a-drag-and-drop-operation-in-windows-forms"></a>Instruções passo a passo: executando uma operação de arrastar e soltar no Windows Forms
Para executar operações de arrastar e soltar em aplicativos baseados no Windows, você deve lidar com uma série de eventos, principalmente os <xref:System.Windows.Forms.Control.DragEnter> <xref:System.Windows.Forms.Control.DragLeave> eventos, e <xref:System.Windows.Forms.Control.DragDrop> . Trabalhando com as informações disponíveis no evento argumentos desses eventos, você pode facilmente orientar as operações do tipo "arrastar e soltar".  
  
## <a name="dragging-data"></a>Arrastando dados  
 Todas as operações do tipo "arrastar e soltar" começam com arrastar. A funcionalidade para habilitar os dados a serem coletados ao arrastar iniciar é implementada no <xref:System.Windows.Forms.Control.DoDragDrop%2A> método.  
  
 No exemplo a seguir, o <xref:System.Windows.Forms.Control.MouseDown> evento é usado para iniciar a operação de arrastar porque ela é a mais intuitiva (a maioria das ações de arrastar e soltar começa com o botão do mouse que está sendo pressionado). No entanto, lembre-se de que qualquer evento pode ser usado para iniciar um procedimento do tipo "arrastar e soltar".  
  
> [!NOTE]
> Determinados controles têm eventos específicos de arrastar personalizados. Os <xref:System.Windows.Forms.ListView> <xref:System.Windows.Forms.TreeView> controles e, por exemplo, têm um <xref:System.Windows.Forms.TreeView.ItemDrag> evento.  
  
#### <a name="to-start-a-drag-operation"></a>Para iniciar uma operação de arrastar  
  
1. No <xref:System.Windows.Forms.Control.MouseDown> evento para o controle em que o arrastar será iniciado, use o `DoDragDrop` método para definir os dados a serem arrastados e o efeito permitido ao arrastar terá. Para obter mais informações, consulte <xref:System.Windows.Forms.DragEventArgs.Data%2A> e <xref:System.Windows.Forms.DragEventArgs.AllowedEffect%2A>.  
  
     O exemplo a seguir mostra como iniciar uma operação de arrastar. O controle em que o arrastar começa é um <xref:System.Windows.Forms.Button> controle, os dados que estão sendo arrastados são a cadeia de caracteres que representa a <xref:System.Windows.Forms.Control.Text%2A> Propriedade do <xref:System.Windows.Forms.Button> controle e os efeitos permitidos estão sendo copiados ou movidos.  
  
    ```vb  
    Private Sub Button1_MouseDown(ByVal sender As Object, ByVal e As System.Windows.Forms.MouseEventArgs) Handles Button1.MouseDown  
       Button1.DoDragDrop(Button1.Text, DragDropEffects.Copy Or DragDropEffects.Move)  
    End Sub  
    ```  
  
    ```csharp  
    private void button1_MouseDown(object sender,
    System.Windows.Forms.MouseEventArgs e)  
    {  
       button1.DoDragDrop(button1.Text, DragDropEffects.Copy |
          DragDropEffects.Move);  
    }  
    ```  
  
    > [!NOTE]
    > Todos os dados podem ser usados como um parâmetro no `DoDragDrop` método; no exemplo acima, a <xref:System.Windows.Forms.Control.Text%2A> Propriedade do <xref:System.Windows.Forms.Button> controle foi usada (em vez de embutir em código um valor ou recuperar dados de um DataSet) porque a propriedade estava relacionada ao local que está sendo arrastado (o <xref:System.Windows.Forms.Button> controle). Lembre-se disso ao incorporar operações do tipo "arrastar e soltar" em seus aplicativos baseados em Windows.  
  
 Embora uma operação de arrastar esteja em vigor, você pode manipular o <xref:System.Windows.Forms.Control.QueryContinueDrag> evento, que "pede permissão" do sistema para continuar a operação de arrastar. Ao lidar com esse método, também é o ponto apropriado para você chamar métodos que terão um efeito na operação de arrastar, como expandir um <xref:System.Windows.Forms.TreeNode> em um <xref:System.Windows.Forms.TreeView> controle quando o cursor passar sobre ele.  
  
## <a name="dropping-data"></a>Descartando dados  
 Depois de começar a arrastar dados de um local em um Formulário ou controle do Windows, você naturalmente desejará soltá-los em algum lugar. O cursor mudará ao cruzar uma área de um formulário ou controle que esteja configurado corretamente para receber os dados soltados. Qualquer área dentro de um formulário ou controle do Windows pode ser feita para aceitar dados eliminados definindo a <xref:System.Windows.Forms.Control.AllowDrop%2A> propriedade e manipulando os <xref:System.Windows.Forms.Control.DragEnter> <xref:System.Windows.Forms.Control.DragDrop> eventos e.  
  
#### <a name="to-perform-a-drop"></a>Para executar uma operação de soltar  
  
1. Defina a <xref:System.Windows.Forms.Control.AllowDrop%2A> propriedade como true.  
  
2. No `DragEnter` caso do controle onde o descarte ocorrerá, verifique se os dados que estão sendo arrastados são de um tipo aceitável (nesse caso, <xref:System.Windows.Forms.Control.Text%2A> ). Em seguida, o código define o efeito que ocorrerá quando a ação de soltar ocorrer em um valor na <xref:System.Windows.Forms.DragDropEffects> enumeração. Para obter mais informações, consulte <xref:System.Windows.Forms.DragEventArgs.Effect%2A>.  
  
    ```vb  
    Private Sub TextBox1_DragEnter(ByVal sender As Object, ByVal e As System.Windows.Forms.DragEventArgs) Handles TextBox1.DragEnter  
       If (e.Data.GetDataPresent(DataFormats.Text)) Then  
         e.Effect = DragDropEffects.Copy  
       Else  
         e.Effect = DragDropEffects.None  
       End If  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_DragEnter(object sender,
    System.Windows.Forms.DragEventArgs e)  
    {  
       if (e.Data.GetDataPresent(DataFormats.Text))
          e.Effect = DragDropEffects.Copy;  
       else  
          e.Effect = DragDropEffects.None;  
    }  
    ```  
  
    > [!NOTE]
    > Você pode definir seu próprio <xref:System.Windows.Forms.DataFormats> especificando seu próprio objeto como o <xref:System.Object> parâmetro do <xref:System.Windows.Forms.DataObject.SetData%2A> método. Ao fazer isso, verifique se o objeto especificado é serializável. Para obter mais informações, consulte <xref:System.Runtime.Serialization.ISerializable>.  
  
3. No <xref:System.Windows.Forms.Control.DragDrop> caso do controle em que o drop ocorrerá, use o <xref:System.Windows.Forms.DataObject.GetData%2A> método para recuperar os dados que estão sendo arrastados. Para obter mais informações, consulte <xref:System.Security.Cryptography.Xml.DataObject.Data%2A>.  
  
     No exemplo a seguir, um <xref:System.Windows.Forms.TextBox> controle é o controle que está sendo arrastado para (onde o descarte ocorrerá). O código define a <xref:System.Windows.Forms.Control.Text%2A> Propriedade do <xref:System.Windows.Forms.TextBox> controle igual aos dados que estão sendo arrastados.  
  
    ```vb  
    Private Sub TextBox1_DragDrop(ByVal sender As Object, ByVal e As System.Windows.Forms.DragEventArgs) Handles TextBox1.DragDrop  
       TextBox1.Text = e.Data.GetData(DataFormats.Text).ToString  
    End Sub  
    ```  
  
    ```csharp  
    private void textBox1_DragDrop(object sender,
    System.Windows.Forms.DragEventArgs e)  
    {  
       textBox1.Text = e.Data.GetData(DataFormats.Text).ToString();  
    }  
    ```  
  
    > [!NOTE]
    > Além disso, você pode trabalhar com a <xref:System.Windows.Forms.DragEventArgs.KeyState%2A> propriedade, de modo que, dependendo das chaves pressionadas durante a operação de arrastar e soltar, certos efeitos ocorram (por exemplo, é padrão para copiar os dados arrastados quando a tecla CTRL é pressionada).  
  
## <a name="see-also"></a>Veja também

- [Como Adicionar Dados à Área de Transferência](how-to-add-data-to-the-clipboard.md)
- [Como Recuperar Dados da Área de Transferência](how-to-retrieve-data-from-the-clipboard.md)
- [Operações do Tipo "Arrastar e Soltar" e Suporte à Área de Transferência](drag-and-drop-operations-and-clipboard-support.md)
