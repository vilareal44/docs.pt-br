---
title: <msmqIntegrationBinding>
ms.date: 03/30/2017
helpviewer_keywords:
- msmqIntegrationBinding Element
ms.assetid: edf277f3-e3bf-4ed8-9f55-83b5788430a7
ms.openlocfilehash: ba28a81dd2ea0684ed863821afd3a8f31c0fb064
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140773"
---
# \<msmqIntegrationBinding>
Define uma associação que fornece suporte ao enfileiramento Roteando mensagens por meio do MSMQ.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<msmqIntegrationBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<msmqIntegrationBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           receiveContextEnabled="Boolean"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           serializationFormat="XML/Binary/ActiveX/ByteArray/Stream"
           timeToLive="TimeSpan"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
  </binding>
</msmqIntegrationBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|closeTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|customDeadLetterQueue|Um URI que contém o local da fila de mensagens mortas por aplicativo, em que são colocadas aquelas que expiraram ou que têm a transferência ou entrega com falha.<br /><br /> A fila de mensagens mortas é uma fila no Gerenciador de filas do aplicativo de envio para a mensagem expirada que falhou ao ser entregue.<br /><br /> O URI especificado por <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> deve usar o esquema net. MSMQ.|  
|deadLetterQueue|A <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> . Value especificando o tipo de fila de mensagens mortas a ser usado, se houver<br /><br /> Uma fila de mensagens mortas é o local em que os emails que falharam ao serem entregues ao aplicativo serão transferidos.<br /><br /> Para mensagens que exigem garantia de exactlyOnce (ou seja, o `exactlyOnce` atributo é definido como `true` ), esse atributo usa como padrão a fila de mensagens mortas transacionais em todo o sistema no MSMQ.<br /><br /> Para mensagens que não exigem garantias, esse atributo usa como padrão `null` .|  
|durável|Um valor booliano que indica se a mensagem é durável ou volátil na fila. Uma mensagem durável sobrevive a uma falha do Gerenciador de filas, enquanto uma mensagem volátil não. As mensagens voláteis são úteis quando os aplicativos exigem latência mais baixa e podem tolerar mensagens perdidas ocasionais. Se o `exactlyOnce` atributo for definido como `true` , as mensagens deverão ser duráveis. O padrão é `true`.|  
|exactlyOnce|Um valor booliano que indica se cada mensagem é entregue apenas uma vez. O remetente será notificado de falhas de entrega. Quando `durable` é `false` , esse atributo é ignorado e as mensagens são transferidas sem a garantia de entrega. O padrão é `true`. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>.|  
|maxReceivedMessageSize|Um inteiro positivo que define o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que é processado por essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536. Esse limite de tamanho de mensagem destina-se a limitar a exposição a ataques de negação de serviço (DoS).|  
|maxRetryCycles|Um inteiro que indica o número de ciclos de repetição usados pelo recurso de detecção de mensagens suspeitas. Uma mensagem se torna uma mensagem suspeita quando ela falha em todas as tentativas de entrega de todos os ciclos. O padrão é 2. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>.|  
|name|Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|openTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|receiveErrorHandling|Um <xref:System.ServiceModel.ReceiveErrorHandling> valor que especifica como as mensagens suspeitas e não exdistribuíveis são tratadas.|  
|receiveRetryCount|Um inteiro que especifica o número máximo de tentativas imediatas que o Gerenciador de fila deve tentar se a transmissão de uma mensagem da fila do aplicativo para o aplicativo falhar.<br /><br /> Se o número máximo de tentativas de entrega for atingido e a mensagem não for acessada pelo aplicativo, a mensagem será enviada a uma fila de repetição para entrega novamente mais tarde. O período de tempo antes que a mensagem seja transferida de volta para a fila de envio é controlada pelo `retryCycleDelay` . Se os ciclos de repetição atingirem o `maxRetryCycles` valor, a mensagem será enviada para a fila de mensagens suspeitas ou uma confirmação negativa será enviada de volta ao remetente.|  
|receiveTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|  
|receiveContextEnabled|Um booliano que especifica se o contexto de recebimento para o processamento de mensagens em filas está habilitado. Quando isso é definido como `true` , um serviço pode "inspecionar" uma mensagem na fila para começar a processá-la e, se algo der errado e uma exceção for lançada, ela permanecerá na fila. Os serviços também podem "bloquear" mensagens para tentar novamente o processamento em um momento posterior. ReceiveContext fornece um mecanismo para "Concluir" a mensagem uma vez processada para que ela possa ser removida da fila. As mensagens não são mais lidas e regravadas em filas pela rede, e as mensagens individuais não estão saltando em diferentes instâncias de serviço durante o processamento.|  
|retryCycleDelay|Um valor TimeSpan que especifica o atraso de tempo entre os ciclos de repetição ao tentar entregar uma mensagem que não pôde ser entregue imediatamente. O valor define apenas o tempo de espera mínimo porque o tempo de espera real pode ser maior. O valor padrão é 00:30:00. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>.|  
|sendTimeout|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|serializationFormat|Define o formato usado para a serialização do corpo da mensagem. Esse atributo é do tipo <xref:System.ServiceModel.MsmqIntegration.MsmqMessageSerializationFormat> .|  
|timeToLive|Um valor TimeSpan que especifica por quanto tempo as mensagens são válidas antes de expirarem e serem colocadas na fila de mensagens mortas. O padrão é 1,00:00:00.<br /><br /> Esse atributo é definido para garantir que as mensagens sensíveis ao tempo não se tornem obsoletas antes de serem processadas pelos aplicativos de recebimento. Uma mensagem em uma fila que não é consumida pelo aplicativo de recebimento dentro do intervalo de tempo especificado é considerada expirada. As mensagens expiradas são enviadas para a fila especial chamada fila de mensagens mortas. O local da fila de mensagens mortas é definido com o `DeadLetterQueue` atributo ou com o padrão apropriado, com base em garantias.|  
|useMsmqTracing|Um valor booliano que especifica se as mensagens processadas por essa associação devem ser rastreadas. O padrão é `false`. Quando o rastreamento está habilitado, as mensagens de relatório são criadas e enviadas para a fila de relatório sempre que a mensagem sai ou chega a um computador do serviço de enfileiramento de mensagens.|  
|useSourceJournal|Um valor booliano que especifica cópias de mensagens processadas por essa associação deve ser armazenado no diário de origem. O padrão é `false`.<br /><br /> Os aplicativos em fila que desejam manter um registro de mensagens que saíram da fila de saída do computador podem copiar as mensagens para uma fila de diário. Quando uma mensagem sai da fila de saída e uma confirmação é recebida de que a mensagem foi recebida no computador de destino, uma cópia da mensagem é mantida na fila de diário do sistema do computador de envio.|  
  
