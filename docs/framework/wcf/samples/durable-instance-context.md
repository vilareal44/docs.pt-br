---
title: Contexto de instância durável
ms.date: 03/30/2017
ms.assetid: 97bc2994-5a2c-47c7-927a-c4cd273153df
ms.openlocfilehash: 567ca62d48e80993328548b11f8b59c4fcd355fe
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600588"
---
# <a name="durable-instance-context"></a>Contexto de instância durável

Este exemplo demonstra como personalizar o tempo de execução de Windows Communication Foundation (WCF) para habilitar contextos de instância durável. Ele usa SQL Server 2005 como armazenamento de backup (SQL Server 2005 Express nesse caso). No entanto, ele também fornece uma maneira de acessar mecanismos de armazenamento personalizados.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Este exemplo envolve a extensão da camada de canal e da camada do modelo de serviço do WCF. Portanto, é necessário entender os conceitos subjacentes antes de entrar nos detalhes da implementação.

Contextos de instância durável podem ser encontrados nos cenários do mundo real com muita frequência. Um aplicativo de carrinho de compras, por exemplo, tem a capacidade de pausar a compra na metade e continuar em outro dia. Então, quando visitamos o carrinho de compras no dia seguinte, nosso contexto original é restaurado. É importante observar que o aplicativo de carrinho de compras (no servidor) não mantém a instância do carrinho de compras enquanto estamos desconectados. Em vez disso, ele persiste seu estado em uma mídia de armazenamento durável e a usa ao construir uma nova instância para o contexto restaurado. Portanto, a instância de serviço que pode ser atendida para o mesmo contexto não é igual à instância anterior (ou seja, não tem o mesmo endereço de memória).

O contexto de instância durável torna-se possível por um protocolo pequeno que troca uma ID de contexto entre o cliente e o serviço. Essa ID de contexto é criada no cliente e transmitida para o serviço. Quando a instância do serviço é criada, o tempo de execução do serviço tenta carregar o estado persistente que corresponde a essa ID de contexto de um armazenamento persistente (por padrão, é um banco de dados SQL Server 2005). Se nenhum estado estiver disponível, a nova instância terá seu estado padrão. A implementação do serviço usa um atributo personalizado para marcar operações que alteram o estado da implementação do serviço para que o tempo de execução possa salvar a instância do serviço depois de chamá-las.

Pela descrição anterior, duas etapas podem ser facilmente diferenciadas para atingir o objetivo:

1. Altere a mensagem que passa pela conexão para transportar a ID de contexto.

2. Altere o comportamento local do serviço para implementar a lógica de instanciação personalizada.

Como a primeira na lista afeta as mensagens na conexão, ela deve ser implementada como um canal personalizado e ser conectada à camada de canal. O último afeta apenas o comportamento local do serviço e, portanto, pode ser implementado estendendo vários pontos de extensibilidade do serviço. Nas próximas seções, cada uma dessas extensões é discutida.

## <a name="durable-instancecontext-channel"></a>Canal de InstanceContext durável

A primeira coisa a ser examinada é uma extensão de camada de canal. A primeira etapa na gravação de um canal personalizado é decidir a estrutura de comunicação do canal. Como um novo protocolo de conexão está sendo introduzido, o canal deve funcionar com quase qualquer outro canal na pilha de canais. Portanto, ele deve dar suporte a todos os padrões de troca de mensagens. No entanto, a funcionalidade principal do canal é a mesma, independentemente de sua estrutura de comunicação. Mais especificamente, do cliente, ele deve gravar a ID de contexto para as mensagens e do serviço ele deve ler essa ID de contexto das mensagens e passá-la para os níveis superiores. Por isso, `DurableInstanceContextChannelBase` é criada uma classe que atua como a classe base abstrata para todas as implementações de canal de contexto de instância durável. Essa classe contém as funções de gerenciamento de máquina de Estado comum e dois membros protegidos para aplicar e ler as informações de contexto de e para mensagens.

```csharp
class DurableInstanceContextChannelBase
{
  //…
  protected void ApplyContext(Message message)
  {
    //…
  }
  protected string ReadContextId(Message message)
  {
    //…
  }
}
```

