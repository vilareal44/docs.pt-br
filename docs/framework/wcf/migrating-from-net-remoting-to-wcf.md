---
title: Migrando de .NET Remoting para o WCF
description: Saiba como migrar um aplicativo que usa a comunicação remota do .NET para usar o WCF. Você pode realizar vários cenários comuns de comunicação remota no WCF.
ms.date: 03/30/2017
ms.assetid: 16902a42-ef80-40e9-8c4c-90e61ddfdfe5
ms.openlocfilehash: f6f526db09806008314980b71233b208d25359fc
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246148"
---
# <a name="migrating-from-net-remoting-to-wcf"></a>Migrando de .NET Remoting para o WCF
Este artigo descreve como migrar um aplicativo que usa a comunicação remota do .NET para usar o Windows Communication Foundation (WCF). Ele compara conceitos semelhantes entre esses produtos e descreve como realizar vários cenários comuns de comunicação remota no WCF.  
  
 O .NET Remoting é um produto herdado que tem suporte apenas para compatibilidade com versões anteriores. Ele não é seguro entre ambientes de confiança mista porque não pode manter os níveis de confiança separados entre cliente e servidor. Por exemplo, você nunca deve expor um ponto de extremidade de comunicação remota do .NET à Internet ou a clientes não confiáveis. Recomendamos que os aplicativos de comunicação remota existentes sejam migrados para tecnologias mais novas e mais seguras. Se o design do aplicativo usar apenas HTTP e for RESTful, recomendamos ASP.NET Web API. Para obter mais informações, consulte ASP.NET Web API. Se o aplicativo for baseado em SOAP ou exigir protocolos não http, como TCP, recomendamos o WCF.  

## <a name="comparing-net-remoting-to-wcf"></a>Comparando o .NET Remoting com o WCF  
 Esta seção compara os blocos de construção básicos da comunicação remota do .NET com seus equivalentes do WCF. Usaremos esses blocos de construção posteriormente para criar alguns cenários comuns de cliente-servidor no WCF. O gráfico a seguir resume as principais semelhanças e diferenças entre o .NET Remoting e o WCF.  
  
||Comunicação remota .NET|WCF|  
|-|-------------------|---------|  
|Tipo de servidor|MarshalByRefObject de subclasse|Marcar com atributo [ServiceContract]|  
|Operações de serviço|Métodos públicos no tipo de servidor|Marcar com atributo [OperationContract]|  
|Serialização|ISerializable ou [Serializable]|DataContractSerializer ou XmlSerializer|  
|Objetos aprovados|Por valor ou por referência|Somente por valor|  
|Erros/exceções|Qualquer exceção serializável|FaultContract\<TDetail>|  
|Objetos de proxy de cliente|Proxies transparentes com rigidez de tipos são criados automaticamente a partir de MarshalByRefObjects|Proxies com rigidez de tipos são gerados sob demanda usando ChannelFactory\<TChannel>|  
|Plataforma necessária|O cliente e o servidor devem usar o Microsoft OS e o .NET|Plataforma cruzada|  
|Formato da mensagem|Particular|Padrões do setor (SOAP, WS-*, etc.)|  
  
### <a name="server-implementation-comparison"></a>Comparação de implementação do servidor  
  
#### <a name="creating-a-server-in-net-remoting"></a>Criando um servidor na comunicação remota do .NET  
 Os tipos de servidor de comunicação remota do .NET devem derivar de MarshalByRefObject e definir métodos que o cliente pode chamar, como o seguinte:  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 Os métodos públicos desse tipo de servidor tornam-se o contrato público disponível para os clientes.  Não há nenhuma separação entre a interface pública do servidor e sua implementação – um tipo manipula ambos.  
  
 Depois que o tipo de servidor tiver sido definido, ele poderá ser disponibilizado para os clientes, como no exemplo a seguir:  
  
```csharp
TcpChannel channel = new TcpChannel(8080);  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingConfiguration.RegisterWellKnownServiceType(  
    typeof(RemotingServer),
    "RemotingServer",
    WellKnownObjectMode.Singleton);  
Console.WriteLine("RemotingServer is running.  Press ENTER to terminate...");  
Console.ReadLine();  
```  
  
 Há várias maneiras de tornar o tipo de comunicação remota disponível como um servidor, incluindo o uso de arquivos de configuração. Este é apenas um exemplo.  
  
#### <a name="creating-a-server-in-wcf"></a>Criando um servidor no WCF  
 A etapa equivalente no WCF envolve a criação de dois tipos – o "contrato de serviço" público e a implementação concreta. A primeira é declarada como uma interface marcada com [ServiceContract]. Os métodos disponíveis para os clientes são marcados com [OperationContract]:  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 A implementação do servidor é definida em uma classe concreta separada, como no exemplo a seguir:  
  
