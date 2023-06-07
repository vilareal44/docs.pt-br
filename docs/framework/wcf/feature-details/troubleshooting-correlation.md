---
title: Correlação de solução de problemas
ms.date: 03/30/2017
ms.assetid: 98003875-233d-4512-a688-4b2a1b0b5371
ms.openlocfilehash: a5942c13bb4026cfeb8f664c60fb658373ffcca5
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596702"
---
# <a name="troubleshooting-correlation"></a>Correlação de solução de problemas
A correlação é usada para relacionar mensagens do serviço de fluxo de trabalho entre si e com a instância de fluxo de trabalho correta, mas se não estiver configurada corretamente, as mensagens não serão recebidas e os aplicativos não funcionarão corretamente. Este tópico fornece uma visão geral de vários métodos para solucionar problemas de correlação e também lista alguns problemas comuns que podem ocorrer quando você usa a correlação.

## <a name="handle-the-unknownmessagereceived-event"></a>Manipular o evento UnknownMessageReceived
 O <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> evento ocorre quando uma mensagem desconhecida é recebida por um serviço, incluindo mensagens que não podem ser correlacionadas a uma instância existente. Para serviços hospedados internamente, esse evento pode ser tratado no aplicativo host.

```csharp
host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
{
    Console.WriteLine("Unknown Message Received:");
    Console.WriteLine(e.Message);
};
```

 Para serviços hospedados na Web, esse evento pode ser tratado pela derivação de uma classe <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> e substituição <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory.CreateWorkflowServiceHost%2A> .

```csharp
class CustomFactory : WorkflowServiceHostFactory
{
    protected override WorkflowServiceHost CreateWorkflowServiceHost(Activity activity, Uri[] baseAddresses)
    {
        // Create the WorkflowServiceHost.
        WorkflowServiceHost host = new WorkflowServiceHost(activity, baseAddresses);

        // Handle the UnknownMessageReceived event.
        host.UnknownMessageReceived += delegate(object sender, UnknownMessageReceivedEventArgs e)
        {
            Console.WriteLine("Unknown Message Received:");
            Console.WriteLine(e.Message);
        };

        return host;
    }
}
```

 Esse personalizado <xref:System.ServiceModel.Activities.Activation.WorkflowServiceHostFactory> pode ser especificado no `svc` arquivo para o serviço.

`<% @ServiceHost Language="C#" Service="OrderServiceWorkflow" Factory="CustomFactory" %>`

 Quando esse manipulador é invocado, a mensagem pode ser recuperada usando a <xref:System.ServiceModel.UnknownMessageReceivedEventArgs.Message%2A> Propriedade do <xref:System.ServiceModel.UnknownMessageReceivedEventArgs> e será semelhante à mensagem a seguir.

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <To s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://localhost:8080/OrderService</To>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
  </s:Header>
  <s:Body>
    <AddItem xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItem>
  </s:Body>
