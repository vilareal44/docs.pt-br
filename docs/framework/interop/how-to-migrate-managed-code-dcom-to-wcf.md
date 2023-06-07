---
title: 'Como: Migrar código DCOM gerenciado para o WCF'
description: Migre chamadas de código gerenciados de Component Object Model distribuído (DCOM) entre servidores e clientes para Windows Communication Foundation (WCF).
ms.date: 03/30/2017
ms.assetid: 52961ffc-d1c7-4f83-832c-786444b951ba
ms.openlocfilehash: cc6ac1dd01e17bb184d1f1faca372134d6130d33
ms.sourcegitcommit: e02d17b2cf9c1258dadda4810a5e6072a0089aee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/01/2020
ms.locfileid: "85619085"
---
# <a name="how-to-migrate-managed-code-dcom-to-wcf"></a>Como: Migrar código DCOM gerenciado para o WCF
O WCF (Windows Communication Foundation) é a opção recomendada e uma escolha segura no lugar do DCOM (Distributed Component Object Model) para chamadas de código gerenciado entre servidores e clientes em um ambiente distribuído. Este artigo mostra como migrar código de DCOM para o WCF para os cenários a seguir.  
  
- O serviço remoto retorna um objeto por valor para o cliente  
  
- O cliente envia um objeto por valor para o serviço remoto  
  
- O serviço remoto retorna um objeto por referência para o cliente  
  
 Por motivos de segurança, enviar um objeto por referência do cliente para o serviço não é permitido no WCF. Um cenário que requer uma conversa de ida e volta entre o cliente e o servidor pode ser obtido no WCF usando um serviço duplex.  Para obter mais informações sobre serviços duplex, consulte [Serviços duplex](../wcf/feature-details/duplex-services.md).  
  
 Para obter mais detalhes sobre a criação de serviços do WCF e clientes para esses serviços, consulte [Programação WCF básica](../wcf/basic-wcf-programming.md), [Projetando e Implementando serviços](../wcf/designing-and-implementing-services.md) e [Compilando clientes](../wcf/building-clients.md).  
  
## <a name="dcom-example-code"></a>Código de exemplo do DCOM  
 Nesses cenários, as interfaces DCOM ilustradas usando o WCF têm a seguinte estrutura:  
  
```csharp  
[ComVisible(true)]  
[Guid("AA9C4CDB-55EA-4413-90D2-843F1A49E6E6")]  
public interface IRemoteService  
{  
   Customer GetObjectByValue();  
   IRemoteObject GetObjectByReference();  
   void SendObjectByValue(Customer customer);  
}  
  
[ComVisible(true)]  
[Guid("A12C98DE-B6A1-463D-8C24-81E4BBC4351B")]  
public interface IRemoteObject  
{  
}  
  
public class Customer  
{  
}  
```  
  
## <a name="the-service-returns-an-object-by-value"></a>O serviço retorna um objeto por valor  
 Para este cenário, você faz uma chamada para um serviço e método retorna um objeto, que é passado por valor do servidor para o cliente. Esse cenário representa a chamada COM a seguir:  
  
```csharp  
public interface IRemoteService  
{  
    Customer GetObjectByValue();  
}  
```  
  
 Nesse cenário, o cliente recebe uma cópia desserializada de um objeto do serviço remoto. O cliente pode interagir com essa cópia local sem fazer uma chamada de volta para o serviço.  Em outras palavras, o cliente tem certeza de que o serviço não estará envolvido de nenhum modo quando os métodos na cópia local forem chamados. O WCF sempre retorna objetos do serviço por valor, portanto, as etapas a seguir descrevem a criação de um serviço WCF normal.  
  
### <a name="step-1-define-the-wcf-service-interface"></a>Etapa 1: definir a interface do serviço WCF  
 Defina uma interface pública para o serviço WCF e marque-a com o atributo [<xref:System.ServiceModel.ServiceContractAttribute>].  Marcar os métodos que você deseja expor para clientes com o atributo [<xref:System.ServiceModel.OperationContractAttribute>]. O exemplo a seguir mostra o uso desses atributos para identificar a interface do lado do servidor e métodos de interface que um cliente pode chamar. O método usado para esse cenário é mostrado em negrito.  
  
