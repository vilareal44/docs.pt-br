---
title: Práticas recomendadas para comunicação em fila
ms.date: 03/30/2017
helpviewer_keywords:
- queues [WCF], best practices
- best practices [WCF], queued communication
ms.assetid: 446a6383-cae3-4338-b193-a33c14a49948
ms.openlocfilehash: af9ed7d64a60042297e071262be7610c4d791a51
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601342"
---
# <a name="best-practices-for-queued-communication"></a>Práticas recomendadas para comunicação em fila
Este tópico fornece as práticas recomendadas para comunicação em fila no Windows Communication Foundation (WCF). As seções a seguir discutem as práticas recomendadas de uma perspectiva de cenário.  
  
## <a name="fast-best-effort-queued-messaging"></a>Mensagens em fila rápidas e de melhor esforço  
 Para cenários que exigem separação que as mensagens em fila fornecem e mensagens rápidas e de alto desempenho com garantias de melhor esforço, use uma fila não transacional e defina a <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> propriedade como `false` .  
  
 Além disso, você pode optar por não incorrer no custo das gravações de disco definindo a <xref:System.ServiceModel.MsmqBindingBase.Durable%2A> propriedade como `false` .  
  
 A segurança tem implicações no desempenho. Para obter mais informações, consulte [Considerações sobre desempenho](performance-considerations.md).  
  
## <a name="reliable-end-to-end-queued-messaging"></a>Mensagens de fila de ponta a ponta confiáveis  
 As seções a seguir descrevem as práticas recomendadas para cenários que exigem mensagens confiáveis de ponta a ponta.  
  
### <a name="basic-reliable-transfer"></a>Transferência confiável básica  
 Para confiabilidade de ponta a ponta, defina a <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A> propriedade como `true` para garantir a transferência. A <xref:System.ServiceModel.MsmqBindingBase.Durable%2A> propriedade pode ser definida como `true` ou `false` dependendo dos seus requisitos (o padrão é `true` ). Em geral, a <xref:System.ServiceModel.MsmqBindingBase.Durable%2A> propriedade é definida `true` como parte da confiabilidade de ponta a ponta. O comprometimento é um custo de desempenho, mas torna a mensagem durável para que a mensagem não seja perdida se um Gerenciador de filas falhar.  
  
### <a name="use-of-transactions"></a>Uso de transações  
 Você deve usar transações para garantir a confiabilidade de ponta a ponta. `ExactlyOnce`as garantias só garantem que as mensagens sejam entregues à fila de destino. Para garantir que a mensagem seja recebida, use transações. Sem transações, se o serviço falhar, você perderá a mensagem que está sendo entregue, mas, na verdade, será entregue ao aplicativo.  
  
### <a name="use-of-dead-letter-queues"></a>Uso de filas de mensagens mortas  
 As filas de mensagens mortas garantem que você será notificado se uma mensagem não for entregue à fila de destino. Você pode usar a fila de mensagens mortas fornecida pelo sistema ou uma fila de mensagens mortas personalizada. Em geral, o uso de uma fila de mensagens mortas personalizada é melhor porque permite enviar mensagens mortas de um aplicativo para uma única fila de mensagens mortas. Caso contrário, todas as mensagens mortas que ocorrem para todos os aplicativos em execução no sistema são entregues a uma única fila. Em seguida, cada aplicativo deve pesquisar por meio da fila de mensagens mortas para localizar os emails que são relevantes para esse aplicativo. Às vezes, o uso de uma fila de mensagens mortas personalizada não é viável, como ao usar o MSMQ 3,0.  
  
 Não é recomendável desativar as filas de mensagens mortas para comunicação confiável de ponta a ponta.  
  
 Para obter mais informações, consulte [usando filas de mensagens mortas para lidar com falhas de transferência de mensagens](using-dead-letter-queues-to-handle-message-transfer-failures.md).  
  
### <a name="use-of-poison-message-handling"></a>Uso de manipulação de mensagens suspeitas  
 A manipulação de mensagens suspeitas fornece a capacidade de recuperar-se da falha em processar mensagens.  
  
 Ao usar o recurso de manipulação de mensagens suspeitas, verifique se a <xref:System.ServiceModel.MsmqBindingBase.ReceiveErrorHandling%2A> propriedade está definida com o valor apropriado. Defini-lo como <xref:System.ServiceModel.ReceiveErrorHandling.Drop> significa que os dados são perdidos. Por outro lado, configurá-lo para <xref:System.ServiceModel.ReceiveErrorHandling.Fault> falha do host de serviço ao detectar uma mensagem suspeita. Usando o MSMQ 3,0, <xref:System.ServiceModel.ReceiveErrorHandling.Fault> é a melhor opção para evitar a perda de dados e mover a mensagem suspeita para fora do caminho. Usando o MSMQ 4,0, <xref:System.ServiceModel.ReceiveErrorHandling.Move> é a abordagem recomendada. <xref:System.ServiceModel.ReceiveErrorHandling.Move>move uma mensagem inviabilizada para fora da fila para que o serviço possa continuar processando novas mensagens. O serviço de mensagem suspeita pode processar a mensagem suspeita separadamente.  
  
 Para obter mais informações, consulte [manipulação de mensagens suspeitas](poison-message-handling.md).  
  
## <a name="achieving-high-throughput"></a>Obtendo alta taxa de transferência  
 Para obter alta taxa de transferência em um único ponto de extremidade, use o seguinte:  
  
