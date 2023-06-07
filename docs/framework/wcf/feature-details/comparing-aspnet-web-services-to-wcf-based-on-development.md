---
title: Comparando os serviços Web ASP.NET com o WCF baseado em desenvolvimento
ms.date: 03/30/2017
ms.assetid: f362d00e-ce82-484f-9d4f-27e579d5c320
ms.openlocfilehash: c6e83bb234751dc477776f0fa540ffa8688dc667
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84597586"
---
# <a name="comparing-aspnet-web-services-to-wcf-based-on-development"></a>Comparando os serviços Web ASP.NET com o WCF baseado em desenvolvimento

O Windows Communication Foundation (WCF) tem uma opção de modo de compatibilidade ASP.NET para permitir que os aplicativos WCF sejam programados e configurados como serviços Web ASP.NET e imitam seu comportamento. As seções a seguir comparam os serviços Web ASP.NET e o WCF com base no que é necessário para desenvolver aplicativos usando ambas as tecnologias.

## <a name="data-representation"></a>Representação de dados

O desenvolvimento de um serviço Web com o ASP.NET geralmente começa com a definição de quaisquer tipos de dados complexos que o serviço usará. O ASP.NET conta com o <xref:System.Xml.Serialization.XmlSerializer> para converter os dados representados por tipos do .NET Framework em XML para transmissão para ou de um serviço e para converter os dados recebidos como XML em objetos do .NET Framework. Definir os tipos de dados complexos que um serviço do ASP.NET usará requer a definição das classes do .NET Framework que o <xref:System.Xml.Serialization.XmlSerializer> pode serializar para e do XML. Essas classes podem ser gravadas manualmente, ou geradas a partir das definições dos tipos no Esquema XML usando o utilitário de linha de comando para suporte de tipos de dados/esquemas XML, o xsd.exe.

Esta é uma lista dos principais problemas que precisam ser conhecidos ao definir as classes do .NET Framework que o <xref:System.Xml.Serialization.XmlSerializer> pode serializar para e do XML:

- Somente as propriedades e os campos públicos dos objetos do .NET Framework são convertidos em XML.

- As instâncias de classes de coleção poderão ser serializadas em XML somente se as classes implementarem a interface <xref:System.Collections.IEnumerable> ou <xref:System.Collections.ICollection>.

- As classes que implementam a interface <xref:System.Collections.IDictionary>, como <xref:System.Collections.Hashtable>, não podem ser serializadas em XML.

- A maioria dos tipos de atributo do namespace <xref:System.Xml.Serialization> pode ser adicionada a uma classe do .NET Framework e a seus membros para controlar como as instâncias da classe serão representadas em XML.

O desenvolvimento de aplicativos WCF geralmente também começa com a definição de tipos complexos. O WCF pode ser feito para usar os mesmos tipos de .NET Framework que os serviços Web do ASP.NET.

O WCF <xref:System.Runtime.Serialization.DataContractAttribute> e <xref:System.Runtime.Serialization.DataMemberAttribute> pode ser adicionado a tipos de .NET Framework para indicar que as instâncias do tipo devem ser serializadas em XML e quais campos ou propriedades em particular do tipo devem ser serializados, conforme mostrado no código de exemplo a seguir.

```csharp
//Example One:
[DataContract]
public class LineItem
{
    [DataMember]
    public string ItemNumber;
    [DataMember]
    public decimal Quantity;
    [DataMember]
    public decimal UnitPrice;
}

//Example Two:
public class LineItem
{
    [DataMember]
    private string itemNumber;
    [DataMember]
    private decimal quantity;
    [DataMember]
    private decimal unitPrice;

    public string ItemNumber
    {
      get
      {
          return this.itemNumber;
      }

      set
      {
          this.itemNumber = value;
      }
    }

    public decimal Quantity
    {
        get
        {
            return this.quantity;
        }

        set
        {
            this.quantity = value;
        }
    }

    public decimal UnitPrice
    {
      get
      {
          return this.unitPrice;
      }

      set
      {
          this.unitPrice = value;
      }
    }
}

//Example Three:
public class LineItem
{
     private string itemNumber;
     private decimal quantity;
     private decimal unitPrice;

     [DataMember]
     public string ItemNumber
     {
       get
       {
          return this.itemNumber;
       }

       set
       {
           this.itemNumber = value;
       }
     }

     [DataMember]
     public decimal Quantity
     {
          get
          {
              return this.quantity;
          }

          set
          {
             this.quantity = value;
          }
     }

     [DataMember]
     public decimal UnitPrice
     {
          get
          {
              return this.unitPrice;
          }

          set
          {
              this.unitPrice = value;
          }
     }
}
```

O <xref:System.Runtime.Serialization.DataContractAttribute> significa que zero ou mais campos ou propriedades de um tipo serão serializados, enquanto <xref:System.Runtime.Serialization.DataMemberAttribute> indica que um campo ou propriedade serão serializados. O <xref:System.Runtime.Serialization.DataContractAttribute> pode ser aplicado a uma classe ou estrutura. O <xref:System.Runtime.Serialization.DataMemberAttribute> pode ser aplicado a um campo ou uma propriedade, e os campos e as propriedades aos quais o atributo é aplicado podem ser públicos ou privados. Instâncias de tipos que têm o <xref:System.Runtime.Serialization.DataContractAttribute> aplicado a eles são chamadas de contratos de dados no WCF. Elas são serializadas em XML por meio do <xref:System.Runtime.Serialization.DataContractSerializer>.

Esta é uma lista das diferenças importantes entre o uso do <xref:System.Runtime.Serialization.DataContractSerializer> e do <xref:System.Xml.Serialization.XmlSerializer> e os vários atributos do namespace <xref:System.Xml.Serialization>.

