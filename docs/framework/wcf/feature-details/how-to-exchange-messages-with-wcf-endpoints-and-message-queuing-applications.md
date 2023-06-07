---
title: Como fazer intercâmbio de mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 62210fd8-a372-4d55-ab9b-c99827d1885e
ms.openlocfilehash: 0775de90903aed27a8d0006614a4b6f2d857eee3
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597092"
---
# <a name="how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications"></a>Como fazer intercâmbio de mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens
Você pode integrar aplicativos MSMQ (enfileiramento de mensagens) existentes com aplicativos Windows Communication Foundation (WCF) usando a associação de integração do MSMQ para converter mensagens MSMQ de e para mensagens do WCF. Isso permite chamar os aplicativos do receptor MSMQ de clientes WCF, bem como chamar serviços WCF de aplicativos de remetente MSMQ.  
  
 Nesta seção, explicaremos como usar para a <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> comunicação em fila entre (1) um cliente WCF e um serviço de aplicativo MSMQ escrito usando System. Messaging e (2) um cliente de aplicativo MSMQ e um serviço WCF.  
  
 Para obter um exemplo completo que demonstra como chamar um aplicativo receptor MSMQ de um cliente WCF, consulte a amostra [Windows Communication Foundation para enfileiramento de mensagens](../samples/wcf-to-message-queuing.md) .  
  
 Para obter um exemplo completo que demonstra como chamar um serviço WCF de um cliente MSMQ, consulte o [enfileiramento de mensagens para Windows Communication Foundation](../samples/message-queuing-to-wcf.md) exemplo.  
  
### <a name="to-create-a-wcf-service-that-receives-messages-from-a-msmq-client"></a>Para criar um serviço WCF que recebe mensagens de um cliente MSMQ  
  
1. Defina uma interface que define o contrato de serviço para o serviço WCF que recebe mensagens em fila de um aplicativo remetente MSMQ, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[S_MsmqToWcf#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmqtowcf/cs/service.cs#1)]
     [!code-vb[S_MsmqToWcf#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmqtowcf/vb/service.vb#1)]  
  
2. Implemente a interface e aplique o <xref:System.ServiceModel.ServiceBehaviorAttribute> atributo à classe, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[S_MsmqToWcf#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_msmqtowcf/cs/service.cs#2)]
     [!code-vb[S_MsmqToWcf#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_msmqtowcf/vb/service.vb#2)]  
  
3. Crie um arquivo de configuração que especifique o <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding> .  

4. Crie uma instância de um <xref:System.ServiceModel.ServiceHost> objeto que usa a associação configurada.  

### <a name="to-create-a-wcf-client-that-sends-messages-to-a-msmq-receiver-application"></a>Para criar um cliente WCF que envia mensagens para um aplicativo receptor MSMQ  
  
1. Defina uma interface que define o contrato de serviço para o cliente WCF que envia mensagens enfileiradas para o receptor MSMQ, conforme mostrado no código de exemplo a seguir.  
  
     [!code-csharp[S_WcfToMsmq#6](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/proxy.cs#6)]
     [!code-vb[S_WcfToMsmq#6](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/proxy.vb#6)]  
  
2. Defina uma classe de cliente que o cliente WCF usa para chamar o receptor MSMQ.  
  
     [!code-csharp[S_WcfToMsmq#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/snippets.cs#2)]
     [!code-vb[S_WcfToMsmq#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/snippets.vb#2)]  
  
3. Crie uma configuração que especifique o uso da Associação MsmqIntegrationBinding.  
  
     [!code-csharp[S_WcfToMsmq#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/snippets.cs#3)]
     [!code-vb[S_WcfToMsmq#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_wcftomsmq/vb/snippets.vb#3)]  
  
4. Crie uma instância da classe de cliente e chame o método definido pelo serviço de recebimento de mensagens.  
  
     [!code-csharp[S_WcfToMsmq#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_wcftomsmq/cs/client.cs#4)]  
  
## <a name="see-also"></a>Consulte também

- [Visão geral de filas](queues-overview.md)
- [Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [Windows Communication Foundation para enfileiramento de mensagens](../samples/wcf-to-message-queuing.md)
- [Instalando o Enfileiramento de Mensagens (MSMQ)](../samples/installing-message-queuing-msmq.md)
- [Enfileiramento de mensagens para o Windows Communication Foundation](../samples/message-queuing-to-wcf.md)
- [Segurança de mensagem através do enfileiramento de mensagem](../samples/message-security-over-message-queuing.md)
