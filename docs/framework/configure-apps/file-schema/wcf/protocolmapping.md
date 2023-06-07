---
title: <protocolMapping>
ms.date: 03/30/2017
ms.assetid: 5076644b-1f33-4f26-9488-87de9fcda04c
ms.openlocfilehash: be4224ef1a8b17653df8123aaf89e105a496355a
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70400015"
---
# \<protocolMapping>
Representa uma seção de configuração para definir um conjunto de mapeamentos de protocolo padrão entre esquemas de protocolo de transporte (por exemplo, http, net. TCP, net. pipe, etc.) e associações do WCF. Ao criar pontos de extremidade padrão em tempo de execução, Windows Communication Foundation (WCF) examina os mapeamentos configurados e decide qual associação usar para um endereço baseado em particular.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<protocolMapping>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<protocolMapping>
  <add binding="String"
       bindingConfiguration="String"
       scheme="http/net.msmq/net.pipe/net.tcp" />
</protocolMapping>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
 Nenhum.  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<filters>](filters-of-routing.md)|Contém um mapeamento de protocolo padrão entre um esquema de protocolo de transporte (por exemplo, http, net. TCP, net. pipe, etc.) e uma associação do WCF.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<system.serviceModel>](system-servicemodel.md)|O elemento raiz de todos os elementos de configuração do WCF.|  
  
## <a name="example"></a>Exemplo  
 O exemplo de configuração a seguir mostra o mapeamento de protocolo padrão no arquivo Machine. config. Você pode substituir esse mapeamento padrão no nível do computador modificando o arquivo Machine. config. Ou, se você quiser apenas substituí-lo no escopo de um aplicativo, poderá substituir esta seção no arquivo de configuração do aplicativo e alterar o mapeamento para esquemas de protocolo individuais.  
  
```xml  
<protocolMapping>
  <add scheme="http"
       binding="basicHttpBinding" />
  <add scheme="net.tcp"
       binding="netTcpBinding" />
  <add scheme="net.pipe"
       binding="netNamedPipeBinding" />
  <add scheme="net.msmq"
       binding="netMsmqBinding" />
</protocolMapping>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.ProtocolMappingSection?displayProperty=nameWithType>
- <xref:System.ServiceModel.Configuration.ProtocolMappingElement?displayProperty=nameWithType>
