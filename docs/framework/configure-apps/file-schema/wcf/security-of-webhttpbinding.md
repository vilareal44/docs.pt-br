---
title: <security> de <webHttpBinding>
ms.date: 03/30/2017
ms.assetid: 727cf3d2-6f56-48ad-a59f-ba423edb9c83
ms.openlocfilehash: 77009dc950a608da9e0db3a7d09be67e1ed46137
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738629"
---
# <a name="security-of-webhttpbinding"></a>\<security> de \<webHttpBinding>
Especifica os requisitos de segurança para um ponto de extremidade configurado com um [\<webHttpBinding>](webhttpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<webHttpBinding>**](webhttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.ServiceModel>
  <bindings>
    <webHttpBinding>
      <binding name = "String">
        <security mode="None/Transport/TransportCredentialOnly">
          <transport clientCredentialType="Basic/Certificate/Digest/None/Ntlm/Windows"
                     proxyCredentialType="Basic/Digest/None/Ntlm/Windows"
                     realm="String" />
        </security>
      </binding>
    </webHttpBinding>
  </bindings>
</system.ServiceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Especifica se a segurança em nível de transporte ou nenhuma segurança é usada por um ponto de extremidade. O padrão é `None`. Esse atributo é do tipo <xref:System.ServiceModel.WebHttpSecurityMode> .|  
  
## <a name="mode-attribute"></a>Atributo Mode  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|A segurança é desabilitada.|  
|Transport|A proteção é fornecida usando HTTPS. O serviço precisa ser configurado com certificados SSL. A mensagem é totalmente protegida usando HTTPS e o serviço é autenticado pelo cliente usando o certificado SSL do serviço. A autenticação do cliente é controlada por meio do `ClientCredentialType` atributo do [\<transport>](transport-of-webhttpbinding.md) .|  
|TransportCredentialOnly|Esse modo não fornece confidencialidade e integridade de mensagens. Ele fornece autenticação de cliente baseada em HTTP. Esse modo deve ser usado com cautela. Ele deve ser usado em ambientes em que a segurança de transporte está sendo fornecida por outros meios (como IPSec) e somente a autenticação do cliente é fornecida pela infraestrutura do WCF.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-webhttpbinding.md)|Define as configurações de segurança de transporte. Este elemento corresponde ao <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement> tipo.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<webHttpBinding>](webhttpbinding.md)|Um elemento de associação que é usado para configurar pontos de extremidade para serviços da Web Windows Communication Foundation (WCF) que respondem a solicitações HTTP em vez de mensagens SOAP.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.WebHttpBindingElement>
- <xref:System.ServiceModel.Configuration.WSHttpSecurityElement>
- <xref:System.ServiceModel.WebHttpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.WebHttpBindingElement.Security%2A>
- <xref:System.ServiceModel.WebHttpSecurity>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Selecionando um tipo de credencial](../../../wcf/feature-details/selecting-a-credential-type.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
- [Modelo de programação WCF Web HTTP](../../../wcf/feature-details/wcf-web-http-programming-model.md)
