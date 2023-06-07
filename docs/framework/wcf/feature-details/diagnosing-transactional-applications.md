---
title: Diagnosticando aplicativos transacionais
ms.date: 03/30/2017
ms.assetid: 4a993492-1088-4d10-871b-0c09916af05f
ms.openlocfilehash: fb3a83083e876cf697621ba70dcf7dd67636f83a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599211"
---
# <a name="diagnosing-transactional-applications"></a>Diagnosticando aplicativos transacionais
Este tópico descreve como usar o recurso de diagnóstico e gerenciamento do Windows Communication Foundation (WCF) para solucionar problemas de um aplicativo transacional.  
  
## <a name="performance-counters"></a>Contadores de desempenho  
 O WCF fornece um conjunto padrão de contadores de desempenho para medir o desempenho do aplicativo transacional. Para obter mais informações, consulte [Performance Counters](../diagnostics/performance-counters/index.md).  
  
 Os contadores de desempenho têm como escopo três níveis diferentes: serviço, ponto de extremidade e operação, conforme descrito nas tabelas a seguir.  
  
### <a name="service-performance-counters"></a>Contadores de desempenho de serviço  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|Fluxo de transações|O número de transações que fluíram para operações neste serviço. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o serviço.|  
|Fluxo de transações por segundo|O número de transações que fluíram para operações neste serviço dentro de cada segundo. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o serviço.|  
|Operações Transacionadas Confirmadas|O número de operações realizadas executadas, cuja transação foi concluída com o resultado confirmado neste serviço. O trabalho feito nessas operações é totalmente confirmado. Os recursos são atualizados de acordo com o trabalho feito na operação.|  
|Operações Transacionadas Confirmadas por Segundo|O número de operações transacionadas executadas, cuja transação foi concluída com o resultado confirmado neste serviço dentro de cada segundo. O trabalho feito nessas operações é totalmente confirmado. Os recursos são atualizados de acordo com o trabalho feito na operação.|  
|Operações de transação abortadas|O número de operações realizadas executadas, cuja transação foi concluída com o resultado anulado neste serviço. O trabalho feito nessas operações é revertido. Os recursos são revertidos para seu estado anterior.|  
|Operações Transacionadas Anuladas por Segundo|O número de operações realizadas executadas, cuja transação foi concluída com o resultado anulado neste serviço dentro de cada segundo. O trabalho feito nessas operações é revertido. Os recursos são revertidos para seu estado anterior.|  
|Operações Transacionadas em Dúvida|O número de operações realizadas executadas, cuja transação foi concluída com um resultado em dúvida neste serviço. O trabalho feito com um resultado em dúvida está em um estado indeterminado. Os recursos são mantidos com resultado pendente.|  
|Operações Transacionadas em Dúvida por Segundo|O número de operações realizadas executadas, cuja transação foi concluída com um resultado em dúvida neste serviço dentro de cada segundo. O trabalho feito com um resultado em dúvida está em um estado indeterminado. Os recursos são mantidos com resultado pendente.|  
  
### <a name="endpoint-performance-counters"></a>Contadores de desempenho de ponto de extremidade  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|Fluxo de transações|O número de transações que fluíram para operações neste ponto de extremidade. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o ponto de extremidade.|  
|Fluxo de transações por segundo|O número de transações que fluíram para operações neste ponto de extremidade dentro de cada segundo. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o ponto de extremidade.|  
  
### <a name="operation-performance-counters"></a>Contadores de desempenho de operação  
  
|Contador de desempenho|Descrição|  
|-------------------------|-----------------|  
|Fluxo de transações|O número de transações que fluíram para operações neste ponto de extremidade. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o ponto de extremidade.|  
|Fluxo de transações por segundo|O número de transações que fluíram para operações neste ponto de extremidade dentro de cada segundo. Esse contador é incrementado sempre que uma transação está presente na mensagem que é enviada para o ponto de extremidade.|  
  
## <a name="windows-management-instrumentation"></a>Instrumentação de Gerenciamento do Windows  
 O WCF expõe dados de inspeção de um serviço em tempo de execução por meio de um provedor de Instrumentação de Gerenciamento do Windows do WCF (WMI). Para obter mais informações sobre como acessar dados do WMI, consulte [usando instrumentação de gerenciamento do Windows para diagnóstico](../diagnostics/wmi/index.md).  
  
 Várias propriedades WMI somente leitura indicam as configurações de transação aplicadas para um serviço. As tabelas a seguir listam todas essas configurações.  
  
 Em um serviço, o `ServiceBehaviorAttribute` tem as seguintes propriedades.  
  
