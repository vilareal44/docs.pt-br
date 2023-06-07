---
title: Enfileiramento no WCF
ms.date: 03/30/2017
ms.assetid: e98d76ba-1acf-42cd-b137-0f8214661112
ms.openlocfilehash: 710ddedeb211ae6b874e2957943dbe60425b7476
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84601108"
---
# <a name="queuing-in-wcf"></a>Enfileiramento no WCF
Esta seção descreve como usar a comunicação em fila no Windows Communication Foundation (WCF).  
  
## <a name="queues-as-a-wcf-transport-binding"></a>Filas como uma associação de transporte do WCF  
 No WCF, os contratos especificam o que está sendo trocado. Os contratos são trocas de mensagens específicas do aplicativo ou dependentes da empresa. O mecanismo usado para trocar mensagens (ou "como") é especificado nas associações. Associações no WCF encapsulam detalhes da troca de mensagens. Eles expõem os botões de configuração para que o usuário controle vários aspectos do transporte ou o protocolo que as associações representam. O enfileiramento no WCF é tratado como qualquer outra associação de transporte, que é uma grande vantagem para muitos aplicativos de enfileiramento. Hoje em dia, muitos aplicativos de enfileiramento são escritos de forma diferente de outros aplicativos distribuídos no estilo RPC (chamada de procedimento remoto), o que dificulta a manutenção e o acompanhamento. Com o WCF, o estilo de escrever um aplicativo distribuído é muito semelhante, tornando-o mais fácil de seguir e manter. Além disso, ao fatorar o mecanismo do Exchange separadamente da lógica de negócios, é mais fácil configurar o transporte ou fazer alterações nele sem afetar o código específico do aplicativo. A figura a seguir ilustra a estrutura de um serviço WCF e cliente usando o MSMQ como um transporte.  
  
 ![Diagrama de aplicativos enfileirados](media/distributed-queue-figure.jpg "Distributed-Queue-figura")  
  
 Como você pode ver na figura anterior, o cliente e o serviço devem definir apenas a semântica do aplicativo, ou seja, o contrato e a implementação. O serviço configura uma associação em fila com configurações preferenciais. O cliente usa a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para gerar um cliente WCF para o serviço e gerar um arquivo de configuração que descreve as associações a serem usadas para enviar mensagens para o serviço. Portanto, para enviar uma mensagem em fila, o cliente cria uma instância de um cliente WCF e invoca uma operação nela. Isso faz com que a mensagem seja enviada para a fila de transmissão e transferida para a fila de destino. Todas as complexidades da comunicação em fila ficam ocultas do aplicativo que está enviando e recebendo mensagens.  
  
 As advertências sobre a associação em fila no WCF incluem:  
  
- Todas as operações de serviço devem ser unidirecionais porque a associação em fila padrão no WCF não oferece suporte à comunicação duplex usando filas. Um exemplo de comunicação bidirecional ([comunicação bidirecional](../samples/two-way-communication.md)) ilustra como usar contratos de 2 1 vias para implementar a comunicação duplex usando filas.  
  
- Para gerar um cliente WCF usando a troca de metadados, é necessário um ponto de extremidade HTTP adicional no serviço para que ele possa ser consultado diretamente para gerar o cliente WCF e obter informações de associação para configurar adequadamente a comunicação em fila.  
  
- Com base na associação em fila, é necessária uma configuração extra fora do WCF. Por exemplo, a <xref:System.ServiceModel.NetMsmqBinding> classe fornecida com o WCF exige que você configure as associações, bem como configurar minimamente o MSMQ (enfileiramento de mensagens).  
  
 As seções a seguir descrevem as associações enfileiradas específicas fornecidas com o WCF, que se baseiam no MSMQ.  
  
