---
title: <endpointDiscovery>
ms.date: 03/30/2017
ms.assetid: 70812717-888a-4748-9640-0df6715ff029
ms.openlocfilehash: 98b1655f42b7b43604ed4ab9d66870ec204a9590
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398013"
---
# \<endpointDiscovery>
Especifica as várias configurações de descoberta para um ponto de extremidade, como sua descoberta, escopos e quaisquer extensões personalizadas para seus metadados.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<behavior>**](behavior-of-endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<endpointDiscovery>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<behaviors>
  <endpointBehaviors>
    <behavior name="String">
      <endpointDiscovery enabled="Boolean">
        <scopes>
          <add scope="URI"/>
        </scopes>
        <extensions />
      </endpointDiscovery>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|Habilitado|Um valor booliano que especifica se a descoberta está habilitada neste ponto de extremidade. O padrão é `false`.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<scopes>](scopes.md)|Uma coleção de URIs de escopo para o ponto de extremidade. Mais de um URI de escopo pode ser associado a um único ponto de extremidade.|  
|[\<extensions>](extensions.md)[de \<endpointDiscovery> ]|Uma coleção de elementos XML que permite que você especifique metadados personalizados a serem publicados para um ponto de extremidade.|  
|\<types>|Uma coleção de interfaces a serem pesquisadas.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<behavior>](behavior-of-endpointbehaviors.md)|Especifica um elemento de comportamento.|  
|||  
  
## <a name="remarks"></a>Comentários  
 Quando adicionado à configuração de comportamento do ponto de extremidade e com o `enabled` atributo definido como `true` , esse elemento de configuração permite sua capacidade de descoberta. Além disso, você pode usar o [\<scopes>](scopes.md) elemento filho para especificar URIs de escopo personalizados que podem ser usados para filtrar pontos de extremidade de serviço durante a consulta, bem como o [\<extensions>](extensions.md) elemento filho para especificar metadados personalizados que devem ser publicados juntamente com os metadados detectáveis padrão (EPR, ContractTypeName, BindingName, Scope e ListenURI).  
  
 Esse elemento de configuração é dependente do [\<serviceDiscovery>](servicediscovery.md) elemento que fornece o controle de nível de serviço de descoberta. Isso significa que as configurações desse elemento serão ignoradas se [\<serviceDiscovery>](servicediscovery.md) não estiver presente na configuração.  
  
## <a name="example"></a>Exemplo  
 O exemplo de configuração a seguir especifica os escopos de filtragem e os metadados de extensão a serem publicados para um ponto de extremidade.  
  
```xml  
<services>
  <service name="CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <endpoint binding="basicHttpBinding"
              address="calculator"
              contract="ICalculatorService"
              behaviorConfiguration="calculatorEndpointBehavior" />
  </service>
</services>
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <serviceDiscovery />
    </behavior>
  </serviceBehaviors>
  <endpointBehaviors>
    <behavior name="calculatorEndpointBehavior">
      <endpointDiscovery enabled="true">
        <scopes>
          <add scope="http://contoso/test1" />
          <add scope="http://contoso/test2" />
        </scopes>
        <extensions>
          <e:Publisher xmlns:e="http://example.org">
            <e:Name>The Example Organization</e:Name>
            <e:Address>One Example Way, ExampleTown, EX 12345</e:Address>
            <e:Contact>support@example.org</e:Contact>
          </e:Publisher>
          <AnotherCustomMetadata>Custom Metadata</AnotherCustomMetadata>
        </extensions>
      </endpointDiscovery>
    </behavior>
  </endpointBehaviors>
</behaviors>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.EndpointDiscoveryBehavior>