</s:Envelope>
```

 Inspecionar mensagens expedidas para o <xref:System.ServiceModel.ServiceHostBase.UnknownMessageReceived> manipulador pode fornecer dicas sobre por que a mensagem não se correlacionau a uma instância do serviço de fluxo de trabalho.

## <a name="use-tracking-to-monitor-the-progress-of-the-workflow"></a>Use o rastreamento para monitorar o progresso do fluxo de trabalho
 O rastreamento fornece uma maneira de monitorar o progresso de um fluxo de trabalho. Por padrão, os registros de rastreamento são emitidos para eventos do ciclo de vida do fluxo de trabalho, eventos do ciclo de vida da atividade, propagação de falha e continuação do indicador. Além disso, os registros de controle personalizados podem ser emitidos por atividades personalizadas. Ao solucionar problemas de correlação, os registros de rastreamento de atividade, os registros de continuação de indicador e os registros de propagação de falha são os mais úteis. Os registros de rastreamento de atividade podem ser usados para determinar o progresso atual do fluxo de trabalho e podem ajudar a identificar qual atividade de mensagens está aguardando mensagens no momento. Os registros de continuação de indicador são úteis porque indicam que uma mensagem foi recebida pelo fluxo de trabalho e os registros de propagação de falha fornecem um registro de quaisquer falhas no fluxo de trabalho. Para habilitar o acompanhamento, especifique o desejado <xref:System.Activities.Tracking.TrackingParticipant> no <xref:System.ServiceModel.Activities.WorkflowServiceHost.WorkflowExtensions%2A> do <xref:System.ServiceModel.Activities.WorkflowServiceHost> . No exemplo a seguir, o `ConsoleTrackingParticipant` (do exemplo de [rastreamento personalizado](../../windows-workflow-foundation/samples/custom-tracking.md) ) é configurado usando o perfil de rastreamento padrão.

```csharp
host.WorkflowExtensions.Add(new ConsoleTrackingParticipant());
```

 Um participante de rastreamento como o ConsoleTrackingParticipant é útil para serviços de fluxo de trabalho autohospedados que têm uma janela de console. Para um serviço hospedado na Web, um participante de rastreamento que registra as informações de rastreamento em um repositório durável deve ser usado, como o interno <xref:System.Activities.Tracking.EtwTrackingParticipant> ou um participante de rastreamento personalizado que registra as informações em um arquivo.

 Para obter mais informações sobre como controlar e configurar o acompanhamento de um serviço de fluxo de trabalho hospedado na Web, consulte rastreamento [e acompanhamento de fluxo de trabalho](../../windows-workflow-foundation/workflow-tracking-and-tracing.md), [Configurando o acompanhamento de um fluxo de trabalho](../../windows-workflow-foundation/configuring-tracking-for-a-workflow.md)e o [acompanhamento &#91;exemplos do WF&#93;](../../windows-workflow-foundation/samples/tracking.md) amostras.

## <a name="use-wcf-tracing"></a>Usar o rastreamento do WCF
 O rastreamento do WCF fornece rastreamento do fluxo de mensagens de e para um serviço de fluxo de trabalho. Essas informações de rastreamento são úteis ao solucionar problemas de correlação, especialmente para correlação baseada em conteúdo. Para habilitar o rastreamento, especifique os ouvintes de rastreamento desejados na `system.diagnostics` seção do `web.config` arquivo se o serviço de fluxo de trabalho for hospedado na Web ou o `app.config` arquivo se o serviço de fluxo de trabalho for auto-hospedado. Para incluir o conteúdo das mensagens no arquivo de rastreamento, especifique `true` for `logEntireMessage` no `messageLogging` elemento na `diagnostics` seção de `system.serviceModel` . No exemplo a seguir, as informações de rastreamento, incluindo o conteúdo das mensagens, estão configuradas para serem gravadas em um arquivo chamado `service.svclog` .

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
  <system.diagnostics>
    <sources>
      <source name="System.ServiceModel" switchValue="Information" propagateActivity="true">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
      <source name="System.ServiceModel.MessageLogging">
        <listeners>
          <add name="corr"/>
        </listeners>
      </source>
    </sources>

    <sharedListeners>
      <add name="corr" type="System.Diagnostics.XmlWriterTraceListener" initializeData="c:\logs\service.svclog">
      </add>
    </sharedListeners>
  </system.diagnostics>

  <system.serviceModel>
    <diagnostics>
      <messageLogging logEntireMessage="true" logMalformedMessages="false"
         logMessagesAtServiceLevel="false" logMessagesAtTransportLevel="true" maxSizeOfMessageToLog="2147483647">
      </messageLogging>
    </diagnostics>
  </system.serviceModel>
</configuration>
```

 Para exibir as informações de rastreamento contidas no `service.svclog` , a [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../service-trace-viewer-tool-svctraceviewer-exe.md) é usada. Isso é especialmente útil ao solucionar problemas de correlação baseada em conteúdo, pois você pode exibir o conteúdo da mensagem e ver exatamente o que está sendo passado e se ele corresponde ao <xref:System.ServiceModel.CorrelationQuery> para a correlação baseada em conteúdo. Para obter mais informações sobre o rastreamento do WCF, consulte [ferramenta do Visualizador de rastreamento de serviço (SvcTraceViewer. exe)](../service-trace-viewer-tool-svctraceviewer-exe.md), [Configurando o rastreamento](../diagnostics/tracing/configuring-tracing.md)e [usando o rastreamento para solucionar problemas de seu aplicativo](../diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md).

