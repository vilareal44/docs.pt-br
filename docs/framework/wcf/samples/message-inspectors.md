---
title: Inspetores de mensagem
description: Saiba como implementar e configurar os inspetores de mensagem de serviço e cliente WCF, que fornecem um mecanismo de validação de mensagens.
ms.date: 03/30/2017
ms.assetid: 9bd1f305-ad03-4dd7-971f-fa1014b97c9b
ms.openlocfilehash: 20abb655a58f9dce4a967ade9b51db90eed2375b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246201"
---
# <a name="message-inspectors"></a>Inspetores de mensagem
Este exemplo demonstra como implementar e configurar os inspetores de mensagem de cliente e serviço.  
  
 Um inspetor de mensagem é um objeto de extensibilidade que pode ser usado no tempo de execução do cliente do modelo de serviço e no tempo de execução de expedição programaticamente ou por meio da configuração e que pode inspecionar e alterar as mensagens depois que elas são recebidas ou antes de serem enviadas.  
  
 Este exemplo implementa um cliente básico e um mecanismo de validação de mensagens de serviço que valida as mensagens de entrada em um conjunto de documentos de esquema XML configuráveis. Observe que esse exemplo não valida mensagens para cada operação. Essa é uma simplificação intencional.  
  
## <a name="message-inspector"></a>Inspetor de Mensagens  
 Os inspetores de mensagem do cliente implementam a <xref:System.ServiceModel.Dispatcher.IClientMessageInspector> interface e os inspetores de mensagem de serviço implementam a <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> interface As implementações podem ser combinadas em uma única classe para formar um inspetor de mensagem que funciona para ambos os lados. Este exemplo implementa um inspetor de mensagem combinado. O Inspetor é construído passando um conjunto de esquemas no qual as mensagens de entrada e saída são validadas e permite que o desenvolvedor especifique se as mensagens de entrada ou saída são validadas e se o inspetor está no modo de expedição ou cliente, o que afeta o tratamento de erros, conforme discutido posteriormente neste tópico.  
  
```csharp
public class SchemaValidationMessageInspector : IClientMessageInspector, IDispatchMessageInspector  
{  
    XmlSchemaSet schemaSet;  
    bool validateRequest;  
    bool validateReply;  
    bool isClientSide;  
    [ThreadStatic]  
    bool isRequest;  
  
    public SchemaValidationMessageInspector(XmlSchemaSet schemaSet,  
         bool validateRequest, bool validateReply, bool isClientSide)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = validateReply;  
        this.validateRequest = validateRequest;  
        this.isClientSide = isClientSide;  
    }  
```  
  
 Qualquer Inspetor de mensagem de serviço (Dispatcher) deve implementar os dois <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector> métodos <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> e <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29> .  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A>é invocado pelo Dispatcher quando uma mensagem é recebida, processada pela pilha de canais e atribuída a um serviço, mas antes de ser desserializada e expedida para uma operação. Se a mensagem de entrada tiver sido criptografada, a mensagem já estará descriptografada quando atingir o Inspetor de mensagem. O método obtém a `request` mensagem passada como um parâmetro de referência, que permite que a mensagem seja inspecionada, manipulada ou substituída conforme necessário. O valor de retorno pode ser qualquer objeto e é usado como um objeto de estado de correlação que é passado para <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> quando o serviço retorna uma resposta à mensagem atual. Neste exemplo, <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> delega a inspeção (validação) da mensagem para o método privado `ValidateMessageBody` e local e não retorna nenhum objeto de estado de correlação. Esse método garante que nenhuma mensagem inválida passe para o serviço.  
  
