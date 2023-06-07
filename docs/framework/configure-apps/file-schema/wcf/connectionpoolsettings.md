---
title: <connectionPoolSettings>
ms.date: 03/30/2017
ms.assetid: 6fa7fa65-2c6e-4eab-b8cf-7690112c0be5
ms.openlocfilehash: 842173c7bd9673a1e08c93d5ed650a42b9d979e5
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400475"
---
# \<connectionPoolSettings>
Especifica configurações de pool de conexões adicionais para uma associação de pipe nomeado.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<customBinding>**](custombinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<namedPipeTransport>**](namedpipetransport.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<connectionPoolSettings>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<connectionPoolSettings groupName="String"
                        idleTimeout="TimeSpan"
                        maxOutboundConnectionsPerEndpoint="Integer" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|`groupName`|Uma cadeia de caracteres que define o nome do pool de conexões usado para canais de saída. No modo de fluxo, as conexões não são compartilhadas, o que significa que o pool de conexões está desabilitado. O padrão é uma cadeia de caracteres "padrão". Você pode modificar esse valor para isolar as conexões de um cliente específico em grupos separados.|  
|`idleTimeout`|Um positivo <xref:System.TimeSpan> que especifica o tempo máximo que a conexão pode ficar ociosa antes de ser desconectada. O padrão é 00:02:00.|  
|`maxOutboundConnectionsPerEndpoint`|Um inteiro positivo que especifica o número máximo de conexões com um ponto de extremidade remoto iniciado pelo serviço. As conexões que excedem o limite são enfileiradas até que um espaço abaixo do limite fique disponível. O `idleTimeout` limita a duração em que as conexões permanecem enfileiradas antes que uma exceção seja lançada. O padrão é 10.<br /><br /> Esse atributo limita o número de conexões ativas simultâneas do cliente para um ponto de extremidade de serviço específico. Se esse valor for excedido por ter mais conexões de cliente ativas, o serviço poderá parecer sem resposta ao cliente. Nesse caso, esse valor deve ser ajustado para exceder o número máximo de conexões de cliente simultâneas esperadas para um ponto de extremidade específico.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<namedPipeTransport>](namedpipetransport.md)|Define um transporte que faz com que um canal transfira mensagens usando pipes nomeados.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.NamedPipeConnectionPoolSettingsElement>
- <xref:System.ServiceModel.Channels.NamedPipeTransportBindingElement.ConnectionPoolSettings%2A>
- <xref:System.ServiceModel.Channels.NamedPipeConnectionPoolSettings>
- <xref:System.ServiceModel.Channels.TransportBindingElement>
- <xref:System.ServiceModel.Channels.CustomBinding>
- [Transportes](../../../wcf/feature-details/transports.md)
- [Selecionando um transporte](../../../wcf/feature-details/choosing-a-transport.md)
- [Associações](../../../wcf/bindings.md)
- [Estendendo associações](../../../wcf/extending/extending-bindings.md)
- [Associações personalizadas](../../../wcf/extending/custom-bindings.md)
- [\<customBinding>](custombinding.md)
