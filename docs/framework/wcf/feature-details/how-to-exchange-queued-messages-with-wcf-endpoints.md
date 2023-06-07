---
title: Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 938e7825-f63a-4c3d-b603-63772fabfdb3
ms.openlocfilehash: 7da7ba1b680bae2b29eeff8fe669e097ea8eda32
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595369"
---
# <a name="how-to-exchange-queued-messages-with-wcf-endpoints"></a>Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF
As filas do garantem que mensagens confiáveis possam ocorrer entre um cliente e um serviço Windows Communication Foundation (WCF), mesmo que o serviço não esteja disponível no momento da comunicação. Os procedimentos a seguir mostram como garantir a comunicação durável entre um cliente e um serviço usando a associação em fila padrão ao implementar o serviço WCF.  
  
 Esta seção explica como usar o <xref:System.ServiceModel.NetMsmqBinding> para a comunicação em fila entre um cliente WCF e um serviço WCF.  
  
### <a name="to-use-queuing-in-a-wcf-service"></a>Para usar o enfileiramento em um serviço WCF  
  
1. Defina um contrato de serviço usando uma interface marcada com o <xref:System.ServiceModel.ServiceContractAttribute> . Marque as operações na interface que fazem parte do contrato de serviço com o <xref:System.ServiceModel.OperationContractAttribute> e especifique-as como unidirecional porque nenhuma resposta ao método é retornada. O código a seguir fornece um exemplo de contrato de serviço e sua definição de operação.  
  
     [!code-csharp[S_Msmq_Transacted#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/service.cs#1)]
     [!code-vb[S_Msmq_Transacted#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/service.vb#1)]  
  
2. Quando o contrato de serviço passa por tipos definidos pelo usuário, você deve definir contratos de dados para esses tipos. O código a seguir mostra dois contratos de dados `PurchaseOrder` e `PurchaseOrderLineItem` . Esses dois tipos definem dados que são enviados para o serviço. (Observe que as classes que definem esse contrato de dados também definem um número de métodos. Esses métodos não são considerados parte do contrato de dados. Somente os membros declarados com o <xref:System.Runtime.Serialization.DataMemberAttribute> atributo fazem parte do contrato de dados.)  
  
     [!code-csharp[S_Msmq_Transacted#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/service.cs#2)]
     [!code-vb[S_Msmq_Transacted#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/service.vb#2)]  
  
3. Implemente os métodos do contrato de serviço definido na interface em uma classe.  
  
     [!code-csharp[S_Msmq_Transacted#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/service.cs#3)]
     [!code-vb[S_Msmq_Transacted#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/service.vb#3)]  
  
     Observe o <xref:System.ServiceModel.OperationBehaviorAttribute> colocado no `SubmitPurchaseOrder` método. Isso especifica que essa operação deve ser chamada dentro de uma transação e que a transação seja concluída automaticamente quando o método for concluído.  
  
4. Crie uma fila transacional usando <xref:System.Messaging> . Você pode optar por criar a fila usando o MMC (console de gerenciamento Microsoft) do MSMQ (enfileiramento de mensagens da Microsoft) em vez disso. Nesse caso, certifique-se de criar uma fila transacional.  
  
     [!code-csharp[S_Msmq_Transacted#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/hostapp.cs#4)]
     [!code-vb[S_Msmq_Transacted#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/hostapp.vb#4)]  
  
5. Defina um <xref:System.ServiceModel.Description.ServiceEndpoint> na configuração que especifica o endereço do serviço e usa a <xref:System.ServiceModel.NetMsmqBinding> associação padrão. Para obter mais informações sobre como usar a configuração do WCF, consulte [Configurando serviços WCF](../configuring-services.md).  

6. Crie um host para o `OrderProcessing` serviço usando <xref:System.ServiceModel.ServiceHost> que lê mensagens da fila e os processa. Abra o host de serviço para disponibilizar o serviço. Exiba uma mensagem que diga ao usuário para pressionar qualquer tecla para encerrar o serviço. Chame `ReadLine` para aguardar que a chave seja pressionada e, em seguida, feche o serviço.  
  
     [!code-csharp[S_Msmq_Transacted#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/hostapp.cs#6)]
     [!code-vb[S_Msmq_Transacted#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/hostapp.vb#6)]  
  
### <a name="to-create-a-client-for-the-queued-service"></a>Para criar um cliente para o serviço em fila  
  
1. O exemplo a seguir mostra como executar o aplicativo de hospedagem e usar a ferramenta svcutil. exe para criar o cliente WCF.  
  
    ```console
    svcutil http://localhost:8000/ServiceModelSamples/service  
    ```  
  
2. Defina um <xref:System.ServiceModel.Description.ServiceEndpoint> na configuração que especifica o endereço e usa a <xref:System.ServiceModel.NetMsmqBinding> associação padrão, conforme mostrado no exemplo a seguir.  

3. Crie um escopo de transação para gravar na fila transacional, chame a `SubmitPurchaseOrder` operação e feche o cliente WCF, conforme mostrado no exemplo a seguir.  
  
     [!code-csharp[S_Msmq_Transacted#8](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/client.cs#8)]
     [!code-vb[S_Msmq_Transacted#8](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/client.vb#8)]  
  
## <a name="example"></a>Exemplo  
 Os exemplos a seguir mostram o código do serviço, o aplicativo de hospedagem, o arquivo app. config e o código do cliente incluídos para este exemplo.  
  
 [!code-csharp[S_Msmq_Transacted#9](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/service.cs#9)]
 [!code-vb[S_Msmq_Transacted#9](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/service.vb#9)]  
  
 [!code-csharp[S_Msmq_Transacted#10](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/hostapp.cs#10)]
 [!code-vb[S_Msmq_Transacted#10](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/hostapp.vb#10)]  

 [!code-csharp[S_Msmq_Transacted#12](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmq_transacted/cs/client.cs#12)]
 [!code-vb[S_Msmq_Transacted#12](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmq_transacted/vb/client.vb#12)]  

## <a name="see-also"></a>Consulte também

- <xref:System.ServiceModel.NetMsmqBinding>
- [Associação transacionada do MSMQ](../samples/transacted-msmq-binding.md)
- [Enfileiramento no WCF](queuing-in-wcf.md)
- [Como trocar mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens](how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [Windows Communication Foundation para enfileiramento de mensagens](../samples/wcf-to-message-queuing.md)
- [Instalando o Enfileiramento de Mensagens (MSMQ)](../samples/installing-message-queuing-msmq.md)
- [Enfileiramento de mensagens para o Windows Communication Foundation](../samples/message-queuing-to-wcf.md)
- [Segurança de mensagem através do enfileiramento de mensagem](../samples/message-security-over-message-queuing.md)