```csharp  
object IDispatchMessageInspector.AfterReceiveRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel, System.ServiceModel.InstanceContext instanceContext)  
{  
    if (validateRequest)  
    {  
        // inspect the message. If a validation error occurs,  
        // the thrown fault exception bubbles up.  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%28System.ServiceModel.Channels.Message%40%2CSystem.Object%29>é invocado sempre que uma resposta estiver pronta para ser enviada de volta a um cliente ou no caso de mensagens unidirecionais, quando a mensagem de entrada tiver sido processada. Isso permite que as extensões sejam chamadas de forma simétrica, independentemente de MEP. Assim como acontece com <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> , a mensagem é passada como um parâmetro de referência e pode ser inspecionada, modificada ou substituída. A validação da mensagem executada neste exemplo é delegada novamente para o `ValidMessageBody` método, mas o tratamento de erros de validação é um pouco diferente nesse caso.  
  
 Se ocorrer um erro de validação no serviço, o `ValidateMessageBody` método lançará as <xref:System.ServiceModel.FaultException> exceções derivadas. No <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.AfterReceiveRequest%2A> , essas exceções podem ser colocadas na infraestrutura do modelo de serviço, onde são transformadas automaticamente em falhas de SOAP e retransmitidas para o cliente. No <xref:System.ServiceModel.Dispatcher.IDispatchMessageInspector.BeforeSendReply%2A> , <xref:System.ServiceModel.FaultException> as exceções não devem ser colocadas na infraestrutura, pois a transformação de exceções de falha geradas pelo serviço ocorre antes que o Inspetor de mensagem seja chamado. Portanto, a implementação a seguir captura a `ReplyValidationFault` exceção conhecida e substitui a mensagem de resposta por uma mensagem de falha explícita. Esse método garante que nenhuma mensagem inválida seja retornada pela implementação do serviço.  
  
```csharp  
void IDispatchMessageInspector.BeforeSendReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        // Inspect the reply, catch a possible validation error
        try  
        {  
            ValidateMessageBody(ref reply, false);  
        }  
        catch (ReplyValidationFault fault)  
        {  
            // if a validation error occurred, the message is replaced  
            // with the validation fault.  
            reply = Message.CreateMessage(reply.Version,
                    fault.CreateMessageFault(), reply.Headers.Action);  
        }  
    }  
