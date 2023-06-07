---
title: Configurando o rastreamento
description: Saiba como habilitar o rastreamento, configurar origens de rastreamento, definir rastreamento e propagação de atividade e definir ouvintes de rastreamento para acessar rastreamentos no WCF.
ms.date: 03/30/2017
helpviewer_keywords:
- tracing [WCF]
ms.assetid: 82922010-e8b3-40eb-98c4-10fc05c6d65d
ms.openlocfilehash: 55d701ee6769099698d2fd869a1502d94237b5a8
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245343"
---
# <a name="configuring-tracing"></a>Configurando o rastreamento
Este tópico descreve como você pode habilitar o rastreamento, configurar fontes de rastreamento para emitir rastreamentos e definir níveis de rastreamento, definir rastreamento de atividade e propagação para dar suporte à correlação de rastreamento de ponta a ponta e definir ouvintes de rastreamento para acessar rastreamentos.  
  
 Para as recomendações de configurações de rastreamento no ambiente de produção ou de depuração, consulte [as configurações recomendadas para rastreamento e log de mensagens](recommended-settings-for-tracing-and-message-logging.md).  
  
> [!IMPORTANT]
> No Windows 8, você deve executar o aplicativo com privilégios elevados (executar como administrador) para que seu aplicativo gere logs de rastreamento.  
  
## <a name="enabling-tracing"></a>Habilitar o rastreamento  
 Windows Communication Foundation (WCF) gera os seguintes dados para rastreamento de diagnóstico:  
  
- Rastreamentos de etapas de processo em todos os componentes dos aplicativos, como chamadas de operação, exceções de código, avisos e outros eventos de processamento significativos.  
  
- Eventos de erro do Windows quando o recurso de rastreamento não funciona. Consulte [log de eventos](../event-logging/index.md).  
  
 O rastreamento do WCF é criado sobre o <xref:System.Diagnostics> . Para usar o rastreamento, você deve definir fontes de rastreamento no arquivo de configuração ou no código. O WCF define uma origem de rastreamento para cada assembly do WCF. A `System.ServiceModel` origem do rastreamento é a fonte de rastreamento mais geral do WCF e as etapas de processamento de registros na pilha de comunicação do WCF, da entrada/saída do transporte até a inserção/saída do código do usuário. A `System.ServiceModel.MessageLogging` origem do rastreamento registra todas as mensagens que fluem pelo sistema.  
  
 O rastreamento não está habilitado por padrão. Para ativar o rastreamento, você deve criar um ouvinte de rastreamento e definir um nível de rastreamento diferente de "desativado" para a origem de rastreamento selecionada na configuração; caso contrário, o WCF não gerará nenhum rastreamento. Se você não especificar um ouvinte, o rastreamento será desabilitado automaticamente. Se um ouvinte for definido, mas nenhum nível for especificado, o nível será definido como "desativado" por padrão, o que significa que nenhum rastreamento é emitido.  
  
 Se você usar pontos de extensibilidade do WCF, como chamadores de operação personalizados, deverá emitir seus próprios rastreamentos. Isso ocorre porque, se você implementar um ponto de extensibilidade, o WCF não poderá mais emitir os rastreamentos padrão no caminho padrão. Se você não implementar o suporte de rastreamento manual emitindo rastreamentos, talvez você não veja os rastreamentos esperados.  
  
 Você pode configurar o rastreamento editando o arquivo de configuração do aplicativo — seja Web.config para aplicativos hospedados na Web ou Appname.exe.config para aplicativos hospedados internamente. Veja a seguir um exemplo de tal edição. Para obter mais informações sobre essas configurações, consulte a seção "Configurando ouvintes de rastreamento para consumir rastreamentos".  
  
```xml  
<configuration>  
   <system.diagnostics>  
      <sources>  
         <source name="System.ServiceModel"
                    switchValue="Information, ActivityTracing"  
                    propagateActivity="true">  
            <listeners>  
               <add name="traceListener"
                   type="System.Diagnostics.XmlWriterTraceListener"
                   initializeData= "c:\log\Traces.svclog" />  
            </listeners>  
         </source>  
      </sources>  
   </system.diagnostics>  
</configuration>  
```  
  
