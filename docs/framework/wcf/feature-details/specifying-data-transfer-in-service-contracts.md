---
title: Especificando transferência de dados em contratos de serviço
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- service contracts [WCF], data transfer
ms.assetid: 7c5a26c8-89c9-4bcb-a4bc-7131e6d01f0c
ms.openlocfilehash: ae05fb5ea0ee4962d9889e2a29399a3913a0d9d5
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600290"
---
# <a name="specifying-data-transfer-in-service-contracts"></a>Especificando transferência de dados em contratos de serviço
O Windows Communication Foundation (WCF) pode ser considerado uma infraestrutura de mensagens. As operações de serviço podem receber mensagens, processá-las e enviar mensagens. As mensagens são descritas usando contratos de operação. Por exemplo, considere o contrato a seguir.  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(string fromCity, string toCity);  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
  
    <OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
End Interface  
```  
  
 Aqui, a `GetAirfare` operação aceita uma mensagem com informações sobre `fromCity` e e `toCity` , em seguida, retorna uma mensagem que contém um número.  
  
 Este tópico explica as várias maneiras em que um contrato de operação pode descrever mensagens.  
  
## <a name="describing-messages-by-using-parameters"></a>Descrevendo mensagens usando parâmetros  
 A maneira mais simples de descrever uma mensagem é usar uma lista de parâmetros e o valor de retorno. No exemplo anterior, os `fromCity` parâmetros de `toCity` cadeia de caracteres e foram usados para descrever a mensagem de solicitação e o valor de retorno flutuante foi usado para descrever a mensagem de resposta. Se o valor de retorno sozinho não for suficiente para descrever uma mensagem de resposta, os parâmetros de saída poderão ser usados. Por exemplo, a operação a seguir tem `fromCity` e `toCity` em sua mensagem de solicitação e um número junto com uma moeda em sua mensagem de resposta:  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, out string currency);  
```  
  
```vb  
<OperationContract()>  
    Function GetAirfare(fromCity As String, toCity As String) As Double  
```  
  
 Além disso, você pode usar parâmetros de referência para fazer com que um parâmetro faça parte da solicitação e da mensagem de resposta. Os parâmetros devem ser de tipos que podem ser serializados (convertidos em XML). Por padrão, o WCF usa um componente chamado <xref:System.Runtime.Serialization.DataContractSerializer> classe para executar essa conversão. Há suporte para a maioria dos tipos primitivos (como `int` ,, `string` `float` e `DateTime` .). Os tipos definidos pelo usuário normalmente devem ter um contrato de dados. Para obter mais informações, consulte [usando contratos de dados](using-data-contracts.md).  
  
```csharp
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
  
    [DataContract]  
    public class Itinerary  
    {  
        [DataMember]  
        public string fromCity;  
        [DataMember]  
        public string toCity;  
   }  
}  
```  
  
```vb  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
    <DataContract()>  
    Class Itinerary  
  
        <DataMember()>  
        Public fromCity As String  
        <DataMember()>  
        Public toCity As String  
    End Class  
End Interface  
```  
  
 Ocasionalmente, o `DataContractSerializer` não é adequado para serializar seus tipos. O WCF dá suporte a um mecanismo de serialização alternativo, o <xref:System.Xml.Serialization.XmlSerializer> , que você também pode usar para serializar parâmetros. O <xref:System.Xml.Serialization.XmlSerializer> permite que você use mais controle sobre o XML resultante usando atributos como o `XmlAttributeAttribute` . Para alternar para o usando o <xref:System.Xml.Serialization.XmlSerializer> para uma operação específica ou para todo o serviço, aplique o <xref:System.ServiceModel.XmlSerializerFormatAttribute> atributo a uma operação ou um serviço. Por exemplo:  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    [XmlSerializerFormat]  
    float GetAirfare(Itinerary itinerary, DateTime date);  
}  
public class Itinerary  
{  
    public string fromCity;  
    public string toCity;  
    [XmlAttribute]  
    public bool isFirstClass;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    <XmlSerializerFormat>  
    GetAirfare(itinerary as Itinerary, date as DateTime) as Double  
  
End Interface  
  
Class Itinerary  
  
