---
title: <discoveryEndpoint>
ms.date: 03/30/2017
ms.assetid: fae2f48b-a635-4e4b-859d-a1432ac37e1c
ms.openlocfilehash: 32b14f8fb3235040a51455f2099a403c8312c699
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855400"
---
# \<discoveryEndpoint>

Este elemento de configuração define um ponto de extremidade padrão com um contrato de descoberta fixo. Quando adicionado à configuração de serviço, ele especifica onde ouvir as mensagens de descoberta. Quando adicionado à configuração do cliente, ele especifica para onde enviar as consultas de descoberta.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<discoveryEndpoint>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <discoveryEndpoint>
      <standardEndpoint discoveryMode="Adhoc/Managed"
                        discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxResponseDelay="Timespan"
                        name="String" />
    </discoveryEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos

As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos

| Atributo        | Descrição |  
| ---------------- | ----------- |  
| DiscoveryMode    | Uma cadeia de caracteres que especifica o modo do protocolo de descoberta. Os valores válidos são "adhoc" e "Managed". No modo gerenciado, o protocolo depende de um proxy de descoberta, que atua como um repositório de serviços detectáveis. O modo adhoc requer que o protocolo use o mecanismo de multicast UDP para localizar os serviços disponíveis. Para obter mais informações sobre a propriedade, consulte <xref:System.ServiceModel.Discovery.DiscoveryEndpoint.DiscoveryMode%2A> . |  
| discoveryVersion | Uma cadeia de caracteres que especifica uma das duas versões do protocolo WS-Discovery. Os valores válidos são WSDiscovery11 e WSDiscoveryApril2005. Esse valor é do tipo <xref:System.ServiceModel.Discovery.DiscoveryVersion>. |  
| maxResponseDelay | Um valor TimeSpan que especifica o valor máximo para o atraso que o protocolo de descoberta aguardará antes de enviar determinadas mensagens, como correspondência de investigação ou resolução de correspondência.<br /><br /> Se todos os ProbeMatches forem enviados ao mesmo tempo, um Storm de rede poderá resultar. Para evitar que isso ocorra, os ProbeMatches são enviados com um atraso aleatório entre cada ProbeMatch. O atraso aleatório está no intervalo de 0 até o valor definido por esse atributo. Se esse atributo for definido como 0, as mensagens ProbeMatches serão enviadas em um loop rígido sem nenhum atraso. Caso contrário, as mensagens ProbeMatches são enviadas com um atraso aleatório, de modo que o tempo total necessário para enviar todas as mensagens de ProbeMatches não exceda o maxResponseDelay. Esse valor só é relevante para os serviços, não é usado por clientes. |  
| `name`           | Uma cadeia de caracteres que especifica o nome da configuração do ponto de extremidade padrão. O nome é usado no `endpointConfiguration` atributo do ponto de extremidade de serviço para vincular um ponto de extremidade padrão à sua configuração. |  
  
### <a name="child-elements"></a>Elementos filho

Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai

| Elemento | Descrição |  
| ------- | ----------- |  
| [\<standardEndpoints>](standardendpoints.md) | Uma coleção de pontos de extremidade padrão que são pontos de extremidade predefinidos com uma ou mais de suas propriedades (endereço, associação, contrato) fixa. |  
  
## <a name="example"></a>Exemplo

O exemplo a seguir demonstra um serviço de escuta nas mensagens de descoberta por meio de um transporte de multicast de rede par. O exemplo especifica explicitamente a versão de abril de 2005 de WS-Discovery.  
  
A configuração de ponto de extremidade padrão é definida por serviço e não pode ser compartilhada entre o serviço. Se outro serviço quiser ter o mesmo ponto de extremidade de descoberta, a mesma configuração precisará ser adicionada à seção desse serviço.  
  
```xml  
<services>
  <service name="CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <endpoint binding="basicHttpBinding"
              address="calculator"
              contract="ICalculatorService" />
    <endpoint name="peerNetDiscovery"
              binding="peerTcpBinding"
              address="net.p2p://discoveryMesh/multicast"
              kind="discoveryEndpoint"
              endpointConfiguration="peerTcpDiscoveryEndpointConfiguration"
              bindingConfiguration="discoveryPeerTcpBindingConfig" />
  </service>
</services>
<standardEndpoints>
  <discoveryEndpoint>
    <standardEndpoint name="peerTcpDiscoveryEndpointConfiguration"
                      version="WSDiscoveryApril2005" />
  </discoveryEndpoint>
</standardEndpoints>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.DiscoveryEndpoint>
