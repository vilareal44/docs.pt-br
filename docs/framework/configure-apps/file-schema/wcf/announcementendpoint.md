---
title: <announcementEndpoint>
ms.date: 03/30/2017
ms.assetid: 034b7c69-a770-4502-8cef-38007bbcd025
ms.openlocfilehash: decaaa1cea5345ff971b16cbb20a85dd803a52d5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850289"
---
# \<announcementEndpoint>
Este elemento de configuração define um ponto de extremidade padrão com um contrato de anúncio fixo. Um serviço pode, opcionalmente, anunciar sua disponibilidade enviando uma mensagem de anúncio online e offline quando ela é aberta ou fechada, respectivamente. Um serviço Windows Communication Foundation (WCF) especifica os pontos de extremidade do comunicado no [\<serviceDiscovery>](servicediscovery.md) elemento e usa o AnnouncementClient para executar os anúncios. Um cliente que deseja escutar o anúncio de outro serviço está realmente agindo como um serviço WCF; Portanto, você precisa configurar os pontos de extremidade do comunicado para esse cliente na [\<services>](services.md) seção.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<announcementEndpoint>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <announcementEndpoint>
      <standardEndpoint discoveryVersion="WSDiscovery11/WSDiscoveryApril2005"
                        maxAnnouncementDelay="Timespan"
                        name="String" />
    </announcementEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|discoveryVersion|Uma cadeia de caracteres que especifica uma das duas versões do protocolo WS-Discovery. Os valores válidos são WSDiscovery11 e WSDiscoveryApril2005. Esse valor é do tipo <xref:System.ServiceModel.Discovery.Configuration.AnnouncementEndpointElement.DiscoveryVersion>.|  
|maxAnnouncementDelay|Um valor TimeSpan que especifica o valor máximo para o atraso que o protocolo de descoberta aguardará antes de enviar uma mensagem de saudação. As mensagens aguardarão um valor de tempo aleatório entre 0 e o valor desse atributo antes de serem enviadas. Esse atributo é usado para definir um atraso pequeno e aleatório para evitar Storm de rede quando uma rede sai e todos os serviços voltam a ficar online ao mesmo tempo.|  
|name|Uma cadeia de caracteres que especifica o nome da configuração do ponto de extremidade padrão. O nome é usado no `endpointConfiguration` atributo do ponto de extremidade de serviço para vincular um ponto de extremidade padrão à sua configuração.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|Uma coleção de pontos de extremidade padrão que são pontos de extremidade predefinidos com uma ou mais de suas propriedades (endereço, associação, contrato) fixa.|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra um cliente que escuta mensagens de anúncios por http e PEERNET.  
  
```xml  
<services>
  <service name="ServiceAnnouncementListener">
    <endpoint name="httpAnnouncementEndpoint"
              kind="announcementEndpoint"
              binding="basicHttpBinding"
              address="announcements" />
    <endpoint name="peerNetAnnouncementEndpoint"
              kind="announcementEndpoint"
              binding="peerTcpBinding"
              address="net.p2p://discoveryMesh/multicast"
              bindingConfiguration="discoveryPeerTcpBindingConfig" />
  ...
  </service>
</services>

<standardEndpoints>
  <announcementEndpoint>
    <standardEndpoint name="httpAnnouncementEndpoint"
                      version="WSDiscoveryApril2005" />
    <standardEndpoint name="peerNetAnnouncementEndpoint"
                      version="WSDiscoveryApril2005" />
  </announcementEndpoint>
</standardEndpoints>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.AnnouncementEndpoint>
