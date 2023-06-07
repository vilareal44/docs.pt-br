---
title: <msmqIntegration>
ms.date: 03/30/2017
ms.assetid: ab677405-1ffe-457a-803f-00c1770e51e2
ms.openlocfilehash: 143557833457f379d410c3b71d87199a5b9e783b
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738899"
---
# \<msmqIntegration>
Especifica um transporte MSMQ para associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<msmqIntegration>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<msmqIntegration customDeadLetterQueue="Uri"
                 deadLetterQueue="Custom/None/System"
                 durable="Boolean"
                 exactlyOnce="Boolean"
                 manualAddressing="Boolean"
                 maxBufferPoolSize="Integer"
                 maxImmediateRetries="Integer"
                 maxReceivedMessageSize="Integer"
                 maxRetryCycles="Integer"
                 rejectAfterLastRetry="Boolean"
                 retryCycleDelay="TimeSpan"
                 serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
                 timeToLive="TimeSpan"
                 useSourceJournal="Boolean"
                 useMsmqTracing="Boolean">
  <msmqTransportSecurity>
  </msmqTransportSecurity>
</msmqIntegration>
```  
  
## <a name="type"></a>Type  
 `Type`  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|customDeadLetterQueue|Um URI que indica o local da fila de mensagens mortas por aplicativo, em que os emails expirados ou que falharam ao serem entregues ao aplicativo são transferidos.<br /><br /> Para mensagens que exigem garantias de ExactlyOnce (ou seja, `exactlyOnce` é definida como `true` ), esse atributo usa como padrão a fila de mensagens mortas transacionais em todo o sistema no MSMQ.<br /><br /> Para mensagens que não exigem nenhuma garantia (ou seja, `exactlyOnce` é definida como `false` ), esse atributo usa como padrão `null` .<br /><br /> O valor deve usar o esquema net. MSMQ. O padrão é `null`.<br /><br /> Se `deadLetterQueue` é definido como `None` ou `System` , esse atributo deve ser definido como `null` . Se esse atributo não for `null` , `deadLetterQueue` deverá ser definido como `Custom` .|  
|deadLetterQueue|Especifica o tipo de fila de mensagens mortas a ser usado.<br /><br /> Os valores válidos incluem<br /><br /> -Personalizado: fila de mensagens mortas personalizada.<br />-Nenhum: nenhuma fila de mensagens mortas deve ser usada.<br />-System: Use a fila de mensagens mortas do sistema.<br /><br /> Esse atributo é do tipo DeadLetterQueue.|  
|durável|Um valor booliano que especifica se as mensagens processadas por essa associação são duráveis ou voláteis. O padrão é `true`.<br /><br /> Uma mensagem durável sobrevive a uma falha do Gerenciador de filas, enquanto uma mensagem volátil não. As mensagens voláteis são úteis quando os aplicativos exigem latência mais baixa e podem tolerar mensagens perdidas ocasionais.<br /><br /> Se `exactlyOnce` é definido como `true` , as mensagens devem ser duráveis.|  
|exactlyOnce|Um booliano que especifica se as mensagens processadas por essa associação serão recebidas exatamente uma vez. O padrão é `true`.<br /><br /> Uma mensagem pode ser enviada com ou sem garantias. Uma garantia permite que um aplicativo Verifique se uma mensagem enviada atingiu a fila de mensagens de recebimento ou, caso não tenha feito isso, o aplicativo pode determinar isso lendo a fila de mensagens mortas.<br /><br /> `exactlyOnce`, quando definido como `true` , indica que o MSMQ garantirá que uma mensagem enviada seja entregue ao recebimento da fila de mensagens apenas uma vez e, se a entrega falhar, a mensagem será enviada para a fila da mensagem de inatividade.<br /><br /> As mensagens enviadas com `exactlyOnce` set para `true` devem ser enviadas somente a uma fila transacional.|  
|manualAddressing|Um valor booliano que permite ao usuário assumir o controle do endereçamento de mensagens. Essa propriedade geralmente é usada em cenários de roteador, em que o aplicativo determina a qual um dos vários destinos enviar uma mensagem.<br /><br /> Quando definido como `true` , o canal pressupõe que a mensagem já foi resolvida e não adiciona nenhuma informação adicional a ela. O usuário pode então endereçar cada mensagem individualmente.<br /><br /> Quando definido como `false` , o mecanismo de endereçamento de Windows Communication Foundation padrão (WCF) cria automaticamente endereços para todas as mensagens.<br /><br /> O padrão é `false`.|  
|maxBufferPoolSize|Um inteiro positivo que especifica o tamanho máximo do pool de buffers. O padrão é 524288.<br /><br /> Muitas partes do WCF usam buffers. Criar e destruir buffers cada vez que eles são usados é caro, e a coleta de lixo para buffers também é dispendiosa. Com os pools de buffers, você pode pegar um buffer do pool, usá-lo e retorná-lo ao pool quando terminar. Portanto, a sobrecarga na criação e na destruição de buffers é evitada.|  
|maxImmediateRetries|Um inteiro que especifica o número máximo de tentativas de repetição imediatas em uma mensagem que é lida da fila do aplicativo. O padrão é 5.<br /><br /> Se o número máximo de tentativas imediatas para a mensagem for tentado e a mensagem não for consumida pelo aplicativo, a mensagem será enviada a uma fila de repetição para tentar novamente em algum momento posterior. Se nenhum ciclo de repetição for especificado, as mensagens serão enviadas para a fila de mensagens suspeitas ou uma confirmação negativa será enviada de volta ao remetente.|  
|maxReceivedMessageSize|Um inteiro positivo que especifica o tamanho máximo da mensagem em bytes, incluindo cabeçalhos. O remetente de uma mensagem recebe uma falha de SOAP quando a mensagem é muito grande para o destinatário. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536.|  
|maxRetryCycles|Um inteiro que especifica o número máximo de ciclos de repetição para tentar a entrega de mensagens para o aplicativo de recebimento. O padrão é <xref:System.Int32.MaxValue>.<br /><br /> Um único ciclo de repetição tenta entregar uma mensagem a um aplicativo um número de vezes especificado. O número de tentativas feitas é definido pelo `maxImmediateRetries` atributo. Se o aplicativo não conseguir consumir a mensagem depois que as tentativas na entrega tiverem sido esgotadas, a mensagem será enviada para uma fila de repetição. Os ciclos de repetição subsequentes consistem na mensagem que está sendo retornada da fila de repetição para a fila do aplicativo para tentar a entrega para o aplicativo novamente, após um atraso especificado pelo `retryCycleDelay` atributo. O `maxRetryCycles` atributo especifica o número de ciclos de repetição que o aplicativo usa para tentar entregar a mensagem.|  
|rejectAfterLastRetry|Um valor booliano que especifica a ação a ser tomada para uma mensagem com falha na entrega após o número máximo de tentativas.<br /><br /> `true`significa que uma confirmação negativa é retornada ao remetente e a mensagem é descartada; `false`significa que a mensagem é enviada para a fila de mensagens suspeitas. O padrão é `false`.<br /><br /> Se o valor for `false` , o aplicativo de recebimento poderá ler a fila de mensagens suspeitas para processar mensagens suspeitas (ou seja, mensagens que falharam na entrega).<br /><br /> O MSMQ 3,0 não dá suporte ao retorno de uma confirmação negativa para o remetente, portanto, esse atributo será ignorado no MSMQ 3,0.|  
|retryCycleDelay|Um <xref:System.TimeSpan> que especifica o tempo de espera entre os ciclos de repetição ao tentar entregar uma mensagem que não pôde ser entregue imediatamente. O padrão é 00:10:00.<br /><br /> Um único ciclo de repetição tenta entregar uma mensagem a um aplicativo de recebimento um número especificado de vezes. O número de tentativas feitas é especificado pelo `maxImmediateRetries` atributo. Se o aplicativo não conseguir consumir a mensagem após o número especificado de repetições imediatas, a mensagem será enviada para uma fila de repetição. Os ciclos de repetição subsequentes consistem na mensagem que está sendo retornada da fila de repetição para a fila do aplicativo para tentar a entrega para o aplicativo novamente, após um atraso especificado pelo `retryCycleDelay` atributo. O número de ciclos de repetição é especificado pelo `maxRetryCycles` atributo.|  
|serializationFormat|Especifica o formatador que é usado para serializar objetos que são enviados como parte de uma mensagem MSMQ. Os valores válidos são<br /><br /> -ActiveX: o formatador ActiveX é usado ao serializar objetos COM.<br />-Binary: serializa o objeto em um pacote binário.<br />-ByteArray: serializa o objeto para uma matriz de bytes.<br />-Stream: serializa o objeto em um fluxo.<br />-XML: serializa o objeto em um pacote XML. O padrão é XML.<br /><br /> Esse atributo é do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat> .|  
|timeToLive|Um <xref:System.TimeSpan> que especifica por quanto tempo as mensagens são válidas antes de expirarem e são colocadas na fila de mensagens mortas. O padrão é 1,00:00:00, o que significa 1 dia.<br /><br /> Esse atributo é definido para garantir que as mensagens sensíveis ao tempo não se tornem obsoletas antes de serem processadas pelos aplicativos de recebimento. Uma mensagem em uma fila que não é consumida pelo aplicativo de recebimento dentro do intervalo de tempo especificado é considerada expirada. As mensagens expiradas são enviadas para a fila especial chamada fila de mensagens mortas. O local da fila de mensagens mortas é definido com o `customDeadLetterQueue` atributo ou com o padrão apropriado, com base em garantias.|  
|useMsmqTracing|Um valor booliano que especifica se as mensagens processadas por essa associação devem ser rastreadas. O padrão é `false`.<br /><br /> Quando o rastreamento está habilitado, as mensagens de relatório são criadas e enviadas para a fila de relatório sempre que a mensagem sai ou chega a um computador do serviço de enfileiramento de mensagens.|  
|useSourceJournal|Um valor booliano que especifica se cópias de mensagens processadas por essa associação devem ser armazenadas na fila do diário de origem. O padrão é `false`.<br /><br /> Os aplicativos em fila que desejam manter um registro de mensagens que saíram da fila de saída do computador podem copiar as mensagens para uma fila de diário. Quando uma mensagem sai da fila de saída e uma confirmação é recebida de que a mensagem foi recebida no computador de destino, uma cópia da mensagem é mantida na fila de diário do sistema do computador de envio.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|msmqTransportSecurity|Especifica as configurações de segurança de transporte para esta associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MsmqTransportSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MsmqIntegrationElement>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transportes](../../../wcf/feature-details/transports.md)
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
