---
title: Bloqueando a execução de um aplicativo finalizando uma operação assíncrona
ms.date: 03/30/2017
ms.technology: dotnet-standard
helpviewer_keywords:
- blocks, asynchronous operations
- AsyncWaitHandle property
- asynchronous programming, blocking applications
- blocking application execution
ms.assetid: cc5e2834-a65b-4df8-b750-7bdb79997fee
dev_langs:
- csharp
- vb
ms.openlocfilehash: 74976176acc0fbb948c514358b7bd323cc20c134
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84289948"
---
# <a name="blocking-application-execution-by-ending-an-async-operation"></a>Bloqueando a execução de um aplicativo finalizando uma operação assíncrona
Aplicativos que não continuem a executar outras tarefas enquanto aguardam os resultados de uma operação assíncrona devem bloquear até que a operação seja concluída. Use uma das opções a seguir para bloquear o thread principal do aplicativo ao aguardar a conclusão de uma operação assíncrona:  
  
- Chame o método_OperationName_ de **end**de operações assíncronas. Esta abordagem será demonstrada neste tópico.  
  
- Use a <xref:System.IAsyncResult.AsyncWaitHandle%2A> Propriedade do <xref:System.IAsyncResult> retornada pelo método **begin**_OperationName_ da operação assíncrona. Para obter um exemplo que demonstra essa abordagem, consulte [Bloqueando a execução de um aplicativo com um AsyncWaitHandle](blocking-application-execution-using-an-asyncwaithandle.md).  
  
 Os aplicativos que usarem o método **End**_OperationName_ para bloqueio até a conclusão de uma operação assíncrona normalmente chamarão o método **Begin**_OperationName_, executarão qualquer trabalho que possa ser feito sem os resultados da operação e, em seguida, chamarão **End**_OperationName_.  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como usar métodos assíncronos na classe <xref:System.Net.Dns> para recuperar informações do Sistema de Nomes de Domínio de computadores especificados pelo usuário. Observe que `null` (`Nothing` no Visual Basic) é passado para os parâmetros <xref:System.Net.Dns.BeginGetHostByName%2A>`requestCallback` e `stateObject` porque esses argumentos não são necessários ao usar essa abordagem.  
  
 [!code-csharp[AsyncDesignPattern#1](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/Async_EndBlock.cs#1)]
 [!code-vb[AsyncDesignPattern#1](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/Async_EndBlock.vb#1)]  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
