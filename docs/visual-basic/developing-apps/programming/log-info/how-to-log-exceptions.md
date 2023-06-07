---
title: 'Como: registrar exceções em log'
ms.date: 07/20/2015
helpviewer_keywords:
- exceptions, logging
- exceptions, tracking
ms.assetid: a26c60e2-ae39-444a-aebb-33eccadc0eeb
ms.openlocfilehash: 59ed7b836126a38f32b7c6f479570a566d236e6c
ms.sourcegitcommit: f8c270376ed905f6a8896ce0fe25b4f4b38ff498
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84410108"
---
# <a name="how-to-log-exceptions-in-visual-basic"></a>Como registrar em log as exceções no Visual Basic

É possível usar os objetos `My.Application.Log` e `My.Log` para registrar em log informações sobre exceções que ocorrem em um aplicativo. Esses exemplos mostram como usar o método `My.Application.Log.WriteException` para registrar em log exceções capturadas explicitamente e exceções sem tratamento.  
  
 Para registrar informações de rastreamento de registros em log, use o método `My.Application.Log.WriteEntry`. Para obter mais informações, consulte <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>.  
  
### <a name="to-log-a-handled-exception"></a>Registrar em log uma exceção manipulada  
  
1. Crie o método que gerará as informações de exceção.  
  
     [!code-vb[VbVbalrMyApplicationLog#9](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#9)]  
  
2. Use um bloco `Try...Catch` para capturar a exceção.  
  
     [!code-vb[VbVbalrMyApplicationLog#6](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#6)]  
  
3. Coloque o código que poderá gerar uma exceção no bloco `Try`.  
  
     Remova a marca de comentário das linhas `Dim` e `MsgBox` para lançar uma exceção <xref:System.NullReferenceException>.  
  
     [!code-vb[VbVbalrMyApplicationLog#7](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#7)]  
  
4. No bloco `Catch`, use o método `My.Application.Log.WriteException` para gravar as informações de exceção.  
  
     [!code-vb[VbVbalrMyApplicationLog#8](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#8)]  
  
     O exemplo a seguir mostra o código completo para registrar em log uma exceção manipulada.  
  
     [!code-vb[VbVbalrMyApplicationLog#10](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/Form1.vb#10)]  
  
### <a name="to-log-an-unhandled-exception"></a>Registrar em log uma exceção sem tratamento  
  
1. Selecione um projeto no **Gerenciador de Soluções**. No menu **projeto** , escolha **Propriedades**.  
  
2. Clique na guia **Aplicativo**.  
  
3. Clique no botão **Exibir Eventos de Aplicativo** para abrir o Editor de Códigos.  
  
     Isso abrirá o arquivo ApplicationEvents.vb.  
  
4. Abra o arquivo ApplicationEvents.vb no Editor de Códigos. No menu **Geral**, escolha **Eventos MyApplication**.  
  
5. No menu **Declarações**, escolha **UnhandledException**.  
  
     A aplicativo gera o evento <xref:Microsoft.VisualBasic.ApplicationServices.WindowsFormsApplicationBase.UnhandledException> antes da execução do aplicativo principal.  
  
6. Adicione o método `My.Application.Log.WriteException` ao manipulador de eventos `UnhandledException`.  
  
     [!code-vb[VbVbalrMyApplicationLog#4](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/MyEventsFake.vb#4)]  
  
     O exemplo a seguir mostra o código completo para registrar em log uma exceção sem tratamento.  
  
     [!code-vb[VbVbalrMyApplicationLog#5](~/samples/snippets/visualbasic/VS_Snippets_VBCSharp/VbVbalrMyApplicationLog/VB/MyEventsFake.vb#5)]  
  
## <a name="see-also"></a>Confira também

- <xref:Microsoft.VisualBasic.Logging.Log?displayProperty=nameWithType>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteEntry%2A>
- <xref:Microsoft.VisualBasic.Logging.Log.WriteException%2A>
- [Trabalhar com logs do aplicativo](working-with-application-logs.md)
- [Como: gravar mensagens de log](how-to-write-log-messages.md)
- [Passo a passo: determinar o local no qual My.Application.Log grava informações](walkthrough-determining-where-my-application-log-writes-information.md)
- [Passo a passo: alterar o local no qual My.Application.Log grava informações](walkthrough-changing-where-my-application-log-writes-information.md)
