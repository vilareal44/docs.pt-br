---
title: Como chamar operações de serviço do WCF de maneira assíncrona
description: Saiba como criar um cliente WCF que pode acessar uma operação de serviço de forma assíncrona usando o modelo de chamada assíncrona controlado por evento.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 0face17f-43ca-417b-9b33-737c0fc360df
ms.openlocfilehash: aa31f64473111800f4cd01907a0446c94f368456
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85247228"
---
# <a name="how-to-call-wcf-service-operations-asynchronously"></a>Como chamar operações de serviço do WCF de maneira assíncrona

Este artigo aborda como um cliente pode acessar uma operação de serviço de forma assíncrona. O serviço neste artigo implementa a `ICalculator` interface. O cliente pode chamar as operações nessa interface de forma assíncrona usando o modelo de chamada assíncrona controlado por evento. (Para obter mais informações sobre o modelo de chamada assíncrona baseado em evento, consulte [programação multi-threaded com o padrão assíncrono baseado em evento](../../../standard/asynchronous-programming-patterns/event-based-asynchronous-pattern-eap.md)). Para obter um exemplo que mostra como implementar uma operação de forma assíncrona em um serviço, consulte [como implementar uma operação de serviço assíncrono](../how-to-implement-an-asynchronous-service-operation.md). Para obter mais informações sobre operações síncronas e assíncronas, consulte operações síncronas [e](../synchronous-and-asynchronous-operations.md)assíncronas.  
  
> [!NOTE]
> Não há suporte para o modelo de chamada assíncrona controlado por evento ao usar um <xref:System.ServiceModel.ChannelFactory%601> . Para obter informações sobre como fazer chamadas assíncronas usando o <xref:System.ServiceModel.ChannelFactory%601> , consulte [como: chamar operações de forma assíncrona usando uma fábrica de canais](how-to-call-operations-asynchronously-using-a-channel-factory.md).  
  
## <a name="procedure"></a>Procedimento  
  
#### <a name="to-call-wcf-service-operations-asynchronously"></a>Para chamar operações do serviço WCF de forma assíncrona  
  
1. Execute a ferramenta [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) com as `/async` Opções de comando e e `/tcv:Version35` em conjunto, conforme mostrado no comando a seguir.  
  
    ```console
    svcutil /n:http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples http://localhost:8000/servicemodelsamples/service/mex /a /tcv:Version35  
    ```  
  
     Isso gera, além das operações assíncronas padrão síncronas e baseadas em delegados, uma classe de cliente WCF que contém:  
  
    - Duas `operationName` > `Async` operações <para uso com a abordagem de chamada assíncrona baseada em evento. Por exemplo:  
  
         [!code-csharp[EventAsync#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#1)]
         [!code-vb[EventAsync#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#1)]  
  
    - A operação conclui eventos do formulário <`operationName` > `Completed` para uso com a abordagem de chamada assíncrona baseada em evento. Por exemplo:  
  
         [!code-csharp[EventAsync#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#2)]
         [!code-vb[EventAsync#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#2)]  
  
    - <xref:System.EventArgs?displayProperty=nameWithType>tipos para cada operação (do formulário <`operationName` > `CompletedEventArgs` ) para uso com a abordagem de chamada assíncrona baseada em evento. Por exemplo:  
  
         [!code-csharp[EventAsync#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/generatedclient.cs#3)]
         [!code-vb[EventAsync#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/generatedclient.vb#3)]  
  
2. No aplicativo de chamada, crie um método de retorno de chamada a ser chamado quando a operação assíncrona for concluída, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[EventAsync#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#4)]
     [!code-vb[EventAsync#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#4)]  
  
3. Antes de chamar a operação, use um novo genérico <xref:System.EventHandler%601?displayProperty=nameWithType> do tipo <`operationName` > `EventArgs` para adicionar o método de manipulador (criado na etapa anterior) ao evento <`operationName` > `Completed` . Em seguida, chame o `operationName` > `Async` método <. Por exemplo:  
  
     [!code-csharp[EventAsync#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#5)]
     [!code-vb[EventAsync#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#5)]  
  
## <a name="example"></a>Exemplo  
  
> [!NOTE]
> As diretrizes de design do modelo assíncrono baseado em eventos declaram que, se mais de um valor for retornado, um valor será retornado como a propriedade `Result` e os outros serão retornados como propriedades no objeto <xref:System.EventArgs>. Um dos resultados disso é que, se um cliente importar metadados usando as opções de comando assíncrono baseadas em eventos e a operação retornar mais de um valor, o <xref:System.EventArgs> objeto padrão retornará um valor como a `Result` propriedade e o restante serão propriedades do <xref:System.EventArgs> objeto. Se você quiser receber o objeto Message como a `Result` propriedade e tiver os valores retornados como propriedades nesse objeto, use a opção de `/messageContract` comando. Isso gera uma assinatura que retorna a mensagem de resposta como a propriedade `Result` no objeto <xref:System.EventArgs>. Todos os valores de retorno internos são propriedades do objeto da mensagem de resposta.  
  
 [!code-csharp[EventAsync#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/eventasync/cs/client.cs#6)]
 [!code-vb[EventAsync#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/eventasync/vb/client.vb#6)]  
  
## <a name="see-also"></a>Veja também

- [Como implementar uma operação de serviço assíncrona](../how-to-implement-an-asynchronous-service-operation.md)
