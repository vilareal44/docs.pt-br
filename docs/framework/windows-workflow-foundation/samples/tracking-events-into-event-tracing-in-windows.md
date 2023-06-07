---
title: Eventos de rastreamento no rastreamento de evento no Windows
ms.date: 03/30/2017
ms.assetid: f812659b-0943-45ff-9430-4defa733182b
ms.openlocfilehash: fa5d86e327bc9c6eca85ed2908775de5f647f410
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144884"
---
# <a name="tracking-events-into-event-tracing-in-windows"></a>Eventos de rastreamento no rastreamento de evento no Windows

Este exemplo demonstra como habilitar o rastreamento de Windows Workflow Foundation (WF) em um serviço de fluxo de trabalho e emitir os eventos de rastreamento no ETW (rastreamento de eventos para Windows). Para emitir registros de acompanhamento de fluxo de trabalho em ETW, o exemplo usa o participante de rastreamento de<xref:System.Activities.Tracking.EtwTrackingParticipant>(ETW).

O fluxo de trabalho no exemplo recebe uma solicitação, atribui o recíproco de dados de entrada à variável de entrada e retorna o recíproco de volta para o cliente. Quando os dados de entrada é 0, uma partilha a exceção ocorre zero que é não tratados que faz com que o fluxo de trabalho nulo. Com o rastreamento ativado, a reputação de erro é emitida a ETW, que pode ajudar a resolver problemas posteriormente o erro. O participante de rastreamento de ETW é configurado com um perfil de rastreamento para assinar a acompanhar registros. O perfil de rastreamento é definido no arquivo web.config e como como um parâmetro de configuração para participante de rastreamento de ETW. O participante de rastreamento de ETW é configurado no arquivo web.config do serviço de fluxo de trabalho e aplicado ao serviço como um comportamento de serviço. Nesse exemplo, você exibe os eventos de rastreamento no log de eventos usando o visualizador de eventos.

## <a name="workflow-tracking-details"></a>Detalhes de acompanhamento de fluxo de trabalho

Windows Workflow Foundation fornece uma infraestrutura de controle para acompanhar a execução de uma instância de fluxo de trabalho. O runtime de rastreamento cria uma instância de fluxo de trabalho para emitir os eventos relacionados ao ciclo de vida de fluxo de trabalho, os eventos de atividades de fluxo de trabalho e eventos personalizados. A tabela a seguir detalha os componentes principais de infraestrutura de rastreamento.

|Componente|Descrição|
|---------------|-----------------|
|runtime de rastreamento|Fornece a infraestrutura para emitir registros de rastreamento.|
|Participantes de rastreamento|Acessa os registros de rastreamento. vem de[!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] com um participante de rastreamento que grava registros de rastreamento como o rastreamento de evento para eventos do Windows (ETW).|
|Controlando o perfil|Um mecanismo de filtragem que permite que um participante de rastreamento assine para um subconjunto de registros de rastreamento emissores de uma instância de fluxo de trabalho.|

A tabela a seguir detalha os registros de rastreamento que o runtime de fluxo de trabalho se emite.

|Controlando o registro|Descrição|
|---------------------|-----------------|
|Registros de rastreamento de instância de fluxo de trabalho.|Descreve o ciclo de vida de instância de fluxo de trabalho. Por exemplo, um registro de instância é emitida quando o fluxo de trabalho inicia ou termina.|
|Estado da atividade que acompanha registros.|Detalha a execução da atividade. Esses registros indicam o estado de uma atividade de fluxo de trabalho como quando uma atividade é agendada ou quando a atividade completa ou quando uma falha é lançada.|
|Registro de ressunção do indexador.|Emitido sempre que um marcador em uma instância de fluxo de trabalho que é.|
|Registros personalizados de rastreamento.|Um autor de fluxo de trabalho pode criar registros personalizados de rastreamento e emitir os dentro da atividade personalizado.|
|<xref:System.Activities.Tracking.ActivityScheduledRecord>|Esse registro é emitida quando agendas de atividade outra atividade.|
|<xref:System.Activities.Tracking.FaultPropagationRecord>|Esse registro é emitida quando uma falha é propagada de uma atividade.|
|<xref:System.Activities.Tracking.CancelRequestedRecord>|Esse registro é emitida quando uma atividade é cancelada por outra atividade.|