- Envio em lote transacionado. O envio em lote transacionado garante que muitas mensagens possam ser lidas em uma única transação. Isso otimiza as confirmações de transação, aumentando o desempenho geral. O custo do envio em lote é que, se ocorrer uma falha em uma única mensagem dentro de um lote, todo o lote será revertido e as mensagens deverão ser processadas uma de cada vez até que seja seguro para o lote novamente. Na maioria dos casos, as mensagens suspeitas são raras, portanto, o envio em lote é a maneira preferida de aumentar o desempenho do sistema, especialmente quando você tem outros gerenciadores de recursos que participam da transação. Para obter mais informações, consulte [envio em lote de mensagens em uma transação](batching-messages-in-a-transaction.md).  
  
- Corrente. A simultaneidade aumenta a taxa de transferência, mas a simultaneidade também afeta a contenção de recursos compartilhados. Para obter mais informações, consulte [Concurrency](../samples/concurrency.md).  
  
- Limitação. Para obter um desempenho ideal, limite o número de mensagens no pipeline do Dispatcher. Para obter um exemplo de como fazer isso, consulte [limitação](../samples/throttling.md).  
  
 Ao usar o envio em lote, lembre-se de que a simultaneidade e a limitação são transvertidas em lotes simultâneos.  
  
 Para obter uma taxa de transferência e disponibilidade mais altas, use um farm de serviços WCF que lidam da fila. Isso requer que todos esses serviços exponham o mesmo contrato no mesmo ponto de extremidade. A abordagem de farm funciona melhor para aplicativos que têm altas taxas de produção de mensagens, pois permite que vários serviços sejam lidos da mesma fila.  
  
 Ao usar farms, lembre-se de que o MSMQ 3,0 não oferece suporte a leituras transacionadas remotas. O MSMQ 4,0 oferece suporte a leituras transacionadas remotas.  
  
 Para obter mais informações, consulte [envio em lote de mensagens em uma transação](batching-messages-in-a-transaction.md) e [diferenças em recursos de enfileiramento no Windows Vista, no Windows Server 2003 e no Windows XP](diff-in-queue-in-vista-server-2003-windows-xp.md).  
  
## <a name="queuing-with-unit-of-work-semantics"></a>Enfileiramento com a semântica de unidade de trabalho  
 Em alguns cenários, um grupo de mensagens em uma fila pode estar relacionado e, portanto, a ordenação dessas mensagens é significativa. Nesses cenários, processe um grupo de mensagens relacionadas em conjunto como uma única unidade: ou todas as mensagens são processadas com êxito ou nenhuma delas. Para implementar esse comportamento, use sessões com filas.  
  
 Para obter mais informações, consulte [agrupando mensagens em fila em uma sessão](grouping-queued-messages-in-a-session.md).  
  
## <a name="correlating-request-reply-messages"></a>Correlacionando mensagens de solicitação-resposta  
 Embora as filas sejam geralmente unidirecionais, em alguns cenários, você pode querer correlacionar uma resposta recebida para uma solicitação enviada anteriormente. Se você precisar dessa correlação, é recomendável aplicar seu próprio cabeçalho de mensagem SOAP que contém informações de correlação com a mensagem. Normalmente, o remetente anexa esse cabeçalho à mensagem, e o receptor, ao processar a mensagem e responder com uma nova mensagem em uma fila de resposta, anexa o cabeçalho da mensagem do remetente que contém as informações de correlação para que o remetente possa identificar a mensagem de resposta com a mensagem de solicitação.  
  
## <a name="integrating-with-non-wcf-applications"></a>Integração com aplicativos não WCF  
 Use `MsmqIntegrationBinding` ao integrar serviços WCF ou clientes com serviços ou clientes não WCF. O aplicativo não WCF pode ser um aplicativo MSMQ escrito usando System. Messaging, COM+, Visual Basic ou C++.  
  
 Ao usar `MsmqIntegrationBinding` o, lembre-se do seguinte:  
  
- Um corpo de mensagem do WCF não é o mesmo que um corpo de mensagem do MSMQ. Ao enviar uma mensagem WCF usando uma associação em fila, o corpo da mensagem do WCF é colocado dentro de uma mensagem MSMQ. A infraestrutura do MSMQ está alheios a essas informações adicionais; Ele vê apenas a mensagem MSMQ.  
  
- `MsmqIntegrationBinding`dá suporte a tipos de serialização populares. Com base no tipo de serialização, o tipo de corpo da mensagem genérica, <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> , usa parâmetros de tipo diferentes. Por exemplo, <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat.ByteArray> requer `MsmqMessage\<byte[]>` e <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat.Stream> requer `MsmqMessage<Stream>` .  
  
- Com a serialização XML, você pode especificar o tipo conhecido usando o `KnownTypes` atributo no [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-servicebehaviors.md) elemento que é usado para determinar como desserializar a mensagem XML.  
  
## <a name="see-also"></a>Consulte também

- [Enfileiramento no WCF](queuing-in-wcf.md)
- [Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF](how-to-exchange-queued-messages-with-wcf-endpoints.md)
- [Como trocar mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens](how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)
- [Agrupamento de mensagens em fila em uma sessão](grouping-queued-messages-in-a-session.md)
- [Mensagens de lote em uma transação](batching-messages-in-a-transaction.md)
- [Usando filas de mensagens mortas para lidar com falhas de transferência de mensagem](using-dead-letter-queues-to-handle-message-transfer-failures.md)
- [Manipulação de mensagens suspeitas](poison-message-handling.md)
- [Diferenças de recursos em fila no Windows Vista, Windows Server 2003, e no Windows XP](diff-in-queue-in-vista-server-2003-windows-xp.md)
- [Mensagens de segurança que usam a segurança de transporte](securing-messages-using-transport-security.md)
- [Protegendo as mensagens com segurança de mensagem](securing-messages-using-message-security.md)
- [Solução de problemas de mensagens em fila](troubleshooting-queued-messaging.md)