- O <xref:System.Xml.Serialization.XmlSerializer> e os atributos do namespace <xref:System.Xml.Serialization> são projetados para permitir que você mapeie os tipos do .NET Framework para qualquer tipo válido definido no Esquema XML e, portanto, oferecem um controle muito preciso sobre como um tipo é representado em XML. O <xref:System.Runtime.Serialization.DataContractSerializer>, o <xref:System.Runtime.Serialization.DataContractAttribute> e o <xref:System.Runtime.Serialization.DataMemberAttribute> fornecem muito pouco controle sobre como um tipo é representado em XML. Você pode especificar somente os namespaces e os nomes usados para representar o tipo e seus campos ou propriedades em XML, e a sequência na qual os campos e as propriedades aparecerão em XML:

  ```csharp
  [DataContract(
  Namespace="urn:Contoso:2006:January:29",
  Name="LineItem")]
  public class LineItem
  {
        [DataMember(Name="ItemNumber",IsRequired=true,Order=0)]
        public string itemNumber;
        [DataMember(Name="Quantity",IsRequired=false,Order = 1)]
        public decimal quantity;
        [DataMember(Name="Price",IsRequired=false,Order = 2)]
        public decimal unitPrice;
  }
  ```

  Tudo o mais sobre a estrutura do XML usado para representar o tipo do .NET é determinado pelo <xref:System.Runtime.Serialization.DataContractSerializer>.

- Não permitindo muito controle sobre como um tipo será representado em XML, o processo de serialização fica altamente previsível para o <xref:System.Runtime.Serialization.DataContractSerializer> e, assim, mais fácil para otimizar. Um benefício prático do design do <xref:System.Runtime.Serialization.DataContractSerializer> é o melhor desempenho, uma melhora de aproximadamente 10% no desempenho.

- Os atributos para uso com o <xref:System.Xml.Serialization.XmlSerializer> não indicam quais campos ou propriedades do tipo são serializados em XML, enquanto o <xref:System.Runtime.Serialization.DataMemberAttribute> para uso com o <xref:System.Runtime.Serialization.DataContractSerializer> mostra explicitamente quais campos ou propriedades são serializados. Portanto, os contratos de dados são contratos explícitos sobre a estrutura dos dados que um aplicativo enviará e receberá.

- O <xref:System.Xml.Serialization.XmlSerializer> só pode converter os membros públicos de um objeto .NET em XML, já o <xref:System.Runtime.Serialization.DataContractSerializer> pode converter os membros dos objetos em XML independentemente dos modificadores de acesso desses membros.

- Como resultado da capacidade de serializar os membros não públicos dos tipos em XML, o <xref:System.Runtime.Serialization.DataContractSerializer> tem menos restrições no tocante aos vários tipos .NET que ele pode serializar em XML. Especificamente, ele pode fazer a conversão em tipos XML como o <xref:System.Collections.Hashtable> que implementam a interface <xref:System.Collections.IDictionary>. O <xref:System.Runtime.Serialization.DataContractSerializer> tem muito mais probabilidade de poder serializar as instâncias de qualquer tipo .NET pré-existente em XML sem precisar modificar a definição do tipo ou desenvolver um wrapper para ele.

- Outra consequência da capacidade do <xref:System.Runtime.Serialization.DataContractSerializer> de acessar os membros não públicos de um tipo é que ele requer confiança total, enquanto o <xref:System.Xml.Serialization.XmlSerializer> não. A permissão de acesso de código de confiança total fornece acesso completo a todos os recursos em um computador que pode ser acessado usando as credenciais sob as quais o código está sendo executado. Essa opção deve ser usada com cuidado como um código totalmente confiável acessa todos os recursos em seu computador.

- O <xref:System.Runtime.Serialization.DataContractSerializer> oferece algum suporte para controle de versão:

  - O <xref:System.Runtime.Serialization.DataMemberAttribute> tem uma propriedade <xref:System.Runtime.Serialization.DataMemberAttribute.IsRequired%2A> que pode receber um valor false para membros adicionados às novas versões de um contrato de dados que não estavam presentes nas versões anteriores, permitindo assim que aplicativos com a versão mais recente do contrato possam processar versões anteriores.

  - Tendo um contrato de dados que implemente a interface <xref:System.Runtime.Serialization.IExtensibleDataObject>, uma pessoa pode permitir que o <xref:System.Runtime.Serialization.DataContractSerializer> passe os membros definidos nas versões mais recentes de um contrato de dados por meio de aplicativos com versões anteriores do contrato.

Independentemente de todas as diferenças, o XML em que o <xref:System.Xml.Serialization.XmlSerializer> serializa um tipo é, por padrão, semanticamente idêntico ao XML em que o <xref:System.Runtime.Serialization.DataContractSerializer> serializa um tipo, desde que o namespace do XML seja explicitamente definido. A classe a seguir, que tem atributos para uso com ambos os serializadores, é convertida em XML semanticamente idêntico pelo <xref:System.Xml.Serialization.XmlSerializer> e pelo <xref:System.Runtime.Serialization.DataContractAttribute> :

```csharp
[Serializable]
[XmlRoot(Namespace="urn:Contoso:2006:January:29")]
[DataContract(Namespace="urn:Contoso:2006:January:29")]
public class LineItem
{
     [DataMember]
     public string ItemNumber;
     [DataMember]
     public decimal Quantity;
     [DataMember]
     public decimal UnitPrice;
}
```

