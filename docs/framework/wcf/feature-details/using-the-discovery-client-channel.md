---
title: Usando o canal cliente Discovery
ms.date: 03/30/2017
ms.assetid: 1494242a-1d64-4035-8ecd-eb4f06c8d2ba
ms.openlocfilehash: a74d0ba77977e158a6c6e469a9b6a88c8d1aac82
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575973"
---
# <a name="using-the-discovery-client-channel"></a>Usando o canal cliente Discovery
Ao gravar um aplicativo cliente WCF, você precisa saber o endereço do ponto de extremidade do serviço que está chamando. Em muitas situações, o endereço do ponto de extremidade de um serviço não é conhecido com antecedência ou o endereço do serviço muda ao longo do tempo. O canal cliente de descoberta permite que você grave um aplicativo cliente do WCF, descreva o serviço que você deseja chamar e o canal do cliente envia automaticamente uma solicitação de investigação. Quando um serviço responde, o canal cliente de descoberta recupera o endereço do ponto de extremidade para o serviço da resposta de investigação e o utiliza para chamar o serviço.  
  
## <a name="using-the-discovery-client-channel"></a>Usando o canal cliente Discovery  
 Para usar o canal cliente de descoberta, adicione uma instância do <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> à pilha de canais do cliente. Como alternativa, você pode usar o <xref:System.ServiceModel.Discovery.DynamicEndpoint> e um <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> é adicionado automaticamente à sua associação, se ainda não estiver presente.  
  
> [!CAUTION]
> É recomendável que o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> seja o elemento mais alto na pilha de canais do cliente. Qualquer elemento de associação que é adicionado sobre o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> deve garantir que o <xref:System.ServiceModel.ChannelFactory> canal ou que ele cria não use o endereço do ponto de extremidade ou `Via` Endereço (passado para o `CreateChannel` método) porque eles não podem conter o endereço correto.  
  
 A <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> classe contém duas propriedades públicas:  
  
1. <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.FindCriteria%2A>, que é usado para descrever o serviço que você deseja chamar.  
  
2. <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpointProvider%2A>que especifica o ponto de extremidade de descoberta para o qual enviar mensagens de descoberta.  
  
 A <xref:System.ServiceModel.Discovery.FindCriteria.%23ctor%2A> propriedade permite especificar o contrato de serviço que você está procurando, todos os URIs de escopo necessários e o número máximo de tempo para tentar abrir o canal. O tipo de contrato é especificado chamando o construtor <xref:System.ServiceModel.Discovery.FindCriteria> . URIs de escopo podem ser adicionados à <xref:System.ServiceModel.Discovery.FindCriteria.Scopes%2A> propriedade. A <xref:System.ServiceModel.Discovery.FindCriteria.MaxResults%2A> propriedade permite que você especifique o número máximo de resultados aos quais o cliente tenta se conectar. Quando uma resposta de investigação é recebida, o cliente tenta abrir o canal usando o endereço do ponto de extremidade da resposta de investigação. Se ocorrer uma exceção, o cliente passará para a próxima resposta de investigação, aguardando que mais respostas sejam recebidas, se necessário. Ele continua a fazer isso até que o canal seja aberto com êxito ou o número máximo de resultados seja atingido. Para obter mais informações sobre essas configurações, consulte <xref:System.ServiceModel.Discovery.FindCriteria>.  
  
 A <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement.DiscoveryEndpointProvider%2A> propriedade permite que você especifique o ponto de extremidade de descoberta a ser usado. Normalmente, isso é um <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> , mas pode ser qualquer ponto de extremidade válido.  
  
 Ao criar a associação a ser usada para se comunicar com o serviço, você deve ter cuidado para usar exatamente a mesma ligação que o serviço. A única diferença é que a associação de cliente tem um <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> na parte superior da pilha. Se o serviço estiver usando uma das associações fornecidas pelo sistema, crie um novo <xref:System.ServiceModel.Channels.CustomBinding> e passe a associação fornecida pelo sistema para o <xref:System.ServiceModel.Channels.CustomBinding.%23ctor%2A> Construtor. Em seguida, você pode adicionar o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> chamando `Insert` na <xref:System.ServiceModel.Channels.CustomBinding.Elements%2A> propriedade.  
  
 Depois de adicionar o <xref:System.ServiceModel.Discovery.DiscoveryClientBindingElement> à sua associação e configurá-lo, você pode criar uma instância da classe de cliente WCF, abri-la e chamar seus métodos. O exemplo a seguir usa o canal do cliente de descoberta para descobrir um serviço WCF que implementa a `ICalculator` classe (usada no tutorial introdução do WCF) e chama seu `Add` método.  
  
```csharp
// Create the DiscoveryClientBindingElement  
DiscoveryClientBindingElement bindingElement = new DiscoveryClientBindingElement();  
// Search for a service that implements the ICalculator interface, attempting to open  
// the channel a maximum of 2 times  
bindingElement.FindCriteria = new FindCriteria(typeof(ICalculator)) { MaxResults = 2 };  
// Use the UdpDiscoveryEndpoint  
bindingElement.DiscoveryEndpoint = new UdpDiscoveryEndpoint();  
  
// The service uses the BasicHttpBinding, so use that and insert the DiscoveryClientBindingElement at the
// top of the stack  
CustomBinding binding = new CustomBinding(new BasicHttpBinding());  
binding.Elements.Insert(0,bindingElement);  
  
try  
{  
    // Create the WCF client and call a method  
    CalculatorClient client = new CalculatorClient(binding, new EndpointAddress("http://schemas.microsoft.com/dynamic"));  
    client.Open();  
    client.Add(1, 1);  
}  
catch (EndpointNotFoundException ex)  
{  
    Console.WriteLine("An exception occurred: " + ex.Message);  
}  
```  
  
## <a name="security-and-the-discovery-client-channel"></a>Segurança e o canal do cliente de descoberta  
 Ao usar o canal do cliente de descoberta, dois pontos de extremidade estão sendo especificados. Uma é usada para mensagens de descoberta, geralmente <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> , e a outra é o ponto de extremidade do aplicativo. Ao implementar um serviço seguro, é necessário tomar cuidado para proteger os dois pontos de extremidade. Para obter mais informações sobre segurança, consulte [protegendo serviços e clientes](securing-services-and-clients.md).