Esses dois métodos fazem uso de `IContextManager` implementações para gravar e ler a ID de contexto de ou para a mensagem. ( `IContextManager` é uma interface personalizada usada para definir o contrato para todos os gerenciadores de contexto.) O canal pode incluir a ID de contexto em um cabeçalho SOAP personalizado ou em um cabeçalho de cookie HTTP. Cada implementação do Gerenciador de contexto herda da `ContextManagerBase` classe que contém a funcionalidade comum para todos os gerenciadores de contexto. O `GetContextId` método nessa classe é usado para originar a ID de contexto do cliente. Quando uma ID de contexto é originada pela primeira vez, esse método a salva em um arquivo de texto cujo nome é construído pelo endereço do ponto de extremidade remoto (os caracteres de nome de arquivo inválidos nos URIs típicos são substituídos por @ caracteres).

Posteriormente, quando a ID de contexto for necessária para o mesmo ponto de extremidade remoto, ela verificará se um arquivo apropriado existe. Se tiver, ele lerá a ID de contexto e retornará. Caso contrário, ele retorna uma ID de contexto gerada recentemente e salva-o em um arquivo. Com a configuração padrão, esses arquivos são colocados em um diretório chamado ContextStore, que reside no diretório Temp do usuário atual. No entanto, esse local é configurável usando o elemento Binding.

O mecanismo usado para transportar a ID de contexto é configurável. Ele pode ser gravado no cabeçalho do cookie HTTP ou em um cabeçalho SOAP personalizado. A abordagem de cabeçalho SOAP personalizada possibilita o uso desse protocolo com protocolos não HTTP (por exemplo, TCP ou pipes nomeados). Há duas classes, ou seja `MessageHeaderContextManager` `HttpCookieContextManager` , que implementam essas duas opções.

Ambos gravam a ID de contexto na mensagem adequadamente. Por exemplo, a `MessageHeaderContextManager` classe a grava em um cabeçalho SOAP no `WriteContext` método.

```csharp
public override void WriteContext(Message message)
{
  string contextId = this.GetContextId();

  MessageHeader contextHeader =
    MessageHeader.CreateHeader(DurableInstanceContextUtility.HeaderName,
      DurableInstanceContextUtility.HeaderNamespace,
      contextId,
      true);

  message.Headers.Add(contextHeader);
}
```

Os `ApplyContext` métodos e `ReadContextId` na `DurableInstanceContextChannelBase` classe invocam o `IContextManager.ReadContext` e `IContextManager.WriteContext` , respectivamente. No entanto, esses gerenciadores de contexto não são criados diretamente pela `DurableInstanceContextChannelBase` classe. Em vez disso, ele usa a `ContextManagerFactory` classe para fazer esse trabalho.

```csharp
IContextManager contextManager =
                ContextManagerFactory.CreateContextManager(contextType,
                this.contextStoreLocation,
                this.endpointAddress);
```

O `ApplyContext` método é invocado pelos canais de envio. Ele injeta a ID de contexto para as mensagens de saída. O `ReadContextId` método é invocado pelos canais de recebimento. Esse método garante que a ID de contexto esteja disponível nas mensagens de entrada e a adicione à `Properties` coleção da `Message` classe. Ele também gera um `CommunicationException` no caso de uma falha ao ler a ID do contexto e, portanto, faz com que o canal seja anulado.

```csharp
message.Properties.Add(DurableInstanceContextUtility.ContextIdProperty, contextId);
```

Antes de continuar, é importante entender o uso da `Properties` coleção na `Message` classe. Normalmente, essa `Properties` coleção é usada ao passar dados de baixo para os níveis superiores da camada de canal. Dessa forma, os dados desejados podem ser fornecidos para os níveis superiores de forma consistente, independentemente dos detalhes do protocolo. Em outras palavras, a camada de canal pode enviar e receber a ID de contexto como um cabeçalho SOAP ou um cabeçalho de cookie HTTP. Mas não é necessário que os níveis superiores saibam sobre esses detalhes porque a camada de canal torna essas informações disponíveis na `Properties` coleção.

Agora, com a `DurableInstanceContextChannelBase` classe em vigor, todas as dez interfaces necessárias (IOutputChannel, IInputChannel, IOutputSessionChannel, IInputSessionChannel, IRequestChannel, IReplyChannel, IRequestSessionChannel, IReplySessionChannel, IDuplexChannel, IDuplexSessionChannel) devem ser implementadas. Eles se assemelham a todos os padrões de troca de mensagens disponíveis (datagrama, simplex, duplex e suas variantes de sessão). Cada uma dessas implementações herda a classe base descrita anteriormente e chama `ApplyContext` e `ReadContextId` adequadamente. Por exemplo, `DurableInstanceContextOutputChannel` -que implementa a interface IOutputChannel – chama o `ApplyContext` método de cada método que envia as mensagens.

