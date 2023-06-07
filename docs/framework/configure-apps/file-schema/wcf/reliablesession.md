---
title: <reliableSession>
ms.date: 03/30/2017
ms.assetid: 129b4a59-37f0-4030-b664-03795d257d29
ms.openlocfilehash: 95f6646041dc2dd7bae7691a0a9f748c844f50b6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738751"
---
# \<reliableSession>
Define a configuração para mensagens WS-Reliable. Quando esse elemento é adicionado a uma associação personalizada, o canal resultante pode dar suporte a garantias de entrega exatamente uma vez.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<reliableSession>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<reliableSession acknowledgementInterval="TimeSpan"
                 flowControlEnabled="Boolean"
                 inactivityTimeout="TimeSpan"
                 maxPendingChannels="Integer"
                 maxRetryCount="Integer"
                 maxTransferWindowSize="Integer"
                 reliableMessagingVersion="Default/WSReliableMessagingFebruary2005/WSReliableMessaging11"
                 ordered="Boolean" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|acknowledgementInterval|Um <xref:System.TimeSpan> valor que contém o intervalo de tempo máximo que o canal vai aguardar para enviar uma confirmação para as mensagens recebidas até esse ponto. O padrão é 00:00:0.2.|  
|flowControlEnabled|Um valor booliano que indica se o controle de fluxo avançado, uma implementação específica da Microsoft de controle de fluxo para mensagens WS-Reliable, está ativado. O padrão é `true`.|  
|inactivityTimeout|Um <xref:System.TimeSpan> valor que especifica a duração máxima em que o canal irá permitir que a outra parte de comunicação não envie mensagens antes de falhar o canal. O padrão é 00:10:00.<br /><br /> A atividade em um canal é definida como recebendo mensagens de aplicativo ou de infraestrutura. Essa propriedade controla a quantidade máxima de tempo para manter uma sessão inativa ativa. Se o tempo mais longo passar sem atividade, a sessão será anulada pela infraestrutura e pelas falhas de canal. **Observação:**  Não é necessário que o aplicativo envie mensagens periodicamente para manter a conexão ativa.|  
|maxPendingChannels|Um inteiro que especifica o número máximo de canais que podem aguardar a aceitação do ouvinte. Esse valor deve estar entre 1 e 16384, inclusive. O padrão é 4.<br /><br /> Os canais estão pendentes quando estão aguardando para serem aceitos. Depois que esse limite for atingido, nenhum canal será criado. Em vez disso, eles são colocados no modo pendente até que esse número fique inativo (aceitando canais pendentes). Esse é um limite por fábrica.<br /><br /> Quando o limite é atingido e um aplicativo remoto tenta estabelecer uma nova sessão confiável, a solicitação é negada e a operação aberta que solicitou essa falha. Esse limite não se aplica ao número de canais de saída pendentes.|  
|maxRetryCount|Um inteiro que especifica o número máximo de vezes que um canal confiável tenta retransmitir uma mensagem para a qual não recebeu uma confirmação, chamando Send em seu canal subjacente.<br /><br /> Esse valor deve ser maior que zero. O padrão é 8.<br /><br /> Esse valor deve ser um número inteiro maior que zero. Se uma confirmação não for recebida após a última retransmissão, as falhas de canal.<br /><br /> Uma mensagem será considerada para ser transferida se sua entrega no destinatário tiver sido confirmada pelo destinatário.<br /><br /> Se uma confirmação não tiver sido recebida dentro de um determinado período de tempo para uma mensagem que foi transmitida, a infraestrutura retransmitirá automaticamente a mensagem. A infraestrutura tenta reenviar a mensagem para, no máximo, o número de vezes especificado por essa propriedade. Se uma confirmação não for recebida após a última retransmissão, as falhas de canal.<br /><br /> A infraestrutura usa um algoritmo de retirada exponencial para determinar quando retransmitir, com base em um tempo médio de ida e volta calculado. O tempo inicialmente começa em 1 segundo antes da retransmissão e dobrando o atraso com cada tentativa, o que resulta em aproximadamente 8,5 minutos passando entre a primeira tentativa de transmissão e a última tentativa de retransmissão. O tempo para a primeira tentativa de retransmissão é ajustado de acordo com o tempo de viagem de ida e volta calculado e o Stretch de tempo que essas tentativas levam variam de acordo. Isso permite que o tempo de retransmissão se adapte dinamicamente a diferentes condições de rede.|  
|maxTransferWindowSize|Um inteiro que especifica o tamanho máximo do buffer. Os valores válidos são de 1 a 4096, inclusive.<br /><br /> No cliente, esse atributo define o tamanho máximo do buffer usado por um canal confiável para manter as mensagens ainda não confirmadas pelo destinatário. A unidade da cota é uma mensagem. Se o buffer estiver cheio, outras operações de envio serão bloqueadas.<br /><br /> No receptor, esse atributo define o tamanho máximo do buffer usado pelo canal para armazenar as mensagens de entrada ainda não expedidas para o aplicativo. Se o buffer estiver cheio, as mensagens adicionais serão silenciosamente descartadas pelo receptor e exigirão a retransmissão pelo cliente.|  
|ordered|Um booliano que especifica se as mensagens são garantidas de chegar na ordem em que foram enviadas. Se essa configuração for `false` , as mensagens poderão chegar fora de ordem. O padrão é `true`.|  
|reliableMessagingVersion|Um valor válido de <xref:System.ServiceModel.ReliableMessagingVersion> que especifica a versão WS-ReliableMessaging a ser usada.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 As sessões confiáveis fornecem recursos para mensagens e sessões confiáveis. A Reliable Messaging repete a comunicação em caso de falha e permite que as garantias de entrega, como a chegada da ordem das mensagens a serem especificadas. As sessões mantêm o estado para clientes entre chamadas. Esse elemento também fornece opcionalmente a entrega de mensagens ordenadas. Essa sessão implementada pode cruzar o SOAP e transportar intermediários.  
  
 Cada elemento de associação representa uma etapa de processamento ao enviar ou receber mensagens. Em tempo de execução, os elementos de associação criam fábricas e ouvintes de canal que são necessários para criar pilhas de canal de entrada e de saída necessárias para enviar e receber mensagens. O `reliableSession` fornece uma camada opcional na pilha que pode estabelecer uma sessão confiável entre pontos de extremidade e configurar o comportamento dessa sessão.  
  
 Para obter mais informações, consulte [sessões confiáveis](../../../wcf/feature-details/reliable-sessions.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra como configurar uma associação personalizada com vários elementos de codificação de transporte e de mensagem, especialmente permitindo sessões confiáveis, que mantém o estado do cliente e especifica garantias de entrega em ordem. Esse recurso é configurado nos arquivos de configuração do aplicativo para o cliente e o serviço. O exemplo mostra a configuração do serviço.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.serviceModel>
    <services>
      <service name="Microsoft.ServiceModel.Samples.CalculatorService"
               behaviorConfiguration="CalculatorServiceBehavior">
        <!-- This endpoint is exposed at the base address provided by host: http://localhost/servicemodelsamples/service.svc -->
        <!-- specify customBinding binding and a binding configuration to use -->
        <endpoint address=""
                  binding="customBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
        <!-- The mex endpoint is exposed at http://localhost/servicemodelsamples/service.svc/mex -->
        <endpoint address="mex"
                  binding="mexHttpBinding"
                  contract="IMetadataExchange" />
      </service>
    </services>
    <!-- custom binding configuration - configures HTTP transport, reliable sessions -->
    <bindings>
      <customBinding>
        <binding name="Binding1">
          <reliableSession />
          <security authenticationMode="SecureConversation"
                    requireSecurityContextCancellation="true">
          </security>
          <compositeDuplex />
          <oneWay />
          <textMessageEncoding messageVersion="Soap12WSAddressing10"
                               writeEncoding="utf-8" />
          <httpTransport authenticationScheme="Anonymous"
                         bypassProxyOnLocal="false"
                         hostNameComparisonMode="StrongWildcard"
                         proxyAuthenticationScheme="Anonymous"
                         realm=""
                         useDefaultWebProxy="true" />
        </binding>
      </customBinding>
    </bindings>
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceMetadata httpGetEnabled="True" />
          <serviceDebug includeExceptionDetailInFaults="False" />
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
</configuration>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ReliableSessionElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Channels.ReliableSessionBindingElement>
- [Sessões confiáveis](../../../wcf/feature-details/reliable-sessions.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
