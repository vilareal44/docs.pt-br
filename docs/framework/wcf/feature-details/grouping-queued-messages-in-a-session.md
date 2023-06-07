---
title: Agrupamento de mensagens em fila em uma sessão
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- queues [WCF]. grouping messages
ms.assetid: 63b23b36-261f-4c37-99a2-cc323cd72a1a
ms.openlocfilehash: 66f51267f20f8cdad8feeedf37435ccfa733146e
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597352"
---
# <a name="grouping-queued-messages-in-a-session"></a>Agrupamento de mensagens em fila em uma sessão
O Windows Communication Foundation (WCF) fornece uma sessão que permite agrupar um conjunto de mensagens relacionadas para processamento por um único aplicativo de recebimento. As mensagens que fazem parte de uma sessão devem fazer parte da mesma transação. Como todas as mensagens fazem parte da mesma transação, se uma mensagem não for processada, toda a sessão será revertida. As sessões têm comportamentos semelhantes em relação a filas de mensagens mortas e filas suspeitas. A propriedade time to Live (TTL) definida em uma associação enfileirada configurada para sessões é aplicada à sessão como um todo. Se apenas algumas das mensagens na sessão forem enviadas antes do tempo de vida expirar, toda a sessão será colocada na fila de mensagens mortas. Da mesma forma, quando as mensagens em uma sessão falham ao serem enviadas a um aplicativo da fila de aplicativos, toda a sessão é colocada na fila de suspeitas (se disponível).  
  
## <a name="message-grouping-example"></a>Exemplo de agrupamento de mensagens  
 Um exemplo em que o agrupamento de mensagens é útil é ao implementar um aplicativo de processamento de pedidos como um serviço WCF. Por exemplo, um cliente envia um pedido para esse aplicativo que contém vários itens. Para cada item, o cliente faz uma chamada para o serviço, o que resulta em uma mensagem separada sendo enviada. É possível que o forneça um para receber o primeiro item e o servidor B receba o segundo item. Cada vez que um item é adicionado, o servidor que processa esse item tem que localizar a ordem apropriada e adicionar o item a ele, o que é altamente ineficiente. Você ainda pode se deparar com essas ineficiências apenas com um único servidor manipulando todas as solicitações, porque o servidor deve manter o controle de todos os pedidos que estão sendo processados no momento e determinar a qual deles o novo item pertence. O agrupamento de todas as solicitações de um único pedido simplifica muito a implementação desse aplicativo. O aplicativo cliente envia todos os itens para uma única ordem em uma sessão, portanto, quando o serviço processa o pedido, ele processa toda a sessão ao mesmo tempo. \  
  
## <a name="procedures"></a>Procedimentos  
  
#### <a name="to-set-up-a-service-contract-to-use-sessions"></a>Para configurar um contrato de serviço para usar sessões  
  
1. Defina um contrato de serviço que exija uma sessão. Faça isso com o <xref:System.ServiceModel.ServiceContractAttribute> atributo especificando:  
  
    ```csharp
    SessionMode=SessionMode.Required  
    ```  
  
2. Marque as operações no contrato como unidirecional, pois esses métodos não retornam nada. Isso é feito com o <xref:System.ServiceModel.OperationContractAttribute> atributo especificando:  
  
    ```csharp  
    [OperationContract(IsOneWay = true)]  
    ```  
  
3. Implemente o contrato de serviço e especifique um <xref:System.ServiceModel.ServiceBehaviorAttribute.InstanceContextMode> de <xref:System.ServiceModel.InstanceContextMode.PerSession?displayProperty=nameWithType> . Isso instancia o serviço apenas uma vez para cada sessão.  
  
    ```csharp  
    [ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]  
    ```  
  
4. Cada operação de serviço requer uma transação. Especifique isso com o <xref:System.ServiceModel.OperationBehaviorAttribute> atributo. A operação que conclui a transação também deve ser definida <xref:System.ServiceModel.OperationBehaviorAttribute.TransactionAutoComplete> como `true` .  
  
    ```csharp  
    [OperationBehavior(TransactionScopeRequired = true, TransactionAutoComplete = true)]
    ```  
  
5. Configure um ponto de extremidade que usa a associação fornecida pelo sistema `NetMsmqBinding` .  
  
6. Crie uma fila transacional usando <xref:System.Messaging> . Você também pode criar a fila usando o MSMQ (enfileiramento de mensagens) ou o MMC. Se você fizer isso, crie uma fila transacional.  
  
7. Crie um host de serviço para o serviço usando o <xref:System.ServiceModel.ServiceHost> .  
  
8. Abra o host de serviço para disponibilizar o serviço.  
  
9. Feche o host de serviço.  
  
#### <a name="to-set-up-a-client"></a>Para configurar um cliente  
  
1. Crie um escopo de transação para gravar na fila transacional.  
  
2. Crie o cliente WCF usando a ferramenta do [Utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) .  
  
3. Faça o pedido.  
  
4. Feche o cliente WCF.  
  
## <a name="example"></a>Exemplo  
  
### <a name="description"></a>Descrição  
 O exemplo a seguir fornece o código para o `IProcessOrder` serviço e para um cliente que usa esse serviço. Ele mostra como o WCF usa sessões enfileiradas para fornecer o comportamento de agrupamento.  
  
### <a name="code-for-the-service"></a>Código para o serviço  
 [!code-csharp[S_Msmq_Session#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/service.cs#1)]
 [!code-vb[S_Msmq_Session#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/service.vb#1)]  

### <a name="code-for-the-client"></a>Código para o cliente  
 [!code-csharp[S_Msmq_Session#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_session/cs/client.cs#3)]
 [!code-vb[S_Msmq_Session#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_session/vb/client.vb#3)]  

## <a name="see-also"></a>Consulte também

- [Sessões e filas](../samples/sessions-and-queues.md)
- [Visão geral de filas](queues-overview.md)
