---
title: Criando aplicativos multicasting usando o transporte UDP
ms.date: 03/30/2017
ms.assetid: 7485154a-6e85-4a67-a9d4-9008e741d4df
ms.openlocfilehash: 40944129ce3b01d8422d5aba4cfbf913c56265ed
ms.sourcegitcommit: ee5b798427f81237a3c23d1fd81fff7fdc21e8d3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/28/2020
ms.locfileid: "84144624"
---
# <a name="creating-multicasting-applications-using-the-udp-transport"></a>Criando aplicativos multicasting usando o transporte UDP
Os aplicativos de multicast enviam mensagens pequenas para um grande número de destinatários ao mesmo tempo, sem a necessidade de estabelecer conexões ponto a ponto. A ênfase desses aplicativos é a velocidade em relação à confiabilidade. Em outras palavras, é mais importante enviar dados oportunos do que garantir que qualquer mensagem específica seja realmente recebida. O WCF agora dá suporte à gravação de aplicativos de multicast usando o <xref:System.ServiceModel.UdpBinding> . Esse transporte é útil em cenários em que um serviço precisa enviar mensagens pequenas para vários clientes simultaneamente. Um aplicativo de cotação de ações é um exemplo desse serviço.  
  
## <a name="implementing-a-multicast-application"></a>Implementando um aplicativo multicast  
 Para implementar um aplicativo multicast, defina um contrato de serviço e para cada componente de software que precise responder às mensagens multicast, implemente o contrato de serviço. Por exemplo, um aplicativo de cotação de ações pode definir um contrato de serviço:  
  
```csharp
// Shared contracts between the client and the service  
[ServiceContract]
interface IStockTicker
{
    [OperationContract(IsOneWay = true)]
    void SendStockInfo(StockInfo[] stockInfo);
}

[DataContract]
class StockInfo
{
    [DataMember]
    public string Symbol;

    [DataMember]
    public float Price;

    public StockInfo(string symbol, float price)
    {
        this.Symbol = symbol;
        this.Price = price;
    }
}
```  
  
 Cada aplicativo que deseja receber mensagens multicast deve hospedar um serviço que expõe essa interface.  Por exemplo, aqui está um exemplo de código que ilustra como receber mensagens multicast:  
  
```csharp
// Service Address
string serviceAddress = "soap.udp://224.0.0.1:40000";
// Binding
UdpBinding myBinding = new UdpBinding();
// Host
ServiceHost host = new ServiceHost(typeof(StockTickerService), new Uri(serviceAddress));
// Add service endpoint
host.AddServiceEndpoint(typeof(IStockTicker), myBinding, string.Empty);
// Openup the service host
host.Open();

Console.WriteLine("Start receiving stock information");
Console.ReadLine();
```  
  
 O aplicativo especifica o endereço UDP em que todos os serviços serão escutados. Um novo <xref:System.ServiceModel.ServiceHost> é criado e um ponto de extremidade de serviço é exposto usando o <xref:System.ServiceModel.UdpBinding> . O <xref:System.ServiceModel.ServiceHost> é então aberto e começará a escutar mensagens de entrada.  
  
 Nesse tipo de cenário, ele é o cliente que realmente envia mensagens de multicast. Cada serviço que está escutando no endereço UDP correto receberá as mensagens de multicast. Aqui está um exemplo de um cliente que envia mensagens de multicast:  
  
```csharp
// Multicast Address
string serviceAddress = "soap.udp://224.0.0.1:40000";

// Binding
UdpBinding myBinding = new UdpBinding();

// Channel factory
ChannelFactory<IStockTicker> factory
    = new ChannelFactory<IStockTicker>(myBinding,
                new EndpointAddress(serviceAddress));

// Call service
IStockTicker proxy = factory.CreateChannel();

while (true)
{
    // This will continue to mulicast stock information
    proxy.SendStockInfo(GetStockInfo());
    Console.WriteLine($"sent stock info at {DateTime.Now}");
    // Wait for one second before sending another update
    System.Threading.Thread.Sleep(new TimeSpan(0, 0, 1));
}
```  
  
 Esse código gera informações de ações e, em seguida, usa o contrato de serviço IStockTicker para enviar mensagens de multicast para serviços de chamada escutando o endereço UDP correto.  
  
### <a name="udp-and-reliable-messaging"></a>UDP e sistema de mensagens confiável  
  A associação UDP não oferece suporte a mensagens confiáveis devido à natureza leve do protocolo UDP. Se você precisar confirmar que as mensagens são recebidas por um ponto de extremidade remoto, use um transporte que ofereça suporte a mensagens confiáveis como HTTP ou TCP. Para obter mais informações sobre mensagens confiáveis, consulte <https://go.microsoft.com/fwlink/?LinkId=231830> .  
  
### <a name="two-way-multicast-messaging"></a>Mensagens multicast bidirecionais  
 Embora as mensagens multicast sejam geralmente unidirecionais, o UdpBinding dá suporte à troca de mensagens de solicitação/resposta. As mensagens enviadas usando o transporte UDP contêm um endereço de e para. Deve-se ter cuidado ao usar o endereço de no, pois ele poderia ser alterado de forma mal-intencionada.  O endereço pode ser verificado usando o seguinte código:  
  
```csharp
if (address.AddressFamily == AddressFamily.InterNetwork)
{
    // IPv4
    byte[] addressBytes = address.GetAddressBytes();
    return ((addressBytes[0] & 0xE0) == 0xE0);
}
else
{
    // IPv6
    return address.IsIPv6Multicast;
}
```  
  
 Esse código verifica o primeiro byte do endereço de para ver se ele contém 0xE0, o que significa que o endereço é um endereço de várias difusões.  
  
### <a name="security-considerations"></a>Considerações de segurança  
 Ao escutar mensagens multicast, um pacote ICMP é enviado ao roteador, notificando-o sobre o endereço de multicast. Qualquer pessoa na sub-rede local que tenha permissões pode escutar esses tipos de pacotes e determinar qual endereço de multicast e porta você está ouvindo.  
  
 Não use o endereço IP do remetente para fins de segurança. Essas informações podem ser falsificadas e podem fazer com que um aplicativo envie respostas para o computador errado. Uma maneira de mitigar essa ameaça é habilitar a segurança em nível de mensagem. No nível da rede, o IPSec (segurança do protocolo Internet) e/ou NAP (proteção de acesso à rede) também pode ser usado.