O participante de rastreamento assinatura para um subconjunto de registros emissores de rastreamento usando perfis de rastreamento. Um perfil de rastreamento contém consultas de rastreamento que permitem assinar para um tipo de registro específico de rastreamento. Controlar perfis pode ser especificado no código ou na configuração.

#### <a name="to-use-this-sample"></a>Para usar este exemplo

1. Usando o Visual Studio 2010, abra o arquivo de solução EtwTrackingParticipantSample. sln.

2. Para criar a solução, pressione CTRL+SHIFT+B.

3. Para executar a solução, pressione F5.

    Por padrão, o serviço está escutando na porta 53797 ( `http://localhost:53797/SampleWorkflowService.xamlx` ).

4. Usando o explorador de arquivos, abra o cliente de teste do WCF.

    O cliente de teste do WCF (WcfTestClient. exe) está localizado na \<Visual Studio 2010 installation folder> pasta \Common7\IDE\

    A pasta de instalação padrão do Visual Studio 2010 é C:\Program Files\Microsoft Visual Studio 10,0.

5. Em cliente de teste do WCF, selecione **Adicionar serviço** no menu **arquivo** .

    Adicione o endereço do ponto de extremidade na caixa de entrada. O padrão é `http://localhost:53797/SampleWorkflowService.xamlx`.

6. Abra o aplicativo visualizador de eventos.

    Antes de invocar o serviço, inicie Visualizador de Eventos no menu **Iniciar** , selecione **executar** e digite `eventvwr.exe` . Certifique-se de que o log de eventos é escutando eventos de rastreamento emissores de serviço de fluxo de trabalho.

7. No modo de exibição de árvore do Visualizador de Eventos, navegue até **Visualizador de eventos**, **aplicativos e logs de serviços**e **Microsoft**. Clique com o botão direito do mouse em **Microsoft** e selecione **Exibir** e, em seguida, **Mostrar logs analíticos e de depuração** para habilitar logs analíticos e de depuração

    Verifique se a opção **Mostrar logs analíticos e de depuração** está marcada.

8. No modo de exibição de árvore no Visualizador de Eventos, navegue até **Visualizador de eventos**, **logs de aplicativos e serviços**, **Microsoft**, **Windows**, **servidor de aplicativos-aplicativos**. Clique com o botão direito do mouse em **analítica** e selecione **habilitar log** para habilitar o log **analítico** .

9. Testar o serviço usando o cliente de teste de WCF clicando duas vezes `GetData`.

    Isso abre o método de `GetData` . A solicitação aceita um parâmetro e garante que o valor é 0, que é o padrão.

     Clique em **invocar**.

10. Observe os eventos emissores de fluxo de trabalho.

    Volte para Visualizador de Eventos e navegue até **Visualizador de eventos**, **logs de aplicativos e serviços**, **Microsoft**, **Windows**, **servidor de aplicativos-aplicativos**. Clique com o botão direito do mouse em **analítica** e selecione **Atualizar**.

    Os eventos de fluxo de trabalho são exibidos no visualizador de eventos. Observe que os eventos de execução de fluxo de trabalho são exibidos e que um deless é uma exceção sem tratamento que corresponde ao erro no fluxo de trabalho. Além disso, um evento de aviso é emitida de atividade de fluxo de trabalho, que indica que a atividade é lançando uma falha.

11. Repita as etapas 9 e 10 com uma entrada de dados diferente de 0, de modo que nenhum erro é lançada.