O Windows Software Development Kit (SDK) inclui uma ferramenta de linha de comando chamada [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Assim como a ferramenta xsd. exe usada com os serviços Web do ASP.NET, o SvcUtil. exe pode gerar definições de tipos de dados do .NET a partir do esquema XML. Os tipos serão contratos de dados se o <xref:System.Runtime.Serialization.DataContractSerializer> puder emitir o XML no formato definido pelo Esquema XML; caso contrário, eles serão destinados à serialização por meio do <xref:System.Xml.Serialization.XmlSerializer>. Svcutil. exe também pode gerar um esquema XML de contratos de dados usando sua `dataContractOnly` opção.

> [!NOTE]
> Embora os serviços Web do ASP.NET usem o <xref:System.Xml.Serialization.XmlSerializer> e o modo de compatibilidade do wcf ASP.net, os serviços WCF imitam o comportamento dos serviços Web ASP.net, a opção de compatibilidade ASP.net não restringe um ao uso do <xref:System.Xml.Serialization.XmlSerializer> . É possível ainda usar o <xref:System.Runtime.Serialization.DataContractSerializer> com os serviços em execução no modo de compatibilidade com o ASP.NET.

## <a name="service-development"></a>Desenvolvimento do serviço

Para desenvolver um serviço usando o ASP.NET, é comum adicionar o atributo <xref:System.Web.Services.WebService> a uma classe e o <xref:System.Web.Services.WebMethodAttribute> a qualquer método dessa classe que será uma operação do serviço:

```csharp
[WebService]
public class Service : T:System.Web.Services.WebService
{
    [WebMethod]
    public string Echo(string input)
    {
       return input;
    }
}
```

O ASP.NET 2.0 permite adicionar os atributos <xref:System.Web.Services.WebService> e <xref:System.Web.Services.WebMethodAttribute> a uma interface, e não a uma classe, e gravar uma classe para implementar a interface:

```csharp
[WebService]
public interface IEcho
{
    [WebMethod]
    string Echo(string input);
}

public class Service : IEcho
{

   public string Echo(string input)
   {
        return input;
    }
}
```

O uso dessa opção deve ser preferencial, pois a interface com o atributo <xref:System.Web.Services.WebService> constitui um contrato das operações executadas pelo serviço que podem ser reutilizadas com várias classes, que, por sua vez, podem implementar o mesmo contrato de diferentes maneiras.

Um serviço WCF é fornecido pela definição de um ou mais pontos de extremidade do WCF. Um ponto de extremidade é definido por um endereço, uma associação e um contrato de serviço. O endereço define em que local o serviço está localizado. A associação especifica como se comunicar com o serviço. O contrato de serviço define as operações que o serviço pode executar.

O contrato de serviço geralmente é definido primeiro, adicionando o <xref:System.ServiceModel.ServiceContractAttribute> e o <xref:System.ServiceModel.OperationContractAttribute> a uma interface:

```csharp
[ServiceContract]
public interface IEcho
{
     [OperationContract]
     string Echo(string input);
}
```

O <xref:System.ServiceModel.ServiceContractAttribute> especifica que a interface define um contrato de serviço do WCF e <xref:System.ServiceModel.OperationContractAttribute> indica que, se houver, os métodos da interface definem as operações do contrato de serviço.

Depois que um contrato de serviço for definido, ele será implementado em uma classe, permitindo que esta implemente a interface através da qual o contrato de serviço será definido:

```csharp
public class Service : IEcho
{
    public string Echo(string input)
    {
       return input;
    }
}
```

Uma classe que implementa um contrato de serviço é conhecida como um tipo de serviço no WCF.

A próxima etapa é vincular um endereço e uma associação a um tipo de serviço. Isso normalmente é feito em um arquivo de configuração, seja editando o arquivo diretamente ou usando um editor de configuração fornecido com o WCF. Aqui está um exemplo de um arquivo de configuração.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<configuration>
     <system.serviceModel>
      <services>
      <service name="Service ">
       <endpoint
        address="EchoService"
        binding="basicHttpBinding"
        contract="IEchoService "/>
      </service>
      </services>
     </system.serviceModel>
</configuration>
```

A associação especifica o conjunto de protocolos que será usado para estabelecer a comunicação com o aplicativo. A tabela a seguir lista as associações fornecidas pelo sistema que representam opções comuns.

|Nome|Finalidade|
|----------|-------------|
|BasicHttpBinding|Interoperabilidade com serviços e clientes Web que oferecem suporte ao WS-BasicProfile 1.1 e ao Basic Security Profile 1.0.|
|WSHttpBinding|Interoperabilidade com serviços e clientes Web que oferecem suporte aos protocolos WS-* sobre HTTP.|
|WSDualHttpBinding|Comunicação HTTP duplex através da qual o receptor de uma mensagem inicial não responde diretamente ao remetente inicial, mas pode transmitir qualquer número de respostas durante um período usando o protocolo HTTP em conformidade com os protocolos WS-*.|
|WSFederationBinding|Comunicação HTTP, em que o acesso aos recursos de um serviço pode ser controlado com base nas credenciais emitidas por um provedor de credenciais explicitamente identificado.|
|NetTcpBinding|Comunicação segura, confiável e de alto desempenho entre entidades de software do WCF em uma rede.|
|NetNamedPipeBinding|Comunicação segura, confiável e de alto desempenho entre entidades de software do WCF no mesmo computador.|
|NetMsmqBinding|Comunicação entre entidades de software WCF usando o MSMQ.|
|MsmqIntegrationBinding|Comunicação entre uma entidade de software WCF e outra entidade de software usando o MSMQ.|
|NetPeerTcpBinding|Comunicação entre entidades de software WCF usando a rede ponto a ponto do Windows.|

A associação fornecida pelo sistema, <xref:System.ServiceModel.BasicHttpBinding>, incorpora o conjunto de protocolos com suporte nos serviços Web ASP.NET.

Associações personalizadas para aplicativos WCF são facilmente definidas como coleções das classes de elementos de associação que o WCF usa para implementar protocolos individuais. Novos elementos de associação podem ser gravados para representar protocolos adicionais.

O comportamento interno dos tipos de serviço pode ser ajustado através das propriedades de uma família de classes denominada comportamentos. Aqui, a classe <xref:System.ServiceModel.ServiceBehaviorAttribute> é usada para especificar que o tipo de serviço será multi-threaded.

```csharp
[ServiceBehavior(ConcurrencyMode=ConcurrencyMode.Multiple)]
public class DerivativesCalculatorServiceType: IDerivativesCalculator
```

Alguns comportamentos, como o <xref:System.ServiceModel.ServiceBehaviorAttribute>, são atributos. Outros, os que têm propriedades que os administradores talvez queiram definir, podem ser modificados na configuração de um aplicativo.

Nos tipos de serviço de programação, a classe <xref:System.ServiceModel.OperationContext> é usada frequentemente. Sua propriedade estática <xref:System.ServiceModel.OperationContext.Current%2A> fornece acesso a informações sobre o contexto em que uma operação está sendo executada. O <xref:System.ServiceModel.OperationContext> é semelhante às classes <xref:System.Web.HttpContext> e <xref:System.EnterpriseServices.ContextUtil>.

## <a name="hosting"></a>Hosting

Os serviços Web ASP.NET são compilados em um assembly de biblioteca de classes. Um arquivo denominado arquivo de serviço é fornecido, com a extensão .asmx, contendo uma diretiva `@ WebService` que identifica a classe que contém o código do serviço e o assembly em que ele está localizado.

`<%@ WebService Language="C#" Class="Service,ServiceAssembly" %>`

O arquivo de serviço é copiado para um aplicativo ASP.NET nos Serviços de Informações da Internet (IIS), e o assembly é copiado para o subdiretório \bin dessa raiz de aplicativo. O aplicativo pode, então, ser acessado através do URL do arquivo de serviço na raiz do aplicativo.

Os serviços WCF podem ser prontamente hospedados no IIS 5,1 ou 6,0, o WAS (serviço de ativação de processos do Windows) fornecido como parte do IIS 7,0 e dentro de qualquer aplicativo .NET. Para hospedar um serviço no IIS 5.1 ou 6.0, o serviço deve usar o HTTP como protocolo de transporte de comunicação.

Para hospedar um serviço no IIS 5.1 ou 6.0, ou no WAS, siga estas etapas:

1. Compile o tipo de serviço em um assembly de biblioteca de classes.

2. Crie um arquivo de serviço com uma extensão .svc com uma diretiva `@ ServiceHost` para identificar o tipo de serviço:

    `<%@ServiceHost language="c#" Service="MyService" %>`

3. Copie o arquivo de serviço para um diretório virtual e o assembly para o subdiretório \bin desse diretório virtual.

4. Copie o arquivo de configuração para o diretório virtual e atribua o nome Web.config a ele.

O aplicativo poderá, então, ser acessado através do URL do arquivo de serviço na raiz do aplicativo.

Para hospedar um serviço WCF em um aplicativo .NET, compile o tipo de serviço em um assembly de biblioteca de classes referenciado pelo aplicativo e programe o aplicativo para hospedar o serviço usando a <xref:System.ServiceModel.ServiceHost> classe. Este é um exemplo da programação básica necessária:

```csharp
string httpBaseAddress = "http://www.contoso.com:8000/";
string tcpBaseAddress = "net.tcp://www.contoso.com:8080/";

Uri httpBaseAddressUri = new Uri(httpBaseAddress);
Uri tcpBaseAddressUri = new Uri(tcpBaseAddress);

Uri[] baseAddresses = new Uri[] {
 httpBaseAddressUri,
 tcpBaseAddressUri};

using(ServiceHost host = new ServiceHost(
typeof(Service), //"Service" is the name of the service type baseAddresses))
{
     host.Open();

     […] //Wait to receive messages
     host.Close();
}
```

Este exemplo mostra como os endereços de um ou mais protocolos de transporte são especificados na construção de um <xref:System.ServiceModel.ServiceHost>. Esses endereços são chamados de endereços básicos.

O endereço fornecido para qualquer ponto de extremidade de um serviço WCF é um endereço relativo a um endereço base do host do ponto de extremidade. O host pode ter um endereço básico para cada protocolo de transporte de comunicação. Na configuração de exemplo do arquivo de configuração anterior, o <xref:System.ServiceModel.BasicHttpBinding> selecionado para o ponto de extremidade usa o HTTP como protocolo de transporte; portanto, o endereço do ponto de extremidade, `EchoService`, é relativo ao endereço básico HTTP do host. No caso do host no exemplo anterior, o endereço base HTTP é `http://www.contoso.com:8000/` . Para um serviço hospedado no IIS ou no WAS, o endereço básico é a URL do arquivo do serviço.

Somente os serviços hospedados no IIS ou WAS, e que são configurados com HTTP como o protocolo de transporte exclusivamente, podem ser feitos para usar a opção modo de compatibilidade do WCF ASP.NET. A ativação dessa opção exige as seguintes etapas.

1. O programador deve adicionar o atributo <xref:System.ServiceModel.Activation.AspNetCompatibilityRequirementsAttribute> ao tipo de serviço e especificar se o modo de compatibilidade com o ASP.NET é permitido ou necessário.

    ```csharp
    [System.ServiceModel.Activation.AspNetCompatibilityRequirements(
          RequirementsMode=AspNetCompatibilityRequirementsMode.Require)]
    public class DerivativesCalculatorServiceType: IDerivativesCalculator
    ```

2. O administrador deve configurar o aplicativo para usar o modo de compatibilidade com o ASP.NET.

    ```xml
    <configuration>
         <system.serviceModel>
          <services>
          […]
          </services>
          <serviceHostingEnvironment aspNetCompatibilityEnabled="true"/>
        </system.serviceModel>
    </configuration>
    ```

    Os aplicativos WCF também podem ser configurados para usar. asmx como a extensão para seus arquivos de serviço em vez de. svc.

    ```xml
    <system.web>
         <compilation>
          <compilation debug="true">
          <buildProviders>
           <remove extension=".asmx"/>
           <add extension=".asmx"
            type="System.ServiceModel.ServiceBuildProvider,
            System.ServiceModel,
            Version=3.0.0.0,
            Culture=neutral,
            PublicKeyToken=b77a5c561934e089" />
          </buildProviders>
          </compilation>
         </compilation>
    </system.web>
    ```

    Essa opção pode evitar que você precise modificar clientes configurados para usar as URLs de arquivos do serviço. asmx ao modificar um serviço para usar o WCF.

## <a name="client-development"></a>Desenvolvimento do cliente

Os clientes para serviços Web ASP.NET são gerados usando a ferramenta de linha de comando, WSDL.exe, que fornece o URL do arquivo .asmx como entrada. A ferramenta correspondente fornecida pelo WCF é a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Ele gera um módulo de código com a definição do contrato de serviço e a definição de uma classe de cliente WCF. Ela gera também um arquivo de configuração com o endereço e a associação do serviço.

Ao programar o cliente de um serviço remoto, geralmente é aconselhável fazer isso de acordo com o padrão assíncrono. O código gerado pela ferramenta WSDL.exe sempre fornece um modelo síncrono e um modelo assíncrono, por padrão. O código gerado pela [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) pode fornecer qualquer um dos padrões. Por padrão, ele é projetado para o modelo síncrono. Se a ferramenta for executada com a opção `/async`, o código gerado será projetado para o modelo assíncrono.

Não há nenhuma garantia de que os nomes nas classes de cliente do WCF geradas pelo ASP. Por padrão, a ferramenta WSDL. exe da rede corresponde aos nomes nas classes de cliente do WCF geradas pela ferramenta svcutil. exe. Em particular, os nomes das propriedades das classes que precisam ser serializadas por meio do <xref:System.Xml.Serialization.XmlSerializer> recebem, por padrão, o sufixo Property no código gerado pela ferramenta Svcutil.exe, o que não acontece com a ferramenta WSDL.exe.

## <a name="message-representation"></a>Representação da mensagem

Os cabeçalhos das mensagens SOAP enviadas e recebidas por serviços Web ASP.NET podem ser personalizados. Uma classe é derivada do <xref:System.Web.Services.Protocols.SoapHeader> para definir a estrutura do cabeçalho e, em seguida, o <xref:System.Web.Services.Protocols.SoapHeaderAttribute> é usado para indicar a presença do cabeçalho.

```csharp
public class SomeProtocol : SoapHeader
{
     public long CurrentValue;
     public long Total;
}

[WebService]
public interface IEcho
{
     SomeProtocol ProtocolHeader
     {
      get;
     set;
     }

     [WebMethod]
     [SoapHeader("ProtocolHeader")]
     string PlaceOrders(PurchaseOrderType order);
}

public class Service: WebService, IEcho
{
     private SomeProtocol protocolHeader;

     public SomeProtocol ProtocolHeader
     {
         get
         {
              return this.protocolHeader;
         }

         set
         {
              this.protocolHeader = value;
         }
     }

     string PlaceOrders(PurchaseOrderType order)
     {
         long currentValue = this.protocolHeader.CurrentValue;
     }
}
```

O WCF fornece os atributos, <xref:System.ServiceModel.MessageContractAttribute> , <xref:System.ServiceModel.MessageHeaderAttribute> e <xref:System.ServiceModel.MessageBodyMemberAttribute> para descrever a estrutura das mensagens SOAP enviadas e recebidas por um serviço.

```csharp
[DataContract]
public class SomeProtocol
{
     [DataMember]
     public long CurrentValue;
     [DataMember]
     public long Total;
}

[DataContract]
public class Item
{
     [DataMember]
     public string ItemNumber;
     [DataMember]
     public decimal Quantity;
     [DataMember]
     public decimal UnitPrice;
}

[MessageContract]
public class ItemMessage
{
     [MessageHeader]
     public SomeProtocol ProtocolHeader;
     [MessageBody]
     public Item Content;
}

[ServiceContract]
public interface IItemService
{
     [OperationContract]
     public void DeliverItem(ItemMessage itemMessage);
}
```

Essa sintaxe produz uma representação explícita da estrutura das mensagens, enquanto a estrutura das mensagens está implícita no código de um serviço Web ASP.NET. Além disso, na sintaxe ASP.NET, os cabeçalhos de mensagens são representados como propriedades do serviço, como a `ProtocolHeader` propriedade no exemplo anterior, enquanto na sintaxe do WCF, eles são representados com mais precisão como propriedades de mensagens. Além disso, o WCF permite que os cabeçalhos de mensagens sejam adicionados à configuração de pontos de extremidade.

```xml
<service name="Service ">
     <endpoint
      address="EchoService"
      binding="basicHttpBinding"
      contract="IEchoService ">
      <headers>
      <dsig:X509Certificate
       xmlns:dsig="http://www.w3.org/2000/09/xmldsig#">
       ...
      </dsig:X509Certificate>
      </headers>
     </endpoint>
</service>
```

Essa opção permite que você evite qualquer referência aos cabeçalhos de protocolo infraestruturais no código de um cliente ou serviço: os cabeçalhos são adicionados às mensagens devido à forma como o ponto de extremidade é configurado.

## <a name="service-description"></a>Descrição do Serviço

A emissão de uma solicitação HTTP GET para o arquivo .asmx de um serviço Web ASP.NET com a consulta WSDL faz com que o ASP.NET gere o WSDL para descrever o serviço. Ela retorna esse WSDL como a resposta à solicitação.

O ASP.NET 2.0 possibilitou a validação da compatibilidade de um serviço com o Basic Profile 1.1 da Web Services-Interoperability Organization (WS-I) e a inserção de uma declaração de que o serviço é compatível em seu WSDL. Isso é feito por meio dos parâmetros `ConformsTo` e `EmitConformanceClaims` do atributo <xref:System.Web.Services.WebServiceBindingAttribute>.

```csharp
[WebService(Namespace = "http://tempuri.org/")]
[WebServiceBinding(
     ConformsTo = WsiProfiles.BasicProfile1_1,
     EmitConformanceClaims=true)]
public interface IEcho
```

O WSDL que o ASP.NET gera para um serviço pode ser personalizado. As personalizações são feitas através da criação de uma classe do <xref:System.Web.Services.Description.ServiceDescriptionFormatExtension> para adicionar itens ao WSDL.

Emitir uma solicitação HTTP GET com o WSDL de consulta para o arquivo. svc de um serviço WCF com um ponto de extremidade HTTP hospedado no IIS 51, 6,0 ou WAS faz com que o WCF responda com o WSDL para descrever o serviço. A emissão de uma solicitação HTTP GET com a consulta WSDL para o endereço básico HTTP de um serviço hospedado em um aplicativo .NET terá o mesmo efeito se httpGetEnabled for definido como true.

No entanto, o WCF também responde a solicitações WS-MetadataExchange com WSDL que ele gera para descrever um serviço. Os Serviços Web ASP.NET não têm suporte interno para solicitações WS-MetadataExchange.

O WSDL gerado pelo WCF pode ser amplamente personalizado. A classe <xref:System.ServiceModel.Description.ServiceMetadataBehavior> fornece alguns recursos para a personalização do WSDL. O WCF também pode ser configurado para não gerar WSDL, mas sim usar um arquivo WSDL estático em uma determinada URL.

```xml
<behaviors>
     <behavior name="DescriptionBehavior">
     <metadataPublishing
      enableMetadataExchange="true"
      enableGetWsdl="true"
      enableHelpPage="true"
      metadataLocation=
      "http://localhost/DerivativesCalculatorService/Service.WSDL"/>
     </behavior>
</behaviors>
```

## <a name="exception-handling"></a>Tratamento de Exceção

Nos serviços Web ASP.NET, as exceções não tratadas são retornadas aos clientes como falhas SOAP. Você também pode lançar explicitamente as instâncias da classe <xref:System.Web.Services.Protocols.SoapException> e ter mais controle sobre o conteúdo da falha SOAP que é passada para o cliente.

Nos serviços WCF, as exceções sem tratamento não são retornadas aos clientes como falhas de SOAP para evitar que informações confidenciais sejam inadvertidamente expostas por meio das exceções. Há uma configuração que permite que as exceções não tratadas sejam retornadas aos clientes para fins de depuração.

Para retornar falhas SOAP aos clientes, você pode lançar instâncias do tipo genérico, <xref:System.ServiceModel.FaultException%601>, usando o tipo de contrato de dados como tipo genérico. Você também pode adicionar atributos <xref:System.ServiceModel.FaultContractAttribute> às operações para especificar as falhas que uma operação pode produzir.

```csharp
[DataContract]
public class MathFault
{
     [DataMember]
     public string operation;
     [DataMember]
     public string problemType;
}

[ServiceContract]
public interface ICalculator
{
     [OperationContract]
     [FaultContract(typeof(MathFault))]
     int Divide(int n1, int n2);
}
```

Isso fará com que as falhas possíveis sejam anunciadas no WSDL do serviço, permitindo que os programadores de cliente antevejam quais falhas podem resultar de uma operação e gravem as instruções catch apropriadas.

```csharp
try
{
     result = client.Divide(value1, value2);
}
catch (FaultException<MathFault> e)
{
 Console.WriteLine("FaultException<MathFault>: Math fault while doing "
  + e.Detail.operation
  + ". Problem: "
  + e.Detail.problemType);
}
```

## <a name="state-management"></a>Gerenciamento do estado

A classe usada para implementar um serviço Web ASP.NET pode ser derivada do <xref:System.Web.Services.WebService>.

```csharp
public class Service : WebService, IEcho
{

 public string Echo(string input)
 {
  return input;
 }
}
```

Nesse caso, a classe pode ser programada para usar a propriedade Context da classe base <xref:System.Web.Services.WebService> para acessar um objeto <xref:System.Web.HttpContext>. O objeto <xref:System.Web.HttpContext> pode ser usado para atualizar e recuperar informações de estado do aplicativo por meio da propriedade Application, e pode ser usado para atualizar e recuperar informações de estado de sessão por meio da propriedade Session.

O ASP.NET fornece um controle considerável sobre o local onde as informações de estado de sessão acessadas por meio da propriedade Session do <xref:System.Web.HttpContext> são armazenadas. Elas podem ser armazenadas em cookies, em um banco de dados, na memória do servidor atual ou na memória de um servidor designado. A opção é feita no arquivo de configuração do serviço.

O WCF fornece objetos extensíveis para o gerenciamento de estado. Os objetos extensíveis são objetos que implementam o <xref:System.ServiceModel.IExtensibleObject%601>. Os objetos extensíveis mais importantes são <xref:System.ServiceModel.ServiceHostBase> e <xref:System.ServiceModel.InstanceContext>. O `ServiceHostBase` permite que você mantenha o estado que todas as instâncias de todos os tipos de serviço no mesmo host podem acessar, enquanto o `InstanceContext` permite que você mantenha o estado que pode ser acessado por qualquer código em execução na mesma instância de um tipo de serviço.

Aqui, o tipo de serviço, `TradingSystem` , tem um <xref:System.ServiceModel.ServiceBehaviorAttribute> que especifica que todas as chamadas da mesma instância de cliente WCF são roteadas para a mesma instância do tipo de serviço.

```csharp
[ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]
public class TradingSystem: ITradingService
```

A classe, `DealData`, define o estado que pode ser acessado por qualquer código em execução na mesma instância de um tipo de serviço.

```csharp
internal class DealData: IExtension<InstanceContext>
{
 public string DealIdentifier = null;
 public Trade[] Trades = null;
}
```

No código do tipo de serviço que implementa uma das operações do contrato de serviço, um objeto de estado `DealData` é adicionado ao estado da instância atual do tipo de serviço.

```csharp
string ITradingService.BeginDeal()
{
 string dealIdentifier = Guid.NewGuid().ToString();
 DealData state = new DealData(dealIdentifier);
 OperationContext.Current.InstanceContext.Extensions.Add(state);
 return dealIdentifier;
}
```

O objeto de estado pode, então, ser recuperado e modificado pelo código que implementa outras operações do contrato de serviço.

```csharp
void ITradingService.AddTrade(Trade trade)
{
 DealData dealData =  OperationContext.Current.InstanceContext.Extensions.Find<DealData>();
 dealData.AddTrade(trade);
}
```

Enquanto o ASP.NET fornece controle sobre onde as informações de estado na <xref:System.Web.HttpContext> classe são realmente armazenadas, o WCF, pelo menos em sua versão inicial, não fornece nenhum controle sobre onde os objetos extensíveis são armazenados. Isso constitui a melhor razão para selecionar o modo de compatibilidade ASP.NET para um serviço WCF. Se o gerenciamento de estado configurável for obrigatório, a opção pelo modo de compatibilidade com o ASP.NET permite que você use os recursos da classe <xref:System.Web.HttpContext> exatamente como eles são usados no ASP.NET e configure onde as informações de estado gerenciadas por meio da classe <xref:System.Web.HttpContext> são armazenadas.

## <a name="security"></a>Segurança

As opções para proteger os serviços Web ASP.NET são as mesmas usadas para proteger qualquer aplicativo do IIS. Como os aplicativos WCF podem ser hospedados não apenas no IIS, mas também em qualquer executável .NET, as opções para proteger aplicativos WCF devem ser tornadas independentes das instalações do IIS. No entanto, os recursos fornecidos para os serviços Web do ASP.NET também estão disponíveis para serviços WCF em execução no modo de compatibilidade do ASP.NET.

### <a name="security-authentication"></a>Segurança: autenticação

O IIS fornece recursos para controlar o acesso aos aplicativos através dos quais você pode selecionar o acesso anônimo ou vários modos de autenticação: Autenticação do Windows, Autenticação Digest, Autenticação Básica e Autenticação do .NET Passport. A opção Autenticação do Windows pode ser usada para controlar o acesso aos serviços Web ASP.NET. No entanto, quando os aplicativos WCF são hospedados no IIS, o IIS deve ser configurado para permitir o acesso anônimo ao aplicativo, para que a autenticação possa ser gerenciada pelo próprio WCF, que dá suporte à autenticação do Windows entre várias outras opções. As outras opções, que são internas, incluem tokens de nome de usuário, certificados X.509, tokens SAML e cartão CardSpace, mas os mecanismos de autenticação personalizados também podem ser definidos.

### <a name="security-impersonation"></a>Segurança: representação

O ASP.NET fornece um elemento de identidade através do qual um serviço Web ASP.NET pode ser feito para representar um usuário específico ou qualquer credencial do usuário fornecida com a solicitação atual. Esse elemento pode ser usado para configurar a representação em aplicativos WCF em execução no modo de compatibilidade ASP.NET.

O sistema de configuração do WCF fornece seu próprio elemento Identity para designar um usuário específico para representar. Além disso, os clientes e serviços WCF podem ser configurados independentemente para representação. Os clientes podem ser configurados para representar o usuário atual quando eles transmitirem solicitações.

```xml
<behaviors>
     <behavior name="DerivativesCalculatorClientBehavior">
      <clientCredentials>
      <windows allowedImpersonationLevel="Impersonation"/>
      </clientCredentials>
     </behavior>
</behaviors>
```

As operações de serviço podem ser configuradas para representar quaisquer credenciais do usuário fornecidas com a solicitação atual.

```csharp
[OperationBehavior(Impersonation = ImpersonationOption.Required)]
public void Receive(Message input)
```

### <a name="security-authorization-using-access-control-lists"></a>Segurança: autorização por meio das listas de controle de acesso

As listas de controle de acesso (ACLs) podem ser usadas para restringir o acesso a arquivos .asmx. No entanto, as ACLs em arquivos WCF. svc são ignoradas, exceto no modo de compatibilidade ASP.NET.

### <a name="security-role-based-authorization"></a>Segurança: autorização baseada em função

A opção Autenticação do Windows do IIS pode ser usada em conjunto com o elemento de autorização fornecido pela linguagem de configuração do ASP.NET, para facilitar a autorização baseada em função dos serviços Web ASP.NET com base nos grupos do Windows aos quais os usuários são atribuídos. O ASP.NET 2.0 introduziu um mecanismo de autorização baseada em função mais genérico: os provedores de função.

Os provedores de função são classes que implementam uma interface básica para obter informações sobre as funções às quais um usuário é atribuído, mas cada provedor de função sabe como recuperar essas informações de uma fonte diferente. O ASP.NET 2.0 fornece um provedor de função que pode recuperar as atribuições de função de um banco de dados do Microsoft SQL Server e outro que pode recuperar as atribuições de função do Gerenciador de Autorização do Windows Server 2003.

O mecanismo do provedor de função pode ser realmente usado independentemente do ASP.NET em qualquer aplicativo .NET, incluindo um aplicativo WCF. O exemplo de configuração a seguir para um aplicativo WCF mostra como o uso de um provedor de função ASP.NET é uma opção selecionada por meio do <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> .

```xml
<system.serviceModel>
     <services>
         <service name="Service.ResourceAccessServiceType"
             behaviorConfiguration="ServiceBehavior">
             <endpoint
              address="ResourceAccessService"
              binding="wsHttpBinding"
              contract="Service.IResourceAccessContract"/>
         </service>
     </services>
     <behaviors>
       <behavior name="ServiceBehavior">
       <serviceAuthorization principalPermissionMode="UseAspNetRoles"/>
      </behavior>
     </behaviors>
</system.serviceModel>
```

### <a name="security-claims-based-authorization"></a>Segurança: autorização baseada em declaração

Uma das inovações mais importantes do WCF é o suporte completo para autorizar o acesso a recursos protegidos com base em declarações. As declarações consistem em um tipo, um direito e um valor, uma carteira de habilitação, por exemplo. Ela cria um conjunto de declarações sobre o portador, uma delas é a data de nascimento do portador. O tipo dessa declaração é a data de nascimento, enquanto o valor da declaração é a data de nascimento do motorista. O direito que uma declaração confere ao portador especifica o que o portador pode fazer com o valor a declaração. No caso da declaração da data de nascimento do motorista, o direito é a posse: o motorista possui a data de nascimento mas não pode, por exemplo, alterá-la. Autorização baseada em declaração inclui a autorização baseada em função, pois as funções são um tipo de declaração.

A autorização baseada em declarações é realizada através da comparação de um conjunto de declarações com os requisitos de acesso da operação e, dependendo do resultado dessa comparação, conceder ou negar o acesso à operação. No WCF, você pode especificar uma classe a ser usada para executar a autorização baseada em declarações, mais uma vez atribuindo um valor à `ServiceAuthorizationManager` propriedade de <xref:System.ServiceModel.Description.ServiceAuthorizationBehavior> .

```xml
<behaviors>
     <behavior name='ServiceBehavior'>
     <serviceAuthorization
     serviceAuthorizationManagerType=
                   'Service.AccessChecker, Service' />
     </behavior>
</behaviors>
```

As classes usadas para executar a autorização baseada em declarações devem derivar do <xref:System.ServiceModel.ServiceAuthorizationManager>, que tem apenas um método substituição, `AccessCheck()`. O WCF chama esse método sempre que uma operação do serviço é invocada e fornece um <xref:System.ServiceModel.OperationContext> objeto, que tem as declarações para o usuário em sua `ServiceSecurityContext.AuthorizationContext` propriedade. O WCF realiza o trabalho de montagem de declarações sobre o usuário de qualquer token de segurança que o usuário forneceu para autenticação, o que deixa a tarefa de avaliar se essas declarações são suficientes para a operação em questão.

Esse WCF monta automaticamente declarações de qualquer tipo de token de segurança é uma inovação altamente significativa, pois torna o código para autorização com base nas declarações totalmente independentes do mecanismo de autenticação. Por outro lado, a autorização que usa ACLs ou funções no ASP.NET está estreitamente associada à autenticação do Windows.

### <a name="security-confidentiality"></a>Segurança: confidencialidade

A confidencialidade das mensagens trocadas com os serviços Web ASP.NET pode ser garantida no nível do transporte através da configuração do aplicativo no IIS para usar o protocolo HTTPS. O mesmo pode ser feito para aplicativos WCF hospedados no IIS. No entanto, os aplicativos WCF hospedados fora do IIS também podem ser configurados para usar um protocolo de transporte seguro. Mais importante, os aplicativos WCF também podem ser configurados para proteger as mensagens antes de serem transportadas, usando o protocolo WS-Security. A proteção do corpo de uma mensagem usando o protocolo WS-Security permite que ela seja transmitida confidencialmente através de intermediários antes de chegar ao seu destino final.

## <a name="globalization"></a>Globalização

O idioma da configuração do ASP.NET permite que você especifique a cultura dos serviços individuais. O WCF não oferece suporte a essa definição de configuração, exceto no modo de compatibilidade ASP.NET. Para localizar um serviço WCF que não usa o modo de compatibilidade ASP.NET, compile o tipo de serviço em assemblies específicos de cultura e tenha pontos de extremidade específicos de cultura separados para cada assembly específico de cultura.

## <a name="see-also"></a>Consulte também

- [Comparando serviços Web do ASP.NET ao WCF com base na finalidade e nos padrões usados](comparing-aspnet-web-services-to-wcf-based-on-purpose-and-standards-used.md)
