---
title: Utilizando filas de mensagens mortas para manuseio de transferência de mensagens com falha
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
ms.assetid: 9e891c6a-d960-45ea-904f-1a00e202d61a
ms.openlocfilehash: f07397b4c10ffec4902dbde37b622978d00f5b63
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594979"
---
# <a name="using-dead-letter-queues-to-handle-message-transfer-failures"></a>Utilizando filas de mensagens mortas para manuseio de transferência de mensagens com falha
As mensagens em fila podem ser entregues com falha. Essas mensagens com falha são registradas em uma fila de mensagens mortas. A entrega com falha pode ser causada por motivos como falhas de rede, uma fila excluída, uma falha de autenticação completa ou uma falha de entrega no prazo.  
  
 As mensagens em fila podem permanecer na fila por um longo período se o aplicativo receptor não lê-las da fila em tempo hábil. Esse comportamento pode não ser apropriado para mensagens com detecção de hora. As mensagens que diferenciam o tempo têm uma propriedade TTL (vida útil) definida na associação em fila, que indica por quanto tempo as mensagens podem estar na fila antes de expirarem. As mensagens expiradas são enviadas para uma fila especial chamada fila de mensagens mortas. As mensagens também podem ser colocadas em uma fila de mensagens mortas por outros motivos, como exceder uma cota de fila ou devido a uma falha de autenticação.  
  
 Em geral, os aplicativos gravam a lógica de compensação para ler mensagens dos motivos de falha e de fila de mensagens mortas. A lógica de compensação depende da causa da falha. Por exemplo, no caso de falha de autenticação, você pode corrigir o certificado anexado à mensagem e reenviar a mensagem. Se a entrega falhou porque a cota de fila de destino foi atingida, você pode tentar novamente a entrega na esperança de que o problema de cota foi resolvido.  
  
 A maioria dos sistemas de enfileiramento tem uma fila de mensagens mortas em todo o sistema, em que todas as mensagens com falha desse sistema são armazenadas. O serviço de enfileiramento de mensagens (MSMQ) fornece duas filas de mensagens mortas de todo o sistema: uma fila de mensagens mortas em todo o sistema transacional, que armazena mensagens que falharam na entrega à fila transacional e uma fila de mensagens mortas em todo o sistema não transacional que armazena as mensagens que falharam na fila de não transacional. Se dois clientes estiverem enviando mensagens para dois serviços diferentes e, portanto, filas diferentes no WCF estiverem compartilhando o mesmo serviço MSMQ a ser enviado, será possível ter uma combinação de mensagens na fila mensagens mortas do sistema. Isso nem sempre é o ideal. Em vários casos (segurança, por exemplo), talvez você não queira que um cliente leia as mensagens de outro cliente de uma fila de mensagens mortas. Uma fila de mensagens mortas compartilhada também exige que os clientes naveguem pela fila para encontrar uma mensagem que eles enviaram, o que pode ser extremamente caro com base no número de mensagens na fila da mensagem de mensagens mortas. Portanto, no WCF `NetMsmqBinding` , `MsmqIntegrationBinding,` e o MSMQ no Windows Vista fornecem uma fila de mensagens mortas personalizada (às vezes chamada de fila de mensagens mortas específica do aplicativo).  
  
 A fila de mensagens mortas personalizada fornece isolamento entre clientes que compartilham o mesmo serviço MSMQ para enviar mensagens.  
  
 No Windows Server 2003 e no Windows XP, Windows Communication Foundation (WCF) fornece uma fila de mensagens mortas em todo o sistema para todos os aplicativos cliente em fila. No Windows Vista, o WCF fornece uma fila de mensagens mortas para cada aplicativo cliente em fila.  
  
## <a name="specifying-use-of-the-dead-letter-queue"></a>Especificando o uso da fila de mensagens mortas  
 Uma fila de mensagens mortas está no Gerenciador de filas do aplicativo de envio. Ele armazena mensagens que expiraram ou que falharam na transferência ou entrega.  
  
 A associação tem as seguintes propriedades de fila de mensagens mortas:  
  
- <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A>  
  
- <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A>  
  
