---
title: <webHttp>
ms.date: 03/30/2017
ms.assetid: 1f9d0754-d41e-44ce-a298-e51cb3096c64
ms.openlocfilehash: 00644d248e6fb85d7cf712620e6ac74405e6b0c3
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70399162"
---
# \<webHttp>
Esse elemento Especifica o <xref:System.ServiceModel.Description.WebHttpBehavior> em um ponto de extremidade por meio da configuração. Esse comportamento, quando usado em conjunto com a [\<webHttpBinding>](webhttpbinding.md) associação padrão, habilita o modelo de programação da Web para um serviço Windows Communication Foundation (WCF).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<webHttp>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<webHttp />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|automaticFormatSelectionEnabled|Quando essa propriedade é definida como `true` , a infraestrutura do WCF determina o melhor formato a ser usado. A seleção automática de formato é desabilitada por padrão para compatibilidade com versões anteriores. A seleção automática de formato pode ser habilitada programaticamente ou por meio da configuração.|  
|DefaultBodyStyle|Especifica o estilo de corpo padrão de mensagens retornadas. Para obter mais informações, consulte <xref:System.ServiceModel.Web.WebMessageBodyStyle> e [formatação de http Web WCF](../../../wcf/feature-details/wcf-web-http-formatting.md).|  
|defaultOutgoingResponseFormat|Especifica o formato de resposta de saída padrão para mensagens. Para obter mais informações, consulte [formatação de http do WCF Web](../../../wcf/feature-details/wcf-web-http-formatting.md).|  
|Não|Obtém ou define o sinalizador que especifica se uma FaultException é gerada quando ocorre um erro de servidor interno (código de status HTTP: 500).|  
|helpEnabled|Obtém ou define um valor que determina se a página de ajuda está habilitada.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica o conjunto de comportamentos de ponto de extremidade.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WebHttpElement>
- <xref:System.ServiceModel.Description.WebHttpBehavior>
- [Integração AJAX e suporte para JSON](../../../wcf/feature-details/ajax-integration-and-json-support.md)
- [\<webHttpBinding>](webhttpbinding.md)
