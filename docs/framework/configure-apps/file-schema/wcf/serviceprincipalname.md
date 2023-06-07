---
title: <servicePrincipalName>
ms.date: 03/30/2017
ms.assetid: 3f3b85d3-20f2-4cd8-8a6a-ee18befbd165
ms.openlocfilehash: da865a19a91d4af6221a13b53a174637d5fb8139
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70854999"
---
# \<servicePrincipalName>
Especifica a identidade de um serviço por seu SPN (nome da entidade de serviço).  
  
Para obter mais informações sobre como configurar o SPN, consulte [identidade de serviço e autenticação](../../../wcf/feature-details/service-identity-and-authentication.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<client>**](client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpoint>**](endpoint-of-client.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<identity>**](identity.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<servicePrincipalName>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<servicePrincipalName value="String" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|value|O nome pelo qual um cliente identifica exclusivamente uma instância de um serviço. Se você instalar várias instâncias de um serviço em computadores em uma floresta, cada instância deverá ter seu próprio SPN. Uma determinada instância de serviço pode ter vários SPNs se houver vários nomes que os clientes podem usar para autenticação.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<identity>](identity.md)|Especifica a identidade do serviço a ser autenticado pelo cliente.|  
  
## <a name="remarks"></a>Comentários  
 Um cliente de Windows Communication Foundation seguro (WCF) que se conecta a um ponto de extremidade com essa identidade usa o SPN ao executar a autenticação SSPI com o ponto de extremidade.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.IdentityElement>
- <xref:System.ServiceModel.EndpointAddress>
- <xref:System.ServiceModel.EndpointAddress.Identity%2A>
- <xref:System.ServiceModel.SpnEndpointIdentity>
- [Identidade e autenticação de serviço](../../../wcf/feature-details/service-identity-and-authentication.md)
- [\<identity>](identity.md)