```csharp
public void Send(Message message, TimeSpan timeout)
{
    // Apply the context information before sending the message.
    this.ApplyContext(message);
    //…
}
```

Por outro lado, `DurableInstanceContextInputChannel` -que implementa a `IInputChannel` interface – chama o `ReadContextId` método em cada método, que recebe as mensagens.

```csharp
public Message Receive(TimeSpan timeout)
{
    //…
      ReadContextId(message);
      return message;
}
```

Além disso, essas implementações de canal delegam as invocações de método ao canal abaixo delas na pilha de canais. No entanto, as variantes de sessão têm uma lógica básica para garantir que a ID de contexto seja enviada e seja lida somente para a primeira mensagem que faz com que a sessão seja criada.

```csharp
if (isFirstMessage)
{
//…
    this.ApplyContext(message);
    isFirstMessage = false;
}
```

Essas implementações de canal são então adicionadas ao tempo de execução do canal do WCF pela `DurableInstanceContextBindingElement` classe e `DurableInstanceContextBindingElementSection` classe adequadamente. Consulte a documentação de exemplo do canal [HttpCookieSession](httpcookiesession.md) para obter mais detalhes sobre elementos de associação e seções de elemento de associação.

## <a name="service-model-layer-extensions"></a>Extensões de camada de modelo de serviço

Agora que a ID de contexto percorreu pela camada de canal, o comportamento do serviço pode ser implementado para personalizar a instanciação. Neste exemplo, um Gerenciador de armazenamento é usado para carregar e salvar o estado de ou para o repositório persistente. Conforme explicado anteriormente, este exemplo fornece um Gerenciador de armazenamento que usa SQL Server 2005 como seu armazenamento de backup. No entanto, também é possível adicionar mecanismos de armazenamento personalizados a essa extensão. Para fazer isso, uma interface pública é declarada, que deve ser implementada por todos os gerenciadores de armazenamento.

```csharp
public interface IStorageManager
{
    object GetInstance(string contextId, Type type);
    void SaveInstance(string contextId, object state);
}
```

A `SqlServerStorageManager` classe contém a `IStorageManager` implementação padrão. Em seu `SaveInstance` método, o objeto fornecido é serializado usando o XmlSerializer e é salvo no banco de dados SQL Server.

```csharp
XmlSerializer serializer = new XmlSerializer(state.GetType());
string data;

using (StringWriter writer = new StringWriter(CultureInfo.InvariantCulture))
{
    serializer.Serialize(writer, state);
    data = writer.ToString();
}

using (SqlConnection connection = new SqlConnection(GetConnectionString()))
{
    connection.Open();

    string update = @"UPDATE Instances SET Instance = @instance WHERE ContextId = @contextId";

    using (SqlCommand command = new SqlCommand(update, connection))
    {
        command.Parameters.Add("@instance", SqlDbType.VarChar, 2147483647).Value = data;
        command.Parameters.Add("@contextId", SqlDbType.VarChar, 256).Value = contextId;

        int rows = command.ExecuteNonQuery();

        if (rows == 0)
        {
            string insert = @"INSERT INTO Instances(ContextId, Instance) VALUES(@contextId, @instance)";
            command.CommandText = insert;
            command.ExecuteNonQuery();
        }
    }
}
```

No `GetInstance` método, os dados serializados são lidos para uma determinada ID de contexto e o objeto construído a partir dele é retornado ao chamador.

```csharp
object data;
using (SqlConnection connection = new SqlConnection(GetConnectionString()))
{
    connection.Open();

    string select = "SELECT Instance FROM Instances WHERE ContextId = @contextId";
    using (SqlCommand command = new SqlCommand(select, connection))
    {
        command.Parameters.Add("@contextId", SqlDbType.VarChar, 256).Value = contextId;
        data = command.ExecuteScalar();
    }
}

if (data != null)
{
    XmlSerializer serializer = new XmlSerializer(type);
    using (StringReader reader = new StringReader((string)data))
    {
        object instance = serializer.Deserialize(reader);
        return instance;
    }
}
```