```csharp  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Web;
. . .  
[ServiceContract]  
public interface ICustomerManager  
{  
    [OperationContract]  
    void StoreCustomer(Customer customer);  
  
    [OperationContract]     Customer GetCustomer(string firstName, string lastName);
  
}  
```  
  
### <a name="step-2-define-the-data-contract"></a>Etapa 2: definir o contrato de dados  
 Em seguida, você deve criar um contrato de dados para o serviço, que descreve como os dados serão trocados entre o serviço e seus clientes.  Classes descritas no contrato de dados devem ser marcadas com o atributo [<xref:System.Runtime.Serialization.DataContractAttribute>]. As propriedades ou os campos individuais que você quiser que sejam visíveis para o cliente e o servidor deverão ser marcados com o atributo [<xref:System.Runtime.Serialization.DataMemberAttribute>]. Se quiser que os tipos derivados de uma classe no contrato de dados sejam permitidos, você deverá identificá-los com o atributo [<xref:System.Runtime.Serialization.KnownTypeAttribute>]. WCF só será serializar ou desserializará tipos na interface do serviço e tipos identificados como conhecidos. Se você tentar usar um tipo que não for um tipo conhecido, ocorrerá uma exceção.  
  
 Para obter mais informações sobre contratos de dados, consulte [Contratos de dados](../wcf/samples/data-contracts.md).  
  
```csharp  
[DataContract]  
[KnownType(typeof(PremiumCustomer))]  
public class Customer  
{  
    [DataMember]  
    public string Firstname;  
    [DataMember]  
    public string Lastname;  
    [DataMember]  
    public Address DefaultDeliveryAddress;  
    [DataMember]  
    public Address DefaultBillingAddress;  
}  
 [DataContract]  
public class PremiumCustomer : Customer  
{  
    [DataMember]  
    public int AccountID;  
}  
  
 [DataContract]  
public class Address  
{  
    [DataMember]  
    public string Street;  
    [DataMember]  
    public string Zipcode;  
    [DataMember]  
    public string City;  
    [DataMember]  
    public string State;  
    [DataMember]  
    public string Country;  
}  
```  
  
### <a name="step-3-implement-the-wcf-service"></a>Etapa 3: implementar o serviço WCF  
 Em seguida, você deve implementar a classe do serviço WCF que implementa a interface definida na etapa anterior.  
  
```csharp  
public class CustomerService: ICustomerManager
{  
    public void StoreCustomer(Customer customer)  
    {  
        // write to a database  
    }  
    public Customer GetCustomer(string firstName, string lastName)  
    {  
        // read from a database  
    }  
}  
```  
  
### <a name="step-4-configure-the-service-and-the-client"></a>Etapa 4: configurar o serviço e o cliente  
 Para executar um serviço WCF, é necessário declarar um ponto de extremidade que expõe essa interface de serviço em uma URL específica usando uma associação do WCF específica. Uma associação especifica os detalhes de transporte, codificação e protocolo para a comunicação entre o servidor e os clientes. Geralmente, você adiciona associações ao arquivo de configuração do projeto de serviço (web.config). O exemplo a seguir mostra uma entrada de associação para o serviço de exemplo:  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Server.CustomerService">  
        <endpoint address="http://localhost:8083/CustomerManager"
                  binding="basicHttpBinding"  
                  contract="Shared.ICustomerManager" />  
      </service>  
    </services>  
  </system.serviceModel>  