> [!NOTE]
> Para editar o arquivo de configuração de um projeto de serviço WCF no Visual Studio, clique com o botão direito do mouse no arquivo de configuração do aplicativo — seja Web.config para aplicativos hospedados na Web ou Appname.exe.config para aplicativo auto-hospedado no **Gerenciador de soluções**. Em seguida, escolha o item de menu de contexto **Editar configuração do WCF** . Isso inicia a [ferramenta do editor de configuração (SvcConfigEditor.exe)](../../configuration-editor-tool-svcconfigeditor-exe.md), que permite que você modifique as definições de configuração dos serviços WCF usando uma interface gráfica do usuário.  
  
## <a name="configuring-trace-sources-to-emit-traces"></a>Configurando fontes de rastreamento para emitir rastreamentos  
 O WCF define uma origem de rastreamento para cada assembly. Os rastreamentos gerados em um assembly são acessados pelos ouvintes definidos para essa fonte. As seguintes origens de rastreamento são definidas:  
  
- System. ServiceModel: registra todos os estágios do processamento do WCF, sempre que a configuração é lida, uma mensagem é processada em transporte, processamento de segurança, uma mensagem é expedida no código do usuário e assim por diante.  
  
- System. ServiceModel. MessageLogging: registra todas as mensagens que fluem pelo sistema.  
  
- System. IdentityModel.  
  
- System. ServiceModel. Activation.  
  
- System. IO. log: registro em log para a interface de .NET Framework para o Sistema de Arquivos de Log Comum (CLFS).  
  
- System. Runtime. Serialization: registra quando objetos são lidos ou gravados.  
  
- CardSpace.  
  
 Você pode configurar cada origem de rastreamento para usar o mesmo ouvinte (compartilhado), conforme indicado no exemplo de configuração a seguir.  
  
```xml  
<configuration>  
    <system.diagnostics>  
        <sources>  
            <source name="System.ServiceModel"
                    switchValue="Information, ActivityTracing"  
                    propagateActivity="true">  
                <listeners>  
                    <add name="xml" />  
                </listeners>  
            </source>  
            <source name="CardSpace">  
                <listeners>  
                    <add name="xml" />  
                </listeners>  
            </source>  
            <source name="System.IO.Log">  
                <listeners>  
                    <add name="xml" />  
                </listeners>  
            </source>  
            <source name="System.Runtime.Serialization">  
                <listeners>  
                    <add name="xml" />  
                </listeners>  
            </source>  
            <source name="System.IdentityModel">  
                <listeners>  
                    <add name="xml" />  
                </listeners>  
            </source>  
        </sources>  
  
        <sharedListeners>  
            <add name="xml"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="c:\log\Traces.svclog" />  
        </sharedListeners>  
    </system.diagnostics>  
</configuration>  
```  
  
 Além disso, você pode adicionar fontes de rastreamento definidas pelo usuário, conforme demonstrado pelo exemplo a seguir, para emitir rastreamentos de código de usuário.  
  
```xml  
<system.diagnostics>  
   <sources>  
       <source name="UserTraceSource" switchValue="Warning, ActivityTracing" >  
          <listeners>  
              <add name="xml"  
                 type="System.Diagnostics.XmlWriterTraceListener"  
                 initializeData="C:\logs\UserTraces.svclog" />  
          </listeners>  
       </source>  
   </sources>  
   <trace autoflush="true" />
</system.diagnostics>  
```  
  
 Para obter mais informações sobre como criar fontes de rastreamento definidas pelo usuário, consulte [estendendo o rastreamento](../../samples/extending-tracing.md).  
  
