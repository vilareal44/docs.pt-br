---
title: Visão geral de descoberta do WCF
ms.date: 03/30/2017
ms.assetid: 84fad0e4-23b1-45b5-a2d4-c9cdf90bbb22
ms.openlocfilehash: e7fd7ae4103600eb5463114987ca4ccbc2e0a1f2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600186"
---
# <a name="wcf-discovery-overview"></a>Visão geral de descoberta do WCF
As APIs de descoberta fornecem um modelo de programação unificado para a publicação dinâmica e a descoberta de serviços Web usando o protocolo WS-Discovery. Essas APIs permitem que os serviços se publiquem e os clientes localizem serviços publicados. Depois que um serviço é detectável, o serviço tem a capacidade de enviar mensagens de comunicado, além de escutar e responder às solicitações de descoberta. Os serviços detectáveis podem enviar mensagens de saudação para anunciar sua chegada em uma rede e até mesmo as mensagens para anunciar sua partida de uma rede. Para localizar um serviço, os clientes enviam uma `Probe` solicitação que contém critérios específicos, como tipo de contrato de serviço, palavras-chave e escopo na rede. Os serviços recebem a `Probe` solicitação e determinam se elas correspondem aos critérios. Se um serviço corresponder, ele responderá enviando uma `ProbeMatch` mensagem de volta ao cliente com as informações necessárias para entrar em contato com o serviço. Os clientes também podem enviar `Resolve` solicitações que permitem encontrar serviços que podem ter alterado seu endereço de ponto de extremidade. Os serviços correspondentes respondem às `Resolve` solicitações enviando uma `ResolveMatch` mensagem de volta ao cliente.  
  
## <a name="ad-hoc-and-managed-modes"></a>Modos ad-hoc e gerenciados  
 A API de descoberta dá suporte a dois modos diferentes: gerenciado e ad-hoc. No modo gerenciado, há um servidor centralizado chamado proxy de descoberta que mantém informações sobre os serviços disponíveis. O proxy de descoberta pode ser preenchido com informações sobre serviços de várias maneiras. Por exemplo, os serviços podem enviar mensagens de comunicado durante a inicialização para o proxy de descoberta ou o proxy pode ler dados de um banco de dado ou de um arquivo de configuração para determinar quais serviços estão disponíveis. O modo como o proxy de descoberta é populado é completamente o desenvolvedor. Os clientes usam o proxy de descoberta para recuperar informações sobre os serviços disponíveis. Quando um cliente pesquisa um serviço, ele envia uma `Probe` mensagem para o proxy de descoberta e o proxy determina se algum dos serviços que ele sabe corresponde ao serviço que o cliente está procurando. Se houver correspondência, o proxy de descoberta enviará uma `ProbeMatch` resposta de volta ao cliente. Em seguida, o cliente pode contatar o serviço diretamente usando as informações de serviço retornadas do proxy. O princípio-chave por trás do modo gerenciado é que as solicitações de descoberta são enviadas de forma unicast para uma autoridade, o proxy de descoberta. O .NET Framework contém os principais componentes que permitem que você crie seu próprio proxy. Os clientes e serviços podem localizar o proxy por vários métodos:  
  
- O proxy pode responder a mensagens ad hoc.  
  
- O proxy pode enviar uma mensagem de anúncio durante a inicialização.  
  
- Os clientes e serviços podem ser escritos para procurar um ponto de extremidade conhecido específico.  
  
 No modo ad hoc, não há nenhum servidor centralizado. Todas as mensagens de descoberta, como anúncios de serviço e solicitações de cliente, são enviadas de maneira multicast. Por padrão, a .NET Framework contém suporte para descoberta ad hoc em relação ao protocolo UDP. Por exemplo, se um serviço for configurado para enviar um anúncio de saudação na inicialização, ele o enviará por um endereço de multicast bem conhecido usando o protocolo UDP. Os clientes precisam escutar ativamente esses anúncios e processá-los de forma adequada. Quando um cliente envia uma `Probe` mensagem para um serviço, ele é enviado pela rede usando um protocolo de multicast. Cada serviço que recebe a solicitação determina se ele corresponde aos critérios na `Probe` mensagem e responde diretamente ao cliente com uma `ProbeMatch` mensagem se o serviço corresponder aos critérios especificados na `Probe` mensagem.  
  
