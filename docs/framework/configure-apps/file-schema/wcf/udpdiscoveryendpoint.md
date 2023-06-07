---
title: <udpDiscoveryEndpoint>
ms.date: 03/30/2017
ms.assetid: 1f485329-2771-43bc-88de-df8f2faa3bb7
ms.openlocfilehash: 1729255c68c75f824b8cd8c87f106a4a9b3550f6
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70854884"
---
# \<udpDiscoveryEndpoint>
Este elemento de configuração define um ponto de extremidade padrão que é pré-configurado para operações de descoberta em uma associação de multicast UDP. Esse ponto de extremidade tem um contrato fixo e dá suporte a duas versões do protocolo WS-Discovery. Além disso, ele tem uma associação de UDP fixa e um endereço padrão, conforme as especificações do WS-Discovery (WS-Discovery de abril de 2005 ou WS-Discovery V1.1).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<udpDiscoveryEndpoint>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <udpDiscoveryEndpoint>
      <standardEndpoint discoveryMode="Adhoc/Managed"
                        discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxResponseDelay="Timespan"
                        multicastAddress="Uri"
                        name="String" />
    </udpDiscoveryEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|DiscoveryMode|Uma cadeia de caracteres que especifica o modo do protocolo de descoberta. Os valores válidos são "adhoc" e "Managed". No modo gerenciado, o protocolo depende de um proxy de descoberta, que atua como um repositório de serviços detectáveis. O modo adhoc requer que o protocolo use o mecanismo de multicast UDP para localizar os serviços disponíveis. Esse valor é do tipo <xref:System.ServiceModel.Discovery.ServiceDiscoveryMode>.|  
|discoveryVersion|Uma cadeia de caracteres que especifica uma das duas versões do protocolo WS-Discovery. Os valores válidos são WSDiscovery11 e WSDiscoveryApril2005. Esse valor é do tipo <xref:System.ServiceModel.Discovery.DiscoveryVersion>.|  
|maxResponseDelay|Um valor TimeSpan que especifica o valor máximo para o atraso que o protocolo de descoberta aguardará antes de enviar determinadas mensagens, como correspondência de investigação ou resolução de correspondência.<br /><br /> Se todos os ProbeMatches forem enviados ao mesmo tempo, um Storm de rede poderá resultar. Para evitar que isso ocorra, os ProbeMatches são enviados com um atraso aleatório entre cada ProbeMatch. O atraso aleatório está no intervalo de 0 até o valor definido por esse atributo. Se esse atributo for definido como 0, as mensagens ProbeMatches serão enviadas em um loop rígido sem nenhum atraso. Caso contrário, as mensagens ProbeMatches são enviadas com um atraso aleatório, de modo que o tempo total necessário para enviar todas as mensagens de ProbeMatches não exceda o maxResponseDelay. Esse valor só é relevante para os serviços, não é usado por clientes.|  
|multicastAddress|Um URI que especifica um endereço de multicast a ser usado para enviar e receber as mensagens de descoberta. O valor padrão é o endereço de multicast como compatível com a especificação de protocolo.|  
|`name`|Uma cadeia de caracteres que especifica o nome da configuração do ponto de extremidade padrão. O nome é usado no `endpointConfiguration` atributo do ponto de extremidade de serviço para vincular um ponto de extremidade padrão à sua configuração.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<udpTransportSettings>](udptransportsettings.md)|Uma coleção de configurações que permitem configurar o transporte UDP para o ponto de extremidade UDP.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|Uma coleção de pontos de extremidade padrão que são pontos de extremidade predefinidos com uma ou mais de suas propriedades (endereço, associação, contrato) fixa.|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um serviço de escuta de mensagens de descoberta em um transporte multicast UDP.  
  
```xml  
<services>
  <service name="CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <endpoint binding="basicHttpBinding"
              address="calculator"
              contract="ICalculatorService" />
    <endpoint name="DiscoveryEndpoint"
              kind="udpDiscoveryEndpoint" />
  </service>
  <standardEndpoints>
    <udpDiscoveryEndpoint>
      <standardEndpoint name="DiscoveryEndpoint"
                        version="WSDiscoveryApril2005" />
    </udpDiscoveryEndpoint>
  </standardEndpoints>
</services>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>
