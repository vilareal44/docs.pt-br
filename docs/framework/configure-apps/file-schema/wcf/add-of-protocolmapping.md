---
title: <add> de <protocolMapping>
ms.date: 03/30/2017
ms.assetid: 08e62249-1641-41d1-91b1-66d7b46244e4
ms.openlocfilehash: 6197d01665d49a7c97ac9e44251abf15faf80a8f
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70850382"
---
# <a name="add-of-protocolmapping"></a>\<add> de \<protocolMapping>
Representa um mapeamento de protocolo padrão entre um esquema de protocolo de transporte (por exemplo, http, net. TCP, net. pipe, etc.) e uma associação de Windows Communication Foundation (WCF). Ao criar pontos de extremidade padrão em tempo de execução, o WCF examina os mapeamentos configurados e decide qual associação usar para um endereço baseado em particular.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<protocolMapping>**](protocolmapping.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<add>**  
  
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
  
|Elemento|Descrição|  
|-------------|-----------------|  
|associação|Uma cadeia de caracteres que especifica o tipo de associação a ser usada para um ponto de extremidade durante a criação do ponto de extremidade padrão.|  
|bindingConfiguration|Uma cadeia de caracteres que especifica o nome da seção de configuração de associação a ser referenciada.|  
|scheme|O esquema do protocolo de transporte a ser usado para o ponto de extremidade padrão.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum.  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<protocolMapping>](protocolmapping.md)|Representa uma seção de configuração para definir mapeamentos de protocolo padrão entre esquemas de protocolo de transporte (por exemplo, http, net. TCP, net. pipe, etc.) e associações de Windows Communication Foundation (WCF).|  
  
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