    Public fromCity As String  
    Public toCity As String  
    <XmlSerializerFormat()>  
    Public isFirstClass As Boolean  
End Class  
```  
  
 Para obter mais informações, consulte [usando a classe XmlSerializer](using-the-xmlserializer-class.md). Lembre-se de que alternar manualmente para o <xref:System.Xml.Serialization.XmlSerializer> conforme mostrado aqui não é recomendado, a menos que você tenha motivos específicos para fazer isso, conforme detalhado no tópico.  
  
 Para isolar os nomes de parâmetro do .NET dos nomes de contrato, você pode usar o <xref:System.ServiceModel.MessageParameterAttribute> atributo e usar a `Name` propriedade para definir o nome do contrato. Por exemplo, o contrato de operação a seguir é equivalente ao primeiro exemplo neste tópico.  
  
```csharp  
[OperationContract]  
public float GetAirfare(  
    [MessageParameter(Name="fromCity")] string originCity,  
    [MessageParameter(Name="toCity")] string destinationCity);  
```  
  
```vb  
<OperationContract()>  
  Function GetAirfare(<MessageParameter(Name := "fromCity")> fromCity As String, <MessageParameter(Name := "toCity")> toCity As String) As Double  
```  
  
## <a name="describing-empty-messages"></a>Descrevendo mensagens vazias  
 Uma mensagem de solicitação vazia pode ser descrita por não ter parâmetros de entrada ou de referência. Por exemplo, em C#:  
  
 `[OperationContract]`  
  
 `public int GetCurrentTemperature();`  
  
 Por exemplo, em Visual Basic:  
  
 `<OperationContract()>`  
  
 `Function GetCurrentTemperature() as Integer`  
  
 Uma mensagem de resposta vazia pode ser descrita por ter um `void` tipo de retorno e nenhum parâmetro de saída ou de referência. Por exemplo, em:  
  
```csharp  
[OperationContract]  
public void SetTemperature(int temperature);  
```  
  
```vb  
<OperationContract()>  
Sub SetTemperature(temperature As Integer)  
```  
  
 Isso é diferente de uma operação unidirecional, como:  
  
```csharp  
[OperationContract(IsOneWay=true)]  
public void SetLightbulbStatus(bool isOn);  
```  
  
```vb  
<OperationContract(IsOneWay:=True)>  
Sub SetLightbulbStatus(isOne As Boolean)  
```  
  
 A `SetTemperatureStatus` operação retorna uma mensagem vazia. Ele poderá retornar uma falha em vez disso, se houver um problema ao processar a mensagem de entrada. A `SetLightbulbStatus` operação não retorna nada. Não é possível comunicar uma condição de falha dessa operação.  
  
## <a name="describing-messages-by-using-message-contracts"></a>Descrevendo mensagens usando contratos de mensagem  
 Talvez você queira usar um único tipo para representar a mensagem inteira. Embora seja possível usar um contrato de dados para essa finalidade, a maneira recomendada de fazer isso é usar um contrato de mensagem — isso evita níveis desnecessários de encapsulamento no XML resultante. Além disso, os contratos de mensagem permitem que você exerça mais controle sobre as mensagens resultantes. Por exemplo, você pode decidir quais partes de informações devem estar no corpo da mensagem e quais devem estar nos cabeçalhos da mensagem. O exemplo a seguir mostra o uso de contratos de mensagem.  
  
```csharp  
[ServiceContract]  
public interface IAirfareQuoteService  
{  
    [OperationContract]  
    GetAirfareResponse GetAirfare(GetAirfareRequest request);  
}  
  
[MessageContract]  
public class GetAirfareRequest  
{  
    [MessageHeader] public DateTime date;  
    [MessageBodyMember] public Itinerary itinerary;  
}  
  
[MessageContract]  
public class GetAirfareResponse  
{  
    [MessageBodyMember] public float airfare;  
    [MessageBodyMember] public string currency;  
}  
  
[DataContract]  
public class Itinerary  
{  
    [DataMember] public string fromCity;  
    [DataMember] public string toCity;  
}  
```  
  
```vb  
<ServiceContract()>  
Public Interface IAirfareQuoteService  
    <OperationContract()>  
    Function GetAirfare(request As GetAirfareRequest) As GetAirfareResponse  
End Interface  
  
<MessageContract()>  
Public Class GetAirfareRequest  
    <MessageHeader()>
    Public Property date as DateTime  
    <MessageBodyMember()>  
    Public Property itinerary As Itinerary  
End Class  
  
<MessageContract()>  
Public Class GetAirfareResponse  
    <MessageBodyMember()>  
    Public Property airfare As Double  
    <MessageBodyMember()> Public Property currency As String  
End Class  
  
<DataContract()>  
Public Class Itinerary  
    <DataMember()> Public Property fromCity As String  
    <DataMember()> Public Property toCity As String  
End Class  
```  
  
 Para obter mais informações, consulte [usando contratos de mensagem](using-message-contracts.md).  
  
 No exemplo anterior, a <xref:System.Runtime.Serialization.DataContractSerializer> classe ainda é usada por padrão. A <xref:System.Xml.Serialization.XmlSerializer> classe também pode ser usada com contratos de mensagem. Para fazer isso, aplique o <xref:System.ServiceModel.XmlSerializerFormatAttribute> atributo à operação ou ao contrato e use tipos compatíveis com a <xref:System.Xml.Serialization.XmlSerializer> classe nos cabeçalhos de mensagem e membros do corpo.  
  
## <a name="describing-messages-by-using-streams"></a>Descrevendo mensagens usando fluxos  
 Outra maneira de descrever as mensagens em operações é usar a <xref:System.IO.Stream> classe ou uma de suas classes derivadas em um contrato de operação ou como um membro do corpo do contrato de mensagem (ela deve ser o único membro nesse caso). Para mensagens de entrada, o tipo deve ser `Stream` — você não pode usar classes derivadas.  
  
 Em vez de invocar o serializador, o WCF recupera dados de um fluxo e os coloca diretamente em uma mensagem de saída ou recupera dados de uma mensagem de entrada e os coloca diretamente em um fluxo. O exemplo a seguir mostra o uso de fluxos.  
  
```csharp  
[OperationContract]  
public Stream DownloadFile(string fileName);  
```  
  
```vb  
<OperationContract()>  
Function DownloadFile(fileName As String) As String  
```  
  
 Não é possível combinar `Stream` e não transmitir dados em um único corpo de mensagem. Use um contrato de mensagem para colocar os dados adicionais em cabeçalhos de mensagem. O exemplo a seguir mostra o uso incorreto de fluxos ao definir o contrato de operação.  
  
```csharp  
//Incorrect:  
// [OperationContract]  
// public void UploadFile (string fileName, Stream fileData);  
```  
  
```vb  
'Incorrect:  
    '<OperationContract()>  
    Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
```  
  
 O exemplo a seguir mostra o uso correto de fluxos ao definir um contrato de operação.  
  
```csharp  
[OperationContract]  
public void UploadFile (UploadFileMessage message);  
//code omitted  
[MessageContract]  
public class UploadFileMessage  
{  
    [MessageHeader] public string fileName;  
    [MessageBodyMember] public Stream fileData;  
}  
```  
  
```vb  
<OperationContract()>  
Public Sub UploadFile(fileName As String, fileData As StreamingContext)  
'Code Omitted  
<MessageContract()>  
Public Class UploadFileMessage  
   <MessageHeader()>  
    Public Property fileName As String  
    <MessageBodyMember()>  
    Public Property fileData As Stream  
End Class  
```  
  
 Para obter mais informações, consulte [grandes dados e streaming](large-data-and-streaming.md).  
  
## <a name="using-the-message-class"></a>Usando a classe de mensagens  
 Para ter total controle programático sobre as mensagens enviadas ou recebidas, você pode usar a <xref:System.ServiceModel.Channels.Message> classe diretamente, conforme mostrado no código de exemplo a seguir.  
  
```csharp  
[OperationContract]  
public void LogMessage(Message m);  
```  
  
```vb  
<OperationContract()>  
Sub LogMessage(m As Message)  
```  
  
 Este é um cenário avançado, que é descrito em detalhes no [uso da classe Message](using-the-message-class.md).  
  
## <a name="describing-fault-messages"></a>Descrevendo as mensagens de falha  
 Além das mensagens descritas pelo valor de retorno e parâmetros de saída ou de referência, qualquer operação que não seja unidirecional pode retornar pelo menos duas mensagens possíveis: sua mensagem de resposta normal e uma mensagem de falha. Considere o contrato de operação a seguir.  
  
```csharp  
[OperationContract]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
```  
  
```vb  
<OperationContract()>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime)  
```  
  
 Essa operação pode retornar uma mensagem normal que contém um `float` número ou uma mensagem de falha que contém um código de falha e uma descrição. Você pode fazer isso lançando uma <xref:System.ServiceModel.FaultException> em sua implementação de serviço.  
  
 Você pode especificar mensagens de falha adicionais possíveis usando o <xref:System.ServiceModel.FaultContractAttribute> atributo. As falhas adicionais devem ser serializáveis usando o <xref:System.Runtime.Serialization.DataContractSerializer> , conforme mostrado no código de exemplo a seguir.  
  
```csharp  
[OperationContract]  
[FaultContract(typeof(ItineraryNotAvailableFault))]  
float GetAirfare(string fromCity, string toCity, DateTime date);  
  
