---
title: 'Como: criar manipuladores de eventos em tempo de execução'
description: Saiba como criar um manipulador de eventos em tempo de execução com o Designer de Formulários do Windows no Visual Studio. Essa ação permite que você conecte manipuladores de eventos em tempo de execução.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
- cpp
helpviewer_keywords:
- Windows Forms, event handling
- event handlers [Windows Forms], creating
- run time [Windows Forms], creating event handlers at
- examples [Windows Forms], event handling
- Button control [Windows Forms], event handlers
ms.assetid: 2e7c9e1a-61fe-444d-8113-3c5bacf1c8cb
ms.openlocfilehash: 857076c46377b3276154d9b193d4bbe51841828f
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325799"
---
# <a name="how-to-create-event-handlers-at-run-time-for-windows-forms"></a>Como: criar manipuladores de eventos em tempo de execução para o Windows Forms

Além de criar eventos usando o Designer de Formulários do Windows no Visual Studio, você também pode criar um manipulador de eventos em tempo de execução. Essa ação permite que você conecte manipuladores de eventos com base em condições no código no tempo de execução em vez de conectá-los quando o programa inicia.

## <a name="create-an-event-handler-at-run-time"></a>Criar um manipulador de eventos em tempo de execução

1. Abra o formulário ao qual você deseja adicionar um manipulador de eventos.

2. Adicione um método ao seu formulário com a assinatura do método para o evento que deseja manipular.

     Por exemplo, se você estivesse manipulando o <xref:System.Windows.Forms.Control.Click> evento de um <xref:System.Windows.Forms.Button> controle, você criaria um método como o seguinte:

    ```vb
    Private Sub Button1_Click(ByVal sender As Object, ByVal e As EventArgs)
       ' Add event handler code here.
    End Sub
    ```

    ```csharp
    private void button1_Click(object sender, System.EventArgs e)
    {
    // Add event handler code here.
    }
    ```

    ```cpp
    private:
       void button1_Click(System::Object ^ sender,
          System::EventArgs ^ e)
       {
          // Add event handler code here.
       }
    ```

3. Adicione código ao manipulador de eventos conforme apropriado para seu aplicativo.

4. Determine para qual formulário ou controle deseja criar um manipulador de eventos.

5. Em um método na classe do formulário, adicione o código que especifica o manipulador de eventos para manipular o evento. Por exemplo, o código a seguir especifica o manipulador `button1_Click` de eventos que manipula o <xref:System.Windows.Forms.Control.Click> evento de um <xref:System.Windows.Forms.Button> controle:

    ```vb
    AddHandler Button1.Click, AddressOf Button1_Click
    ```

    ```csharp
    button1.Click += new EventHandler(button1_Click);
    ```

    ```cpp
    button1->Click += gcnew System::EventHandler(this, &Form1::button1_Click);
    ```

     O <xref:System.ComponentModel.EventHandlerList.AddHandler%2A> método demonstrado no código de Visual Basic acima estabelece um manipulador de eventos de clique para o botão.

## <a name="see-also"></a>Veja também

- [Criando manipuladores de eventos no Windows Forms](creating-event-handlers-in-windows-forms.md)
- [Visão geral de manipuladores de eventos](event-handlers-overview-windows-forms.md)
- [Solucionando problemas de manipuladores de eventos herdados no Visual Basic](../../visual-basic/programming-guide/language-features/events/troubleshooting-inherited-event-handlers.md)