## <a name="common-context-exchange-correlation-issues"></a>Problemas comuns de correlação do Exchange de contexto
 Determinados tipos de correlação exigem que um tipo específico de associação seja usado para que a correlação funcione corretamente. Os exemplos incluem a correlação de solicitação-resposta, que requer uma associação bidirecional, como <xref:System.ServiceModel.BasicHttpBinding> , e a correlação do Exchange de contexto, que requer uma associação baseada em contexto, como <xref:System.ServiceModel.BasicHttpContextBinding> . A maioria das associações dá suporte a operações bidirecionais, portanto, isso não é um problema comum para a correlação de solicitação-resposta, mas há apenas algumas associações baseadas em contexto, incluindo <xref:System.ServiceModel.BasicHttpContextBinding> , <xref:System.ServiceModel.WSHttpContextBinding> e <xref:System.ServiceModel.NetTcpContextBinding> . Se uma dessas associações não for usada, a chamada inicial para um serviço de fluxo de trabalho terá êxito, mas as chamadas subsequentes falharão com o seguinte <xref:System.ServiceModel.FaultException> .

```output
There is no context attached to the incoming message for the service
and the current operation is not marked with "CanCreateInstance = true".
In order to communicate with this service check whether the incoming binding
supports the context protocol and has a valid context initialized.
```

 As informações de contexto usadas para correlação de contexto podem ser retornadas pelo <xref:System.ServiceModel.Activities.SendReply> para a <xref:System.ServiceModel.Activities.Receive> atividade que inicializa a correlação de contexto ao usar uma operação bidirecional, ou pode ser especificada pelo chamador se a operação for unidirecional. Se o contexto não for enviado pelo chamador ou retornado pelo serviço de fluxo de trabalho, o mesmo <xref:System.ServiceModel.FaultException> descrito anteriormente será retornado quando uma operação subsequente for invocada.

## <a name="common-request-reply-correlation-issues"></a>Problemas comuns de correlação de solicitação-resposta
 A correlação de solicitação-resposta é usada com um <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> par para implementar uma operação bidirecional em um serviço de fluxo de trabalho e com um <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> par que invoca uma operação bidirecional em outro serviço Web. Ao invocar uma operação bidirecional em um serviço WCF, o serviço pode ser um serviço WCF baseado em código imperativo tradicional ou pode ser um serviço de fluxo de trabalho. Para usar a correlação de solicitação-resposta, uma associação bidirecional deve ser usada, como <xref:System.ServiceModel.BasicHttpBinding> , e as operações devem ser bidirecionais.

 Se o serviço de fluxo de trabalho tiver operações bidirecionais em paralelo, ou sobrepostas <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> ou <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> pares, o gerenciamento de identificadores de correlação implícito fornecido pelo <xref:System.ServiceModel.Activities.WorkflowServiceHost> pode não ser suficiente, especialmente em cenários de alto estresse, e as mensagens podem não ser roteadas corretamente. Para evitar que esse problema ocorra, é recomendável que você sempre especifique explicitamente um <xref:System.ServiceModel.Activities.CorrelationHandle> ao usar a correlação de solicitação-resposta. Ao usar os modelos **SendAndReceiveReply** e **ReceiveAndSendReply** da seção mensagens da caixa de **ferramentas** no designer de fluxo de trabalho, um <xref:System.ServiceModel.Activities.CorrelationHandle> é explicitamente configurado por padrão. Ao criar um fluxo de trabalho usando código, o <xref:System.ServiceModel.Activities.CorrelationHandle> é especificado no <xref:System.ServiceModel.Activities.Receive.CorrelationInitializers%2A> da primeira atividade no par. No exemplo a seguir, uma <xref:System.ServiceModel.Activities.Receive> atividade é configurada com um <xref:System.ServiceModel.Activities.CorrelationInitializer.CorrelationHandle%2A> especificado explícito no <xref:System.ServiceModel.Activities.RequestReplyCorrelationInitializer> .