</configuration>  
```  
  
 Em seguida, você precisa configurar o cliente de acordo com as informações de associação especificadas pelo serviço. Para fazer isso, adicione o seguinte ao arquivo de configuração (app.config) do aplicativo do cliente.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
      <endpoint name="customermanager"
                address="http://localhost:8083/CustomerManager"
                binding="basicHttpBinding"
                contract="Shared.ICustomerManager"/>  
    </client>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="step-5-run-the-service"></a>Etapa 5: executar o serviço  
 Por fim, você pode fazer auto-hospedagem em um aplicativo de console, adicionando as linhas a seguir ao aplicativo de serviço e iniciando o aplicativo. Para obter mais informações sobre outras maneiras de hospedar um aplicativo de serviço WCF, consulte [Serviços de hospedagem](../wcf/hosting-services.md).  
  
```csharp  
ServiceHost customerServiceHost = new ServiceHost(typeof(CustomerService));  
customerServiceHost.Open();  
```  
  
### <a name="step-6-call-the-service-from-the-client"></a>Etapa 6: chamar o serviço do cliente  
 Para chamar o serviço do cliente, você precisa criar uma fábrica de canais para o serviço e solicitar um canal, o que permitirá que você chame diretamente o método `GetCustomer`, diretamente do cliente. O canal implementa a interface do serviço e manipula a lógica subjacente de solicitação/resposta para você.  O valor retornado dessa chamada de método que é a cópia desserializada da resposta do serviço.  
  
```csharp  
ChannelFactory<ICustomerManager> factory =
     new ChannelFactory<ICustomerManager>("customermanager");  
ICustomerManager service = factory.CreateChannel();  
Customer customer = service.GetCustomer("Mary", "Smith");  
```  
  
## <a name="the-client-sends-a-by-value-object-to-the-server"></a>O cliente envia um objeto por valor para o servidor  
 Nesse cenário, o cliente envia um objeto para o servidor, por valor. Isso significa que o servidor receberá uma cópia desserializada do objeto.  O servidor pode chamar métodos de cópia e obter a garantia de que não há nenhum retorno de chamada no código do cliente. Conforme mencionado anteriormente, as trocas de dados de WCF normais são por valor.  Isso garante que chamar métodos em um desses objetos resultará apenas em execução local – não invocará código no cliente.  
  
 Esse cenário representa a chamada de método COM a seguir:  
  
```csharp  
public interface IRemoteService  
{  
    void SendObjectByValue(Customer customer);  
}  
```  
  
 Esse cenário usa o mesmo contrato de dados e interface de serviço, conforme mostrado no primeiro exemplo. Além disso, o cliente e o serviço serão configurados da mesma maneira. Neste exemplo, um canal é criado para enviar o objeto e executar da mesma maneira. No entanto, neste exemplo, você criará um cliente que chamará o serviço, passando um objeto por valor. O método de serviço que o cliente chamará no contrato de serviço é mostrado em negrito:  
  
```csharp  
[ServiceContract]  
public interface ICustomerManager  
{  
    [OperationContract]     void StoreCustomer(Customer customer);  
  
    [OperationContract]  
    Customer GetCustomer(string firstName, string lastName);  
}  
```  
  
### <a name="add-code-to-the-client-that-sends-a-by-value-object"></a>Adicionar código para o cliente que envia um objeto por valor  
 O código a seguir mostra como o cliente cria um novo objeto de cliente por valor, cria um canal para se comunicar com o serviço `ICustomerManager` e envia o objeto de cliente para ele.  
  
 O objeto de cliente será serializado e enviado para o serviço, em que ele será desserializado pelo serviço gerando uma nova cópia desse objeto.  Qualquer método que o serviço chamar nesse objeto será executado apenas localmente no servidor. É importante observar que esse código ilustra o envio de um tipo derivado (`PremiumCustomer`).  O contrato de serviço espera um objeto `Customer`, mas o contrato de dados de serviço usa o atributo [<xref:System.Runtime.Serialization.KnownTypeAttribute>] para indicar que `PremiumCustomer` também é permitido.  As tentativas do WCF de serializar ou desserializar qualquer outro tipo via interface de serviço falharão.  
  
```csharp  
PremiumCustomer customer = new PremiumCustomer();  
customer.Firstname = "John";  
customer.Lastname = "Doe";  
customer.DefaultBillingAddress = new Address();  
customer.DefaultBillingAddress.Street = "One Microsoft Way";  
customer.DefaultDeliveryAddress = customer.DefaultBillingAddress;  
customer.AccountID = 42;  
  