```  
  
 O Inspetor de mensagem do cliente é muito semelhante. Os dois métodos que devem ser implementados do <xref:System.ServiceModel.Dispatcher.IClientMessageInspector> são <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.AfterReceiveReply%2A> e <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> .  
  
 <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A>é invocado quando a mensagem é composta pelo aplicativo cliente ou pelo formatador de operação. Assim como os inspetores de mensagem do Dispatcher, a mensagem pode ser apenas inspecionada ou totalmente substituída. Neste exemplo, o Inspetor delega para o mesmo `ValidateMessageBody` método auxiliar local que também é usado para os inspetores de mensagem de expedição.  
  
 A diferença comportamental entre a validação do cliente e do serviço (conforme especificado no Construtor) é que a validação do cliente gera exceções locais que são colocadas no código do usuário porque elas ocorrem localmente e não devido a uma falha de serviço. Em geral, a regra é que os inspetores de distribuidor de serviço lançam falhas e os inspetores de cliente geram exceções.  
  
 Essa <xref:System.ServiceModel.Dispatcher.IClientMessageInspector.BeforeSendRequest%2A> implementação garante que nenhuma mensagem inválida seja enviada ao serviço.  
  
```csharp  
object IClientMessageInspector.BeforeSendRequest(ref System.ServiceModel.Channels.Message request, System.ServiceModel.IClientChannel channel)  
{  
    if (validateRequest)  
    {  
        ValidateMessageBody(ref request, true);  
    }  
    return null;  
}  
```  
  
 A `AfterReceiveReply` implementação garante que nenhuma mensagem inválida recebida do serviço seja retransmitida para o código de usuário do cliente.  
  
```csharp  
void IClientMessageInspector.AfterReceiveReply(ref System.ServiceModel.Channels.Message reply, object correlationState)  
{  
    if (validateReply)  
    {  
        ValidateMessageBody(ref reply, false);  
    }  
}  
```  
  
 O coração desse Inspetor de mensagem específico é o `ValidateMessageBody` método. Para executar seu trabalho, ele encapsula uma validação <xref:System.Xml.XmlReader> em volta da subárvore de conteúdo do corpo da mensagem passada. O leitor é populado com o conjunto de esquemas que o Inspetor de mensagem contém e o retorno de chamada de validação é definido como um delegado referente ao `InspectionValidationHandler` que é definido junto com esse método. Para executar a validação, a mensagem é então lida e colocada em spool em um fluxo de memória com suporte <xref:System.Xml.XmlDictionaryWriter> . Se ocorrer um erro ou aviso de validação no processo, o método de retorno de chamada será invocado.  
  
 Se nenhum erro ocorrer, uma nova mensagem será construída que copiará as propriedades e os cabeçalhos da mensagem original e usará o infoset validado agora no fluxo de memória, que é encapsulado por um <xref:System.Xml.XmlDictionaryReader> e adicionado à mensagem de substituição.  
  
```csharp  
void ValidateMessageBody(ref System.ServiceModel.Channels.Message message, bool isRequest)  
{  
    if (!message.IsFault)  
    {  
        XmlDictionaryReaderQuotas quotas =
                new XmlDictionaryReaderQuotas();  
        XmlReader bodyReader =
            message.GetReaderAtBodyContents().ReadSubtree();  
        XmlReaderSettings wrapperSettings =
                              new XmlReaderSettings();  
        wrapperSettings.CloseInput = true;  
        wrapperSettings.Schemas = schemaSet;  
        wrapperSettings.ValidationFlags =
                                XmlSchemaValidationFlags.None;  
        wrapperSettings.ValidationType = ValidationType.Schema;  
        wrapperSettings.ValidationEventHandler += new
           ValidationEventHandler(InspectionValidationHandler);  
        XmlReader wrappedReader = XmlReader.Create(bodyReader,
                                            wrapperSettings);  
  
        // pull body into a memory backed writer to validate  
        this.isRequest = isRequest;  
        MemoryStream memStream = new MemoryStream();  
        XmlDictionaryWriter xdw =  
              XmlDictionaryWriter.CreateBinaryWriter(memStream);  
        xdw.WriteNode(wrappedReader, false);  
        xdw.Flush(); memStream.Position = 0;  
        XmlDictionaryReader xdr =
        XmlDictionaryReader.CreateBinaryReader(memStream, quotas);  
  
        // reconstruct the message with the validated body  
        Message replacedMessage =
            Message.CreateMessage(message.Version, null, xdr);  
        replacedMessage.Headers.CopyHeadersFrom(message.Headers);  
        replacedMessage.Properties.CopyProperties(message.Properties);  
        message = replacedMessage;  
    }  
}  
```  
  
 O `InspectionValidationHandler` método é chamado pela validação <xref:System.Xml.XmlReader> sempre que ocorre um erro ou aviso de validação de esquema. A implementação a seguir funciona apenas com erros e ignora todos os avisos.  
  
 Na primeira consideração, pode parecer possível injetar uma validação <xref:System.Xml.XmlReader> na mensagem com o Inspetor de mensagem e permitir que a validação aconteça enquanto a mensagem é processada e sem armazenar a mensagem em buffer. No entanto, isso significa que esse retorno de chamada gera as exceções de validação em algum lugar na infraestrutura do modelo de serviço ou o código do usuário como nós XML inválidos são detectados, resultando em um comportamento imprevisível. A abordagem de buffer protege o código do usuário de mensagens inválidas, inteiramente.  
  
 Conforme discutido anteriormente, as exceções geradas pelo manipulador diferem entre o cliente e o serviço. No serviço, as exceções são derivadas de <xref:System.ServiceModel.FaultException> , no cliente, as exceções são exceções personalizadas regulares.  
  
```csharp  
        void InspectionValidationHandler(object sender, ValidationEventArgs e)  
{  
    if (e.Severity == XmlSeverityType.Error)  
    {  
        // We are treating client and service side validation errors  
        // differently here. Client side errors cause exceptions  
        // and are thrown straight up to the user code. Service side  
        // validations cause faults.  
        if (isClientSide)  
        {  
            if (isRequest)  
            {  
                throw new RequestClientValidationException(e.Message);  
            }  
            else  
            {  
                throw new ReplyClientValidationException(e.Message);  
            }  
        }  
        else  
        {  
            if (isRequest)  
            {  
                // this fault is caught by the ServiceModel
                // infrastructure and turned into a fault reply.  
                throw new RequestValidationFault(e.Message);  
             }  
             else  
             {  
                // this fault is caught and turned into a fault message  
                // in BeforeSendReply in this class  
                throw new ReplyValidationFault(e.Message);  
              }  
          }  
      }  
    }  
```  
  
## <a name="behavior"></a>Comportamento  
 Os inspetores de mensagem são extensões para o tempo de execução do cliente ou de expedição. Essas extensões são configuradas usando *comportamentos*. Um comportamento é uma classe que altera o comportamento do tempo de execução do modelo de serviço, alterando a configuração padrão ou adicionando extensões (como inspetores de mensagem) a ela.  
  
 A classe a seguir `SchemaValidationBehavior` é o comportamento usado para adicionar o Inspetor de mensagem do exemplo ao cliente ou ao tempo de execução de expedição. A implementação é, em ambos os casos, básica. No <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyClientBehavior%2A> e no <xref:System.ServiceModel.Description.IEndpointBehavior.ApplyDispatchBehavior%2A> , o Inspetor de mensagem é criado e adicionado à <xref:System.ServiceModel.Dispatcher.ClientRuntime.MessageInspectors%2A> coleção do respectivo tempo de execução.  
  
```csharp
public class SchemaValidationBehavior : IEndpointBehavior  
{  
    XmlSchemaSet schemaSet;
    bool validateRequest;
    bool validateReply;  
  
