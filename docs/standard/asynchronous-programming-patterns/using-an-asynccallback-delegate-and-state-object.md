---
title: Usando um delegado AsyncCallback e um objeto de estado
ms.date: 03/30/2017
ms.technology: dotnet-standard
dev_langs:
- csharp
- vb
helpviewer_keywords:
- asynchronous programming, delegates
- AsyncCallback delegate, samples
- asynchronous programming, state objects
- IAsyncResult interface, samples
ms.assetid: e3e5475d-c5e9-43f0-928e-d18df8ca1f1d
ms.openlocfilehash: e52ed550510253aba9401931c0f612211c7d1bf5
ms.sourcegitcommit: 33deec3e814238fb18a49b2a7e89278e27888291
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/02/2020
ms.locfileid: "84276420"
---
# <a name="using-an-asynccallback-delegate-and-state-object"></a>Usando um delegado AsyncCallback e um objeto de estado
Ao usar um representante <xref:System.AsyncCallback> para processar os resultados da operação assíncrona em um thread separado, você pode usar um objeto de estado para passar informações entre os retornos de chamada e recuperar um resultado final. Este tópico demonstra essa prática expandindo o exemplo em [Usando um representante AsyncCallback para finalizar uma operação assíncrona](using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir demonstra como usar métodos assíncronos na classe <xref:System.Net.Dns> para recuperar informações do DNS (Sistema de Nomes de Domínio) de computadores especificados pelo usuário. Este exemplo define e usa a classe `HostRequest` para armazenar informações de estado. Um objeto `HostRequest` é criado para cada nome de computador inserido pelo usuário. Este objeto é passado para o método <xref:System.Net.Dns.BeginGetHostByName%2A>. O método `ProcessDnsInformation` é chamado sempre que uma solicitação é concluída. O objeto `HostRequest` é recuperado usando a propriedade <xref:System.IAsyncResult.AsyncState%2A>. O método `ProcessDnsInformation` usa o objeto `HostRequest` para armazenar o retornado <xref:System.Net.IPHostEntry> pela solicitação ou um <xref:System.Net.Sockets.SocketException> lançado pela solicitação. Quando todas as solicitações são concluídas, o aplicativo faz a iteração pelos objetos `HostRequest` e exibe as informações de DNS ou a mensagem de erro <xref:System.Net.Sockets.SocketException>.  
  
 [!code-csharp[AsyncDesignPattern#5](../../../samples/snippets/csharp/VS_Snippets_CLR/AsyncDesignPattern/CS/AsyncDelegateWithStateObject.cs#5)]
 [!code-vb[AsyncDesignPattern#5](../../../samples/snippets/visualbasic/VS_Snippets_CLR/AsyncDesignPattern/VB/AsyncDelegateWithStateObject.vb#5)]  
  
## <a name="see-also"></a>Veja também

- [EAP (Padrão Assíncrono baseado em Evento)](event-based-asynchronous-pattern-eap.md)
- [Visão geral do padrão assíncrono baseado em evento](event-based-asynchronous-pattern-overview.md)
- [Usando um delegado AsyncCallback para finalizar uma operação assíncrona](using-an-asynccallback-delegate-to-end-an-asynchronous-operation.md)