```csharp
Variable<CorrelationHandle> RRHandle = new Variable<CorrelationHandle>();

Receive StartOrder = new Receive
{
    CanCreateInstance = true,
    ServiceContractName = OrderContractName,
    OperationName = "StartOrder",
    CorrelationInitializers =
    {
        new RequestReplyCorrelationInitializer
        {
            CorrelationHandle = RRHandle
        }
    }
};

SendReply ReplyToStartOrder = new SendReply
{
    Request = StartOrder,
    Content = ... // Contains the return value, if any.
};

// Construct a workflow using StartOrder and ReplyToStartOrder.
```

 A persistência não é permitida entre um <xref:System.ServiceModel.Activities.Receive> / <xref:System.ServiceModel.Activities.SendReply> par ou um <xref:System.ServiceModel.Activities.Send> / <xref:System.ServiceModel.Activities.ReceiveReply> par. Uma zona de não persistência é criada e dura até que ambas as atividades sejam concluídas. Se uma atividade, como uma atividade de atraso, estiver nessa zona de não persistência e fizer com que o fluxo de trabalho se torne ocioso, o fluxo de trabalho não persistirá mesmo se ele estiver configurado para persistir os fluxos de trabalho quando eles ficarem ociosos. Se uma atividade, como uma atividade Persist, tentar persistir explicitamente na zona no-Persist, uma exceção fatal será lançada, o fluxo de trabalho abortará e um <xref:System.ServiceModel.FaultException> será retornado ao chamador. A mensagem de exceção fatal é "System. InvalidOperationException: as atividades de persistência não podem estar contidas em blocos de persistência.". Essa exceção não é retornada para o chamador, mas pode ser observada se o controle estiver habilitado. A mensagem para o <xref:System.ServiceModel.FaultException> retornado para o chamador é "a operação não pôde ser executada porque WorkflowInstance ' 5836145b-7da2-49d0-A052-a49162adeab6 ' foi concluída".

 Para obter mais informações sobre a correlação de solicitação-resposta, consulte [solicitação-resposta](request-reply-correlation.md).

## <a name="common-content-correlation-issues"></a>Problemas comuns de correlação de conteúdo
 A correlação baseada em conteúdo é usada quando um serviço de fluxo de trabalho recebe várias mensagens e um dado nas mensagens trocadas identifica a instância desejada. A correlação baseada em conteúdo usa esses dados na mensagem, como um número de cliente ou ID do pedido, para rotear mensagens para a instância de fluxo de trabalho correta. Esta seção descreve vários problemas comuns que podem ocorrer ao usar a correlação baseada em conteúdo.

### <a name="ensure-the-identifying-data-is-unique"></a>Verifique se os dados de identificação são exclusivos
 Os dados usados para identificar a instância são codificados em hash em uma chave de correlação. Deve-se ter cuidado para garantir que os dados usados para correlação sejam exclusivos ou outras colisões na chave com hash possam ocorrer e fazer com que as mensagens sejam roteadas erroneamente. Por exemplo, uma correlação baseada apenas em um nome de cliente pode causar uma colisão porque pode haver vários clientes que têm o mesmo nome. Os dois-pontos (:) Não deve ser usado como parte dos dados usados para correlacionar a mensagem porque ela já está sendo usada para delimitar a chave e o valor da consulta de mensagem para formar a cadeia de caracteres que é subsequentemente com hash. Se a persistência estiver sendo usada, certifique-se de que os dados de identificação atuais não foram usados por uma instância persistente anteriormente. Desabilitar temporariamente a persistência pode ajudar a identificar esse problema. O rastreamento do WCF pode ser usado para exibir a chave de correlação calculada e é útil para depurar esse tipo de problema.

### <a name="race-conditions"></a>Condições de corrida
 Há um pequeno intervalo de tempo entre o serviço que recebe uma mensagem e a correlação realmente inicializada, durante a qual as mensagens de acompanhamento serão ignoradas. Se um serviço de fluxo de trabalho inicializar a correlação baseada em conteúdo usando dados passados do cliente em uma operação unidirecional e o chamador enviar mensagens de acompanhamento imediatos, essas mensagens serão ignoradas durante esse intervalo. Isso pode ser evitado usando uma operação bidirecional para inicializar a correlação ou usando um <xref:System.ServiceModel.Activities.TransactedReceiveScope> .

