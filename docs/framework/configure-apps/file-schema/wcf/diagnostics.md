---
title: <diagnostics>
ms.date: 03/30/2017
ms.assetid: 0c2f95c4-cc12-4fb5-a70c-7fc6fa95db58
ms.openlocfilehash: 2749bc6c66d491a8a160d98b508fb43aa027b806
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70398049"
---
# \<diagnostics>
O `diagnostics` elemento define as configurações que podem ser usadas por um administrador para inspeção e controle de tempo de execução.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;**\<diagnostics>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <diagnostics etwProviderId="String"
               performanceCounters="Off/ServiceOnly/All/Default"
               wmiProviderEnabled="Boolean">
    <endToEndTracing activityTracing="Boolean"
                     messageFlowTracing="Boolean"
                     propagateActivity="Boolean" />
    <messageLogging logEntireMessage="Boolean"
                    logMalformedMessages="Boolean"
                    logMessagesAtServiceLevel="Boolean"
                    logMessagesAtTransportLevel="Boolean"
                    maxMessagesToLog="Integer"
                    maxSizeOfMessageToLog="Integer">
      <filters>
        <clear />
      </filters>
    </messageLogging>
  </diagnostics>
</system.serviceModel>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|etwProviderId|Uma cadeia de caracteres que especifica o identificador para o provedor de rastreamento de eventos, que grava eventos em sessões do ETW.|  
|performanceCounters|Especifica se contadores de desempenho para o assembly estão habilitados. Os valores válidos são<br /><br /> -Desativado: os contadores de desempenho estão desabilitados.<br />-Service only: somente contadores de desempenho relevantes para esse serviço estão habilitados.<br />-Todos: contadores de desempenho podem ser exibidos em tempo de execução.<br />-Padrão: uma única instância de contador de desempenho _WCF_Admin é criada. Esta instância é usada para habilitar a coleta de dados SQM usados pela infraestrutura. Nenhum dos valores de contador para esta instância estão atualizados e, portanto, permanecerão em zero. Esse será o valor padrão se nenhuma configuração estiver presente para o WCF.|  
|wmiProviderEnabled|Um valor booliano que especifica se o provedor WMI para o assembly está habilitado. O provedor WMI é necessário para que o usuário tenha acesso em tempo de execução aos recursos de inspeção e controle do Windows Communication Foundation (WCF). O padrão é `false`.|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<endToEndTracing>](endtoendtracing.md)|Um elemento de configuração que permite habilitar e desabilitar diferentes aspectos de rastreamento de ponta a ponta durante a execução de um aplicativo de serviço.|  
|[\<messageLogging>](messagelogging.md)|Descreve as configurações para o log de mensagens do WCF.|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|serviceModel|O elemento raiz de todos os elementos de configuração do WCF.|  
  
## <a name="remarks"></a>Comentários  
 A `diagnostics` seção define as configurações de diagnóstico para todos os serviços localizados em um assembly. Não é possível definir configurações de diagnóstico separadas no nível de serviço, a menos que haja apenas um serviço no assembly. Os atributos são definidos de acordo com os requisitos da seção.  
  
## <a name="example"></a>Exemplo  
  
```xml  
<diagnostics wmiProviderEnabled="false"
             performanceCounters="all">
  <messageLogging logEntireMessage="true"
                  logMalformedMessages="true"
                  logMessagesAtServiceLevel="true"
                  logMessagesAtTransportLevel="true"
                  maxMessagesToLog="42"
                  maxSizeOfMessageToLog="42">
    <filters>
      <clear />
    </filters>
  </messageLogging>
</diagnostics>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
