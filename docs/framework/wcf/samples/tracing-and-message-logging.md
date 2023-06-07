---
title: Registro de mensagem e rastreamento
description: Saiba como usar a ferramenta Visualizador de rastreamento de serviço (SvcTraceViewer.exe) para exibir rastreamentos e logs de mensagens usando este exemplo WFC.
ms.date: 03/30/2017
helpviewer_keywords:
- Tracing and logging
ms.assetid: a4f39bfc-3c5e-4d51-a312-71c5c3ce0afd
ms.openlocfilehash: bb49334252c2415223b0f8f5559a6dc838d175e3
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246019"
---
# <a name="tracing-and-message-logging"></a>Registro de mensagem e rastreamento
Este exemplo demonstra como habilitar o rastreamento e o log de mensagens. Os rastreamentos e os logs de mensagens resultantes são exibidos usando a [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer.exe)](../service-trace-viewer-tool-svctraceviewer-exe.md). Este exemplo é baseado na [introdução](getting-started-sample.md).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
## <a name="tracing"></a>Rastreamento  
 Windows Communication Foundation (WCF) usa o mecanismo de rastreamento definido no <xref:System.Diagnostics> namespace. Nesse modelo de rastreamento, os dados de rastreamento são produzidos por fontes de rastreamento que os aplicativos implementam. Cada fonte é identificada por um nome. Os consumidores de rastreamento criam ouvintes de rastreamento para as fontes de rastreamento para as quais desejam recuperar informações. Para receber dados de rastreamento, você deve criar um ouvinte para a origem do rastreamento. No WCF, isso pode ser feito adicionando o código a seguir ao arquivo de configuração do cliente ou do serviço definindo a fonte de rastreamento do modelo de serviço `switchValue` :  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-service.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
</system.diagnostics>  
```  
  
 Para obter mais informações sobre fontes de rastreamento, consulte a seção origem do rastreamento no tópico [Configurando o rastreamento](../diagnostics/tracing/configuring-tracing.md) .  
  
## <a name="activity-tracing-and-propagation"></a>Rastreamento e propagação de atividades  
 Ter `ActivityTracing` habilitado e `propagateActivity` definido como `true` nas `system.ServiceModel` fontes de rastreamento para o cliente e o serviço fornecem a correlação de rastreamentos dentro de unidades lógicas de processamento (atividades), entre atividades em pontos de extremidade (por meio de transferências de atividade) e entre atividades que abrangem vários pontos de extremidade (por meio da propagação de ID de atividade).  
  
 Esses três mecanismos (atividades, transferências e propagação) podem ajudá-lo a localizar a causa raiz de um erro mais rapidamente usando a ferramenta do Visualizador de rastreamento de serviço. Para obter mais informações, consulte [usando o Visualizador de rastreamento de serviço para exibir rastreamentos correlacionados e solução de problemas](../diagnostics/tracing/using-service-trace-viewer-for-viewing-correlated-traces-and-troubleshooting.md).  
  
 É possível estender o rastreamento fornecido pelo ServiceModel criando rastreamentos de atividade definidos pelo usuário. O rastreamento de atividade definido pelo usuário permite que o usuário crie atividades de rastreamento para:  
  
- Agrupa rastreamentos em unidades lógicas de trabalho.  
  
- Correlacione atividades por meio de transferências e propagação.  
  
- Diminua o custo de desempenho do rastreamento do WCF (por exemplo, o custo de espaço em disco de um arquivo de log).  
  
 Para obter mais informações sobre o rastreamento de atividade definido pelo usuário, consulte o exemplo de [rastreamento de extensão](extending-tracing.md) .  
  
## <a name="message-logging"></a>Registro em log de mensagens  
 O log de mensagens pode ser habilitado no cliente e no serviço de qualquer aplicativo WCF. Para habilitar o log de mensagens, você deve adicionar o seguinte código ao cliente ou ao serviço:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <diagnostics>  
      <!-- Enable Message Logging here. -->  
      <!-- log all messages received or sent at the transport or service model levels -->  
      <messageLogging logEntireMessage="true"  
                      maxMessagesToLog="300"  
                      logMessagesAtServiceLevel="true"  
                      logMalformedMessages="true"  
                      logMessagesAtTransportLevel="true" />  
    </diagnostics>  
  </system.serviceModel>  
</configuration>  
```  
  
 Quando uma mensagem é gravada, o tipo de rastreamento depende se ele está sendo rastreado no cliente ou no servidor. Por exemplo, uma mensagem "Adicionar" enviada a um cliente é rastreada sob a categoria "TransportWrite" no cliente, enquanto a mesma mensagem é rastreada sob a categoria "TransportRead" no serviço.  
  
 Configure o ouvinte de rastreamento adicionando o seguinte código à <xref:System.Diagnostics> seção do arquivo de App.config do cliente ou do arquivo de Web.config do serviço:  
  
```xml  
<system.diagnostics>  
    <sources>  
      <source name="System.ServiceModel" switchValue="Information,ActivityTracing"  
        propagateActivity="true">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
      <source name="System.ServiceModel.MessageLogging">  
        <listeners>  
          <add name="xml" />  
        </listeners>  
      </source>  
    </sources>  
    <sharedListeners>  
      <add initializeData="C:\logs\TracingAndLogging-client.svclog" type="System.Diagnostics.XmlWriterTraceListener"  
        name="xml" />  
    </sharedListeners>  
    <trace autoflush="true" />  
  </system.diagnostics>  
```  
  
 As mensagens são registradas em formato XML no diretório de destino especificado no arquivo de configuração.  
  
> [!NOTE]
> Os arquivos de rastreamento não são criados sem criar inicialmente o diretório de log. Verifique se o diretório C:\logs\ existe ou especifique um diretório de log alternativo na configuração do ouvinte. Consulte as instruções de instalação inicial no final deste documento para obter mais informações.  
  
 Para obter mais informações sobre o log de mensagens, consulte o tópico [Configurando o log de mensagens](../diagnostics/configuring-message-logging.md) .  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Antes de executar o rastreamento e o log de mensagens de exemplo, crie o diretório C:\logs\ para o serviço para o qual gravar os arquivos. svclog Connector. O nome desse diretório é definido no arquivo de configuração como o caminho para os rastreamentos e mensagens a serem registrados e podem ser alterados. Conceda ao serviço de rede do usuário acesso de gravação ao diretório de logs.  
  
3. Para criar a edição C#, C++ ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Management\TracingAndLogging`  
  
## <a name="see-also"></a>Veja também

- [Rastreamento](../diagnostics/tracing/index.md)
- [AppFabric que monitora Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
