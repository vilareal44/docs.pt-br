---
title: <security> de <netPeerBinding>
ms.date: 03/30/2017
ms.assetid: 1ef40d8c-f903-4426-9b08-da81462766d8
ms.openlocfilehash: 3d1ac85073c44f683fe0c054737c5ec7ed1cbf52
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738666"
---
# <a name="security-of-netpeerbinding"></a>\<security> de \<netPeerBinding>
Define as configurações de segurança do [\<netPeerTcpBinding>](netpeertcpbinding.md) , incluindo o tipo de autenticação usado e a segurança usada para o transporte de mensagens.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netPeerTcpBinding>**](netpeertcpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<security>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<netPeerBinding>
  <binding>
    <security mode="Message/None/Transport//TransportWithMessageCredential">
      <transport credentialType="Certificate/Password" />
    </security>
  </binding>
</netPeerBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|mode|Opcional. Especifica o tipo de segurança usado por pares configurados com essa associação. O valor padrão é `Message`. Esse atributo é do tipo <xref:System.ServiceModel.SecurityMode> .|  
  
## <a name="mode-attribute"></a>Atributo de modo  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Mensagem|A segurança SOAP fornece autenticação, integridade e confidencialidade.|  
|Nenhum|A segurança é desabilitada.|  
|Transport|A proteção é fornecida usando HTTPS.|  
|TransportWithMessageCredential|O HTTPS fornece autenticação e confidencialidade. As mensagens SOAP fornecem tipos de credenciais avançados.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<transport>](transport-of-netpeertcpbinding.md)|Define o tipo de transporte para mensagens protegidas enviadas por pares configurados com essa associação. Esse elemento é do tipo <xref:System.ServiceModel.Configuration.PeerTransportSecurityElement> .|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação do [\<netPeerTcpBinding>](netpeertcpbinding.md) .|  
  
## <a name="remarks"></a>Comentários  
 A segurança pode ser específica de mensagens ou de transporte.  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.PeerSecurityElement>
- <xref:System.ServiceModel.NetPeerTcpBinding.Security%2A>
- <xref:System.ServiceModel.Configuration.NetPeerTcpBindingElement.Security%2A>
- <xref:System.ServiceModel.PeerSecuritySettings>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Selecionando um tipo de credencial](../../../wcf/feature-details/selecting-a-credential-type.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