### <a name="correlation-query-issues"></a>Problemas de consulta de correlação
 As consultas de correlação são usadas para especificar quais dados em uma mensagem são usados para correlacionar a mensagem. Esses dados são especificados usando uma consulta XPath. Se as mensagens para um serviço não estiverem sendo expedidas mesmo que tudo pareça correto, uma estratégia para solucionar problemas é especificar um valor literal que corresponda ao valor dos dados da mensagem em vez de uma consulta XPath. Para especificar um valor literal, use a `string` função. No exemplo a seguir, um <xref:System.ServiceModel.MessageQuerySet> é configurado para usar um valor literal de `11445` para `OrderId` e a consulta XPath é comentada.

```csharp
MessageQuerySet = new MessageQuerySet
{
    {
        "OrderId",
        //new XPathMessageQuery("sm:body()/tempuri:StartOrderResponse/tempuri:OrderId")
        new XPathMessageQuery("string('11445')")
    }
}
```

 Se uma consulta XPath estiver configurada incorretamente, de modo que nenhum dado de correlação seja recuperado, uma falha será retornada com a seguinte mensagem: "uma consulta de correlação gerou um conjunto de resultados vazio. Verifique se as consultas de correlação para o ponto de extremidade estão configuradas corretamente. " Uma maneira rápida de solucionar isso é substituir a consulta XPath por um valor literal, conforme descrito na seção anterior. Esse problema pode ocorrer se você usar o construtor de consultas XPath nas caixas de diálogo **Adicionar inicializadores de correlação** ou **definição de CorrelatesOn** e seu serviço de fluxo de trabalho usar contratos de mensagem. No exemplo a seguir, uma classe de contrato de mensagem é definida.

```csharp
[MessageContract]
public class AddItemMessage
{
    [MessageHeader]
    public string CartId;

    [MessageBodyMember]
    public string Item;
}
```

 Este contrato de mensagem é usado por uma <xref:System.ServiceModel.Activities.Receive> atividade em um fluxo de trabalho. O `CartId` no cabeçalho da mensagem é usado para correlacionar a mensagem à instância correta. Se a consulta XPath que recupera o `CartId` for criada usando as caixas de diálogo de correlação no designer de fluxo de trabalho, a seguinte consulta XPath incorreta será gerada.

```
sm:body()/xg0:AddItemMessage/xg0:CartId
```

 Essa consulta XPath estaria correta se a <xref:System.ServiceModel.Activities.Receive> atividade usava parâmetros para os dados, mas como está usando um contrato de mensagem, ele está incorreto. A consulta XPath a seguir é a consulta XPath correta para recuperar o `CartId` do cabeçalho.

```
sm:header()/tempuri:CartId
```

Isso pode ser confirmado examinando o corpo da mensagem.

```xml
<s:Envelope xmlns:s="http://schemas.xmlsoap.org/soap/envelope/">
  <s:Header>
    <Action s:mustUnderstand="1" xmlns="http://schemas.microsoft.com/ws/2005/05/addressing/none">http://tempuri.org/IService/AddItem</Action>
    <h:CartId xmlns:h="http://tempuri.org/">80c95b41-c98d-4660-a6c1-99412206e54c</h:CartId>
  </s:Header>
  <s:Body>
    <AddItemMessage xmlns="http://tempuri.org/">
      <Item>Books</Item>
    </AddItemMessage>
  </s:Body>
</s:Envelope>
```

O exemplo a seguir mostra uma <xref:System.ServiceModel.Activities.Receive> atividade configurada para uma `AddItem` operação que usa o contrato de mensagem anterior para receber dados. A consulta XPath está configurada corretamente.

```xaml
<Receive CorrelatesWith="[CCHandle] OperationName="AddItem" ServiceContractName="p:IService">
  <Receive.CorrelatesOn>
    <XPathMessageQuery x:Key="key1">
      <XPathMessageQuery.Namespaces>
        <ssx:XPathMessageContextMarkup>
          <x:String x:Key="xg0">http://schemas.datacontract.org/2004/07/MessageContractWFService</x:String>
        </ssx:XPathMessageContextMarkup>
      </XPathMessageQuery.Namespaces>sm:header()/tempuri:CartId</XPathMessageQuery>
  </Receive.CorrelatesOn>
  <ReceiveMessageContent DeclaredMessageType="m:AddItemMessage">
    <p1:OutArgument x:TypeArguments="m:AddItemMessage">[AddItemMessage]</p1:OutArgument>
  </ReceiveMessageContent>
</Receive>
```
