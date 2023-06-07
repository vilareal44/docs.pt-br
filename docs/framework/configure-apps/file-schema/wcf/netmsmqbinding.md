---
title: <netMsmqBinding>
ms.date: 03/30/2017
ms.assetid: a68b44d7-7799-43a3-9e63-f07c782810a6
ms.openlocfilehash: 7456c6373c64e07b73e15e7e2bb229dce4032121
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140740"
---
# \<netMsmqBinding>
Define uma associação em fila adequada para comunicação entre computadores.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<netMsmqBinding>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<netMsmqBinding>
  <binding closeTimeout="TimeSpan"
           customDeadLetterQueue="Uri"
           deadLetterQueue="Uri"
           durable="Boolean"
           exactlyOnce="Boolean"
           maxBufferPoolSize="Integer"
           maxReceivedMessageSize="Integer"
           maxRetryCycles="Integer"
           name="String"
           openTimeout="TimeSpan"
           poisonMessageHandling="Disabled/EnabledIfSupported"
           queueTransferProtocol="Native/Srmp/SrmpSecure"
           receiveErrorHandling="Drop/Fault/Move/Reject"
           receiveTimeout="TimeSpan"
           receiveRetryCount="Integer"
           rejectAfterLastRetry="Boolean"
           retryCycleDelay="TimeSpan"
           sendTimeout="TimeSpan"
           timeToLive="TimeSpan"
           useActiveDirectory="Boolean"
           useMsmqTracing="Boolean"
           useSourceJournal="Boolean">
    <security>
      <message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
               clientCredentialType="None/Windows/UserName/Certificate/InfoCard" />
      <transport msmqAuthenticationMode="None/WindowsDomain/Certificate"
                 msmqEncryptionAlgorithm="RC4Stream/AES"
                 msmqProtectionLevel="None/Sign/EncryptAndSign"
                 msmqSecureHashAlgorithm="MD5/SHA1/SHA256/SHA512" />
    </security>
    <readerQuotas maxArrayLength="Integer"
                  maxBytesPerRead="Integer"
                  maxDepth="Integer"
                  maxNameTableCharCount="Integer"
                  maxStringContentLength="Integer" />
  </binding>
</netMsmqBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`closeTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de fechamento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`customDeadLetterQueue`|Um URI que contém o local da fila de mensagens mortas por aplicativo, em que são colocadas aquelas que expiraram ou que têm a transferência ou entrega com falha.<br /><br /> A fila de mensagens mortas é uma fila no Gerenciador de filas do aplicativo de envio para a mensagem expirada que falhou ao ser entregue.<br /><br /> O URI especificado por <xref:System.ServiceModel.MsmqBindingBase.CustomDeadLetterQueue%2A> deve usar o esquema net. MSMQ.|  
|`deadLetterQueue`|Um <xref:System.ServiceModel.MsmqBindingBase.DeadLetterQueue%2A> valor que especifica o tipo de fila de mensagens mortas a ser usado, se houver.<br /><br /> Uma fila de mensagens mortas é o local em que os emails que falharam ao serem entregues ao aplicativo serão transferidos.<br /><br /> Para mensagens que exigem `exactlyOnce` garantia (ou seja, o `exactlyOnce` atributo é definido como `true` ), esse atributo usa como padrão a fila de mensagens mortas transacionais em todo o sistema no MSMQ.<br /><br /> Para mensagens que não exigem garantias, esse atributo usa como padrão `null` .|  
|`durable`|Um valor booliano que indica se a mensagem é durável ou volátil na fila. Uma mensagem durável sobrevive a uma falha do Gerenciador de filas, enquanto uma mensagem volátil não. As mensagens voláteis são úteis quando os aplicativos exigem latência mais baixa e podem tolerar mensagens perdidas ocasionais. Se o `exactlyOnce` atributo for definido como `true` , as mensagens deverão ser duráveis. O padrão é `true`.|  
|`exactlyOnce`|Um valor booliano que indica se cada mensagem processada por essa associação é entregue apenas uma vez. O remetente será notificado de falhas de entrega. Quando `durable` é `false` , esse atributo é ignorado e as mensagens são transferidas sem a garantia de entrega. O padrão é `true`. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.ExactlyOnce%2A>.|  
|`maxBufferPoolSize`|Um inteiro que especifica o tamanho máximo do pool de buffers para essa associação. O padrão é 8.|  
|`maxReceivedMessageSize`|Um inteiro positivo que define o tamanho máximo da mensagem, em bytes, incluindo cabeçalhos, que é processado por essa associação. O remetente de uma mensagem que excede esse limite receberá uma falha de SOAP. O receptor remove a mensagem e cria uma entrada do evento no log de rastreamento. O padrão é 65536. Esse limite de tamanho de mensagem destina-se a limitar a exposição a ataques de negação de serviço (DoS).|  
|`maxRetryCycles`|Um inteiro que indica o número de ciclos de repetição usados pelo recurso de detecção de mensagens suspeitas. Uma mensagem se torna uma mensagem suspeita quando ela falha em todas as tentativas de entrega de todos os ciclos. O padrão é 3. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.MaxRetryCycles%2A>.|  
|`name`|Atributo obrigatório. Uma cadeia de caracteres que contém o nome da configuração da associação. Esse valor deve ser exclusivo porque é usado como uma identificação para a associação. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
|`openTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de abertura. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`QueueTransferProtocol`|Um <xref:System.ServiceModel.QueueTransferProtocol> valor válido que especifica o transporte de canal de comunicação enfileirado que essa associação usa. O MSMQ não oferece suporte ao endereçamento de Active Directory ao usar o protocolo de mensagens confiáveis SOAP. Portanto, você não deve definir esse atributo como `Srmp` ou `Srmps` quando o `useActiveDirectory` atributo é definido como `true` .|  
|`receiveErrorHandling`|Um <xref:System.ServiceModel.ReceiveErrorHandling> valor que especifica como as mensagens suspeitas e não exdistribuíveis são tratadas.|  
|`receiveRetryCount`|Um inteiro que especifica o número máximo de vezes que o Gerenciador de filas deve tentar enviar uma mensagem antes de transferi-la para a fila de repetição.|  
|`receiveTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de recebimento. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:10:00.|  
|`retryCycleDelay`|Um valor TimeSpan que especifica o atraso de tempo entre os ciclos de repetição ao tentar entregar uma mensagem que não pôde ser entregue imediatamente. O valor define apenas o tempo de espera mínimo porque o tempo de espera real pode ser maior. O valor padrão é 00:10:00. Para obter mais informações, consulte <xref:System.ServiceModel.MsmqBindingBase.RetryCycleDelay%2A>.|  
|`sendTimeout`|Um <xref:System.TimeSpan> valor que especifica o intervalo de tempo fornecido para a conclusão de uma operação de envio. Esse valor deve ser maior ou igual a <xref:System.TimeSpan.Zero> . O padrão é 00:01:00.|  
|`timeToLive`|Um valor TimeSpan que especifica por quanto tempo as mensagens são válidas antes de expirarem e serem colocadas na fila de mensagens mortas. O padrão é 1,00:00:00.<br /><br /> Esse atributo é definido para garantir que as mensagens sensíveis ao tempo não se tornem obsoletas antes de serem processadas pelos aplicativos de recebimento. Uma mensagem em uma fila que não é consumida pelo aplicativo de recebimento dentro do intervalo de tempo especificado é considerada expirada. As mensagens expiradas são enviadas para a fila especial chamada fila de mensagens mortas. O local da fila de mensagens mortas é definido com o `DeadLetterQueue` atributo ou com o padrão apropriado, com base em garantias.|  
|`usingActiveDirectory`|Um valor booliano que especifica se os endereços de fila devem ser convertidos usando Active Directory.<br /><br /> Os endereços de fila MSMQ podem consistir em nomes de caminho ou nomes de formato diretos. Com um nome de formato direto, o MSMQ resolve o nome do computador usando DNS, NetBIOS ou IP. Com um nome de caminho, o MSMQ resolve o nome do computador usando Active Directory.<br /><br /> Por padrão, o transporte em fila do Windows Communication Foundation (WCF) converte o URI de uma fila de mensagens em um nome de formato direto. Ao definir a `UseActiveDirectory` propriedade como true, um aplicativo pode especificar que o transporte em fila deve resolver o nome do computador usando Active Directory em vez de DNS, NetBIOS ou IP.|  
|`useMsmqTracing`|Um valor booliano que especifica se as mensagens processadas por essa associação devem ser rastreadas. O padrão é `false`. Quando o rastreamento está habilitado, as mensagens de relatório são criadas e enviadas para a fila de relatório sempre que a mensagem sai ou chega a um computador do serviço de enfileiramento de mensagens.|  
|`useSourceJournal`|Um valor booliano que especifica cópias de mensagens processadas por essa associação deve ser armazenado no diário de origem. O padrão é `false`.<br /><br /> Os aplicativos em fila que desejam manter um registro de mensagens que saíram da fila de saída do computador podem copiar as mensagens para uma fila de diário. Quando uma mensagem sai da fila de saída e uma confirmação é recebida de que a mensagem foi recebida no computador de destino, uma cópia da mensagem é mantida na fila de diário do sistema do computador de envio.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<readerQuotas>](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/ms731325(v=vs.100))|Define as restrições sobre a complexidade de mensagens SOAP que podem ser processadas por pontos de extremidade configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.XmlDictionaryReaderQuotasElement> .|  
|[\<security>](security-of-netmsmqbinding.md)|Define as configurações de segurança para a associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.NetMsmqSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<bindings>](bindings.md)|Esse elemento contém uma coleção de associações padrão e personalizadas.|  
  
## <a name="remarks"></a>Comentários  
 A `netMsmqBinding` associação fornece suporte para enfileiramento aproveitando o MSMQ (Microsoft Message Queuing) como um transporte e habilita o suporte para aplicativos livremente acoplados, isolamento de falha, nivelamento de carga e operações desconectadas. Para obter uma discussão sobre esses recursos, consulte [filas no WCF](../../../wcf/feature-details/queues-in-wcf.md).  
  
## <a name="example"></a>Exemplo  
  
```xml  
<configuration>
  <system.ServiceModel>
    <bindings>
      <netMsmqBinding>
        <binding closeTimeout="00:00:10"
                 openTimeout="00:00:20"
                 receiveTimeout="00:00:30"
                 sendTimeout="00:00:40"
                 deadLetterQueue="net.msmq://localhost/blah"
                 durable="true"
                 exactlyOnce="true"
                 maxReceivedMessageSize="1000"
                 maxRetries="11"
                 maxRetryCycles="12"
                 poisonMessageHandling="Disabled"
                 rejectAfterLastRetry="false"
                 retryCycleDelay="00:05:55"
                 timeToLive="00:11:11"
                 sourceJournal="true"
                 useMsmqTracing="true"
                 useActiveDirectory="true">
          <security>
            <message clientCredentialType="Windows" />
          </security>
        </binding>
      </netMsmqBinding>
    </bindings>
  </system.ServiceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.NetMsmqBinding>
- <xref:System.ServiceModel.Configuration.NetMsmqBindingElement>
- [\<binding>](bindings.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [Filas no WCF](../../../wcf/feature-details/queues-in-wcf.md)