## <a name="benefits-of-using-wcf-discovery"></a>Benefícios do uso da descoberta do WCF  
 Como a descoberta do WCF é implementada usando o protocolo WS-Discovery, ela é interoperável com outros clientes, serviços e proxies que implementam o WS-Discovery também. A descoberta do WCF se baseia nas APIs do WCF existentes, o que facilita a adição da funcionalidade de descoberta aos seus serviços e clientes existentes. A descoberta de serviço pode ser facilmente adicionada por meio das definições de configuração do aplicativo. Além disso, a descoberta do WCF também dá suporte ao uso do protocolo de descoberta em outros transportes, como o peer net, a sobreposição de nomenclatura e o HTTP. A descoberta do WCF oferece suporte a um modo gerenciado de operação em que um proxy de descoberta é usado. Isso pode reduzir o tráfego de rede à medida que as mensagens são enviadas diretamente para o proxy de descoberta em vez de enviar mensagens multicast para toda a rede. A descoberta do WCF também permite mais flexibilidade ao trabalhar com serviços Web. Por exemplo, você pode alterar o endereço de um serviço sem precisar reconfigurar o cliente ou o serviço. Quando um cliente deve acessar o serviço, ele pode emitir uma `Probe` mensagem por meio de uma `Find` solicitação e esperar que o serviço responda com seu endereço atual. A descoberta do WCF permite que um cliente procure um serviço com base em diferentes critérios, incluindo tipos de contrato, elementos de associação, namespace, escopo e palavras-chave ou números de versão. A descoberta do WCF permite a descoberta de tempo de execução e de design. Adicionar descoberta ao seu aplicativo pode ser usado para habilitar outros cenários, como tolerância a falhas e configuração automática.  
  
## <a name="service-publication"></a>Publicação de serviço  
 Para tornar um serviço detectável, um <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> deve ser adicionado ao host de serviço e um ponto de extremidade de descoberta deve ser adicionado para especificar onde ouvir as mensagens de descoberta. O exemplo de código a seguir mostra como um serviço auto-hospedado pode ser modificado para torná-lo detectável.  
  
```csharp  
Uri baseAddress = new Uri($"http://{System.Net.Dns.GetHostName()}:8000/discovery/scenarios/calculatorservice/{Guid.NewGuid().ToString()}/");

// Create a ServiceHost for the CalculatorService type.
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
    // Add calculator endpoint
    serviceHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), string.Empty);

    // ** DISCOVERY ** //
    // Make the service discoverable by adding the discovery behavior
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());

    // ** DISCOVERY ** //
    // Add the discovery endpoint that specifies where to publish the services
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());

    // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();

    // The service can now be accessed.
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.ReadLine();
}
```  
  
 Uma <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> instância deve ser adicionada a uma descrição de serviço para que o serviço seja detectável. Uma <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> instância deve ser adicionada ao host de serviço para informar ao serviço onde ouvir as solicitações de descoberta. Neste exemplo, um <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> (que é derivado de <xref:System.ServiceModel.Discovery.DiscoveryEndpoint> ) é adicionado para especificar que o serviço deve escutar solicitações de descoberta por meio do transporte multicast UDP. O <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> é usado para descoberta ad hoc porque todas as mensagens são enviadas de forma multicast.  
  
## <a name="announcement"></a>Anúncio  
 Por padrão, a publicação do serviço não envia mensagens de comunicado. O serviço deve ser configurado para enviar mensagens de comunicado. Isso fornece flexibilidade adicional para gravadores de serviço porque eles podem anunciar o serviço separadamente da escuta de mensagens de descoberta. O comunicado de serviço também pode ser usado como um mecanismo para registrar serviços com um proxy de descoberta ou outros registros de serviço. O código a seguir mostra como configurar um serviço para enviar mensagens de anúncio por uma associação UDP.  
  
```csharp  
Uri baseAddress = new Uri($"http://{System.Net.Dns.GetHostName()}:8000/discovery/scenarios/calculatorservice/{Guid.NewGuid().ToString()}/");

// Create a ServiceHost for the CalculatorService type.
using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress))
{
    // Add calculator endpoint
    serviceHost.AddServiceEndpoint(typeof(ICalculator), new WSHttpBinding(), string.Empty);

    // ** DISCOVERY ** //
    // Make the service discoverable by adding the discovery behavior
    ServiceDiscoveryBehavior discoveryBehavior = new ServiceDiscoveryBehavior();
    serviceHost.Description.Behaviors.Add(discoveryBehavior);

    // Send announcements on UDP multicast transport
    discoveryBehavior.AnnouncementEndpoints.Add(
      new UdpAnnouncementEndpoint());

    // ** DISCOVERY ** //
    // Add the discovery endpoint that specifies where to publish the services
    serviceHost.Description.Endpoints.Add(new UdpDiscoveryEndpoint());

    // Open the ServiceHost to create listeners and start listening for messages.
    serviceHost.Open();

    // The service can now be accessed.
    Console.WriteLine("Press <ENTER> to terminate service.");
    Console.ReadLine();
}
```  
  
