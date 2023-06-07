---
title: Configurações recomendadas para registro de rastreamento e mensagens
description: Saiba mais sobre as configurações recomendadas de rastreamento e registro de mensagens para diferentes ambientes operacionais no WCF.
ms.date: 03/30/2017
ms.assetid: c6aca6e8-704e-4779-a9ef-50c46850249e
ms.openlocfilehash: 71067a4d6f4cec65a148a8162c40e44d82b85784
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245317"
---
# <a name="recommended-settings-for-tracing-and-message-logging"></a>Configurações recomendadas para registro de rastreamento e mensagens
Este tópico descreve o rastreamento recomendado e as configurações de log de mensagens para diferentes ambientes operacionais.  
  
## <a name="recommended-settings-for-a-production-environment"></a>Configurações recomendadas para um ambiente de produção  
 Para um ambiente de produção, se você estiver usando fontes de rastreamento do WCF, defina o `switchValue` como aviso. Se você estiver usando a origem de rastreamento do WCF `System.ServiceModel` , defina o `switchValue` atributo como `Warning` e o `propagateActivity` atributo como `true` . Se você estiver usando uma fonte de rastreamento definida pelo usuário, defina o `switchValue` atributo como `Warning, ActivityTracing` . Isso pode ser feito manualmente usando a [ferramenta do editor de configuração (SvcConfigEditor.exe)](../../configuration-editor-tool-svcconfigeditor-exe.md). Se você não antecipar um impacto no desempenho, poderá definir o `switchValue` atributo como `Information` em todos os casos mencionados anteriormente, o que gera uma quantidade muito grande de dados de rastreamento. O exemplo a seguir demonstra essas configurações recomendadas.  
  
```xml  
<configuration>  
 <system.diagnostics>  
  <sources>  
    <source name="System.ServiceModel"  
            switchValue="Warning"  
            propagateActivity="true" >  
      <listeners>  
        <add name="xml"/>  
      </listeners>  
    </source>  
    <source name="myUserTraceSource"  
            switchValue="Warning, ActivityTracing">  
      <listeners>  
        <add name="xml"/>  
      </listeners>  
    </source>  
  </sources>  
  <sharedListeners>  
    <add name="xml"  
         type="System.Diagnostics.XmlWriterTraceListener"  
               initializeData="C:\logs\Traces.svclog" />  
  </sharedListeners>  
 </system.diagnostics>  
  
<system.serviceModel>  
  <diagnostics wmiProviderEnabled="true">  
  </diagnostics>  
 </system.serviceModel>  
</configuration>  
```  
  
## <a name="recommended-settings-for-deployment-or-debugging"></a>Configurações recomendadas para implantação ou depuração  
 Para implantação ou ambiente de depuração, escolha `Information` ou `Verbose` , junto com `ActivityTracing` para uma fonte de rastreamento ou definida pelo usuário `System.ServiceModel` . Para aprimorar a depuração, você também deve adicionar uma fonte de rastreamento adicional ( `System.ServiceModel.MessageLogging` ) à configuração para habilitar o log de mensagens. Observe que o `switchValue` atributo não tem impacto sobre essa origem de rastreamento.  
  
 O exemplo a seguir demonstra as configurações recomendadas, usando um ouvinte compartilhado que utiliza o `XmlWriterTraceListener` .  
  
```xml  
<configuration>  
 <system.diagnostics>  
  <sources>  
    <source name="System.ServiceModel"  
            switchValue="Information, ActivityTracing"  
            propagateActivity="true" >  
      <listeners>  
        <add name="xml"/>  
      </listeners>  
    </source>  
    <source name="System.ServiceModel.MessageLogging">  
      <listeners>  
        <add name="xml"/>  
      </listeners>  
    </source>  
    <source name="myUserTraceSource"  
            switchValue="Information, ActivityTracing">  
      <listeners>  
        <add name="xml"/>  
      </listeners>  
    </source>  
  </sources>  
  <sharedListeners>  
    <add name="xml"  
         type="System.Diagnostics.XmlWriterTraceListener"  
               initializeData="C:\logs\Traces.svclog" />  
  </sharedListeners>  
 </system.diagnostics>  
  
 <system.serviceModel>  
  <diagnostics wmiProviderEnabled="true">  
      <messageLogging
           logEntireMessage="true"
           logMalformedMessages="true"  
           logMessagesAtServiceLevel="true"
           logMessagesAtTransportLevel="true"  
           maxMessagesToLog="3000"
       />  
  </diagnostics>  
 </system.serviceModel>  
</configuration>  
```  
  
## <a name="using-wmi-to-modify-settings"></a>Usando o WMI para modificar as configurações  
 Você pode usar o WMI para alterar as definições de configuração em tempo de execução (habilitando o `wmiProviderEnabled` atributo na configuração, conforme demonstrado no exemplo de configuração anterior). Por exemplo, você pode usar o WMI no CIM Studio para alterar os níveis de origem de rastreamento de aviso para informações em tempo de execução. Você deve estar ciente de que o custo de desempenho da depuração dinâmica dessa maneira pode ser muito alto. Para obter mais informações sobre como usar o WMI, consulte o tópico [usando instrumentação de gerenciamento do Windows para diagnóstico](../wmi/index.md) .  
  
## <a name="enable-correlated-events-in-aspnet-tracing"></a>Habilitar eventos correlacionados no rastreamento de ASP.NET  
 Os eventos ASP.NET não definem a ID de correlação (ActivityId), a menos que o rastreamento de eventos do ASP.NET esteja ativado. Para ver os eventos correlacionados corretamente, você precisa ativar o rastreamento de eventos ASP.NET usando o comando a seguir no console de comando, que pode ser invocado acessando **Iniciar**, **executar** e digitar **cmd**,  
  
```console  
logman start mytrace -pf logman.providers -o test.etl –ets  
```  
  
 Para desativar o rastreamento de eventos ASP.NET, use o comando a seguir,  
  
```console
logman stop mytrace -ets  
```  
  
## <a name="see-also"></a>Veja também

- [Usando Instrumentação de Gerenciamento do Windows para diagnóstico](../wmi/index.md)