Controlando perfis permitem que você assine a eventos que são emitidas em runtime em que o estado de uma instância de fluxo de trabalho muda. Dependendo dos requisitos de monitoramento, você pode criar um perfil que é muito grosseiro, que assina um pequeno conjunto de alterações de estado de alto nível em um fluxo de trabalho. Por outro lado, você pode criar um perfil muito preciso cujas ambas as saída é suficiente ricas posteriormente recompilar toda a execução. O exemplo demonstra os eventos emissores do runtime de fluxo de trabalho a ETW usando `HealthMonitoring Tracking Profile`, que se emite um pequeno conjunto de eventos. Um perfil diferente que emite mais eventos de rastreamento de fluxo de trabalho também é fornecido no Web.config que é chamado `Troubleshooting Tracking Profile`. Quando [!INCLUDE[netfx_current_short](../../../../includes/netfx-current-short-md.md)] é instalado, um perfil padrão com um nome vazia é configurado no arquivo Machine.config. Este perfil é usado pela configuração do comportamento de rastreamento de ETW quando nenhum nome de perfil ou um nome vazia de perfil são especificados.

O perfil de acompanhamento de monitoramento de integridade emite-se registros de instância de fluxo de trabalho e registros de propagação de falha de atividade. Este perfil é criado adicionando o seguinte perfil de rastreamento em um arquivo de configuração Web.config.

```xml
<tracking>
  <profiles>
    <trackingProfile name="HealthMonitoring Tracking Profile">
      <workflow activityDefinitionId="*">
        <workflowInstanceQueries>
          <workflowInstanceQuery>
            <states>
              <state name="Started"/>
              <state name="Completed"/>
              <state name="Aborted"/>
              <state name="UnhandledException"/>
            </states>
          </workflowInstanceQuery>
        </workflowInstanceQueries>
        <faultPropagationQueries>
          <faultPropagationQuery faultSourceActivityName ="*" faultHandlerActivityName="*"/>
        </faultPropagationQueries>
      </workflow>
    </trackingProfile>
  </profiles>
</tracking>
```

 O perfil pode ser alterado alterando a configuração de `EtwTrackingParticipant` a seguir.

```xml
<behaviors>
  <serviceBehaviors>
    <behavior>
      <etwTracking profileName="HealthMonitoring Tracking Profile"/>
    </behavior>
  </serviceBehaviors>
</behaviors>
```

#### <a name="to-clean-up-optional"></a>Para limpar (opcional)

1. Visualizador de EventosAberto.

2. Navegue até **Visualizador de eventos**, **logs de aplicativos e serviços**, **Microsoft**, **Windows**, **servidor de aplicativos-aplicativos**. Clique com o botão direito do mouse em **analítica** e selecione **desabilitar log**.

3. Navegue até **Visualizador de eventos**, **logs de aplicativos e serviços**, **Microsoft**, **Windows**, **servidor de aplicativos-aplicativos**. Clique com o botão direito do mouse em **analítica** e selecione **limpar log**.

4. Escolha a opção **limpar** para limpar os eventos.

## <a name="known-issue"></a>Problema conhecido

> [!NOTE]
> Há um problema conhecido em Visualizador de Eventos onde pode não decodifica eventos de ETW. Você pode ver a uma mensagem de erro semelhante ao seguinte.
>
> A descrição para a ID \<id> do evento da origem Microsoft-Windows-Application Server – Applications não pode ser encontrada. O componente que gera esse evento não está instalado no computador local ou a instalação está corrompida. Você pode instalar ou reparar o componente no computador local.
>
> Se você encontrar esse erro, atualização de clique no painel ações. O evento agora deve decodificar corretamente.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WF\Basic\Tracking\EtwTracking`

## <a name="see-also"></a>Consulte também

- [AppFabric que monitora Exemplos](https://docs.microsoft.com/previous-versions/appfabric/ff383407(v=azure.10))