### <a name="msmq"></a>MSMQ  
 O transporte em fila no WCF usa o MSMQ para sua comunicação em fila.  
  
 O MSMQ é fornecido como um componente opcional com o Windows e é executado como um serviço NT. Ele captura mensagens para transmissão em uma fila de transmissão e para entrega em uma fila de destino. Os gerenciadores de filas do MSMQ implementam um protocolo de transferência de mensagens confiável para que as mensagens não sejam perdidas na transmissão. O protocolo pode ser baseado em SOAP ou nativo, como o SRMP (protocolo SOAP de mensagem confiável).  
  
 No MSMQ, as filas podem ser transacionais ou não transacionais. Uma fila transacional permite que as mensagens sejam capturadas e entregues em uma transação e, em seguida, armazenadas permanentemente na fila. As mensagens enviadas para uma fila transacional são transferidas exatamente uma vez na ordem. Você pode usar uma fila não transacional para enviar mensagens voláteis e duráveis. Uma mensagem enviada para uma fila não transacional não carrega nenhuma garantia de transferência confiável; Portanto, as mensagens podem ser perdidas.  
  
 As filas do MSMQ também podem ser protegidas usando uma identidade do Windows registrada com o serviço de diretório Active Directory. Ao instalar o MSMQ, você pode instalar Active Directory integração, o que exige que o computador faça parte de uma rede de domínio do Windows.  
  
 Para obter mais informações sobre o MSMQ, consulte [instalando o MSMQ (enfileiramento de mensagens)](../samples/installing-message-queuing-msmq.md).  
  
### <a name="netmsmqbinding"></a>NetMsmqBinding  
 O [\<netMsmqBinding>](../../configure-apps/file-schema/wcf/netmsmqbinding.md) é a associação enfileirada que o WCF fornece para que dois pontos de extremidade do WCF se comuniquem usando o MSMQ. A associação, portanto, expõe propriedades que são específicas do MSMQ. No entanto, nem todos os recursos e propriedades do MSMQ são expostos no `NetMsmqBinding` . O Compact `NetMsmqBinding` é projetado com um conjunto ideal de recursos que a maioria dos clientes deve encontrar suficiente.  
  
 O `NetMsmqBinding` manifesta os conceitos de enfileiramento principal abordados até o momento na forma de propriedades nas associações. Essas propriedades, por sua vez, se comunicam com o MSMQ como transferir e entregar as mensagens. Uma discussão sobre as categorias de propriedade está nas seções a seguir. Para obter mais informações, consulte os tópicos conceituais que descrevem as propriedades específicas mais completamente.  
  
#### <a name="exactlyonce-and-durable-properties"></a>Propriedades de ExactlyOnce e duráveis  
 As `ExactlyOnce` `Durable` Propriedades e afetam o modo como as mensagens são transferidas entre filas:  
  
- `ExactlyOnce`: Quando definido como `true` (o padrão), o canal em fila garante que a mensagem, se entregue, não seja duplicada. Ele também garante que a mensagem não seja perdida. Se a mensagem não puder ser entregue ou o tempo de vida da mensagem expirar antes que a mensagem possa ser entregue, a mensagem com falha junto com o motivo da falha de entrega será registrada em uma fila de mensagens mortas. Quando definido como `false` , o canal em fila faz um esforço para transferir a mensagem. Nesse caso, opcionalmente, você pode escolher uma fila de mensagens mortas.  
  
- `Durable:`Quando definido como `true` (o padrão), o canal em fila garante que o MSMQ armazene a mensagem permanentemente no disco. Portanto, se o serviço MSMQ parar e reiniciar, as mensagens no disco serão transferidas para a fila de destino ou entregues ao serviço. Quando definido como `false` , as mensagens são armazenadas no repositório volátil e são perdidas na interrupção e na reinicialização do serviço MSMQ.  
  
 Para `ExactlyOnce` a transferência confiável, o MSMQ requer que a fila seja transacional. Além disso, o MSMQ requer uma transação para ler de uma fila transacional. Assim, ao usar o `NetMsmqBinding` , lembre-se de que uma transação é necessária para enviar ou receber mensagens quando `ExactlyOnce` é definido como `true` . Da mesma forma, o MSMQ exige que a fila seja não transacional para garantias de melhor esforço, como quando `ExactlyOnce` é `false` e para mensagens voláteis. Portanto, ao definir `ExactlyOnce` como `false` ou durável para `false` , você não pode enviar ou receber usando uma transação.  
  