//code omitted  
  
[DataContract]  
public class ItineraryNotAvailableFault  
{  
    [DataMember]  
    public bool IsAlternativeDateAvailable;  
  
    [DataMember]  
    public DateTime alternativeSuggestedDate;  
}  
```  
  
```vb  
<OperationContract()>  
<FaultContract(GetType(ItineraryNotAvailableFault))>  
Function GetAirfare(fromCity As String, toCity As String, date as DateTime) As Double  
  
'Code Omitted  
<DataContract()>  
Public Class  
  <DataMember()>  
  Public Property IsAlternativeDateAvailable As Boolean  
  <DataMember()>  
  Public Property alternativeSuggestedDate As DateTime  
End Class  
```  
  
 Essas falhas adicionais podem ser geradas lançando um <xref:System.ServiceModel.FaultException%601> dos tipos de contrato de dados apropriados. Para obter mais informações, consulte [tratamento de exceções e falhas](../extending/handling-exceptions-and-faults.md).  
  
 Você não pode usar a <xref:System.Xml.Serialization.XmlSerializer> classe para descrever as falhas. O <xref:System.ServiceModel.XmlSerializerFormatAttribute> não tem nenhum efeito sobre os contratos de falha.  
  
## <a name="using-derived-types"></a>Usando tipos derivados  
 Talvez você queira usar um tipo base em uma operação ou um contrato de mensagem e, em seguida, usar um tipo derivado ao invocar a operação de fato. Nesse caso, você deve usar o <xref:System.ServiceModel.ServiceKnownTypeAttribute> atributo ou algum mecanismo alternativo para permitir o uso de tipos derivados. Considere a seguinte operação.  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
```  
  