|Nome|Type|Descrição|  
|----------|----------|-----------------|  
|ReleaseServiceInstanceOnTransactionComplete|Boolean|Especifica se o objeto de serviço é reciclado quando a transação atual é concluída.|  
|TransactionAutoCompleteOnSessionClose|Boolean|Especifica se as transações pendentes são concluídas quando a sessão atual é fechada.|  
|TransactionIsolationLevel|Uma cadeia de caracteres que contém um valor válido da <xref:System.Transactions.IsolationLevel> enumeração.|Especifica o nível de isolamento da transação ao qual esse serviço dá suporte.|  
|TransactionTimeout|<xref:System.DateTime>|Especifica o período no qual uma transação deve ser concluída.|  
  
 O `ServiceTimeoutsBehavior` tem a seguinte propriedade.  
  
|Nome|Type|Descrição|  
|----------|----------|-----------------|  
|TransactionTimeout|<xref:System.DateTime>|Especifica o período no qual uma transação deve ser concluída.|  
  
 Em uma associação, o `TransactionFlowBindingElement` tem as seguintes propriedades.  
  
|Nome|Type|Descrição|  
|----------|----------|-----------------|  
|TransactionProtocol|Uma cadeia de caracteres que contém um valor válido do <xref:System.ServiceModel.TransactionProtocol> tipo.|Especifica o protocolo de transação a ser usado no fluxo de uma transação.|  
|TransactionFlow|Boolean|Especifica se o fluxo de transações de entrada está habilitado.|  
  
 Em uma operação, o `OperationBehaviorAttribute` tem as seguintes propriedades:  
  
|Nome|Type|Descrição|  
|----------|----------|-----------------|  
|TransactionAutoComplete|Boolean|Especifica se a transação atual deve ser confirmada automaticamente se não ocorrer nenhuma exceção não tratada.|  
|TransactionScopeRequired|Boolean|Especifica se a operação requer uma transação.|  
  
 Em uma operação, o `TransactionFlowAttribute` tem as seguintes propriedades.  
  
|Nome|Type|Descrição|  
|----------|----------|-----------------|  
|TransactionFlowOption|Uma cadeia de caracteres que contém um valor válido da <xref:System.ServiceModel.TransactionFlowOption> enumeração.|Especifica a extensão para a qual o fluxo de transações é necessário.|  
  
## <a name="tracing"></a>Rastreamento  
 Os rastreamentos permitem que você monitore e analise falhas em seus aplicativos transacionais. Você pode habilitar o rastreamento usando as seguintes maneiras:  
  
- Rastreamento padrão do WCF  
  
     Esse tipo de rastreamento é o mesmo que rastrear qualquer aplicativo WCF. Para obter mais informações, consulte [Configurando o rastreamento](../diagnostics/tracing/configuring-tracing.md).  
  
- Rastreamento de WS-AtomicTransaction  
  
     O rastreamento WS-AtomicTransaction pode ser habilitado usando o [Utilitário de configuração WS-AtomicTransaction (wsatConfig. exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md). Esse rastreamento fornece informações sobre o estado de transações e participantes em um sistema. Para habilitar também o rastreamento de modelo de serviço interno, você pode definir a `HKLM\SOFTWARE\Microsoft\WSAT\3.0\ServiceModelDiagnosticTracing` chave do registro como um valor válido da <xref:System.Diagnostics.SourceLevels> enumeração. Você pode habilitar o log de mensagens da mesma maneira que outros aplicativos WCF.  
  
- `System.Transactions`rastreio  
  
     Ao usar o protocolo OleTransactions, as mensagens de protocolo não podem ser rastreadas. O suporte de rastreamento que a <xref:System.Transactions> infraestrutura fornece (que usa OleTransactions) permite que os usuários exibam eventos que ocorreram para as transações. Para habilitar o rastreamento de um <xref:System.Transactions> aplicativo, inclua o código a seguir no `App.config` arquivo de configuração.  
  
    ```xml  
    <configuration>  
      <system.diagnostics>  
         <sources>  
            <source name="System.Transactions" switchValue="Verbose, ActivityTracing">  
               <listeners>  
                  <add name="Text"  
                     type="System.Diagnostics.XmlWriterTraceListener"  
                     initializeData="SysTx.log"  
                     traceOutputOptions="Callstack" />  
               </listeners>  
            </source>  
         </sources>  
         <trace autoflush="true" indentsize="4">  
         </trace>  
      </system.diagnostics>  
    </configuration>  
    ```  
  
     Isso também habilita o rastreamento do WCF, pois o WCF também utiliza a <xref:System.Transactions> infraestrutura.  
  
## <a name="see-also"></a>Consulte também

- [Administração e diagnóstico](../diagnostics/index.md)
- [Configurando o rastreamento](../diagnostics/tracing/configuring-tracing.md)
- [Utilitário de configuração de WS-AtomicTransaction (wsatConfig.exe)](../ws-atomictransaction-configuration-utility-wsatconfig-exe.md)