ChannelFactory<ICustomerManager> factory =  
   new ChannelFactory<ICustomerManager>("customermanager");  
ICustomerManager customerManager = factory.CreateChannel();  
customerManager.StoreCustomer(customer);  
```  
  
## <a name="the-service-returns-an-object-by-reference"></a>O serviço retorna um objeto por referência  
 Para este cenário, o aplicativo cliente faz uma chamada para o serviço remoto e o método retorna um objeto, que é passado por referência do serviço para o cliente.  
  
 Conforme mencionado anteriormente, os serviços WCF sempre retornam o objeto por valor.  No entanto, você pode obter um resultado semelhante usando a classe <xref:System.ServiceModel.EndpointAddress10>.  O <xref:System.ServiceModel.EndpointAddress10> é um objeto serializável por valor que pode ser usado pelo cliente para obter um objeto por referência de sessão no servidor.  
  
 O comportamento do objeto por referência no WCF mostrado nesse cenário é diferente do observado com DCOM.  No DCOM, o servidor pode retornar um objeto por referência para o cliente diretamente e o cliente pode chamar os métodos desse objeto, que são executados no servidor.  No entanto, no WCF, o objeto retornado é sempre por valor.  O cliente deve tomar esse objeto por valor representado por <xref:System.ServiceModel.EndpointAddress10> e usá-lo para criar seu próprio objeto de sessão por referência.  As chamadas de método do cliente no objeto de sessão são executadas no servidor. Em outras palavras, esse objeto por referência no WCF é um serviço WCF normal que está configurado para ser de sessão.  
  
 No WCF, uma sessão é uma maneira de correlacionar várias mensagens enviadas entre dois pontos de extremidade.  Isso significa que, depois que um cliente obtiver uma conexão para esse serviço, uma sessão será estabelecida entre o cliente e o servidor.  O cliente usará uma única instância exclusiva do objeto do lado do servidor para todas as interações dele nessa única sessão. Contratos WCF de sessão são semelhantes aos padrões de solicitação/resposta de rede voltados para conexão.  
  
 Esse cenário é representado pelo método DCOM a seguir.  
  
```csharp  
public interface IRemoteService  
{  
    IRemoteObject GetObjectByReference();  
}  
```  
  
### <a name="step-1-define-the-sessionful-wcf-service-interface-and-implementation"></a>Etapa 1: definir a interface do serviço WCF de sessão e implementação  
 Primeiro, defina uma interface de serviço WCF que contém o objeto de sessão.  
  
 Nesse código, o objeto de sessão é marcado com o atributo `ServiceContract`, que o identifica como uma interface de serviço WCF normal.  Além disso, a propriedade <xref:System.ServiceModel.ServiceContractAttribute.SessionMode%2A> é definida para indicar que ele será um serviço de sessão.  
  
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
  
 O código a seguir mostra a implementação do serviço.  
  
 O serviço é marcado com o atributo [ServiceBehavior] e a propriedade InstanceContextMode dele é definida como InstanceContextMode.PerSessions para indicar que uma instância exclusiva desse tipo deve ser criada para cada sessão.  
  
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
  
### <a name="step-2-define-the-wcf-factory-service-for-the-sessionful-object"></a>Etapa 2: definir o serviço de fábrica do WCF para o objeto de sessão  
 O serviço que cria o objeto de sessão deve ser definido e implementado. O código a seguir mostra como fazer isso. Esse código cria outro serviço WCF que retorna um objeto <xref:System.ServiceModel.EndpointAddress10>.  Essa é uma forma serializável de um ponto de extremidade que pode ser usada para criar o objeto de sessão.  
  
```csharp  
[ServiceContract]  
    public interface ISessionBoundFactory  
    {  
        [OperationContract]  
        EndpointAddress10 GetInstanceAddress();  
    }  