```vb
<OperationContract()>  
    Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
```  
  
 Suponha que dois tipos, `Book` e `Magazine` derivam de `LibraryItem` . Para usar esses tipos na `IsLibraryItemAvailable` operação, você pode alterar a operação da seguinte maneira:  
  
 `[OperationContract]`  
  
 `[ServiceKnownType(typeof(Book))]`  
  
 `[ServiceKnownType(typeof(Magazine))]`  
  
 `public bool IsLibraryItemAvailable(LibraryItem item);`  
  
 Como alternativa, você pode usar o <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo quando o padrão <xref:System.Runtime.Serialization.DataContractSerializer> estiver em uso, conforme mostrado no código de exemplo a seguir.  
  
```csharp  
[OperationContract]  
public bool IsLibraryItemAvailable(LibraryItem item);  
  
// code omitted
  
[DataContract]  
[KnownType(typeof(Book))]  
[KnownType(typeof(Magazine))]  
public class LibraryItem  
{  
    //code omitted  
}  
```  
  
```vb  
<OperationContract()>  
Function IsLibraryItemAvailable(item As LibraryItem) As Boolean  
  
'Code Omitted  
<DataContract()>  
<KnownType(GetType(Book))>  
<KnownType(GetType(Magazine))>  
Public Class LibraryItem  
  'Code Omitted  
End Class  
```  
  
 Você pode usar o <xref:System.Xml.Serialization.XmlIncludeAttribute> atributo ao usar o <xref:System.Xml.Serialization.XmlSerializer> .  
  
 Você pode aplicar o <xref:System.ServiceModel.ServiceKnownTypeAttribute> atributo a uma operação ou a todo o serviço. Ele aceita um tipo ou o nome do método a ser chamado para obter uma lista de tipos conhecidos, assim como o <xref:System.Runtime.Serialization.KnownTypeAttribute> atributo. Para obter mais informações, consulte [tipos conhecidos de contrato de dados](data-contract-known-types.md).  
  
