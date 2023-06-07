---
title: <windowsStreamSecurity>
ms.date: 03/30/2017
ms.assetid: 926bea29-90c7-4a26-9cf0-fb4aa44f6f70
ms.openlocfilehash: dab8505a9ddb348a6f7fe16ae9acb3a0119a8b06
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73735889"
---
# \<windowsStreamSecurity>
Especifique as configurações de segurança de fluxo do Windows da associação personalizada.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<windowsStreamSecurity>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<windowsStreamSecurity protectionLevel="None/Sign/EncryptAndSign" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|protectionLevel|Define a segurança em nível de mensagem. A assinatura de mensagens reduz o risco de violação de terceiros com a mensagem enquanto ela está sendo transferida. A criptografia fornece privacidade no nível de dados durante o transporte. Os valores válidos incluem os seguintes:<br /><br /> -Nenhum: sem proteção.<br />-Sign: as mensagens são assinadas.<br />-EncryptAndSign: as mensagens são assinadas e criptografadas.<br /><br /> O padrão é EncryptAndSign.<br /><br /> Esse atributo é do tipo <xref:System.Net.Security.ProtectionLevel> .|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 Os transportes que usam um protocolo orientado a fluxo, como TCP e pipes nomeados, dão suporte a atualizações de transporte baseadas em fluxo. Especificamente, o WCF fornece atualizações de segurança. A configuração dessa segurança de transporte é encapsulada por esse elemento de configuração, bem como pelo [\<sslStreamSecurity>](sslstreamsecurity.md) , que pode ser configurado e adicionado a uma associação personalizada  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Channels.CustomBinding>
- <xref:System.ServiceModel.Configuration.WindowsStreamSecurityElement>
- <xref:System.ServiceModel.Channels.WindowsStreamSecurityBindingElement>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