> [!NOTE]
> Certifique-se de que a fila correta (transacional ou não transacional) seja criada com base nas configurações nas associações. Se `ExactlyOnce` for `true` , use uma fila transacional; caso contrário, use uma fila não transacional.  
  
#### <a name="dead-letter-queue-properties"></a>Propriedades da fila de mensagens mortas  
 Essa fila é usada para armazenar mensagens que falham na entrega. O usuário pode gravar a lógica de compensação que lê mensagens fora da fila de mensagens mortas.  
  
 Muitos sistemas de enfileiramento fornecem uma fila de mensagens mortas em todo o sistema. O MSMQ fornece uma fila de mensagens mortas não transacionais em todo o sistema para as mensagem que falha na entrega a filas não transacionais e uma fila de mensagens mortas transacionais em todo o sistema para o envio de falhas para filas transacionais.  
  
 Se vários clientes que enviam mensagens para filas de destino diferentes compartilharem o serviço MSMQ, todas as mensagens enviadas pelos clientes vão para a mesma fila de mensagens mortas. Isso nem sempre é preferível. Para um melhor isolamento, o WCF e o MSMQ no Windows Vista fornecem uma fila de mensagens mortas personalizada (ou uma fila de mensagens mortas específica do aplicativo) que o usuário pode especificar para armazenar mensagens que falham na entrega. Portanto, clientes diferentes não compartilham a mesma fila de mensagens mortas.  
  
 A associação tem duas propriedades de interesse:  
  
- `DeadLetterQueue`: Essa propriedade é uma enumeração que indica se uma fila de mensagens mortas é solicitada. A enumeração também contém o tipo de fila de mensagens mortas, se uma for solicitada. Os valores são `None`, `System` e `Custom`. Para obter mais informações sobre a interpretação dessas propriedades, consulte [usando filas de mensagens mortas para lidar com falhas de transferência de mensagens](using-dead-letter-queues-to-handle-message-transfer-failures.md)  
  
- `CustomDeadLetterQueue`: Essa propriedade é o endereço Uniform Resource Identifier (URI) da fila de mensagens mortas específica do aplicativo. Isso será necessário se `DeadLetterQueue` .`Custom` é escolhido.  
  
#### <a name="poison-message-handling-properties"></a>Propriedades de tratamento de mensagens suspeitas  
 Quando o serviço lê mensagens da fila de destino em uma transação, o serviço pode falhar ao processar a mensagem por vários motivos. Em seguida, a mensagem é colocada de volta na fila para ser lida novamente. Para lidar com mensagens que falham repetidamente, um conjunto de propriedades de manipulação de mensagens suspeitas pode ser configurado na associação. Há quatro propriedades: `ReceiveRetryCount` ,, `MaxRetryCycles` `RetryCycleDelay` e `ReceiveErrorHandling` . Para obter mais informações sobre essas propriedades, consulte [manipulação de mensagens suspeitas](poison-message-handling.md).  
  
#### <a name="security-properties"></a>Propriedades de segurança  
 O MSMQ expõe seu próprio modelo de segurança, como ACLs (listas de controle de acesso) em uma fila ou enviando mensagens autenticadas. O `NetMsmqBinding` expõe essas propriedades de segurança como parte de suas configurações de segurança de transporte. Há duas propriedades na associação para segurança de transporte: `MsmqAuthenticationMode` e `MsmqProtectionLevel` . As configurações nessas propriedades dependem de como o MSMQ é configurado. Para obter mais informações, consulte [protegendo mensagens usando a segurança de transporte](securing-messages-using-transport-security.md).  
  
 Além da segurança do transporte, a mensagem SOAP propriamente dita pode ser protegida usando a segurança da mensagem. Para obter mais informações, consulte [protegendo mensagens usando a segurança da mensagem](securing-messages-using-message-security.md).  
  
 `MsmqTransportSecurity`também expõe duas propriedades, `MsmqEncryptionAlgorithm` e `MsmqHashAlgorithm` . Essas são enumerações de algoritmos diferentes para escolher a criptografia de transferência de fila para fila de mensagens e o hash das assinaturas.  
  