```csharp
public class WCFServer : IWCFServer  
{  
    public Customer GetCustomer(int customerId) { … }  
}  
```  
  
 Depois que esses tipos tiverem sido definidos, o servidor WCF poderá ser disponibilizado para os clientes, como no exemplo a seguir:  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
Uri baseAddress = new Uri("net.tcp://localhost:8000/wcfserver");  
  
using (ServiceHost serviceHost = new ServiceHost(typeof(WCFServer), baseAddress))  
{  
    serviceHost.AddServiceEndpoint(typeof(IWCFServer), binding, baseAddress);  
    serviceHost.Open();  
  
    Console.WriteLine($"The WCF server is ready at {baseAddress}.");
    Console.WriteLine("Press <ENTER> to terminate service...");  
    Console.WriteLine();  
    Console.ReadLine();  
}  
```  
  
> [!NOTE]
> O TCP é usado em ambos os exemplos para mantê-los o mais semelhante possível. Consulte as instruções detalhadas do cenário mais adiante neste tópico para obter exemplos usando HTTP.  
  
 Há várias maneiras de configurar e hospedar serviços WCF. Esse é apenas um exemplo, conhecido como "auto-hospedado". Para obter mais informações, consulte estes tópicos:  
  
- [Como definir um contrato de serviço](how-to-define-a-wcf-service-contract.md)  
  
- [Configurando serviços usando arquivos de configuração](configuring-services-using-configuration-files.md)  
  
- [Hospedando serviços](hosting-services.md)  
  
### <a name="client-implementation-comparison"></a>Comparação de implementação do cliente  
  
#### <a name="creating-a-client-in-net-remoting"></a>Criando um cliente no .NET Remoting  
 Depois que um objeto de servidor de comunicação remota do .NET tiver sido disponibilizado, ele poderá ser consumido por clientes, como no exemplo a seguir:  
  
```csharp
TcpChannel channel = new TcpChannel();  
ChannelServices.RegisterChannel(channel, ensureSecurity : true);  
RemotingServer server = (RemotingServer)Activator.GetObject(  
                            typeof(RemotingServer),
                            "tcp://localhost:8080/RemotingServer");  
  