Os usuários desses gerenciadores de armazenamento não devem instanciá-los diretamente. Eles usam a `StorageManagerFactory` classe, que é abstraida dos detalhes de criação do Gerenciador de armazenamento. Essa classe tem um membro estático, `GetStorageManager` , que cria uma instância de um determinado tipo de Gerenciador de armazenamento. Se o parâmetro de tipo for `null` , esse método criará uma instância da `SqlServerStorageManager` classe padrão e a retornará. Ele também valida o tipo fornecido para certificar-se de que ele implementa a `IStorageManager` interface.

```csharp
public static IStorageManager GetStorageManager(Type storageManagerType)
{
IStorageManager storageManager = null;

if (storageManagerType == null)
{
    return new SqlServerStorageManager();
}
else
{
    object obj = Activator.CreateInstance(storageManagerType);

    // Throw if the specified storage manager type does not
    // implement IStorageManager.
    if (obj is IStorageManager)
    {
        storageManager = (IStorageManager)obj;
    }
    else
    {
        throw new InvalidOperationException(
                  ResourceHelper.GetString("ExInvalidStorageManager"));
    }

    return storageManager;
}
}
```

A infraestrutura necessária para ler e gravar instâncias do armazenamento persistente é implementada. Agora, as etapas necessárias para alterar o comportamento do serviço devem ser realizadas.

Como a primeira etapa desse processo, precisamos salvar a ID de contexto, que veio pela camada de canal para o InstanceContext atual. InstanceContext é um componente de tempo de execução que atua como o link entre o Dispatcher do WCF e a instância do serviço. Ele pode ser usado para fornecer estado adicional e comportamento à instância do serviço. Isso é essencial porque, na comunicação de sessão, a ID de contexto é enviada somente com a primeira mensagem.

O WCF permite estender seu componente de tempo de execução de InstanceContext adicionando um novo estado e comportamento usando seu padrão de objeto extensível. O padrão de objeto extensível é usado no WCF para estender as classes de tempo de execução existentes com a nova funcionalidade ou para adicionar novos recursos de estado a um objeto. Há três interfaces no padrão de objeto extensível-IExtensibleObject \<T> , IExtension e \<T> IExtensionCollection \<T> :

- A \<T> interface IExtensibleObject é implementada por objetos que permitem extensões que personalizam sua funcionalidade.

- A \<T> interface IExtension é implementada por objetos que são extensões de classes do tipo T.

- A \<T> interface IExtensionCollection é uma coleção de IExtensions que permite recuperar IExtensions por seu tipo.

Portanto, uma classe InstanceContextExtension deve ser criada para implementar a interface IExtension e definir o estado necessário para salvar a ID de contexto. Essa classe também fornece o estado para manter o Gerenciador de armazenamento em uso. Depois que o novo estado for salvo, não será possível modificá-lo. Portanto, o estado é fornecido e salvo na instância no momento em que está sendo construído e só pode ser acessado usando propriedades somente leitura.

```csharp
// Constructor
public DurableInstanceContextExtension(string contextId,
            IStorageManager storageManager)
{
    this.contextId = contextId;
    this.storageManager = storageManager;
}

// Read only properties
public string ContextId
{
    get { return this.contextId; }
}

public IStorageManager StorageManager
{
    get { return this.storageManager; }
}
```

A classe InstanceContextInitializer implementa a interface IInstanceContextInitializer e adiciona a extensão de contexto de instância à coleção Extensions do InstanceContext que está sendo construído.

```csharp
public void Initialize(InstanceContext instanceContext, Message message)
{
    string contextId =
  (string)message.Properties[DurableInstanceContextUtility.ContextIdProperty];

    DurableInstanceContextExtension extension =
                new DurableInstanceContextExtension(contextId,
                     storageManager);
    instanceContext.Extensions.Add(extension);
}
```

Conforme descrito anteriormente, a ID de contexto é lida da `Properties` coleção da `Message` classe e passada para o construtor da classe de extensão. Isso demonstra como as informações podem ser trocadas entre as camadas de maneira consistente.

