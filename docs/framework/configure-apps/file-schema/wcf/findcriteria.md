---
title: <findCriteria>
ms.date: 03/30/2017
ms.assetid: 5454cd19-6bf5-4ba8-94d1-f58d10dc1917
ms.openlocfilehash: 44e068ee205bc5e04382164e7ab00716b2c07dcf
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855157"
---
# \<findCriteria>
Um elemento de configuração que fornece um conjunto de critérios usados por um aplicativo cliente para pesquisar um serviço de descoberta. Os critérios podem ser agrupados em critérios de pesquisa (especificando quais serviços você está procurando) e localizar os critérios de encerramento (por quanto tempo a pesquisa deve durar).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<standardEndpoints>**](standardendpoints.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<dynamicEndpoint>**](dynamicendpoint.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<standardEndpoint>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<discoveryClientSettings>**](discoveryclientsettings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<findCriteria>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <standardEndpoints>
    <dynamicEndpoint>
      <standardEndpoint>
        <discoveryClientSettings discoveryEndpoint="String">
          <findCriteria duration="TimeSpan"
                        maxResults="Integer"
                        scopeMatchBy="Uri">
            <contractTypeNames>
              <add name="String"
                   namespace="String" />
            </contractTypeNames>
            <extensions />
            <scopes>
              <add scope="URI" />
            </scopes>
          </findCriteria>
        </discoveryClientSettings>
      </standardEndpoint>
    </dynamicEndpoint>
  </standardEndpoints>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|duration|Um valor TimeSpan que especifica o tempo máximo de espera para respostas de serviços na rede. A duração padrão é de 20 segundos.|  
|maxResults|Um inteiro que especifica o número máximo de respostas a aguardar, de serviços em uma rede ou na Internet. Se as respostas máximas forem recebidas antes que o valor especificado no `duration` atributo tenha decorrido, a operação Localizar será encerrada.|  
|scopeMatchBy|Um URI que especifica o algoritmo de correspondência a ser usado ao corresponder os escopos na mensagem de investigação com o do ponto de extremidade.<br /><br /> Há cinco regras de correspondência de escopo com suporte. Se você não especificar uma regra de correspondência de escopo, `ScopeMatchByPrefix` será usado. Para obter mais informações sobre isso, consulte <xref:System.ServiceModel.Discovery.FindCriteria>.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<contractTypeNames>](contracttypenames.md)|Uma coleção de elementos de configuração que contém os nomes dos tipos de contrato de serviço de fluxo de trabalho.|  
|\<extensions> de \<findCriteria>|Uma coleção de objetos de elemento XML que fornecem extensões.|  
|[\<scopes>](scopes.md)|Uma coleção de objetos que contêm URIs absolutos que são usados durante uma operação de localização para localizar um serviço ou serviços específicos.<br /><br /> Se o serviço específico for encontrado, uma correspondência bem-sucedida terá sido feita entre o URI do serviço e o URI do escopo, às vezes com a ajuda de regras de escopo que lidam com complicações de correspondência.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<standardEndpoints>](standardendpoints.md)|Contém as configurações necessárias para um aplicativo participar do processo de descoberta de serviço como um cliente.|  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Discovery.FindCriteria>
- <xref:System.ServiceModel.Discovery.Configuration.FindCriteriaElement>
