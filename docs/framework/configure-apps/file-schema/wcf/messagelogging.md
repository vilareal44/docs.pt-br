---
title: <messageLogging>
ms.date: 03/30/2017
ms.assetid: 1d06a7e6-9633-4a12-8c5d-123adbbc19c5
ms.openlocfilehash: 9291c38af28c18d20e23e34e8316b4a9fe523123
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "70855126"
---
# \<messageLogging>
Esse elemento define as configurações para os recursos de log de mensagens do Windows Communication Foundation (WCF).  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<diagnostics>**](diagnostics.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<messageLogging>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<system.serviceModel>
  <diagnostics>
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
|`logEntireMessage`|Um valor booliano que especifica se a mensagem inteira (cabeçalho e corpo da mensagem) é registrada. O padrão é `false` , o que significa que apenas o cabeçalho da mensagem é registrado. Essa configuração afeta todos os níveis de log de mensagens (serviço, transporte e malformados).|  
|`logMalformedMessages`|Um valor booliano que especifica se as mensagens malformadas são registradas. As mensagens malformadas não contam para o `maxMessagesToLog` . O padrão é `false`.|  
|`logMessagesAtServiceLevel`|Um valor booliano que especifica se as mensagens são rastreadas no nível de serviço (antes das transformações relacionadas à criptografia e ao transporte). O padrão é `false`.|  
|`logMessagesAtTransportLevel`|Um valor booliano que especifica se as mensagens são rastreadas no nível de transporte. Todos os filtros especificados no arquivo de configuração são aplicados e somente as mensagens que correspondem aos filtros são rastreadas. O padrão é `false`.|  
|`maxMessagesToLog`|Um inteiro positivo que especifica o número máximo de mensagens a serem registradas. O padrão é 1000.|  
|`maxSizeOfMessageToLog`|Um inteiro positivo que especifica o tamanho máximo, em bytes, de uma mensagem a ser registrada. Mensagens maiores que o limite não serão registradas em log. Essa configuração afeta todos os níveis de rastreamento. O padrão é 262144 (0x4000).|  
  
### <a name="child-elements"></a>Elementos filho  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|filtros|O `filters` elemento contém uma coleção de filtros XPath. Quando o log de mensagens de transporte estiver habilitado ( `logMessagesAtTransportLevel` is `true` ), somente as mensagens correspondentes aos filtros serão registradas em log.<br /><br /> Os filtros são aplicados somente na camada de transporte. O nível de serviço e o log de mensagens malformados não são afetados por filtros.<br /><br /> O único atributo para esse elemento, `filter` , é um XpathFilter.<br /><br /> `<filters>     <add xmlns:soap="http://www.w3.org/2003/05/soap-envelope">/soap:Envelope</add> </filters>`|  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|diagnóstico|Define as configurações do WCF para inspeção e controle de tempo de execução para o administrador.|  
  
## <a name="remarks"></a>Comentários  
 As mensagens são registradas em três níveis diferentes na pilha: serviço, transporte e malformados. Cada nível pode ser ativado separadamente.  
  
 Os filtros XPath podem ser adicionados para registrar mensagens específicas de log nos níveis de transporte e de serviço. Se nenhum filtro for definido, todas as mensagens serão registradas. Os filtros são aplicados somente aos cabeçalhos da mensagem. O corpo é ignorado. O WCF ignora o corpo da mensagem para melhorar o desempenho. Se você quiser filtrar com base no conteúdo do corpo, poderá criar um ouvinte personalizado com um filtro que faça isso.  
  
 Você precisa criar um ouvinte de rastreamento para ativar o rastreamento de mensagens. O ouvinte em si pode ser qualquer ouvinte que funcione com a <xref:System.Diagnostics> arquitetura de rastreamento. O exemplo a seguir demonstra como criar um ouvinte desse tipo.  
  
```xml  
<system.diagnostics>
  <sources>
    <source name="System.ServiceModel"
            switchValue="Verbose">
      <listeners>
        <clear />
        <add type="System.Diagnostics.DefaultTraceListener"
             name="Default"
             traceOutputOptions="None" />
        <add name="ServiceModel Listener"
             traceOutputOptions="None" />
      </listeners>
    </source>
    <source name="System.ServiceModel.MessageLogging">
      <listeners>
        <clear />
        <add type="System.Diagnostics.DefaultTraceListener"
             name="Default"
             traceOutputOptions="None" />
        <add name="MessageLogging Listener"
             traceOutputOptions="None" />
      </listeners>
    </source>
  </sources>
  <sharedListeners>
    <add initializeData="C:\ItProTools\TraceLog.xml"
         type="System.Diagnostics.XmlWriterTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         name="ServiceModel Listener"
         traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" />
    <add initializeData="C:\ItProTools\MessageLog.log"
         type="System.Diagnostics.XmlWriterTraceListener, System, Version=2.0.0.0, Culture=neutral, PublicKeyToken=b77a5c561934e089"
         name="MessageLogging Listener"
         traceOutputOptions="LogicalOperationStack, DateTime, Timestamp, ProcessId, ThreadId, Callstack" />
  </sharedListeners>
</system.diagnostics>
```  
  
## <a name="example"></a>Exemplo  
  
```xml  
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
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.DiagnosticSection>
- <xref:System.ServiceModel.Diagnostics>
- <xref:System.ServiceModel.Configuration.DiagnosticSection.MessageLogging%2A>
- <xref:System.ServiceModel.Configuration.MessageLoggingElement>
- [Configurando registros de mensagens em log](../../../wcf/diagnostics/configuring-message-logging.md)