    public SchemaValidationBehavior(XmlSchemaSet schemaSet, bool
                           inspectRequest, bool inspectReply)  
    {  
        this.schemaSet = schemaSet;  
        this.validateReply = inspectReply;  
        this.validateRequest = inspectRequest;  
    }  
    #region IEndpointBehavior Members  
  
    public void AddBindingParameters(ServiceEndpoint endpoint,
       System.ServiceModel.Channels.BindingParameterCollection
                                            bindingParameters)  
    {  
    }  
  
    public void ApplyClientBehavior(ServiceEndpoint endpoint,
            System.ServiceModel.Dispatcher.ClientRuntime clientRuntime)  
    {  
        SchemaValidationMessageInspector inspector =
           new SchemaValidationMessageInspector(schemaSet,
                      validateRequest, validateReply, true);  
            clientRuntime.MessageInspectors.Add(inspector);  
    }  
  
    public void ApplyDispatchBehavior(ServiceEndpoint endpoint,
         System.ServiceModel.Dispatcher.EndpointDispatcher
                                          endpointDispatcher)  
    {  
        SchemaValidationMessageInspector inspector =
           new SchemaValidationMessageInspector(schemaSet,
                        validateRequest, validateReply, false);  
   endpointDispatcher.DispatchRuntime.MessageInspectors.Add(inspector);  
    }  
  
   public void Validate(ServiceEndpoint endpoint)  
   {  
   }  
  
    #endregion  
}  
```  
  
> [!NOTE]
> Esse comportamento específico não dobra como um atributo e, portanto, não pode ser adicionado declarativamente a um tipo de contrato de um tipo de serviço. Essa é uma decisão por design feita porque a coleção de esquema não pode ser carregada em uma declaração de atributo e se referir a um local de configuração extra (por exemplo, às configurações do aplicativo) nesse atributo significa criar um elemento de configuração que não seja consistente com o restante da configuração do modelo de serviço. Portanto, esse comportamento só pode ser adicionado imperativamente por meio de código e por meio de uma extensão de configuração de modelo de serviço.  
  
## <a name="adding-the-message-inspector-through-configuration"></a>Adicionando o Inspetor de mensagem por meio da configuração  
 Para configurar um comportamento personalizado em um ponto de extremidade no arquivo de configuração do aplicativo, o modelo de serviço requer implementadores para criar um *elemento de extensão* de configuração representado por uma classe derivada de <xref:System.ServiceModel.Configuration.BehaviorExtensionElement> . Essa extensão deve ser adicionada à seção de configuração do modelo de serviço para extensões, conforme mostrado para a seguinte extensão discutida nesta seção.  
  
```xml  
<system.serviceModel>  
…  
   <extensions>  
      <behaviorExtensions>  
        <add name="schemaValidator" type="Microsoft.ServiceModel.Samples.SchemaValidationBehaviorExtensionElement, MessageInspectors, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>  
      </behaviorExtensions>  
    </extensions>  
…  
</system.serviceModel>  
```  
  
 As extensões podem ser adicionadas no arquivo de configuração Application ou ASP.NET, que é a opção mais comum ou no arquivo de configuração do computador.  
  
 Quando a extensão é adicionada a um escopo de configuração, o comportamento pode ser adicionado a uma configuração de comportamento, como mostrado no código a seguir. As configurações de comportamento são elementos reutilizáveis que podem ser aplicados a vários pontos de extremidade, conforme necessário. Como o comportamento específico a ser configurado aqui implementa <xref:System.ServiceModel.Description.IEndpointBehavior> , ele só é válido na seção de configuração respectiva no arquivo de configuração.  
  
```xml  
<system.serviceModel>  
   <behaviors>  
      …  
     <endpointBehaviors>  
        <behavior name="HelloServiceEndpointBehavior">  
          <schemaValidator validateRequest="True" validateReply="True">  
            <schemas>  
              <add location="messages.xsd" />
            </schemas>  
          </schemaValidator>  
        </behavior>  
      </endpointBehaviors>  
      …  
    </behaviors>  
