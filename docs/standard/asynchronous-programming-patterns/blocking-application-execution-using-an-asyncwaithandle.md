---
title: Bloqueando a execução de aplicativos com um AsyncWaitHandle
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- blocks, asynchronous operations
- ending asynchronous operations
- asynchronous programming, ending operations
- asynchronous programming, blocking applications
- stopping asynchronous operations
- blocking application execution
ms.assetid: 3e32daf2-8161-4e8f-addd-9fd9ff101b03
ms.openlocfilehash: 6184e52c3f6e39e452331af27b83520e498062aa
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289922"
---
# <a name="blocking-application-execution-using-an-asyncwaithandle"></a>Bloqueando a execução de aplicativos com um AsyncWaitHandle
Aplicativos que não continuem a executar outras tarefas enquanto aguardam os resultados de uma operação assíncrona devem bloquear até que a operação seja concluída. Use uma das opções a seguir para bloquear o thread principal do aplicativo ao aguardar a conclusão de uma operação assíncrona:  
  
- Use a <xref:System.IAsyncResult.AsyncWaitHandle%2A> Propriedade do <xref:System.IAsyncResult> retornada pelo método **begin**_OperationName_ da operação assíncrona. Esta abordagem será demonstrada neste tópico.  
  
- Chame o método **End**_OperationName_ da operação assíncrona. Veja um exemplo que demonstra essa abordagem em [Bloqueando a execução de um aplicativo ao encerrar uma operação assíncrona](blocking-application-execution-by-ending-an-async-operation.md).  
  
 Aplicativos que usam um ou mais objetos <xref:System.Threading.WaitHandle> para bloquear até a conclusão de uma operação assíncrona, tipicamente chamam o método **Begin**_OperationName_, executam qualquer trabalho que possa ser feito sem os resultados da operação e, em seguida, bloqueiam até a conclusão das operações assíncronas. Um aplicativo pode bloquear em uma única operação ao chamar um dos métodos <xref:System.Threading.WaitHandle.WaitOne%2A> usando o <xref:System.IAsyncResult.AsyncWaitHandle%2A>. Para bloquear enquanto espera a conclusão de um conjunto de operações assíncronas, armazene os objetos <xref:System.IAsyncResult.AsyncWaitHandle%2A> associados em uma matriz e chame um dos métodos <xref:System.Threading.WaitHandle.WaitAll%2A>. Para bloquear enquanto espera a conclusão de um conjunto de operações assíncronas, armazene os objetos <xref:System.IAsyncResult.AsyncWaitHandle%2A> associados em uma matriz e chame um dos métodos <xref:System.Threading.WaitHandle.WaitAny%2A>.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como usar métodos assíncronos na classe DNS para recuperar informações do Sistema de Nomes de Domínio de computadores especificados pelo usuário. O exemplo demonstra o bloqueio usando o <xref:System.Threading.WaitHandle> associado com a operação assíncrona. Observe que `null` (`Nothing` no Visual Basic) é passado para os parâmetros <xref:System.Net.Dns.BeginGetHostByName%2A>`requestCallback` e `stateObject` por não serem necessários ao usar essa abordagem.  
  
 [!code-csharp[AsyncDesignPattern#2](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/Async_EndBlockWait.cs#2)]
 [!code-vb[AsyncDesignPattern#2](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/Async_EndBlockWait.vb#2)]  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
