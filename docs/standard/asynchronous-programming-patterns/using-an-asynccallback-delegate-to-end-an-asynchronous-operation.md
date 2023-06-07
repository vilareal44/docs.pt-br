---
title: Usando um delegado AsyncCallback para finalizar uma operação assíncrona
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- ending asynchronous operations
- asynchronous programming, ending operations
- AsyncCallback delegate
- stopping asynchronous operations
ms.assetid: 9d97206c-8917-406c-8961-7d0909d84eeb
ms.openlocfilehash: 8766e64c52688e820d0eb6a259e39926555d97bd
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276343"
---
# <a name="using-an-asynccallback-delegate-to-end-an-asynchronous-operation"></a>Usando um delegado AsyncCallback para finalizar uma operação assíncrona
Os aplicativos que podem executar outras tarefas enquanto aguardam os resultados de uma operação assíncrona não devem bloquear a espera até que a operação seja concluída. Use uma das opções a seguir para continuar a execução das instruções ao aguardar a conclusão de uma operação assíncrona:  
  
- Use um representante do <xref:System.AsyncCallback> para processar os resultados da operação assíncrona em um thread separado. Esta abordagem será demonstrada neste tópico.  
  
- Use a propriedade <xref:System.IAsyncResult.IsCompleted%2A> do <xref:System.IAsyncResult> retornado do método **Begin**_OperationName_ da operação assíncrona para determinar se a operação foi concluída. Veja um exemplo que demonstra essa abordagem em [Sondagem do status de uma operação assíncrona](polling-for-the-status-of-an-asynchronous-operation.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como usar métodos assíncronos na classe <xref:System.Net.Dns> para recuperar informações do DNS (Sistema de Nomes de Domínio) de computadores especificados pelo usuário. Este exemplo cria um representante <xref:System.AsyncCallback> que referencia o método `ProcessDnsInformation`. Esse método é chamado uma vez para cada solicitação assíncrona para obter informações de DNS.  
  
 Observe que o host especificado pelo usuário é passado para o parâmetro <xref:System.Net.Dns.BeginGetHostByName%2A><xref:System.Object>. Para obter um exemplo que demonstra a definição e uso de um objeto de estado mais complexo, veja [Usar um representante AsyncCallback e um objeto de estado](using-an-asynccallback-delegate-and-state-object.md).  
  
 [!code-csharp[AsyncDesignPattern#4](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/AsyncDelegateNoStateObject.cs#4)]
 [!code-vb[AsyncDesignPattern#4](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/AsyncDelegateNoState.vb#4)]  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
- [Chamando métodos assíncronos usando IAsyncResult](calling-asynchronous-methods-using-iasyncresult.md)
- [Usando um representante AsyncCallback e um objeto de estado](using-an-asynccallback-delegate-and-state-object.md)