## <a name="configuring-trace-listeners-to-consume-traces"></a>Configurando ouvintes de rastreamento para consumir rastreamentos  
 Em tempo de execução, os feeds do WCF rastreiam dados para os ouvintes, que processam os dados. O WCF fornece vários ouvintes predefinidos para <xref:System.Diagnostics> , que diferem no formato usado para saída. Você também pode adicionar tipos de ouvinte personalizados.  
  
 Você pode usar `add` para especificar o nome e o tipo do ouvinte de rastreamento que deseja usar. Em nossa configuração de exemplo, nomeamos o ouvinte `traceListener` e adicionamos o ouvinte de rastreamento de .NET Framework padrão ( `System.Diagnostics.XmlWriterTraceListener` ) como o tipo que desejamos usar. Você pode adicionar qualquer número de ouvintes de rastreamento para cada fonte. Se o ouvinte de rastreamento emitir o rastreamento para um arquivo, você deverá especificar o local e o nome do arquivo de saída no arquivo de configuração. Isso é feito definindo `initializeData` o nome do arquivo para esse ouvinte. Se você não especificar um nome de arquivo, um nome de arquivo aleatório será gerado com base no tipo de ouvinte usado. Se <xref:System.Diagnostics.XmlWriterTraceListener> for usado, um nome de arquivo sem extensão será gerado. Se você implementar um ouvinte personalizado, também poderá usar esse atributo para receber dados de inicialização diferentes de um nome de arquivo. Por exemplo, você pode especificar um identificador de banco de dados para esse atributo.  
  
 Você pode configurar um ouvinte de rastreamento personalizado para enviar rastreamentos na conexão, por exemplo, para um banco de dados remoto. Como um implantador de aplicativos, você deve impor o controle de acesso apropriado nos logs de rastreamento no computador remoto.  
  
 Você também pode configurar um ouvinte de rastreamento programaticamente. Para obter mais informações, consulte [como: criar e inicializar ouvintes de rastreamento](../../../debug-trace-profile/how-to-create-and-initialize-trace-listeners.md) e [criar um TraceListener personalizado](https://docs.microsoft.com/archive/msdn-magazine/2006/april/clr-inside-out-extending-system-diagnostics).  
  
> [!CAUTION]
> Como `System.Diagnostics.XmlWriterTraceListener` o não é thread-safe, a origem do rastreamento pode bloquear recursos exclusivamente durante a saída de rastreamentos. Quando muitos threads geram rastreamentos para uma origem de rastreamento configurada para usar esse ouvinte, pode ocorrer contenção de recursos, o que resulta em um problema de desempenho significativo. Para resolver esse problema, você deve implementar um ouvinte personalizado que seja thread-safe.  
  
## <a name="trace-level"></a>Nível de rastreamento:  
 O nível de rastreamento é controlado pela `switchValue` configuração da origem do rastreamento. Os níveis de rastreamento disponíveis são descritos na tabela a seguir.  
  
|Nível de rastreamento:|Natureza dos eventos rastreados|Conteúdo dos eventos rastreados|Eventos rastreados|Destino do usuário|  
|-----------------|----------------------------------|-----------------------------------|--------------------|-----------------|  
|Desativado|N/D|N/D|Nenhum rastreamento é emitido.|N/D|  
|Crítico|Eventos "negativos": eventos que indicam um processamento inesperado ou uma condição de erro.||As exceções sem tratamento, incluindo as seguintes, são registradas em log:<br /><br /> -OutOfMemoryexception<br />-ThreadAbortException (o CLR invoca qualquer ThreadAbortExceptionHandler)<br />-StackOverflowException (não pode ser capturado)<br />- ConfigurationErrorsException<br />-SEHException<br />-Erros de início do aplicativo<br />-Eventos FailFast<br />-Travamentos do sistema<br />-Mensagens suspeitas: rastreamentos de mensagem que causam falha no aplicativo.|Administradores<br /><br /> Desenvolvedores de aplicativos|  
|Erro|Eventos "negativos": eventos que indicam um processamento inesperado ou uma condição de erro.|Ocorreu um processamento inesperado. O aplicativo não pôde executar uma tarefa conforme o esperado. No entanto, o aplicativo ainda está em execução.|Todas as exceções são registradas.|Administradores<br /><br /> Desenvolvedores de aplicativos|  
|Aviso|Eventos "negativos": eventos que indicam um processamento inesperado ou uma condição de erro.|Um possível problema ocorreu ou pode ocorrer, mas o aplicativo ainda funciona corretamente. No entanto, ele pode não continuar a funcionar corretamente.|-O aplicativo está recebendo mais solicitações do que as configurações de limitação permitem.<br />-A fila de recebimento está perto da capacidade máxima configurada.<br />-Tempo limite excedido.<br />-As credenciais são rejeitadas.|Administradores<br /><br /> Desenvolvedores de aplicativos|  
|Informações|Eventos "positivos": eventos que marcam Marcos com êxito|Marcos importantes e bem-sucedidos da execução do aplicativo, independentemente de o aplicativo estar funcionando corretamente ou não.|Em geral, as mensagens úteis para monitorar e diagnosticar o status do sistema, medir o desempenho ou a criação de perfil são geradas. Você pode usar essas informações para o planejamento de capacidade e o gerenciamento de desempenho:<br /><br /> -Os canais são criados.<br />-Os ouvintes de ponto de extremidade são criados.<br />-A mensagem entra/sai do transporte.<br />-O token de segurança é recuperado.<br />-A definição de configuração é leitura.|Administradores<br /><br /> Desenvolvedores de aplicativos<br /><br /> Desenvolvedores de produtos.|  
|Detalhado|Eventos "positivos": eventos que marcam Marcos bem-sucedidos.|Eventos de nível baixo para código de usuário e serviço são emitidos.|Em geral, você pode usar esse nível para depuração ou otimização de aplicativo.<br /><br /> -Compreendido o cabeçalho da mensagem.|Administradores<br /><br /> Desenvolvedores de aplicativos<br /><br /> Desenvolvedores de produtos.|  
|ActivityTracing||Eventos de fluxo entre atividades de processamento e componentes.|Esse nível permite que os administradores e desenvolvedores correlacionem aplicativos no mesmo domínio de aplicativo:<br /><br /> -Rastreia os limites de atividade, como iniciar/parar.<br />-Rastreia para transferências.|Todos|  
|Todos||O aplicativo pode funcionar corretamente. Todos os eventos são emitidos.|Todos os eventos anteriores.|Tudo|  
  
 Os níveis de detalhado para crítico são empilhados em cima um do outro, ou seja, cada nível de rastreamento inclui todos os níveis acima dele, exceto o nível desativado. Por exemplo, um ouvinte ouvindo no nível de aviso recebe rastreamentos críticos, de erro e de aviso. O nível All inclui eventos de detalhado para eventos de rastreamento de atividade e críticos.  
  
> [!CAUTION]
> Os níveis de informações, detalhados e ActivityTracing geram muitos rastreamentos, o que pode afetar negativamente a taxa de transferência de mensagens se você tiver usado todos os recursos disponíveis no computador.  
  
## <a name="configuring-activity-tracing-and-propagation-for-correlation"></a>Configurando o rastreamento de atividade e a propagação para correlação  
 O `activityTracing` valor especificado para o `switchValue` atributo é usado para habilitar o rastreamento de atividade, que emite rastreamentos para limites de atividade e transferências em pontos de extremidade.  
  
> [!NOTE]
> Ao usar determinados recursos de extensibilidade no WCF, você pode obter um <xref:System.NullReferenceException> quando o rastreamento de atividade está habilitado. Para corrigir esse problema, verifique o arquivo de configuração do aplicativo e verifique se o `switchValue` atributo da origem do rastreamento não está definido como `activityTracing` .  
  
 O `propagateActivity` atributo indica se a atividade deve ser propagada para outros pontos de extremidade que participam da troca de mensagens. Ao definir esse valor como `true` , você pode fazer os arquivos de rastreamento gerados por quaisquer dois pontos de extremidade e observar como um conjunto de rastreamentos em um ponto de extremidade fluiu para um conjunto de rastreamentos em outro ponto.  
  
 Para obter mais informações sobre rastreamento de atividade e propagação, consulte [propagação](propagation.md).  
  
 Ambos `propagateActivity` os `ActivityTracing` valores Boolianos e booleanos se aplicam ao rastreador System. ServiceModel. O `ActivityTracing` valor também se aplica a qualquer origem de rastreamento, incluindo WCF ou aqueles definidos pelo usuário.  
  
 Você não pode usar o `propagateActivity` atributo com fontes de rastreamento definidas pelo usuário. Para a propagação da ID da atividade de código do usuário, certifique-se de não definir ServiceModel `ActivityTracing` , enquanto ainda tem o `propagateActivity` atributo ServiceModel definido como `true` .  
  
## <a name="see-also"></a>Veja também

- [Rastreamento](index.md)
- [Administração e diagnóstico](../index.md)
- [Como criar e inicializar ouvintes de rastreamento](../../../debug-trace-profile/how-to-create-and-initialize-trace-listeners.md)
- [Criando um TraceListener personalizado](https://docs.microsoft.com/archive/msdn-magazine/2006/april/clr-inside-out-extending-system-diagnostics)
