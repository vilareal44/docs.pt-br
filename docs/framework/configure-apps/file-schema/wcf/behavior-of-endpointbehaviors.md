---
title: <behavior> de <endpointBehaviors>
ms.date: 03/30/2017
ms.assetid: b90ca3bc-3c22-4174-b903-e3a39898bd27
ms.openlocfilehash: 489678a5adeae3965acae90a847c4b087478354d
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "74140803"
---
# <a name="behavior-of-endpointbehaviors"></a>\<behavior> de \<endpointBehaviors>
O `behavior` elemento contém uma coleção de configurações para o comportamento de um ponto de extremidade. Cada comportamento é indexado pela sua `name`. Os pontos de extremidade podem ser vinculados a cada comportamento por meio desse nome. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<behaviors>**](behaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<endpointBehaviors>**](endpointbehaviors.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<behavior>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.ServiceModel>
  <behaviors>
    <endpointBehaviors>
      <behavior name="String" />
    </endpointBehaviors>
  </behaviors>
</system.ServiceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|name|Uma cadeia de caracteres exclusiva que contém o nome da configuração do comportamento. Esse valor é uma cadeia de caracteres definida pelo usuário que deve ser exclusiva, pois ele atua como a cadeia de caracteres de identificação para o elemento. A partir do .NET Framework 4, associações e comportamentos não precisam ter um nome. Para obter mais informações sobre configurações padrão e associações e comportamentos do sem nome, consulte [configuração simplificada](../../../wcf/simplified-configuration.md) e [configuração simplificada para serviços WCF](../../../wcf/samples/simplified-configuration-for-wcf-services.md).|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<clientCredentials>](clientcredentials.md)|Especifica as credenciais usadas para autenticar o cliente para um serviço.|  
|[\<callbackDebug>](callbackdebug.md)|Especifica a depuração de serviço para um objeto de retorno de chamada Windows Communication Foundation (WCF).|  
|[\<callbackTimeouts>](callbacktimeouts.md)|Especifica o tempo limite para o retorno de chamada do cliente.|  
|[\<clientVia>](clientvia.md)|Especifica a rota que uma mensagem deve tomar.|  
|[\<dataContractSerializer>](datacontractserializer.md)|Contém dados de configuração para o DataContractSerializer.|  
|[\<dispatcherSynchronization>](dispatchersynchronization.md)|Especifica um comportamento de ponto de extremidade que permite que um serviço envie respostas de forma assíncrona.|  
|[\<enableWebScript>](enablewebscript.md)|Habilita o comportamento do ponto de extremidade que torna possível consumir o serviço de páginas da Web do ASP.NET AJAX. O comportamento só deve ser usado em conjunto com a \<webHttpBinding> associação padrão ou com o \<webMessageEncoding> elemento de associação.|  
|[\<endpointDiscovery>](endpointdiscovery.md)|Especifica as várias configurações de descoberta para um ponto de extremidade, como sua descoberta, escopos e quaisquer extensões personalizadas para seus metadados.|  
|[\<soapProcessing>](soapprocessing.md)|Define o comportamento de ponto de extremidade de cliente usado para realizar marshaling de mensagens entre diferentes tipos de associação e versões de mensagem.|  
|[\<synchronousReceive>](synchronousreceive-element.md)|Especifica o comportamento de tempo de execução para receber mensagens em um aplicativo de serviço ou cliente. Ele não tem nenhum atributo ou elemento filho.|  
|[\<transactedBatching>](transactedbatching.md)|Especifica se o envio em lote de transações tem suporte para operações de recebimento.|  
|[\<webHttp>](webhttp.md)|Especifica o WebHttpBehavior em um ponto de extremidade por meio da configuração. Esse comportamento, quando usado em conjunto com a \<webHttpBinding> associação padrão, habilita o modelo de programação da Web para um serviço WCF.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<endpointBehaviors>](endpointbehaviors.md)|Uma coleção de elementos de comportamento de ponto de extremidade.|
