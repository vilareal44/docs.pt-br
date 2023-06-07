---
title: Modelo de programação assíncrona (APM)
description: Saiba mais sobre o modelo de programação assíncrona (APM) no .NET. Descubra como iniciar e encerrar uma operação assíncrona.
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- ending asynchronous operations
- starting asynchronous operations
- beginning asynchronous operations
- asynchronous programming, ending operations
- asynchronous programming
- stopping asynchronous operations
- asynchronous programming, beginning operations
ms.assetid: c9b3501e-6bc6-40f9-8efd-4b6d9e39ccf0
ms.openlocfilehash: 5ab5d15d24aac80ef4a31c039f7af9dacce4a8d8
ms.sourcegitcommit: 5fd4696a3e5791b2a8c449ccffda87f2cc2d4894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/15/2020
ms.locfileid: "84769178"
---
# <a name="asynchronous-programming-model-apm"></a>Modelo de programação assíncrona (APM)
Uma operação assíncrona que utiliza o padrão de design <xref:System.IAsyncResult> é implementada como dois métodos chamados `BeginOperationName` e `EndOperationName` que começam e terminam a operação assíncrona *OperationName*, respectivamente. Por exemplo, a classe <xref:System.IO.FileStream> fornece os métodos <xref:System.IO.FileStream.BeginRead%2A> e <xref:System.IO.FileStream.EndRead%2A> para ler os bytes de um arquivo de forma assíncrona. Esses métodos implementam a versão assíncrona do método <xref:System.IO.FileStream.Read%2A>.  
  
> [!NOTE]
> A partir do .NET Framework 4, a Biblioteca de Paralelismo de Tarefas fornece um novo modelo de programação paralela e assíncrona. Para obter mais informações, consulte a [TPL (biblioteca paralela de tarefas)](../parallel-programming/task-parallel-library-tpl.md) e o [padrão assíncrono baseado em tarefa (toque)](task-based-asynchronous-pattern-tap.md)).  
  
 Depois de chamar `BeginOperationName`, um aplicativo pode continuar a executar as instruções no thread de chamada, enquanto a operação assíncrona ocorre em um thread diferente. Cada chamada para `BeginOperationName`, o aplicativo também deve chamar `EndOperationName` para obter os resultados da operação.  
  
## <a name="beginning-an-asynchronous-operation"></a>Começando uma operação assíncrona  
 O método `BeginOperationName` inicia a operação assíncrona *OperationName* e retorna um objeto que implementa a interface <xref:System.IAsyncResult>. Os objetos <xref:System.IAsyncResult> armazenam as informações sobre uma operação assíncrona. A tabela a seguir mostra informações sobre uma operação assíncrona.  
  
|Membro|Description|  
|------------|-----------------|  
|<xref:System.IAsyncResult.AsyncState%2A>|Um objeto específico do aplicativo opcional que contém informações sobre a operação assíncrona.|  
|<xref:System.IAsyncResult.AsyncWaitHandle%2A>|Um <xref:System.Threading.WaitHandle> que pode ser usado para bloquear a execução do aplicativo até que a operação assíncrona seja concluída.|  
|<xref:System.IAsyncResult.CompletedSynchronously%2A>|Um valor que indica se a operação assíncrona foi concluída no thread usado para chamar `BeginOperationName` em vez de concluir em um thread <xref:System.Threading.ThreadPool> separado.|  
|<xref:System.IAsyncResult.IsCompleted%2A>|Um valor que indica se a operação assíncrona foi concluída.|  
  
 Um método `BeginOperationName` usa qualquer parâmetro declarado na assinatura da versão síncrona do método que é passado por valor ou referência. Os parâmetros de saída não são parte da assinatura do método `BeginOperationName`. A assinatura do método `BeginOperationName` também inclui dois parâmetros adicionais. O primeiro deles define um delegado <xref:System.AsyncCallback> que faz referência a um método que é chamado quando a operação assíncrona é concluída. O chamador pode especificar `null` (`Nothing` no Visual Basic) se não desejar um método chamado quando a operação for concluída. O segundo parâmetro adicional é um objeto definido pelo usuário. Esse objeto pode ser usado para passar informações de estado específico do aplicativo para o método invocado quando a operação assíncrona é concluída. Se um método `BeginOperationName` usar parâmetros adicionais específicos da operação, como uma matriz de bytes para armazenar os bytes lidos de um arquivo, o <xref:System.AsyncCallback> e o objeto de estado do aplicativo serão os últimos parâmetros da assinatura de método `BeginOperationName`.  
  
 `BeginOperationName` retorna o controle para o thread de chamada imediatamente. Se o método `BeginOperationName` gerar exceções, as exceções serão geradas antes de a operação assíncrona ser iniciada. Se o método `BeginOperationName` gerar exceções, o método de retorno de chamada não será invocado.  
  
