---
title: <headers>
ms.date: 03/30/2017
ms.assetid: c79b897d-8ea3-40b5-a8b6-2471941f7ed3
ms.openlocfilehash: 76b3cbf6b867a983c203141bcd901b2b7b4038d5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855175"
---
# \<headers>
Um ponto de extremidade pode ser resolvido por um ou mais cabeçalhos SOAP além de seu URI básico. Um conjunto de cenários em que isso é útil é um conjunto de cenários intermediários de SOAP em que um ponto de extremidade exige que clientes desse ponto de extremidade incluam cabeçalhos SOAP direcionados a intermediários. Este elemento de configuração pode ser usado para definir esses cabeçalhos de endereço personalizados. As entradas na coleção de cabeçalhos do ponto de extremidade são elementos XML definidos pelo usuário. Cada elemento deve ser um XML bem formado.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<headers>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<headers>
  <region xmlns="Uri">"String"</region>
  <member xmlns="Uri">"String"</member>
</headers>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
 Elementos XML definidos pelo usuário.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<endpoint>](endpoint-of-client.md)|Configura tipos diferentes de pontos de extremidade.|  
  
## <a name="remarks"></a>Comentários  
 Os cabeçalhos opcionais fornecem informações de endereçamento mais detalhadas para identificar ou interagir com o ponto de extremidade. Por exemplo, os cabeçalhos podem indicar como processar uma mensagem de entrada, em que o ponto de extremidade deve enviar uma mensagem de resposta ou qual instância de um serviço usar para processar uma mensagem de entrada de um usuário específico quando várias instâncias estão disponíveis.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Headers%2A>
- <xref:System.ServiceModel.Channels.AddressHeaderCollection>
- [Pontos de extremidade: endereços, associações e contratos](../../../wcf/feature-details/endpoints-addresses-bindings-and-contracts.md)