</system.serviceModel>  
```  
  
 O `<schemaValidator>` elemento que configura o Inspetor de mensagem é apoiado pela `SchemaValidationBehaviorExtensionElement` classe. A classe expõe duas propriedades públicas booleanas chamadas `ValidateRequest` e `ValidateReply` . Ambos são marcados com um <xref:System.Configuration.ConfigurationPropertyAttribute> . Esse atributo constitui o link entre as propriedades de código e os atributos XML que podem ser vistos no elemento de configuração XML anterior. A classe também tem uma propriedade `Schemas` marcada com o <xref:System.Configuration.ConfigurationCollectionAttribute> e é do tipo `SchemaCollection` , que também faz parte desse exemplo, mas que é omitido deste documento para fins de brevidade. Essa propriedade junto com a coleção e a classe de elemento de coleção `SchemaConfigElement` faz backup do `<schemas>` elemento no trecho de configuração anterior e permite adicionar uma coleção de esquemas ao conjunto de validação.  
  
 O `CreateBehavior` método substituído transforma os dados de configuração em um objeto de comportamento quando o tempo de execução avalia os dados de configuração à medida que cria um cliente ou um ponto de extremidade.  
  
```csharp
public class SchemaValidationBehaviorExtensionElement : BehaviorExtensionElement  
{  
    public SchemaValidationBehaviorExtensionElement()  
    {  
    }  
  
    public override Type BehaviorType
    {
        get  
        {  
            return typeof(SchemaValidationBehavior);  
        }
    }  
  
    protected override object CreateBehavior()  
    {  
        XmlSchemaSet schemaSet = new XmlSchemaSet();  
        foreach (SchemaConfigElement schemaCfg in this.Schemas)  
        {  
            Uri baseSchema = new
                Uri(AppDomain.CurrentDomain.BaseDirectory);  
            string location = new
                Uri(baseSchema,schemaCfg.Location).ToString();  
            XmlSchema schema =
                XmlSchema.Read(new XmlTextReader(location), null);  
            schemaSet.Add(schema);  
        }  
     return new
     SchemaValidationBehavior(schemaSet,ValidateRequest,ValidateReply);  
    }  
  
[ConfigurationProperty("validateRequest",DefaultValue=false,IsRequired=false)]  
public bool ValidateRequest  
{  
    get { return (bool)base["validateRequest"]; }  
    set { base["validateRequest"] = value; }  
}  
  
[ConfigurationProperty("validateReply", DefaultValue = false, IsRequired = false)]  
        public bool ValidateReply  
        {  
            get { return (bool)base["validateReply"]; }  
            set { base["validateReply"] = value; }  
        }  
  
     //Declare the Schema collection property.  
     //Note: the "IsDefaultCollection = false" instructs
     //.NET Framework to build a nested section of
     //the kind <Schema> ...</Schema>.  
    [ConfigurationProperty("schemas", IsDefaultCollection = true)]  
    [ConfigurationCollection(typeof(SchemasCollection),  
        AddItemName = "add",  
        ClearItemsName = "clear",  
        RemoveItemName = "remove")]  
    public SchemasCollection Schemas  
    {  
        get  
        {  
            SchemasCollection SchemasCollection =  
            (SchemasCollection)base["schemas"];  
            return SchemasCollection;  
        }  
    }  
}  
```  
  
## <a name="adding-message-inspectors-imperatively"></a>Adição imperativa de inspetores de mensagem  
 Exceto por meio de atributos (que não tem suporte neste exemplo para o motivo citado anteriormente) e configuração, os comportamentos podem ser facilmente adicionados a um tempo de execução de cliente e serviço usando código imperativo. Neste exemplo, isso é feito no aplicativo cliente para testar o Inspetor de mensagem do cliente. A `GenericClient` classe é derivada de <xref:System.ServiceModel.ClientBase%601> , que expõe a configuração do ponto de extremidade ao código do usuário. Antes que o cliente seja aberto implicitamente, a configuração do ponto de extremidade pode ser alterada, por exemplo, adicionando comportamentos, conforme mostrado no código a seguir. A adição do comportamento no serviço é amplamente equivalente à técnica do cliente mostrada aqui e deve ser executada antes da abertura do host de serviço.  
  
```csharp  
try  
{  
    Console.WriteLine("*** Call 'Hello' with generic client, with client behavior");  
    GenericClient client = new GenericClient();  
  
    // Configure client programmatically, adding behavior  
    XmlSchema schema = XmlSchema.Read(new StreamReader("messages.xsd"),
                                                          null);  
    XmlSchemaSet schemaSet = new XmlSchemaSet();  
    schemaSet.Add(schema);  
    client.Endpoint.Behaviors.Add(new
                SchemaValidationBehavior(schemaSet, true, true));  
  
    Console.WriteLine("--- Sending valid client request:");  
    GenericCallValid(client, helloAction);  
    Console.WriteLine("--- Sending invalid client request:");  
    GenericCallInvalid(client, helloAction);  
  
    client.Close();  
}  
catch (Exception e)  
{  
    DumpException(e);  
}  
```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\MessageInspectors`  