A próxima etapa importante é substituir o processo de criação da instância de serviço. O WCF permite implementar comportamentos de instanciação personalizados e conectá-los ao tempo de execução usando a interface IInstanceProvider. A nova `InstanceProvider` classe é implementada para fazer esse trabalho. O tipo de serviço esperado do provedor de instância é aceito no construtor. Posteriormente, isso é usado para criar novas instâncias. Na `GetInstance` implementação, uma instância de um Gerenciador de armazenamento é criada procurando por uma instância persistente. Se retornar `null` , uma nova instância do tipo de serviço será instanciada e retornada ao chamador.

```csharp
public object GetInstance(InstanceContext instanceContext, Message message)
{
    object instance = null;

    DurableInstanceContextExtension extension =
    instanceContext.Extensions.Find<DurableInstanceContextExtension>();

    string contextId = extension.ContextId;
    IStorageManager storageManager = extension.StorageManager;

    instance = storageManager.GetInstance(contextId, serviceType);

    instance ??= Activator.CreateInstance(serviceType);
    return instance;
}
```

A próxima etapa importante é instalar as `InstanceContextExtension` classes, `InstanceContextInitializer` e `InstanceProvider` no tempo de execução do modelo de serviço. Um atributo personalizado pode ser usado para marcar as classes de implementação de serviço para instalar o comportamento. O `DurableInstanceContextAttribute` contém a implementação para esse atributo e implementa a `IServiceBehavior` interface para estender o tempo de execução do serviço inteiro.

Essa classe tem uma propriedade que aceita o tipo do Gerenciador de armazenamento a ser usado. Dessa forma, a implementação permite que os usuários especifiquem sua própria `IStorageManager` implementação como parâmetro deste atributo.

Na `ApplyDispatchBehavior` implementação, o `InstanceContextMode` do `ServiceBehavior` atributo atual está sendo verificado. Se essa propriedade for definida como singleton, a habilitação da instanciação durável não será possível e uma `InvalidOperationException` será gerada para notificar o host.

```csharp
ServiceBehaviorAttribute serviceBehavior =
    serviceDescription.Behaviors.Find<ServiceBehaviorAttribute>();

if (serviceBehavior != null &&
     serviceBehavior.InstanceContextMode == InstanceContextMode.Single)
{
    throw new InvalidOperationException(
       ResourceHelper.GetString("ExSingletonInstancingNotSupported"));
}
```

Depois disso, as instâncias do Gerenciador de armazenamento, o inicializador de contexto de instância e o provedor de instância são criados e instalados no `DispatchRuntime` ponto de extremidade criado para cada.

```csharp
IStorageManager storageManager =
    StorageManagerFactory.GetStorageManager(storageManagerType);

InstanceContextInitializer contextInitializer =
    new InstanceContextInitializer(storageManager);

InstanceProvider instanceProvider =
    new InstanceProvider(description.ServiceType);

foreach (ChannelDispatcherBase cdb in serviceHostBase.ChannelDispatchers)
{
    ChannelDispatcher cd = cdb as ChannelDispatcher;

    if (cd != null)
    {
        foreach (EndpointDispatcher ed in cd.Endpoints)
        {
            ed.DispatchRuntime.InstanceContextInitializers.Add(contextInitializer);
            ed.DispatchRuntime.InstanceProvider = instanceProvider;
        }
    }
}
```

No Resumo até agora, este exemplo produziu um canal que habilitou o protocolo de conexão personalizada para a troca de ID de contexto personalizado e também substitui o comportamento de instanciação padrão para carregar as instâncias do armazenamento persistente.

O que resta é uma maneira de salvar a instância do serviço no armazenamento persistente. Conforme discutido anteriormente, já existe a funcionalidade necessária para salvar o estado em uma `IStorageManager` implementação. Agora, devemos integrar isso ao tempo de execução do WCF. É necessário outro atributo que seja aplicável aos métodos na classe de implementação de serviço. Esse atributo deve ser aplicado aos métodos que alteram o estado da instância do serviço.

A `SaveStateAttribute` classe implementa essa funcionalidade. Ele também implementa a `IOperationBehavior` classe para modificar o tempo de execução do WCF para cada operação. Quando um método é marcado com esse atributo, o tempo de execução do WCF invoca o `ApplyBehavior` método enquanto o apropriado `DispatchOperation` está sendo construído. Há uma única linha de código nesta implementação de método:

```csharp
dispatch.Invoker = new OperationInvoker(dispatch.Invoker);
```