## <a name="specifying-the-use-and-style"></a>Especificando o uso e o estilo  
 Ao descrever serviços usando WSDL (Web Services Description Language), os dois estilos comumente usados são documento e RPC (chamada de procedimento remoto). No estilo do documento, todo o corpo da mensagem é descrito usando o esquema, e o WSDL descreve as várias partes do corpo da mensagem referindo-se aos elementos dentro desse esquema. No estilo RPC, o WSDL refere-se a um tipo de esquema para cada parte de mensagem, em vez de um elemento. Em alguns casos, você precisa selecionar manualmente um desses estilos. Você pode fazer isso aplicando o <xref:System.ServiceModel.DataContractFormatAttribute> atributo e configurando a `Style` Propriedade (quando o <xref:System.Runtime.Serialization.DataContractSerializer> estiver em uso) ou definindo `Style` no <xref:System.ServiceModel.XmlSerializerFormatAttribute> atributo (ao usar o <xref:System.Xml.Serialization.XmlSerializer> ).  
  
 Além disso, o <xref:System.Xml.Serialization.XmlSerializer> oferece suporte a duas formas de XML serializados: `Literal` e `Encoded` . `Literal`é a forma mais aceita e é a única forma com a qual o <xref:System.Runtime.Serialization.DataContractSerializer> dá suporte. `Encoded`é um formulário herdado descrito na seção 5 da especificação SOAP e não é recomendado para novos serviços. Para alternar para `Encoded` o modo, defina a `Use` propriedade no <xref:System.ServiceModel.XmlSerializerFormatAttribute> atributo como `Encoded` .  
  
 Na maioria dos casos, você não deve alterar as configurações padrão para `Style` as `Use` Propriedades e.  
  
## <a name="controlling-the-serialization-process"></a>Controlando o processo de serialização  
 Você pode fazer várias coisas para personalizar a maneira como os dados são serializados.  
  
### <a name="changing-server-serialization-settings"></a>Alterando as configurações de serialização do servidor  
 Quando o padrão <xref:System.Runtime.Serialization.DataContractSerializer> estiver em uso, você poderá controlar alguns aspectos do processo de serialização no serviço aplicando o <xref:System.ServiceModel.ServiceBehaviorAttribute> atributo ao serviço. Especificamente, você pode usar a `MaxItemsInObjectGraph` propriedade para definir a cota que limita o número máximo de objetos que o <xref:System.Runtime.Serialization.DataContractSerializer> desserializa. Você pode usar a `IgnoreExtensionDataObject` propriedade para desativar o recurso de controle de versão de ida e volta. Para obter mais informações sobre cotas, confira [Considerações sobre segurança de dados](security-considerations-for-data.md). Para obter mais informações sobre ida e volta, consulte [contratos de dados compatíveis com o encaminhamento](forward-compatible-data-contracts.md).  
  
```csharp  
[ServiceBehavior(MaxItemsInObjectGraph=100000)]  
public class MyDataService:IDataService  
{  
    public DataPoint[] GetData()  
    {  
       // Implementation omitted  
    }  
}  
```  
  
```vb  
<ServiceBehavior(MaxItemsInObjectGraph:=100000)>  
Public Class MyDataService Implements IDataService  
  
    Function GetData() As DataPoint()  
         ‘ Implementation omitted  
    End Function  
End Interface  
```  
  
