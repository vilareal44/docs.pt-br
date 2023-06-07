---
title: 'Como: registrar mensagens em log quando o aplicativo é iniciado ou encerrado'
ms.date: 07/20/2015
helpviewer_keywords:
- event logs, shutdown
- My.Application.Log object, logging
- Startup event [Visual Basic]
- event logs, startup
- Shutdown event [Visual Basic]
- My.Log object, logging
ms.assetid: 67624d05-cddf-48b7-8c36-5c99baa4c621
ms.openlocfilehash: ac5fb17e527bcbcb55f98ec0ced06c152555ce6c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410069"
---
# <a name="how-to-log-messages-when-the-application-starts-or-shuts-down-visual-basic"></a>Como registrar mensagens em log quando o aplicativo é iniciado ou encerrado (Visual Basic)

É possível usar os objetos `My.Application.Log` e `My.Log` para registrar em log as informações sobre eventos que ocorrem em seu aplicativo. Este exemplo mostra como usar o método `My.Application.Log.WriteEntry` com os eventos `Startup` e `Shutdown` para gravar informações de rastreamento.  
  
### <a name="to-access-the-applications-event-handler-code"></a>Para acessar o código do manipulador de eventos do aplicativo  
  
1. Selecione um projeto no **Gerenciador de Soluções**. No menu **projeto** , escolha **Propriedades**.  
  
2. Clique na guia **Aplicativo**.  
  
3. Clique no botão **Exibir Eventos de Aplicativo** para abrir o Editor de Códigos.  
  
     Isso abrirá o arquivo ApplicationEvents.vb.  
  
### <a name="to-log-messages-when-the-application-starts"></a>Para registrar mensagens em log quando o aplicativo é iniciado  
  
1. Abra o arquivo ApplicationEvents.vb no Editor de Códigos. No menu **Geral**, escolha **Eventos MyApplication**.  
  
2. No menu **Declarações**, escolha **Inicialização**.  
  
     A aplicativo gera o evento <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Startup> antes da execução do aplicativo principal.  
  
3. Adicione o método `My.Application.Log.WriteEntry` ao manipulador de eventos `Startup`.  
  
     [!code-vb[VbVbalrMyApplicationLog#1](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/MyEventsFake.vb#1)]  
  
### <a name="to-log-messages-when-the-application-shuts-down"></a>Para registrar mensagens em log quando o aplicativo é desligado  
  
1. Abra o arquivo ApplicationEvents.vb no Editor de Códigos. No menu **Geral**, escolha **Eventos MyApplication**.  
  
2. No menu **Declarações**, escolha **Desligamento**.  
  
     A aplicativo gera o evento <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.Shutdown> após a execução do aplicativo principal, mas antes de desligar.  
  
3. Adicione o método `My.Application.Log.WriteEntry` ao manipulador de eventos `Shutdown`.  
  
     [!code-vb[VbVbalrMyApplicationLog#2](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/MyEventsFake.vb#2)]  
  
## <a name="example"></a>Exemplo  

 Você pode usar o **Designer de Projeto** para acessar os eventos do aplicativo no Editor de Códigos. Para obter mais informações, consulte [Página de aplicativo, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic).  
  
 [!code-vb[VbVbalrMyApplicationLog#3](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/MyEventsFake.vb#3)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [Página de Aplicativo, Designer de Projeto (Visual Basic)](/visualstudio/ide/reference/application-page-project-designer-visual-basic)
- [Trabalhar com logs do aplicativo](working-with-application-logs.md)