```  
  
 A seguir está a implementação deste serviço. Essa implementação mantém uma fábrica de canais única para criar objetos de sessão.  Quando `GetInstanceAddress` é chamado, ele cria um canal e cria um objeto <xref:System.ServiceModel.EndpointAddress10> que aponta para o endereço remoto associado a esse canal.   <xref:System.ServiceModel.EndpointAddress10> é um tipo de dados que pode ser retornado ao cliente por valor.
  
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
  
### <a name="step-3-configure-and-start-the-wcf-services"></a>Etapa 3: configurar e iniciar os serviços WCF  
 Para hospedar esses serviços, você precisará fazer as adições a seguir ao arquivo de configuração do servidor (web.config).  
  
1. Adicione uma seção `<client>` que descreve o ponto de extremidade para o objeto de sessão.  Nesse cenário, o servidor também atua como um cliente e deve ser configurado para permitir isso.  
  
2. Na seção `<services>`, declare pontos de extremidade de serviço para a o objeto de fábrica e de sessão.  Isso permite que o cliente se comunique com os pontos de extremidade de serviço, adquira o <xref:System.ServiceModel.EndpointAddress10> e crie o canal de sessão.  
  
 Este é um exemplo de arquivo de configuração com estas configurações:  
  
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
  
 Adicione as linhas a seguir a um aplicativo de console, para hospedar automaticamente o serviço e iniciar o aplicativo.  
  
```csharp  
ServiceHost factoryHost = new ServiceHost(typeof(SessionBoundFactory));  
factoryHost.Open();  
  
ServiceHost sessionBoundServiceHost = new ServiceHost(  
typeof(MySessionBoundObject));  
sessionBoundServiceHost.Open();  
```  
  
### <a name="step-4-configure-the-client-and-call-the-service"></a>Etapa 4: configurar o cliente e chamar o serviço  
 Configure o cliente para se comunicar com os serviços WCF, criando as seguintes entradas no arquivo de configuração de aplicativo do projeto (app.config).  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <client>  
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
  
 Para chamar o serviço, adicione o código ao cliente para fazer o seguinte:  
  
1. Crie um canal para o serviço `ISessionBoundFactory`.  
  
2. Use o canal para invocar o serviço `ISessionBoundFactory` e obter um objeto <xref:System.ServiceModel.EndpointAddress10>.  
  
3. Use o <xref:System.ServiceModel.EndpointAddress10> para criar um canal para obter um objeto de sessão.  
  
4. Chame os métodos `SetCurrentValue` e `GetCurrentValue` para demonstrar que ela permanece a mesma instância do objeto que é usada em várias chamadas.  
  
```csharp  
ChannelFactory<ISessionBoundFactory> factory =  
        new ChannelFactory<ISessionBoundFactory>("factory");  
  
ISessionBoundFactory sessionBoundFactory = factory.CreateChannel();  
  
EndpointAddress10 address = sessionBoundFactory.GetInstanceAddress();  
  
ChannelFactory<ISessionBoundObject> sessionBoundObjectFactory =  
    new ChannelFactory<ISessionBoundObject>(  
        new NetTcpBinding(),  
        address.ToEndpointAddress());  
  
ISessionBoundObject sessionBoundObject =  
        sessionBoundObjectFactory.CreateChannel();  
  
sessionBoundObject.SetCurrentValue("Hello");  
if (sessionBoundObject.GetCurrentValue() == "Hello")  
{  
    Console.WriteLine("Session-full instance management works as expected");  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Programação básica do WCF](../wcf/basic-wcf-programming.md)
- [Serviços de design e implantação](../wcf/designing-and-implementing-services.md)
- [Compilando clientes](../wcf/building-clients.md)
- [Serviços duplex](../wcf/feature-details/duplex-services.md)