## <a name="reading-messages-from-the-dead-letter-queue"></a>Lendo mensagens da fila de mensagem mortas  
 Um aplicativo que lê mensagens de uma fila de mensagem mortas é semelhante a um serviço WCF que lê de uma fila de aplicativo, exceto pelas seguintes diferenças secundárias:  
  
- Para ler mensagens de uma fila de mensagens mortas de sistema transacional, o Uniform Resource Identifier (URI) deve estar no formato: net. MSMQ://localhost/System $;D eadXact.  
  
- Para ler mensagens de uma fila de mensagens mortas não transacional do sistema, o URI deve estar no formato: net. MSMQ://localhost/System $;D eadLetter.  
  
- Para ler mensagens de uma fila de mensagens mortas personalizada, o URI deve estar no formato: net. MSMQ://localhost/Private/, \<*custom-dlq-name*> em que *Custom-DLQ-Name* é o nome da fila de mensagens mortas personalizada.  
  
 Para obter mais informações sobre como endereçar filas, consulte [pontos de extremidade de serviço e endereçamento de fila](service-endpoints-and-queue-addressing.md).  
  
 A pilha do WCF no receptor corresponde aos endereços que o serviço está escutando com o endereço na mensagem. Se os endereços corresponderem, a mensagem será expedida; caso contrário, a mensagem não será despachada. Isso pode causar problemas durante a leitura da fila de mensagens mortas, pois, em geral, elas são endereçadas para o serviço e não para o serviço fila inativo. Portanto, o serviço de leitura da fila de mensagens mortas deve instalar um filtro de endereço `ServiceBehavior` que instrui a pilha a corresponder todas as mensagens na fila independentemente do destinatário. Especificamente, você deve adicionar um `ServiceBehavior` com o <xref:System.ServiceModel.AddressFilterMode.Any> parâmetro ao serviço de leitura de mensagens da fila de mensagem mortas.  
  
## <a name="poison-message-handling-from-the-dead-letter-queue"></a>Manipulação de mensagens suspeitas da fila de mensagens mortas  
 A manipulação de mensagens suspeitas está disponível em filas de mensagens mortas, com algumas condições. Como não é possível criar subfilas de filas do sistema, ao ler da fila de mensagens mortas do sistema, o `ReceiveErrorHandling` não pode ser definido como `Move` . Observe que, se você estiver lendo de uma fila de mensagens mortas personalizada, poderá ter subfilas e, portanto, `Move` será uma disposição válida para a mensagem suspeita.  
  
 Quando `ReceiveErrorHandling` é definido como `Reject` , ao ler da fila de mensagens mortas personalizada, a mensagem suspeita é colocada na fila de mensagens mortas do sistema. Se estiver lendo na fila de mensagens mortas do sistema, a mensagem será descartada (limpa). Uma rejeição de uma fila de mensagens mortas do sistema no MSMQ descarta (limpa) a mensagem.  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir mostra como criar uma fila de mensagem mortas e como usá-la para processar mensagens expiradas. O exemplo se baseia no exemplo em [como trocar mensagens em fila com pontos de extremidade do WCF](how-to-exchange-queued-messages-with-wcf-endpoints.md). O exemplo a seguir mostra como gravar o código do cliente no serviço de processamento de pedidos que usa uma fila de mensagens mortas para cada aplicativo. O exemplo também mostra como processar mensagens da fila de mensagens mortas.  
  
 Veja a seguir o código de um cliente que especifica uma fila de mensagens mortas para cada aplicativo.  
  
 [!code-csharp[S_DeadLetter#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_deadletter/cs/client.cs#1)]
 [!code-vb[S_DeadLetter#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_deadletter/vb/client.vb#1)]  
  
 Veja a seguir o código para o arquivo de configuração do cliente.  

 Veja a seguir o código de um serviço de processamento de mensagens de uma fila de mensagens mortas.  
  
 [!code-csharp[S_DeadLetter#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/s_deadletter/cs/dlservice.cs#3)]
 [!code-vb[S_DeadLetter#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/s_deadletter/vb/dlservice.vb#3)]  
  
 Veja a seguir o código para o arquivo de configuração do serviço fila de mensagens mortas.  

## <a name="see-also"></a>Consulte também

- [Visão geral de filas](queues-overview.md)
- [Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [Manipulação de mensagens suspeitas](poison-message-handling.md)