### <a name="serialization-behaviors"></a>Comportamentos de serialização  
 Dois comportamentos estão disponíveis no WCF, o <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> e o <xref:System.ServiceModel.Description.XmlSerializerOperationBehavior> que são conectados automaticamente dependendo de qual serializador está em uso para uma operação específica. Como esses comportamentos são aplicados automaticamente, você normalmente não precisa estar atento a eles.  
  
 No entanto, o `DataContractSerializerOperationBehavior` tem as `MaxItemsInObjectGraph` `IgnoreExtensionDataObject` Propriedades, e `DataContractSurrogate` que você pode usar para personalizar o processo de serialização. As duas primeiras propriedades têm o mesmo significado, conforme discutido na seção anterior. Você pode usar a `DataContractSurrogate` propriedade para habilitar os substitutos de contrato de dados, que são um mecanismo poderoso para personalizar e estender o processo de serialização. Para obter mais informações, consulte [substitutos de contrato de dados](../extending/data-contract-surrogates.md).  
  
 Você pode usar o `DataContractSerializerOperationBehavior` para personalizar a serialização de cliente e de servidor. O exemplo a seguir mostra como aumentar a `MaxItemsInObjectGraph` cota no cliente.  
  
```csharp  
ChannelFactory<IDataService> factory = new ChannelFactory<IDataService>(binding, address);  
foreach (OperationDescription op in factory.Endpoint.Contract.Operations)  
{  
    DataContractSerializerOperationBehavior dataContractBehavior =  
                op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
    if (dataContractBehavior != null)  
    {  
        dataContractBehavior.MaxItemsInObjectGraph = 100000;  
    }  
}  
IDataService client = factory.CreateChannel();  
```  
  
```vb  
Dim factory As ChannelFactory(Of IDataService) = New ChannelFactory(Of IDataService)(binding, address)  
For Each op As OperationDescription In factory.Endpoint.Contract.Operations  
        Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
        If dataContractBehavior IsNot Nothing Then  
            dataContractBehavior.MaxItemsInObjectGraph = 100000  
        End If  
     Next  
    Dim client As IDataService = factory.CreateChannel  
```  
  
Este é o código equivalente no serviço, no caso de hospedagem interna:
  
```csharp  
ServiceHost serviceHost = new ServiceHost(typeof(IDataService))  
foreach (ServiceEndpoint ep in serviceHost.Description.Endpoints)  
{  
foreach (OperationDescription op in ep.Contract.Operations)  
{  
        DataContractSerializerOperationBehavior dataContractBehavior =  
           op.Behaviors.Find<DataContractSerializerOperationBehavior>()  
                as DataContractSerializerOperationBehavior;  
        if (dataContractBehavior != null)  
        {  
            dataContractBehavior.MaxItemsInObjectGraph = 100000;  
        }  
}  
}  
serviceHost.Open();  
```  
  
```vb  
Dim serviceHost As ServiceHost = New ServiceHost(GetType(IDataService))  
        For Each ep As ServiceEndpoint In serviceHost.Description.Endpoints  
            For Each op As OperationDescription In ep.Contract.Operations  
                Dim dataContractBehavior As DataContractSerializerOperationBehavior = op.Behaviors.Find(Of DataContractSerializerOperationBehavior)()  
  
                If dataContractBehavior IsNot Nothing Then  
                    dataContractBehavior.MaxItemsInObjectGraph = 100000  
                End If  
            Next  
        Next  
        serviceHost.Open()  
```  
  
 No caso hospedado na Web, você deve criar uma nova `ServiceHost` classe derivada e usar uma fábrica de host de serviço para conectá-la.  
  
### <a name="controlling-serialization-settings-in-configuration"></a>Controlando as configurações de serialização na configuração  
 O `MaxItemsInObjectGraph` e o `IgnoreExtensionDataObject` podem ser controlados por meio da configuração usando o `dataContractSerializer` comportamento do ponto de extremidade ou serviço, conforme mostrado no exemplo a seguir.  
  
```xml  
<configuration>  
    <system.serviceModel>  
        <behaviors>  
            <endpointBehaviors>  
                <behavior name="LargeQuotaBehavior">  
                    <dataContractSerializer  
                      maxItemsInObjectGraph="100000" />  
                </behavior>  
            </endpointBehaviors>  
        </behaviors>  
        <client>  
            <endpoint address="http://example.com/myservice"  
                  behaviorConfiguration="LargeQuotaBehavior"  
                binding="basicHttpBinding" bindingConfiguration=""
                            contract="IDataService"  
                name="" />  
        </client>  
    </system.serviceModel>  
</configuration>  
```  
  
