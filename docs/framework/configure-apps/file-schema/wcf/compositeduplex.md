---
title: <compositeDuplex>
ms.date: 03/30/2017
ms.assetid: 725004d1-ce88-4405-a220-78e89844f81f
ms.openlocfilehash: c3bae4dfee36e9de62c27bbccecd9a31a5b7d459
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736784"
---
# \<compositeDuplex>
Define o elemento de associação que é usado quando o cliente deve expor um ponto de extremidade para que o serviço envie mensagens de volta ao cliente.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<compositeDuplex>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<compositeDuplex clientBaseAddress="URI" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|clientBaseAddress|Um URI que define o endereço do canal traseiro no modo duplex. O serviço usa esse endereço para fazer contato e estabelecer uma conexão com o cliente.<br /><br /> Se esse atributo não estiver definido, um endereço padrão " `full qualified name+default port\TemporaryIndigoAddress\guid` " será gerado. O padrão é `null`.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<binding>](bindings.md)|Define todos os recursos de associação da associação personalizada.|  
  
## <a name="remarks"></a>Comentários  
 Esse elemento de configuração é usado com transportes que não permitem comunicações de duplex nativamente, por exemplo, HTTP. O TCP, por outro lado, permite comunicações duplex nativamente e não requer o uso desse elemento de ligação para que o serviço envie mensagens de volta para um cliente.  
  
 O cliente deve expor um endereço para que o serviço faça contato e estabeleça uma conexão. Esse endereço de cliente é fornecido pelo `clientBaseAddress` atributo. Observe que Windows Communication Foundation (WCF) gera automaticamente um ClientBaseAddress se um não é definido explicitamente pelo usuário.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<compositeDuplex clientBaseAddress="http://www.contoso.com" />
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.CompositeDuplexElement>
- <xref:System.ServiceModel.Channels.CompositeDuplexBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
