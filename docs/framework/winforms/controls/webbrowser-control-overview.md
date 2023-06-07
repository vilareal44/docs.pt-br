---
title: Visão geral do controle WebBrowser
description: Saiba como usar o controle WebBrowser para combinar diretamente controles da Web com controles de Windows Forms em um único aplicativo.
ms.date: 03/30/2017
f1_keywords:
- WebBrowser
helpviewer_keywords:
- WebBrowser control [Windows Forms], about
- Web pages [Windows Forms], displaying in applications
ms.assetid: 6e3e1cc2-9c48-4136-9659-e99e4e60b7e9
ms.openlocfilehash: 6a0548bb0f5905d8f848ab13fb82d32b50caa891
ms.sourcegitcommit: dc2feef0794cf41dbac1451a13b8183258566c0e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85325744"
---
# <a name="webbrowser-control-overview"></a>Visão geral do controle WebBrowser
O <xref:System.Windows.Forms.WebBrowser> controle fornece um wrapper gerenciado para o controle ActiveX do WebBrowser. O wrapper gerenciado permite exibir páginas da Web em seus aplicativos cliente dos Windows Forms. Você pode usar o <xref:System.Windows.Forms.WebBrowser> controle para duplicar a funcionalidade de navegação na Web do Internet Explorer em seu aplicativo ou pode desabilitar a funcionalidade padrão do Internet Explorer e usar o controle como um visualizador de documento HTML simples. Você também pode usar o controle para adicionar elementos de interface do usuário baseados em DHTML ao seu formulário e ocultar o fato de que eles estão hospedados no <xref:System.Windows.Forms.WebBrowser> controle. Essa abordagem permite combinar perfeitamente controles Web com controles dos Windows Forms em um único aplicativo.  
  
## <a name="frequently-used-properties-methods-and-events"></a>Propriedades, métodos e eventos usados com frequência  
 O <xref:System.Windows.Forms.WebBrowser> controle tem várias propriedades, métodos e eventos que você pode usar para implementar controles encontrados no Internet Explorer. Por exemplo, você pode usar o método `Navigate` para implementar uma barra de endereços e os métodos `GoBack`, `GoForward`, `Stop` e `Refresh` para implementar os botões de navegação em uma barra de ferramentas. Você pode manipular o evento `Navigated` para atualizar a barra de endereços com o valor da propriedade `Url` e a barra de título com o valor da propriedade `DocumentTitle`.  
  
 Se você quiser gerar seu próprio conteúdo de página dentro de seu aplicativo, defina a propriedade `DocumentText`. Se você estiver familiarizado com o DOM (Modelo de Objeto do Documento) HTML, também poderá manipular o conteúdo da página da Web por meio da propriedade `Document`. Com essa propriedade, você pode armazenar e modificar documentos na memória, em vez de navegar entre arquivos.  
  
 A propriedade `Document` também permite que você chame os métodos implementados no código de script da página da Web do seu código de aplicativo cliente. Para acessar o código do aplicativo cliente do seu código de script, defina a propriedade `ObjectForScripting`. O objeto especificado pode ser acessado pelo seu código de script como o objeto `window.external`.  
  
|Nome|Descrição|  
|----------|-----------------|  
|Propriedade <xref:System.Windows.Forms.WebBrowser.Document%2A>|Obtém um objeto que fornece acesso gerenciado para DOM (Modelo de Objeto do Documento) HTML da página da Web atual.|  
|Evento <xref:System.Windows.Forms.WebBrowser.DocumentCompleted>|Ocorre quando uma página da Web conclui o carregamento.|  
|Propriedade <xref:System.Windows.Forms.WebBrowser.DocumentText%2A>|Obtém ou define o conteúdo HTML da página da Web atual.|  
|Propriedade <xref:System.Windows.Forms.WebBrowser.DocumentTitle%2A>|Obtém o título da página da Web atual.|  
|Método <xref:System.Windows.Forms.WebBrowser.GoBack%2A>|Navega para a página anterior no histórico.|  
|Método <xref:System.Windows.Forms.WebBrowser.GoForward%2A>|Navega para a próxima página no histórico.|  
|Método <xref:System.Windows.Forms.WebBrowser.Navigate%2A>|Navega para a URL especificada.|  
|Evento <xref:System.Windows.Forms.WebBrowser.Navigating>|Ocorre antes do início de navegação, permitindo que a ação seja cancelada.|  
|Propriedade <xref:System.Windows.Forms.WebBrowser.ObjectForScripting%2A>|Obtém ou define um objeto que o código de script da página da Web pode usar para se comunicar com o aplicativo.|  
|Método <xref:System.Windows.Forms.WebBrowser.Print%2A>|Imprime a página da Web atual.|  
|Método <xref:System.Windows.Forms.WebBrowser.Refresh%2A>|Recarrega a página da Web atual.|  
|Método <xref:System.Windows.Forms.WebBrowser.Stop%2A>|Interrompe a navegação atual e para elementos de página dinâmicos, como sons e animação.|  
|Propriedade <xref:System.Windows.Forms.WebBrowser.Url%2A>|Obtém ou define a URL da página da Web atual. Configurar essa propriedade leva o controle para a nova URL.|  
  
## <a name="see-also"></a>Veja também

- <xref:System.Windows.Forms.WebBrowser>
- <xref:System.Windows.Forms.WebBrowserDocumentCompletedEventArgs>
- <xref:System.Windows.Forms.WebBrowserDocumentCompletedEventHandler>
- <xref:System.Windows.Forms.WebBrowserEncryptionLevel>
- <xref:System.Windows.Forms.WebBrowserNavigatedEventArgs>
- <xref:System.Windows.Forms.WebBrowserNavigatedEventHandler>
- <xref:System.Windows.Forms.WebBrowserNavigatingEventArgs>
- <xref:System.Windows.Forms.WebBrowserNavigatingEventHandler>
- <xref:System.Windows.Forms.WebBrowserProgressChangedEventArgs>
- <xref:System.Windows.Forms.WebBrowserReadyState>
- <xref:System.Windows.Forms.WebBrowserRefreshOption>
- [Como navegar até uma URL com o controle WebBrowser](how-to-navigate-to-a-url-with-the-webbrowser-control.md)
- [Como imprimir com um controle WebBrowser](how-to-print-with-a-webbrowser-control.md)
- [Como adicionar recursos do navegador da Web a um Aplicativo do Windows Forms](how-to-add-web-browser-capabilities-to-a-windows-forms-application.md)
- [Como criar um visualizador de documento HTML em um Aplicativo dos Windows Forms](how-to-create-an-html-document-viewer-in-a-windows-forms-application.md)
- [Como implementar a comunicação bidirecional entre o código DHTML e o código do aplicativo cliente](implement-two-way-com-between-dhtml-and-client.md)
- [Segurança de WebBrowser](webbrowser-security.md)