Essa instrução cria uma instância do `OperationInvoker` tipo e a atribui à `Invoker` propriedade de `DispatchOperation` que está sendo construída. A `OperationInvoker` classe é um wrapper para o chamador de operação padrão criado para o `DispatchOperation` . Essa classe implementa a interface `IOperationInvoker`. Na `Invoke` implementação do método, a invocação do método real é delegada para o chamador da operação interna. No entanto, antes de retornar os resultados, o Gerenciador de armazenamento no `InstanceContext` é usado para salvar a instância do serviço.

```csharp
object result = innerOperationInvoker.Invoke(instance,
    inputs, out outputs);

// Save the instance using the storage manager saved in the
// current InstanceContext.
InstanceContextExtension extension =
    OperationContext.Current.InstanceContext.Extensions.Find<InstanceContextExtension>();

extension.StorageManager.SaveInstance(extension.ContextId, instance);
return result;
```

## <a name="using-the-extension"></a>Usando a extensão

A camada de canal e as extensões de camada de modelo de serviço são feitas e agora podem ser usadas em aplicativos WCF. Os serviços devem adicionar o canal à pilha de canais usando uma associação personalizada e, em seguida, marcar as classes de implementação de serviço com os atributos apropriados.

```csharp
[DurableInstanceContext]
[ServiceBehavior(InstanceContextMode=InstanceContextMode.PerSession)]
public class ShoppingCart : IShoppingCart
{
//…
     [SaveState]
     public int AddItem(string item)
     {
         //…
     }
//…
 }
```

Os aplicativos cliente devem adicionar o DurableInstanceContextChannel à pilha de canais usando uma associação personalizada. Para configurar o canal de forma declarativa no arquivo de configuração, a seção elemento de associação deve ser adicionada à coleção de extensões do elemento de associação.

```xml
<system.serviceModel>
 <extensions>
   <bindingElementExtensions>
     <add name="durableInstanceContext"
type="Microsoft.ServiceModel.Samples.DurableInstanceContextBindingElementSection, DurableInstanceContextExtension, Version=1.0.0.0, Culture=neutral, PublicKeyToken=null"/>
   </bindingElementExtensions>
 </extensions>
</system.serviceModel>
```

Agora, o elemento Binding pode ser usado com uma associação personalizada, assim como outros elementos de ligação padrão:

```xml
<bindings>
 <customBinding>
   <binding name="TextOverHttp">
     <durableInstanceContext contextType="HttpCookie"/>
     <reliableSession />
     <textMessageEncoding />
     <httpTransport />
   </binding>
 </customBinding>
</bindings>
```

## <a name="conclusion"></a>Conclusão

Este exemplo mostrou como criar um canal de protocolo personalizado e como personalizar o comportamento do serviço para habilitá-lo.

A extensão pode ser melhorada, permitindo que os usuários especifiquem a `IStorageManager` implementação usando uma seção de configuração. Isso torna possível modificar o armazenamento de backup sem recompilar o código do serviço.

Além disso, você pode tentar implementar uma classe (por exemplo, `StateBag` ), que encapsula o estado da instância. Essa classe é responsável por persistir o estado sempre que ele for alterado. Dessa forma, você pode evitar o uso do `SaveState` atributo e executar o trabalho persistente com mais precisão (por exemplo, você pode persistir o estado quando o estado é realmente alterado, em vez de salvá-lo toda vez que um método com o `SaveState` atributo for chamado).

Quando você executa o exemplo, a saída a seguir é exibida. O cliente adiciona dois itens ao seu carrinho de compras e obtém a lista de itens em seu carrinho de compras do serviço. Pressione ENTER em cada janela do console para desligar o serviço e o cliente.

```console
Enter the name of the product: apples
Enter the name of the product: bananas

Shopping cart currently contains the following items.
apples
bananas
Press ENTER to shut down client
```

> [!NOTE]
> A recriação do serviço substitui o arquivo de banco de dados. Para observar o estado preservado entre várias execuções do exemplo, não se esqueça de recompilar o exemplo entre as execuções.

#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

> [!NOTE]
> Você deve estar executando o SQL Server 2005 ou o SQL Express 2005 para executar este exemplo. Se estiver executando o SQL Server 2005, você deverá modificar a configuração da cadeia de conexão do serviço. Ao executar a máquina cruzada, SQL Server só é necessária no computador do servidor.

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Instancing\Durable`