## <a name="ending-an-asynchronous-operation"></a>Encerrando uma operação assíncrona  
 O método `EndOperationName` termina a operação assíncrona *OperationName*. O valor retornado do método `EndOperationName` é do mesmo tipo retornado pela sua contraparte síncrona e é específico para a operação assíncrona. Por exemplo, o método <xref:System.IO.FileStream.EndRead%2A> retorna o número de bytes lidos de um <xref:System.IO.FileStream> e o método <xref:System.Net.Dns.EndGetHostByName%2A> retorna um objeto <xref:System.Net.IPHostEntry> que contém informações sobre um computador host. O método `EndOperationName` usa qualquer parâmetro declarado de referência na assinatura da versão síncrona do método. Além dos parâmetros do método síncrono, o método `EndOperationName` também inclui um parâmetro <xref:System.IAsyncResult>. Os chamadores devem passar a instância retornada pela chamada correspondente para `BeginOperationName`.  
  
 Se a operação assíncrona representada pelo objeto <xref:System.IAsyncResult> não tiver sido concluída quando `EndOperationName` for chamado, `EndOperationName` bloqueará o thread de chamada até que a operação assíncrona seja concluída. As exceções geradas pela operação assíncrona são geradas do método `EndOperationName`. O efeito da chamada para o método `EndOperationName` várias vezes com o mesmo <xref:System.IAsyncResult> não é definido. Da mesma forma, chamar o método `EndOperationName` com um <xref:System.IAsyncResult> que não foi retornado pelo método Begin relacionado também não será definido.  
  
> [!NOTE]
> Para qualquer um dos cenários indefinidos, os implementadores devem considerar a geração de <xref:System.InvalidOperationException>.  
  
> [!NOTE]
> Os implementadores deste padrão de design devem notificar o chamador de que a operação assíncrona está concluída, definindo <xref:System.IAsyncResult.IsCompleted%2A> como true, chamando o método de retorno de chamada assíncrono (se tiver sido especificado) e sinalizando o <xref:System.IAsyncResult.AsyncWaitHandle%2A>.  
  
 Os desenvolvedores de aplicativos tem várias opções de design para acessar os resultados da operação assíncrona. A opção correta depende se o aplicativo tem instruções que podem ser executadas enquanto a operação é concluída. Se um aplicativo não puder executar trabalho adicional até que ele receba os resultados da operação assíncrona, o aplicativo deverá ser bloqueado até que os resultados estejam disponíveis. Para bloquear até que uma operação assíncrona seja concluída, você poderá usar uma das seguintes abordagens:  
  
- Chamar `EndOperationName` do thread principal do aplicativo, bloqueando a execução do aplicativo até que a operação seja concluída. Veja um exemplo que ilustra essa técnica em [Bloqueando a execução de um aplicativo encerrando uma operação assíncrona](blocking-application-execution-by-ending-an-async-operation.md).  
  
- Use o <xref:System.IAsyncResult.AsyncWaitHandle%2A> para impedir a execução do aplicativo até que uma ou mais operações sejam concluídas. Veja um exemplo que ilustra essa técnica em [Bloqueando a execução de um aplicativo com um AsyncWaitHandle](blocking-application-execution-using-an-asyncwaithandle.md).  
  
 Os aplicativos que não precisarem ser bloqueados enquanto a operação assíncrona é concluída podem usar uma das seguintes abordagens:  
  
- Sondagem de status de conclusão da operação, verificando a propriedade <xref:System.IAsyncResult.IsCompleted%2A> periodicamente e chamando `EndOperationName` quando a operação for concluída. Veja um exemplo que ilustra essa técnica em [Sondagem do status de uma operação assíncrona](polling-for-the-status-of-an-asynchronous-operation.md).  
  
- Use um delegado <xref:System.AsyncCallback> para especificar um método a ser invocado quando a operação for concluída. Veja um exemplo que ilustra essa técnica em [Usar um representante AsyncCallback para finalizar uma operação assíncrona](using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md).  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Chamar métodos síncronos de forma assíncrona](calling-synchronous-methods-asynchronously.md)
- [Usando um representante AsyncCallback e um objeto de estado](using-an-asynccallback-delegate-and-state-object.md)