## <a name="serializationformat-attribute"></a>{serializationFormat} Attribute  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Xml|Formato XML|  
|Binário|Formato binário|  
|ActiveX|Formato do ActiveX|  
|ByteArray|Serializa o objeto em uma matriz de bytes.|  
|Fluxo|O corpo formatado como um fluxo|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-msmqintegrationbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.MsmqIntegrationSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento de associação pode ser usado para permitir que aplicativos Windows Communication Foundation (WCF) enviem mensagens para e recebam mensagens de aplicativos MSMQ existentes que usam COM, APIs nativas do MSMQ ou os tipos definidos no <xref:System.Messaging?displayProperty=nameWithType> namespace você pode usar esse elemento de configuração para especificar maneiras de endereçar a fila, garantir a transferência, se as mensagens devem ser permanentemente armazenadas e como as mensagens devem ser protegidas e autenticadas Para obter mais informações, consulte [como: trocar mensagens com pontos de extremidade WCF e aplicativos de enfileiramento de mensagens](../../../wcf/feature-details/how-to-exchange-messages-with-wcf-endpoints-and-message-queuing-applications.md).  
  
## <a name="example"></a>Exemplo  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <msmqIntegrationBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxImmediateRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 useSourceJournal="true"
                 useMsmqTracing="true"
                 serializationFormat="Binary">
          <security mode="None" />
        </binding>
      </msmqIntegrationBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.MsmqIntegrationBindingElement>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBinding>
- <xref:System.ServiceModel.MsmqIntegration.MsmqIntegrationBindingElement>
- [\<binding>](bindings.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
