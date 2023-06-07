---
title: <standardEndpoints>
ms.date: 03/30/2017
ms.assetid: d62153d7-a6e6-462a-a784-cca61e9c2ba1
ms.openlocfilehash: 76a5303650c4e2b2887d29f511d3088c78b58fe2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399510"
---
# \<standardEndpoints>
Esta seção de configuração permite que você defina uma coleção de pontos de extremidade padrão, que são pontos de extremidade pré-configurados reutilizáveis. Um ponto de extremidade padrão terá um ou mais dos atributos de endereço, associação e contrato definidos como um valor fixo. Por exemplo, no ponto de extremidade de descoberta, o contrato é fixo. Você também pode usar pontos de extremidade padrão para estender o ponto final de serviço com novas propriedades semelhantes à definição de associações personalizadas.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<standardEndpoints>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<announcementEndpoint>](announcementendpoint.md)|Define um ponto de extremidade padrão com um contrato de comunicado fixo. Um serviço pode, opcionalmente, anunciar sua disponibilidade enviando uma mensagem de anúncio online e offline quando ela é aberta ou fechada, respectivamente. Um serviço Windows Communication Foundation (WCF) especifica os pontos de extremidade do comunicado no [\<serviceDiscovery>](servicediscovery.md) elemento e usa o AnnouncementClient para executar os anúncios. Um cliente que deseja escutar o anúncio de outro serviço está realmente agindo como um serviço WCF; Portanto, você precisa configurar os pontos de extremidade do comunicado para esse cliente na [\<services>](services.md) seção.|  
|[\<discoveryEndpoint>](discoveryendpoint.md)|Define um ponto de extremidade padrão com um contrato de descoberta fixo. Quando adicionado à configuração de serviço, ele especifica onde ouvir as mensagens de descoberta. Quando adicionado à configuração do cliente, ele especifica para onde enviar as consultas de descoberta.|  
|[\<dynamicEndpoint>](dynamicendpoint.md)|Este elemento de configuração define um ponto de extremidade padrão que contém informações para permitir que um aplicativo funcione como um programa cliente que pode encontrar o endereço do ponto de extremidade dinamicamente no tempo de execução.|  
|[\<mexEndpoint>](mexendpoint.md)|Define um ponto de extremidade padrão com um contrato IMetadataExchange fixo. Como todos os pontos de extremidade de troca de metadados especificam IMetadataExchange como seu contrato, você pode usar esse ponto padrão em vez de definir um para você mesmo.|  
|[\<udpAnnouncementEndpoint>](udpannouncementendpoint.md)|Define um ponto de extremidade padrão que é usado pelos serviços para enviar mensagens de anúncio por uma associação UDP. Ele tem um contrato fixo e dá suporte a duas versões de descoberta. Além disso, ele tem uma associação de UDP fixa e um valor de endereço padrão, conforme as especificações do WS-Discovery (WS-Discovery de abril de 2005 ou WS-Discovery versão 1.1). Você pode especificar o endereço de multicast a ser usado para enviar e receber as mensagens de anúncio.|  
|[\<udpDiscoveryEndpoint>](udpdiscoveryendpoint.md)|Define um ponto de extremidade padrão que é pré-configurado para operações de descoberta em uma associação multicast UDP. Esse ponto de extremidade tem um contrato fixo e dá suporte a duas versões do protocolo WS-Discovery. Além disso, ele tem uma associação de UDP fixa e um endereço padrão, conforme as especificações do WS-Discovery (WS-Discovery de abril de 2005 ou WS-Discovery V1.1).|  
|[\<webHttpEndpoint>](webhttpendpoint.md)|Define um ponto de extremidade padrão com uma [\<webHttpBinding>](webhttpbinding.md) Associação fixa que adiciona automaticamente o [\<webHttp>](webhttp.md) comportamento. Use esse ponto de extremidade ao escrever um serviço REST.|  
|[\<webScriptEndpoint>](webscriptendpoint.md)|Define um ponto de extremidade padrão com uma [\<webHttpBinding>](webhttpbinding.md) Associação fixa que adiciona automaticamente o [\<enableWebScript>](enablewebscript.md) comportamento. Use esse ponto de extremidade quando estiver escrevendo um serviço que é chamado de um aplicativo ASP.NET AJAX.|  
|[\<workflowControlEndpoint>](workflowcontrolendpoint.md)|Define um ponto de extremidade padrão para controlar a execução de instâncias de fluxo de trabalho (criar, executar, suspender, encerrar, etc.).|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|\<system.ServiceModel>|O elemento raiz de todos os elementos de configuração do WCF.|  
  
## <a name="see-also"></a>Confira também

- [Pontos de extremidade padrão](../../../wcf/feature-details/standard-endpoints.md)