#### <a name="other-properties"></a>Outras propriedades  
 Além das propriedades anteriores, outras propriedades específicas do MSMQ expostas na associação incluem:  
  
- `UseSourceJournal`: Uma propriedade para indicar que o diário de origem está ativado. O diário de origem é um recurso MSMQ que controla as mensagens que foram transmitidas com êxito da fila de transmissão.  
  
- `UseMsmqTracing`: Uma propriedade para indicar que o rastreamento MSMQ está ativado. O rastreamento MSMQ envia mensagens de relatório para uma fila de relatório sempre que uma mensagem sai ou chega em um computador que hospeda um Gerenciador de filas MSMQ.  
  
- `QueueTransferProtocol`: Uma enumeração do protocolo a ser usado para transferências de mensagens da fila para a fila. O MSMQ implementa um protocolo de transferência de fila para fila nativo e um protocolo baseado em SOAP chamado SRMP (SOAP Reliable Messaging Protocol). SRMP é usado ao usar o transporte HTTP para transferências de fila para fila. SRMP Secure é usado ao usar HTTPS para transferências de fila para fila.  
  
- `UseActiveDirectory`: Um valor booliano para indicar se o Active Directory deve ser usado para resolução de endereço de fila. Por padrão, isso é desativado. Para obter mais informações, consulte [pontos de extremidade de serviço e endereçamento de fila](service-endpoints-and-queue-addressing.md).  
  
### <a name="msmqintegrationbinding"></a>MsmqIntegrationBinding  
 O `MsmqIntegrationBinding` é usado quando você deseja que um ponto de extremidade WCF se comunique com um aplicativo MSMQ existente escrito em APIs C, C++, com ou System. Messaging.  
  
 As propriedades de associação são as mesmas de `NetMsmqBinding` . No entanto, as seguintes diferenças se aplicam:  
  
- O contrato de operação para `MsmqIntegrationBinding` está restrito a usar um único parâmetro do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> em que o parâmetro de tipo é o tipo de corpo.  
  
- Muitas das propriedades de mensagens nativas do MSMQ são expostas no <xref:System.ServiceModel.MsmqIntegration.MsmqMessage%601> para uso.  
  
- Para ajudar com a serialização e desserialização do corpo da mensagem, serializadores, como XML e ActiveX, são fornecidos.  
  
### <a name="sample-code"></a>Exemplo de código  
 Para obter instruções passo a passo sobre como escrever serviços WCF que usam o MSMQ, consulte os seguintes tópicos:  
  
- [Como trocar mensagens com pontos de extremidade do WCF e aplicativos de enfileiramento de mensagens](how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md)  
  
- [Como fazer intercâmbio de mensagens em fila com pontos de extremidade do WCF](how-to-exchange-queued-messages-with-wcf-endpoints.md)  
  
 Para obter um exemplo de código concluído ilustrando o uso do MSMQ no WCF, consulte os seguintes tópicos:  
  
- [Associação transacionada do MSMQ](../samples/transacted-msmq-binding.md)  
  
- [Comunicação em fila volátil](../samples/volatile-queued-communication.md)  
  
- [Filas de mensagens de inatividade](../samples/dead-letter-queues.md)  
  
- [Sessões e filas](../samples/sessions-and-queues.md)  
  
- [Comunicação bidirecional](../samples/two-way-communication.md)
  
- [SRMP](../samples/srmp.md)  
  
- [Segurança de mensagem através do enfileiramento de mensagem](../samples/message-security-over-message-queuing.md)  
  
## <a name="see-also"></a>Consulte também

- [Pontos de extremidade de serviço e endereçamento de fila](service-endpoints-and-queue-addressing.md)
- [Hospedagem na Web de um aplicativo na fila](web-hosting-a-queued-application.md)