## <a name="service-discovery"></a>Descoberta de Serviços  
 Um aplicativo cliente pode usar a <xref:System.ServiceModel.Discovery.DiscoveryClient> classe para localizar serviços. O desenvolvedor cria uma instância da <xref:System.ServiceModel.Discovery.DiscoveryClient> classe que passa em um ponto de extremidade de descoberta que especifica onde enviar `Probe` ou `Resolve` mensagens. Em seguida, o cliente chama <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> que especifica os critérios de pesquisa dentro de uma <xref:System.ServiceModel.Discovery.FindCriteria> instância. Se os serviços correspondentes forem encontrados, <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> o retornará uma coleção de <xref:System.ServiceModel.Discovery.EndpointDiscoveryMetadata> . O código a seguir mostra como chamar o `Find` método e, em seguida, conectar-se a um serviço descoberto.  
  
```csharp  
class Client
{
    static EndpointAddress serviceAddress;
  
    static void Main()
    {  
        if (FindService())
        {
            InvokeService();
        }
    }  
  
    // ** DISCOVERY ** //  
    static bool FindService()  
    {  
        Console.WriteLine("\nFinding Calculator Service ..");  
        DiscoveryClient discoveryClient =
            new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
        Collection<EndpointDiscoveryMetadata> calculatorServices =
            (Collection<EndpointDiscoveryMetadata>)discoveryClient.Find(new FindCriteria(typeof(ICalculator))).Endpoints;  
  
        discoveryClient.Close();  
  
        if (calculatorServices.Count == 0)  
        {  
            Console.WriteLine("\nNo services are found.");  
            return false;  
        }  
        else  
        {  
            serviceAddress = calculatorServices[0].Address;  
            return true;  
        }  
    }  
  
    static void InvokeService()  
    {  
        Console.WriteLine("\nInvoking Calculator Service at {0}\n", serviceAddress);  
  
        // Create a client  
        CalculatorClient client = new CalculatorClient();  
        client.Endpoint.Address = serviceAddress;  
        client.Add(10,3);  
    }
}  
```  
  
## <a name="discovery-and-message-level-security"></a>Descoberta e segurança em nível de mensagem  
 Ao usar a segurança em nível de mensagem, é necessário especificar um <xref:System.ServiceModel.EndpointIdentity> no ponto de extremidade de descoberta de serviço e uma correspondência <xref:System.ServiceModel.EndpointIdentity> no ponto de extremidade de descoberta do cliente. Para obter mais informações sobre segurança de nível de mensagem, consulte [segurança da mensagem](message-security-in-wcf.md).  
  
## <a name="discovery-and-web-hosted-services"></a>Descoberta e serviços hospedados na Web  
 Para que os serviços WCF sejam detectáveis, eles devem estar em execução. Os serviços WCF hospedados no IIS ou não são executados até que o IIS/WAS receba uma mensagem associada ao serviço, portanto, eles não podem ser detectáveis por padrão.  Há duas opções para tornar a descoberta de serviços hospedados na Web:  
  
1. Usar o recurso de início automático do Windows Server AppFabric  
  
2. Usar um proxy de descoberta para se comunicar em nome do serviço  
  
 O Windows Server AppFabric tem um recurso de início automático que permitirá que um serviço seja iniciado antes de receber qualquer mensagem. Com esse conjunto de início automático, um serviço hospedado do IIS/WAS pode ser configurado para ser detectável. Para obter mais informações sobre o recurso de início automático, consulte o [recurso de início automático do Windows Server AppFabric](https://docs.microsoft.com/previous-versions/appfabric/ee677260(v=azure.10)). Além de ativar o recurso de início automático, você deve configurar o serviço para descoberta. Para obter mais informações, consulte [como: programaticamente adicionar detectabilidade a um serviço WCF e cliente](how-to-programmatically-add-discoverability-to-a-wcf-service-and-client.md)[Configurando a descoberta em um arquivo de configuração](configuring-discovery-in-a-configuration-file.md).  
  
 Um proxy de descoberta pode ser usado para se comunicar em nome do serviço WCF quando o serviço não estiver em execução. O proxy pode escutar a investigação ou resolver mensagens e responder ao cliente. O cliente pode enviar mensagens diretamente para o serviço. Quando o cliente envia uma mensagem para o serviço, ele será instanciado para responder à mensagem. Para obter mais informações sobre como implementar um proxy de descoberta, consulte [implementando um proxy de descoberta](implementing-a-discovery-proxy.md).  
  
> [!NOTE]
> Para que a descoberta do WCF funcione corretamente, todas as NICs (controlador de interface de rede) só devem ter um endereço IP.