### <a name="shared-type-serialization-object-graph-preservation-and-custom-serializers"></a>Serialização de tipo compartilhado, preservação de grafo de objeto e serializadores personalizados  
 O <xref:System.Runtime.Serialization.DataContractSerializer> serializa o uso de nomes de contrato de dados e não de nomes de tipo .net. Isso é consistente com as filosofias de arquitetura orientada a serviços e permite um grande grau de flexibilidade — os tipos .NET podem mudar sem afetar o contrato de fio. Em casos raros, talvez você queira serializar nomes de tipo .NET reais, apresentando assim um forte acoplamento entre o cliente e o servidor, semelhante à tecnologia de comunicação remota .NET Framework. Essa não é uma prática recomendada, exceto em casos raros que geralmente ocorrem ao migrar para o WCF de .NET Framework comunicação remota. Nesse caso, você deve usar a <xref:System.Runtime.Serialization.NetDataContractSerializer> classe em vez da <xref:System.Runtime.Serialization.DataContractSerializer> classe.  
  
 <xref:System.Runtime.Serialization.DataContractSerializer>Normalmente, o serializa grafos de objeto como árvores de objetos. Ou seja, se o mesmo objeto for referenciado mais de uma vez, ele será serializado mais de uma vez. Por exemplo, considere uma `PurchaseOrder` instância que tenha dois campos do tipo endereço chamado `billTo` e `shipTo` . Se ambos os campos estiverem definidos para a mesma instância de endereço, haverá duas instâncias de endereço idênticas após a serialização e a desserialização. Isso é feito porque não há uma maneira interoperável padrão para representar grafos de objeto em XML (exceto para o padrão codificado em SOAP herdado disponível no <xref:System.Xml.Serialization.XmlSerializer> , conforme descrito na seção anterior em `Style` e `Use` ). A serialização de grafos de objeto como árvores tem determinadas desvantagens, por exemplo, grafos com referências circulares não podem ser serializados. Ocasionalmente, é necessário mudar para a serialização real do grafo de objetos, mesmo que ele não seja interoperável. Isso pode ser feito usando o <xref:System.Runtime.Serialization.DataContractSerializer> construído com o `preserveObjectReferences` parâmetro definido como `true` .  
  
 Ocasionalmente, os serializadores internos não são suficientes para seu cenário. Na maioria dos casos, você ainda pode usar a <xref:System.Runtime.Serialization.XmlObjectSerializer> abstração da qual o <xref:System.Runtime.Serialization.DataContractSerializer> e o <xref:System.Runtime.Serialization.NetDataContractSerializer> derivam.  
  
 Os três casos anteriores (preservação de tipo do .NET, preservação do grafo de objeto e `XmlObjectSerializer` serialização totalmente baseada em personalização) exigem que um serializador personalizado seja conectado. Para fazer isso, execute estas etapas:  
  
1. Escreva seu próprio comportamento derivado do <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> .  
  
2. Substitua os dois `CreateSerializer` métodos para retornar seu próprio serializador (ou o <xref:System.Runtime.Serialization.NetDataContractSerializer> , <xref:System.Runtime.Serialization.DataContractSerializer> com `preserveObjectReferences` definido como `true` , ou seu próprio personalizado <xref:System.Runtime.Serialization.XmlObjectSerializer> ).  
  
3. Antes de abrir o host de serviço ou criar um canal de cliente, remova o <xref:System.ServiceModel.Description.DataContractSerializerOperationBehavior> comportamento existente e conecte a classe derivada personalizada que você criou nas etapas anteriores.  
  
 Para obter mais informações sobre conceitos de serialização avançada, consulte [serialização e desserialização](serialization-and-deserialization.md).  
  
## <a name="see-also"></a>Consulte também

- [Usando a classe XmlSerializer](using-the-xmlserializer-class.md)
- [Como habilitar transmissão](how-to-enable-streaming.md)
- [Como criar um contrato de dados básicos para uma classe ou estrutura](how-to-create-a-basic-data-contract-for-a-class-or-structure.md)