RemotingCustomer customer = server.GetCustomer(42);  
Console.WriteLine($"Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 A instância RemotingServer retornada de Activator. GetObject () é conhecida como "proxy transparente". Ele implementa a API pública para o tipo RemotingServer no cliente, mas todos os métodos chamam o objeto Server em execução em um processo ou computador diferente.  
  
#### <a name="creating-a-client-in-wcf"></a>Criando um cliente no WCF  
 A etapa equivalente no WCF envolve o uso de uma fábrica de canais para criar o proxy explicitamente. Como a comunicação remota, o objeto proxy pode ser usado para invocar operações no servidor, como no exemplo a seguir:  
  
```csharp
NetTcpBinding binding = new NetTcpBinding();  
String url = "net.tcp://localhost:8000/wcfserver";  
EndpointAddress address = new EndpointAddress(url);  
ChannelFactory<IWCFServer> channelFactory =
    new ChannelFactory<IWCFServer>(binding, address);  
IWCFServer server = channelFactory.CreateChannel();  
  
Customer customer = server.GetCustomer(42);  
Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
```  
  
 Este exemplo mostra a programação no nível do canal porque ele é mais semelhante ao exemplo de comunicação remota. Também está disponível a abordagem **Adicionar referência de serviço** no Visual Studio que gera código para simplificar a programação do cliente. Para obter mais informações, consulte estes tópicos:  
  
- [Programação de nível de canal do cliente](./extending/client-channel-level-programming.md)  
  
- [Como adicionar, atualizar ou remover uma referência de serviço](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference)  
  
### <a name="serialization-usage"></a>Uso de serialização  
 Tanto a comunicação remota do .NET quanto o WCF usam a serialização para enviar objetos entre o cliente e o servidor, mas diferem nessas maneiras importantes:  
  
1. Eles usam serializadores e convenções diferentes para indicar o que serializar.  
  
2. A comunicação remota do .NET dá suporte à serialização "por referência" que permite o acesso de método ou propriedade em uma camada para executar o código na outra camada, que está entre limites de segurança. Esse recurso expõe vulnerabilidades de segurança e é um dos principais motivos pelos quais os pontos de extremidade de comunicação remota nunca devem ser expostos a clientes não confiáveis.  
  
3. A serialização usada pela comunicação remota é opcional (exclua explicitamente o que não serializar) e a serialização do WCF é opcional (marque explicitamente quais membros serão serializados).  
  
#### <a name="serialization-in-net-remoting"></a>Serialização na comunicação remota do .NET  
 O .NET Remoting dá suporte a duas maneiras de serializar e desserializar objetos entre o cliente e o servidor:  
  
- *Por valor* – os valores do objeto são serializados entre limites de camada e uma nova instância desse objeto é criada na outra camada. Todas as chamadas para métodos ou Propriedades dessa nova instância são executadas apenas localmente e não afetam o objeto ou a camada original.  
  
- *Por referência* – uma "referência de objeto" especial é serializada entre limites de camada. Quando uma camada interage com métodos ou propriedades desse objeto, ela se comunica de volta ao objeto original na camada original. Objetos de referência podem fluir em qualquer direção – servidor para cliente ou cliente para servidor.  
  
 Tipos de valor em comunicação remota são marcados com o atributo [Serializable] ou implementam ISerializable, como no exemplo a seguir:  
  
```csharp
[Serializable]  
public class RemotingCustomer  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 Tipos de referência derivam da classe MarshalByRefObject, como no exemplo a seguir:  
  
```csharp
public class RemotingCustomerReference : MarshalByRefObject  
{  
    public string FirstName { get; set; }  
    public string LastName { get; set; }  
    public int CustomerId { get; set; }  
}  
```  
  
 É extremamente importante entender as implicações dos objetos por referência de comunicação remota. Se qualquer camada (cliente ou servidor) enviar um objeto por referência para a outra camada, todas as chamadas de método executarão novamente a camada que possui o objeto. Por exemplo, um cliente que chama métodos em um objeto por referência retornado pelo servidor executará o código no servidor. Da mesma forma, um servidor que chama métodos em um objeto por referência fornecido pelo cliente executará o código de volta no cliente. Por esse motivo, o uso da comunicação remota do .NET é recomendado somente em ambientes totalmente confiáveis. Expor um ponto de extremidade de comunicação remota do .NET público para clientes não confiáveis tornará um servidor de comunicação remota vulnerável a ataques.  
  
#### <a name="serialization-in-wcf"></a>Serialização no WCF  
 O WCF dá suporte apenas à serialização de valor. A maneira mais comum de definir um tipo para troca entre cliente e servidor é como no exemplo a seguir:  
  
```csharp
[DataContract]  
public class WCFCustomer  
{  
    [DataMember]  
    public string FirstName { get; set; }  
  
    [DataMember]  
    public string LastName { get; set; }  
  
    [DataMember]  
    public int CustomerId { get; set; }  
}  
```  
  
 O atributo [DataContract] identifica esse tipo como um que pode ser serializado e desserializado entre o cliente e o servidor. O atributo [DataMember] identifica as propriedades individuais ou os campos a serem serializados.  
  
 Quando o WCF envia um objeto entre as camadas, ele serializa apenas os valores e cria uma nova instância do objeto na outra camada. Todas as interações com os valores do objeto ocorrem somente localmente – elas não se comunicam com a outra camada da forma como os objetos de referência de comunicação remota do .NET fazem. Para obter mais informações, consulte [serialização e desserialização](./feature-details/serialization-and-deserialization.md).  
  
### <a name="exception-handling-capabilities"></a>Recursos de manipulação de exceção  
  
#### <a name="exceptions-in-net-remoting"></a>Exceções na comunicação remota do .NET  
 As exceções geradas por um servidor remoto são serializadas, enviadas ao cliente e lançadas localmente no cliente, como qualquer outra exceção. As exceções personalizadas podem ser criadas por meio da subclasse do tipo de exceção e de sua marcação com [Serializable].   A maioria das exceções de estrutura já está marcada dessa forma, permitindo que a maioria seja lançada pelo servidor, serializada e relançada no cliente. Embora esse design seja conveniente durante o desenvolvimento, as informações do lado do servidor podem ser inadvertidamente divulgadas ao cliente. Esse é um dos muitos motivos pelos quais a comunicação remota deve ser usada somente em ambientes totalmente confiáveis.  
  
#### <a name="exceptions-and-faults-in-wcf"></a>Exceções e falhas no WCF  
 O WCF não permite que tipos de exceção arbitrárias sejam retornados do servidor para o cliente, pois isso pode levar à divulgação inadvertida de informações. Se uma operação de serviço lançar uma exceção inesperada, ela fará com que uma FaultException de uso geral seja gerada no cliente. Essa exceção não carrega nenhuma informação por quê ou onde o problema ocorreu, e para alguns aplicativos isso é suficiente. Os aplicativos que precisam comunicar informações de erro mais avançadas para o cliente fazem isso definindo um contrato de falha.  
  
 Para fazer isso, primeiro crie um tipo [DataContract] para transportar as informações de falha.  
  
```csharp
[DataContract]  
public class CustomerServiceFault  
{  
    [DataMember]  
    public string ErrorMessage { get; set; }  
  
    [DataMember]  
    public int CustomerId {get;set;}  
}  
```  
  
 Especifique o contrato de falha a ser usado para cada operação de serviço.  
  
```csharp
[ServiceContract]  
public interface IWCFServer  
{  
    [OperationContract]  
    [FaultContract(typeof(CustomerServiceFault))]  
    Customer GetCustomer(int customerId);  
}  
```  
  
 O servidor relata as condições de erro lançando uma FaultException.  
  
```csharp
throw new FaultException<CustomerServiceFault>(  
    new CustomerServiceFault() {
        CustomerId = customerId,
        ErrorMessage = "Illegal customer Id"
    });  
```  
  
 E sempre que o cliente fizer uma solicitação ao servidor, ele poderá detectar falhas como exceções normais.  
  
```csharp
try  
{  
    Customer customer = server.GetCustomer(-1);  
}  
catch (FaultException<CustomerServiceFault> fault)  
{  
    Console.WriteLine($"Fault received: {fault.Detail.ErrorMessage}");
}  
```  
  
 Para obter mais informações sobre contratos de falha, consulte <xref:System.ServiceModel.FaultException> .  
  
### <a name="security-considerations"></a>Considerações de segurança  
  
#### <a name="security-in-net-remoting"></a>Segurança na comunicação remota do .NET  
 Alguns canais de comunicação remota do .NET dão suporte a recursos de segurança como autenticação e criptografia na camada de canal (IPC e TCP). O canal HTTP depende do Serviços de Informações da Internet (IIS) para autenticação e criptografia. Apesar desse suporte, você deve considerar o .NET Remoting um protocolo de comunicação não seguro e usá-lo somente em ambientes totalmente confiáveis. Nunca exponha um ponto de extremidade de comunicação remota pública para a Internet ou clientes não confiáveis.  
  
#### <a name="security-in-wcf"></a>Segurança no WCF  
 O WCF foi projetado tendo em mente a segurança, em parte para abordar os tipos de vulnerabilidades encontrados na comunicação remota do .NET. O WCF oferece segurança no nível de transporte e de mensagem e oferece muitas opções de autenticação, autorização, criptografia e assim por diante. Para obter mais informações, consulte estes tópicos:  
  
- [Segurança](./feature-details/security.md)  
  
- [Diretrizes de segurança do WCF](./feature-details/security-guidance-and-best-practices.md)  
  
## <a name="migrating-to-wcf"></a>Migrando para o WCF  
  
### <a name="why-migrate-from-remoting-to-wcf"></a>Por que migrar da comunicação remota para o WCF?  
  
- **O .NET Remoting é um produto herdado.** Conforme descrito em [.NET Remoting](https://docs.microsoft.com/previous-versions/dotnet/netframework-4.0/72x4h507%28v=vs.100%29), ele é considerado um produto herdado e não é recomendado para um novo desenvolvimento. O WCF ou o ASP.NET Web API são recomendados para aplicativos novos e existentes.  
  
- **O WCF usa padrões de plataforma cruzada.** O WCF foi projetado com a interoperabilidade entre plataformas em mente e dá suporte a muitos padrões do setor (SOAP, WS-Security, WS-Trust, etc.). Um serviço WCF pode interoperar com clientes em execução em sistemas operacionais diferentes do Windows. A comunicação remota foi projetada principalmente para ambientes em que ambos os aplicativos cliente e servidor são executados usando .NET Framework em um sistema operacional Windows.
  
- **O WCF tem segurança interna.** O WCF foi projetado tendo em mente a segurança e oferece muitas opções de autenticação, segurança de nível de transporte, segurança de nível de mensagem, etc. A comunicação remota foi projetada para tornar mais fácil para os aplicativos interoperarem, mas não foram projetados para serem seguros em ambientes não confiáveis. O WCF foi projetado para funcionar em ambientes confiáveis e não confiáveis.  
  
### <a name="migration-recommendations"></a>Recomendações de migração  
 A seguir estão as etapas recomendadas para migrar do .NET Remoting para o WCF:  
  
- **Crie o contrato de serviço.** Defina os tipos de interface de serviço e marque-os com o atributo [ServiceContract]. Marque todos os métodos aos quais os clientes terão permissão para chamar [OperationContract].  
  
- **Crie o contrato de dados.** Defina os tipos de dados que serão trocados entre o servidor e o cliente e marque-os com o atributo [DataContract]. Marque todos os campos e propriedades que o cliente terá permissão para usar com [DataMember].  
  
- **Crie o contrato de falha (opcional).** Crie os tipos que serão trocados entre o servidor e o cliente quando forem encontrados erros. Marque esses tipos com [DataContract] e [DataMember] para torná-los serializáveis. Para todas as operações de serviço que você marcou com [OperationContract], também marque-as com [FaultContract] para indicar quais erros podem ser retornados.  
  
- **Configure e hospede o serviço.** Depois que o contrato de serviço tiver sido criado, a próxima etapa será configurar uma associação para expor o serviço em um ponto de extremidade. Para obter mais informações, consulte [pontos de extremidade: endereços, associações e contratos](./feature-details/endpoints-addresses-bindings-and-contracts.md).  
  
 Depois que um aplicativo de comunicação remota é migrado para o WCF, ainda é importante remover dependências na comunicação remota do .NET. Isso garante que todas as vulnerabilidades de comunicação remota sejam removidas do aplicativo. Essas etapas incluem o seguinte:  
  
- **Descontinuar o uso de MarshalByRefObject.** O tipo MarshalByRefObject existe somente para comunicação remota e não é usado pelo WCF. Qualquer tipo de aplicativo que a subclasse MarshalByRefObject deve ser removida ou alterada.  
  
- **Descontinue o uso de [Serializable] e ISerializable.** O atributo [Serializable] e a interface ISerializable foram originalmente projetados para serializar tipos em ambientes confiáveis e são usados pela comunicação remota. A serialização do WCF depende dos tipos marcados com [DataContract] e [DataMember]. Os tipos de dados usados por um aplicativo devem ser modificados para usar [DataContract] e não para usar ISerializable ou [Serializable].  
  
### <a name="migration-scenarios"></a>Cenários de migração  
 Agora, vejamos como realizar os seguintes cenários comuns de comunicação remota no WCF:  
  
1. O servidor retorna um objeto por valor para o cliente  
  
2. O servidor retorna um objeto por referência ao cliente  
  
3. O cliente envia um objeto por valor para o servidor  
  
> [!NOTE]
> O envio de um objeto por referência do cliente para o servidor não é permitido no WCF.  
  
 Ao ler esses cenários, suponha que nossas interfaces de linha de base para .NET Remoting sejam semelhantes ao exemplo a seguir. A implementação de comunicação remota do .NET não é importante aqui porque queremos ilustrar apenas como usar o WCF para implementar a funcionalidade equivalente.  
  
```csharp
public class RemotingServer : MarshalByRefObject  
{  
    // Demonstrates server returning object by-value  
    public Customer GetCustomer(int customerId) {…}  
  
    // Demonstrates server returning object by-reference  
    public CustomerReference GetCustomerReference(int customerId) {…}  
  
    // Demonstrates client passing object to server by-value  
    public bool UpdateCustomer(Customer customer) {…}  
}  
```  
  
#### <a name="scenario-1-service-returns-an-object-by-value"></a>Cenário 1: o serviço retorna um objeto por valor  
 Este cenário demonstra um servidor que retorna um objeto ao cliente por valor. O WCF sempre retorna objetos do servidor por valor, portanto, as etapas a seguir descrevem simplesmente como criar um serviço WCF normal.  
  
1. Comece definindo uma interface pública para o serviço WCF e marque-a com o atributo [ServiceContract]. Usamos [OperationContract] para identificar os métodos do lado do servidor que nosso cliente chamará.  
  
   ```csharp
   [ServiceContract]  
   public interface ICustomerService  
   {  
       [OperationContract]  
       Customer GetCustomer(int customerId);  
  
       [OperationContract]  
       bool UpdateCustomer(Customer customer);  
   }  
   ```  
  
2. A próxima etapa é criar o contrato de dados para esse serviço. Fazemos isso criando classes (não interfaces) marcadas com o atributo [DataContract]. As propriedades ou os campos individuais que queremos que sejam visíveis para o cliente e para o servidor são marcados com [DataMember]. Se quisermos que tipos derivados sejam permitidos, devemos usar o atributo [KnownType] para identificá-los. Os únicos tipos do WCF permitirão que sejam serializados ou desserializados para esse serviço são aqueles na interface de serviço e esses "tipos conhecidos". A tentativa de trocar qualquer outro tipo que não esteja nesta lista será rejeitada.  
  
   ```csharp
   [DataContract]  
   [KnownType(typeof(PremiumCustomer))]  
   public class Customer  
   {  
       [DataMember]  
       public string FirstName { get; set; }  
  
       [DataMember]  
       public string LastName { get; set; }  
  
       [DataMember]  
       public int CustomerId { get; set; }  
   }  
  
   [DataContract]  
   public class PremiumCustomer : Customer
   {  
       [DataMember]  
       public int AccountId { get; set; }  
   }  
   ```  
  
3. Em seguida, fornecemos a implementação para a interface de serviço.  
  
   ```csharp  
   public class CustomerService : ICustomerService  
   {  
       public Customer GetCustomer(int customerId)  
       {  
           // read from database  
       }  
  
       public bool UpdateCustomer(Customer customer)  
       {  
           // write to database  
       }  
   }  
   ```  
  
4. Para executar o serviço WCF, precisamos declarar um ponto de extremidade que expõe essa interface de serviço em uma URL específica usando uma associação WCF específica. Isso normalmente é feito adicionando as seções a seguir ao arquivo de web.config do projeto do servidor.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
5. O serviço WCF pode então ser iniciado com o seguinte código:  
  
   ```csharp
   ServiceHost customerServiceHost = new ServiceHost(typeof(CustomerService));  
       customerServiceHost.Open();  
   ```  
  
     Quando esse ServiceHost é iniciado, ele usa o arquivo web.config para estabelecer o contrato, a associação e o ponto de extremidade apropriados. Para obter mais informações sobre arquivos de configuração, consulte [Configurando serviços usando arquivos de configuração](./configuring-services-using-configuration-files.md). Esse estilo de inicialização do servidor é conhecido como hospedagem interna. Para saber mais sobre outras opções de Hospedagem de serviços WCF, consulte [serviços de hospedagem](./hosting-services.md).  
  
6. O app.config do projeto do cliente deve declarar informações de associação correspondentes para o ponto de extremidade do serviço. A maneira mais fácil de fazer isso no Visual Studio é usar **Adicionar referência de serviço**, que atualizará automaticamente o arquivo de app.config. Como alternativa, essas mesmas alterações podem ser adicionadas manualmente.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     Para obter mais informações sobre como usar **Adicionar referência de serviço**, consulte [como adicionar, atualizar ou remover uma referência de serviço](/visualstudio/data-tools/how-to-add-update-or-remove-a-wcf-data-service-reference).  
  
7. Agora, podemos chamar o serviço WCF do cliente. Fazemos isso criando uma fábrica de canais para esse serviço, solicitando-o por um canal e chamando diretamente o método que desejamos nesse canal. Podemos fazer isso porque o canal implementa a interface do serviço e manipula a lógica de solicitação/resposta subjacente para nós. O valor de retorno dessa chamada de método é a cópia desserializada da resposta do servidor.  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   Customer customer = service.GetCustomer(42);  
   Console.WriteLine($"  Customer {customer.FirstName} {customer.LastName} received.");
   ```  
  
 Os objetos retornados pelo WCF do servidor para o cliente são sempre por valor. Os objetos são cópias desserializadas dos dados enviados pelo servidor. O cliente pode chamar métodos nessas cópias locais sem nenhum perigo de invocar o código do servidor por meio de retornos de chamada.  
  
#### <a name="scenario-2-server-returns-an-object-by-reference"></a>Cenário 2: o servidor retorna um objeto por referência  
 Este cenário demonstra o servidor que fornece um objeto ao cliente por referência. Na comunicação remota do .NET, isso é manipulado automaticamente para qualquer tipo derivado de MarshalByRefObject, que é serializado por referência. Um exemplo desse cenário é permitir que vários clientes tenham objetos do lado do servidor de sessão independentes. Como mencionado anteriormente, os objetos retornados por um serviço WCF são sempre por valor, portanto, não há nenhum equivalente direto de um objeto por referência, mas é possível obter algo semelhante à semântica por referência usando um <xref:System.ServiceModel.EndpointAddress10> objeto. Esse é um objeto de valor serializável que pode ser usado pelo cliente para obter uma sessão por objeto de referência no servidor. Isso habilita o cenário de ter vários clientes com objetos do lado do servidor de sessão independentes.  
  
1. Primeiro, precisamos definir um contrato de serviço do WCF que corresponda ao próprio objeto de sessão.  
  
   ```csharp
   [ServiceContract(SessionMode = SessionMode.Allowed)]  
       public interface ISessionBoundObject  
       {  
           [OperationContract]  
           string GetCurrentValue();  
  
           [OperationContract]  
           void SetCurrentValue(string value);  
       }  
   ```  
  
    > [!TIP]
    > Observe que o objeto de sessão está marcado com [ServiceContract], tornando-o uma interface de serviço WCF normal. Definir a Propriedade SessionMode indica que será um serviço de sessão. No WCF, uma sessão é uma maneira de correlacionar várias mensagens enviadas entre dois pontos de extremidade. Isso significa que, depois que um cliente obtiver uma conexão para esse serviço, uma sessão será estabelecida entre o cliente e o servidor. O cliente usará uma única instância exclusiva do objeto do lado do servidor para todas as interações dele nessa única sessão.  
  
2. Em seguida, precisamos fornecer a implementação dessa interface de serviço. Ao denotar isso com [serviceInstance] e definir o InstanceContextmode, dizemos ao WCF que desejamos usar uma instância exclusiva desse tipo para cada sessão.  
  
   ```csharp
   [ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]  
       public class MySessionBoundObject : ISessionBoundObject  
       {  
           private string _value;  
  
           public string GetCurrentValue()  
           {  
               return _value;  
           }  
  
           public void SetCurrentValue(string val)  
           {  
               _value = val;  
           }  
  
       }  
   ```  
  
3. Agora precisamos de uma maneira de obter uma instância desse objeto de sessão. Fazemos isso criando outra interface de serviço do WCF que retorna um objeto EndpointAddress10. Essa é uma forma serializável de um ponto de extremidade que o cliente pode usar para criar o objeto de sessão.  
  
   ```csharp
   [ServiceContract]  
       public interface ISessionBoundFactory  
       {  
           [OperationContract]  
           EndpointAddress10 GetInstanceAddress();  
       }  
   ```  
  
     E implementamos este serviço WCF:  
  
   ```csharp
   public class SessionBoundFactory : ISessionBoundFactory  
       {  
           public static ChannelFactory<ISessionBoundObject> _factory =
               new ChannelFactory<ISessionBoundObject>("sessionbound");  

           public SessionBoundFactory()  
           {  
           }  
  
           public EndpointAddress10 GetInstanceAddress()  
           {  
               IClientChannel channel = (IClientChannel)_factory.CreateChannel();  
               return EndpointAddress10.FromEndpointAddress(channel.RemoteAddress);  
           }  
       }  
   ```  
  
     Essa implementação mantém uma fábrica de canais única para criar objetos de sessão. Quando GetInstanceAddress () é chamado, ele cria um canal e cria um objeto EndpointAddress10 que aponta efetivamente para o endereço remoto associado a esse canal. EndpointAddress10 é simplesmente um tipo de dados que pode ser retornado ao cliente por valor.  
  
4. Precisamos modificar o arquivo de configuração do servidor fazendo as duas ações a seguir, conforme mostrado no exemplo abaixo:  
  
    1. Declare uma \<client> seção que descreve o ponto de extremidade para o objeto de sessão. Isso é necessário porque o servidor também atua como um cliente nessa situação.  
  
    2. Declare pontos de extremidade para a fábrica e o objeto de sessão. Isso é necessário para permitir que o cliente se comunique com os pontos de extremidade de serviço para adquirir o EndpointAddress10 e criar o canal de sessão.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
         <client>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
        </client>  
        <services>  
          <service name="Server.CustomerService">  
            <endpoint address="http://localhost:8083/CustomerService"  
                      binding="basicHttpBinding"  
                      contract="Shared.ICustomerService" />  
          </service>  
          <service name="Server.MySessionBoundObject">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundObject"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundObject" />  
          </service>  
          <service name="Server.SessionBoundFactory">  
            <endpoint address="net.tcp://localhost:8081/SessionBoundFactory"  
                      binding="netTcpBinding"  
                      contract="Shared.ISessionBoundFactory" />  
          </service>  
        </services>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     E, em seguida, podemos iniciar esses serviços:  
  
   ```csharp
   ServiceHost factoryHost = new ServiceHost(typeof(SessionBoundFactory));  
   factoryHost.Open();  
  
   ServiceHost sessionHost = new ServiceHost(typeof(MySessionBoundObject));  
   sessionHost.Open();  
   ```  
  
5. Configuramos o cliente declarando esses mesmos pontos de extremidade no arquivo de app.config do seu projeto.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <client>  
          <endpoint name="customerservice"  
                    address="http://localhost:8083/CustomerService"  
                    binding="basicHttpBinding"  
                    contract="Shared.ICustomerService"/>  
          <endpoint name="sessionbound"  
                    address="net.tcp://localhost:8081/SessionBoundObject"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundObject"/>  
          <endpoint name="factory"  
                    address="net.tcp://localhost:8081/SessionBoundFactory"  
                    binding="netTcpBinding"  
                    contract="Shared.ISessionBoundFactory"/>  
        </client>  
      </system.serviceModel>  
    </configuration>  
    ```  
  
6. Para criar e usar esse objeto de sessão, o cliente deve executar as seguintes etapas:  
  
    1. Crie um canal para o serviço ISessionBoundFactory.  
  
    2. Use esse canal para invocar esse serviço para obter um EndpointAddress10.  
  
    3. Use o EndpointAddress10 para criar um canal para obter um objeto de sessão.  
  
    4. Interaja com o objeto de sessão para demonstrar que ele permanece a mesma instância em várias chamadas.  
  
   ```csharp
   ChannelFactory<ISessionBoundFactory> channelFactory =
       new ChannelFactory<ISessionBoundFactory>("factory");  
   ISessionBoundFactory sessionFactory = channelFactory.CreateChannel();  
  
   EndpointAddress10 address1 = sessionFactory.GetInstanceAddress();  
   EndpointAddress10 address2 = sessionFactory.GetInstanceAddress();  
  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory1 =
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),
                                               address1.ToEndpointAddress());  
   ChannelFactory<ISessionBoundObject> sessionObjectFactory2 =
       new ChannelFactory<ISessionBoundObject>(new NetTcpBinding(),
                                               address2.ToEndpointAddress());  
  
   ISessionBoundObject sessionInstance1 = sessionObjectFactory1.CreateChannel();  
   ISessionBoundObject sessionInstance2 = sessionObjectFactory2.CreateChannel();  
  
   sessionInstance1.SetCurrentValue("Hello");  
   sessionInstance2.SetCurrentValue("World");  
  
   if (sessionInstance1.GetCurrentValue() == "Hello" &&  
       sessionInstance2.GetCurrentValue() == "World")  
   {  
       Console.WriteLine("sessionful server object works as expected");  
   }  
   ```  
  
 O WCF sempre retorna objetos por valor, mas é possível dar suporte ao equivalente de semântica por referência por meio do uso de EndpointAddress10. Isso permite que o cliente solicite uma instância de serviço WCF de sessão, após a qual ela pode interagir com ela, como qualquer outro serviço WCF.  
  
#### <a name="scenario-3-client-sends-server-a-by-value-instance"></a>Cenário 3: o cliente envia uma instância por valor do servidor  
 Este cenário demonstra o cliente que envia uma instância de objeto não primitivo para o servidor por valor. Como o WCF só envia objetos por valor, esse cenário demonstra o uso normal do WCF.  
  
1. Use o mesmo serviço WCF do cenário 1.  
  
2. Use o cliente para criar um novo objeto por valor (cliente), crie um canal para se comunicar com o serviço ICustomerService e envie o objeto para ele.  
  
   ```csharp
   ChannelFactory<ICustomerService> factory =  
       new ChannelFactory<ICustomerService>("customerservice");  
   ICustomerService service = factory.CreateChannel();  
   PremiumCustomer customer = new PremiumCustomer {
   FirstName = "Bob",
   LastName = "Jones",
   CustomerId = 43,
   AccountId = 99};  
   bool success = service.UpdateCustomer(customer);  
   Console.WriteLine($"  Server returned {success}.");
   ```  
  
     O objeto Customer será serializado e enviado para o servidor, onde será desserializado em uma nova cópia desse objeto.  
  
    > [!NOTE]
    > Esse código também ilustra o envio de um tipo derivado (PremiumCustomer). A interface de serviço espera um objeto de cliente, mas o atributo [KnownType] na classe Customer indicou PremiumCustomer também foi permitido. O WCF falhará em qualquer tentativa de serializar ou desserializar qualquer outro tipo por meio dessa interface de serviço.  
  
 As trocas de dados WCF normais são por valor. Isso garante que a invocação de métodos em um desses objetos de dados seja executada apenas localmente – ele não invocará o código na outra camada. Embora seja possível obter algo como objetos de referência retornados *do* servidor, não é possível que um cliente passe um objeto por referência *para* o servidor. Um cenário que requer uma conversa de ida e volta entre o cliente e o servidor pode ser obtido no WCF usando um serviço duplex. Para obter mais informações, consulte [duplex Services](./feature-details/duplex-services.md).  
  
## <a name="summary"></a>Resumo  
 A comunicação remota do .NET é uma estrutura de comunicação destinada a ser usada somente em ambientes totalmente confiáveis. Ele é um produto herdado e tem suporte apenas para compatibilidade com versões anteriores. Ele não deve ser usado para criar novos aplicativos. Por outro lado, o WCF foi projetado tendo em mente a segurança e é recomendado para aplicativos novos e existentes. A Microsoft recomenda que os aplicativos de comunicação remota existentes sejam migrados para usar o WCF ou ASP.NET Web API em vez disso.
