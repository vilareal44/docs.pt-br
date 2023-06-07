---
title: Como consumir eventos em um aplicativo Web Forms
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- events [.NET Framework], Web Forms
- Web Forms controls, and events
- event handlers [.NET Framework], Web Forms
- events [.NET Framework], consuming
- Web Forms, event handling
ms.assetid: 73bf8638-c4ec-4069-b0bb-a1dc79b92e32
ms.openlocfilehash: 3490b6fb89bfe6d7ac778078f58381bb5172e2fe
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84288479"
---
# <a name="how-to-consume-events-in-a-web-forms-application"></a>Como consumir eventos em um aplicativo Web Forms
Um cenário comum em aplicativos de Formulários Web do ASP.NET é popular uma página da Web com controles e executar uma ação específica com base no controle em que o usuário clica. Por exemplo, um controle <xref:System.Web.UI.WebControls.Button?displayProperty=nameWithType> gera um evento quando o usuário clica na página da Web. Ao manipular o evento, o aplicativo pode executar a lógica de aplicativo apropriada para esse clique do botão.  
  
### <a name="to-handle-a-button-click-event-on-a-webpage"></a>Para tratar um evento de clique de botão em um página da Web  
  
1. Crie uma página de Formulários Web do ASP.NET (página da Web) que tem um controle <xref:System.Web.UI.WebControls.Button> com o valor `OnClick` definido para o nome do método que você definirá na próxima etapa.  
  
    ```xml  
    <asp:Button ID="Button1" runat="server" Text="Click Me" OnClick="Button1_Click" />  
    ```  
  
2. Defina um manipulador de eventos que corresponda à assinatura de representante de evento <xref:System.Web.UI.WebControls.Button.Click> e que tenha o nome que você definiu para o valor `OnClick`.  
  
    ```csharp  
    protected void Button1_Click(object sender, EventArgs e)  
    {  
        // perform action  
    }  
    ```  
  
    ```vb  
    Protected Sub Button1_Click(sender As Object, e As EventArgs) Handles Button1.Click  
        ' perform action  
    End Sub  
    ```  
  
     O evento <xref:System.Web.UI.WebControls.Button.Click> usa a classe <xref:System.EventHandler> para o tipo de representante e a classe <xref:System.EventArgs> para os dados do evento. A estrutura da página ASP.NET gera automaticamente o código que cria uma instância de <xref:System.EventHandler> e adiciona essa instância ao evento <xref:System.Web.UI.WebControls.Button.Click> da instância <xref:System.Web.UI.WebControls.Button>.  
  
3. No método do manipulador de eventos que você definiu na etapa 2, adicione código para executar as ações que são necessárias quando o evento ocorre.  
  
## <a name="see-also"></a>Veja também

- [Eventos](index.md)
