---
title: Como adicionar programaticamente a capacidade de descoberta para um cliente e serviço do WCF
ms.date: 03/30/2017
ms.assetid: 4f7ae7ab-6fc8-4769-9730-c14d43f7b9b1
ms.openlocfilehash: c28815d1d208d3e91785a13d95e03c09c0f02ed9
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596988"
---
# <a name="how-to-programmatically-add-discoverability-to-a-wcf-service-and-client"></a>Como adicionar programaticamente a capacidade de descoberta para um cliente e serviço do WCF
Este tópico explica como tornar um serviço Windows Communication Foundation (WCF) detectável. Ele se baseia no exemplo de [hospedagem interna](https://docs.microsoft.com/dotnet/framework/wcf/samples/self-host) .  
  
### <a name="to-configure-the-existing-self-host-service-sample-for-discovery"></a>Para configurar o exemplo de serviço de auto-host existente para descoberta  
  
1. Abra a solução de hospedagem interna no Visual Studio 2012. O exemplo está localizado no diretório TechnologySamples\Basic\Service\Hosting\SelfHost  
  
2. Adicione uma referência ao `System.ServiceModel.Discovery.dll` projeto de serviço do. Você pode ver uma mensagem de erro dizendo "System. ServiceModel. Discovery. dll ou uma de suas dependências requer uma versão mais recente do .NET Framework do que aquela especificada no projeto... " Se você vir essa mensagem, clique com o botão direito do mouse no projeto no Gerenciador de Soluções e escolha **Propriedades**. Na janela **Propriedades do projeto** , verifique se a **estrutura de destino** é [!INCLUDE[netfx_current_long](../../../../includes/netfx-current-long-md.md)] .  
  
3. Abra o arquivo Service.cs e adicione a instrução a seguir `using` .  
  
    ```csharp  
    using System.ServiceModel.Discovery;  
    ```  
  
4. No `Main()` método, dentro da `using` instrução, adicione uma <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> instância ao host de serviço.  
  
    ```csharp  
    public static void Main()  
    {  
        // Create a ServiceHost for the CalculatorService type.  
        using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
        {  
            // Add a ServiceDiscoveryBehavior  
            serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());
  
            // ...  
        }  
    }  
    ```  
  
     O <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> especifica que o serviço ao qual ele é aplicado é detectável.  
  
5. Adicione um <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> ao host de serviço logo após o código que adiciona o <xref:System.ServiceModel.Discovery.ServiceDiscoveryBehavior> .  
  
    ```csharp  
    // Add ServiceDiscoveryBehavior  
    serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
  
    // Add a UdpDiscoveryEndpoint  
    serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
    ```  
  
     Esse código especifica que as mensagens de descoberta devem ser enviadas para o ponto de extremidade de descoberta UDP padrão.  
  
### <a name="to-create-a-client-application-that-uses-discovery-to-call-the-service"></a>Para criar um aplicativo cliente que usa a descoberta para chamar o serviço  
  
1. Adicione um novo aplicativo de console à solução chamada `DiscoveryClientApp` .  
  
2. Adicione uma referência a `System.ServiceModel.dll` e`System.ServiceModel.Discovery.dll`  
  
3. Copie os arquivos GeneratedClient.cs e app. config do projeto de cliente existente para o novo projeto DiscoveryClientApp. Para fazer isso, clique com o botão direito do mouse nos arquivos no **Gerenciador de soluções**, selecione **copiar**e, em seguida, selecione o projeto **DiscoveryClientApp** , clique com o botão direito do mouse e selecione **colar**.  
  
4. Abra Program.cs.  
  
5. Adicione as seguintes declarações de `using`.  
  
    ```csharp  
    using System.ServiceModel;  
    using System.ServiceModel.Discovery;  
    using Microsoft.ServiceModel.Samples;  
    ```  
  
6. Adicione um método estático chamado `FindCalculatorServiceAddress()` à `Program` classe.  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
    }  
    ```  
  
     Esse método usa a descoberta para pesquisar o `CalculatorService` serviço.  
  
7. Dentro do `FindCalculatorServiceAddress` método, crie uma nova <xref:System.ServiceModel.Discovery.DiscoveryClient> instância, passando uma <xref:System.ServiceModel.Discovery.UdpDiscoveryEndpoint> para o construtor.  
  
    ```csharp  
    static EndpointAddress FindCalculatorServiceAddress()  
    {  
        // Create DiscoveryClient  
        DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
    }  
    ```  
  
     Isso informa ao WCF que a <xref:System.ServiceModel.Discovery.DiscoveryClient> classe deve usar o ponto de extremidade de descoberta UDP padrão para enviar e receber mensagens de descoberta.  
  
8. Na próxima linha, chame o <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> método e especifique uma <xref:System.ServiceModel.Discovery.FindCriteria> instância que contenha o contrato de serviço que você deseja pesquisar. Nesse caso, especifique `ICalculator` .  
  
    ```csharp  
    // Find ICalculatorService endpoints
    FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
    ```  
  
9. Após a chamada para <xref:System.ServiceModel.Discovery.DiscoveryClient.Find%2A> , verifique se há pelo menos um serviço de correspondência e retorne o <xref:System.ServiceModel.EndpointAddress> do primeiro serviço de correspondência. Caso contrário, retorna `null` .  
  
    ```csharp  
    if (findResponse.Endpoints.Count > 0)  
    {  
        return findResponse.Endpoints[0].Address;  
    }  
    else  
    {  
        return null;  
    }  
    ```  
  
10. Adicione um método estático chamado `InvokeCalculatorService` à `Program` classe.  
  
    ```csharp  
    static void InvokeCalculatorService(EndpointAddress endpointAddress)  
    {  
    }  
    ```  
  
     Esse método usa o endereço do ponto de extremidade retornado de `FindCalculatorServiceAddress` para chamar o serviço de calculadora.  
  
11. Dentro do `InvokeCalculatorService` método, crie uma instância da `CalculatorServiceClient` classe. Essa classe é definida pelo exemplo de [hospedagem interna](https://docs.microsoft.com/dotnet/framework/wcf/samples/self-host) . Ele foi gerado usando svcutil. exe.  
  
    ```csharp  
    // Create a client  
    CalculatorClient client = new CalculatorClient();  
    ```  
  
12. Na próxima linha, defina o endereço do ponto de extremidade do cliente para o endereço do ponto de extremidade retornado de `FindCalculatorServiceAddress()` .  
  
    ```csharp  
    // Connect to the discovered service endpoint  
    client.Endpoint.Address = endpointAddress;  
    ```  
  
13. Imediatamente após o código da etapa anterior, chame os métodos expostos pelo serviço de calculadora.  
  
    ```csharp  
    Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
    double value1 = 100.00D;  
    double value2 = 15.99D;  
  
    // Call the Add service operation.  
    double result = client.Add(value1, value2);  
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Subtract service operation.  
    result = client.Subtract(value1, value2);  
    Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Multiply service operation.  
    result = client.Multiply(value1, value2);  
    Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
    // Call the Divide service operation.  
    result = client.Divide(value1, value2);  
    Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
    Console.WriteLine();  
  
    //Closing the client gracefully closes the connection and cleans up resources  
    client.Close();  
    ```  
  
14. Adicione o código ao `Main()` método na `Program` classe a ser chamada `FindCalculatorServiceAddress` .  
  
    ```csharp  
    public static void Main()  
    {  
        EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
    }  
    ```  
  
15. Na próxima linha, chame `InvokeCalculatorService()` e passe o endereço do ponto de extremidade retornado de `FindCalculatorServiceAddress()` .  
  
    ```csharp  
    if (endpointAddress != null)  
    {  
        InvokeCalculatorService(endpointAddress);  
    }  
  
    Console.WriteLine("Press <ENTER> to exit.");  
    Console.ReadLine();  
    ```  
  
### <a name="to-test-the-application"></a>Para testar o aplicativo  
  
1. Abra um prompt de comando com privilégios elevados e execute Service. exe.  
  
2. Abra um prompt de comando e execute Discoveryclientapp. exe.  
  
3. A saída de Service. exe deve ser parecida com a saída a seguir.  
  
    ```output  
    Received Add(100,15.99)  
    Return: 115.99  
    Received Subtract(100,15.99)  
    Return: 84.01  
    Received Multiply(100,15.99)  
    Return: 1599  
    Received Divide(100,15.99)  
    Return: 6.25390869293308  
    ```  
  
4. A saída de Discoveryclientapp. exe deve ser semelhante à saída a seguir.  
  
    ```output  
    Invoking CalculatorService at http://localhost:8000/ServiceModelSamples/service  
    Add(100,15.99) = 115.99  
    Subtract(100,15.99) = 84.01  
    Multiply(100,15.99) = 1599  
    Divide(100,15.99) = 6.25390869293308  
  
    Press <ENTER> to exit.  
    ```  
  
## <a name="example"></a>Exemplo  
 Veja a seguir uma lista do código para este exemplo. Como esse código é baseado no exemplo de [hospedagem interna](https://docs.microsoft.com/dotnet/framework/wcf/samples/self-host) , somente os arquivos que são alterados são listados. Para obter mais informações sobre o exemplo de hospedagem interna, consulte [instruções de instalação](https://docs.microsoft.com/dotnet/framework/wcf/samples/set-up-instructions).  
  
```csharp  
// Service.cs  
using System;  
using System.Configuration;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
  
namespace Microsoft.ServiceModel.Samples  
{  
    // See SelfHost sample for service contract and implementation  
    // ...  
  
        // Host the service within this EXE console application.  
        public static void Main()  
        {  
            // Create a ServiceHost for the CalculatorService type.  
            using (ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService)))  
            {  
                // Add the ServiceDiscoveryBehavior to make the service discoverable  
                serviceHost.Description.Behaviors.Add(new ServiceDiscoveryBehavior());  
                serviceHost.AddServiceEndpoint(new UdpDiscoveryEndpoint());  
  
                // Open the ServiceHost to create listeners and start listening for messages.  
                serviceHost.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
            }  
        }  
    }  
}  
```  
  
```csharp  
// Program.cs  
using System;  
using System.Collections.Generic;  
using System.Linq;  
using System.ServiceModel;  
using System.ServiceModel.Discovery;  
using Microsoft.ServiceModel.Samples;  
using System.Text;  
  
namespace DiscoveryClientApp  
{  
    class Program  
    {  
        static EndpointAddress FindCalculatorServiceAddress()  
        {  
            // Create DiscoveryClient  
            DiscoveryClient discoveryClient = new DiscoveryClient(new UdpDiscoveryEndpoint());  
  
            // Find ICalculatorService endpoints
            FindResponse findResponse = discoveryClient.Find(new FindCriteria(typeof(ICalculator)));  
  
            if (findResponse.Endpoints.Count > 0)  
            {  
                return findResponse.Endpoints[0].Address;  
            }  
            else  
            {  
                return null;  
            }  
        }  
  
        static void InvokeCalculatorService(EndpointAddress endpointAddress)  
        {  
            // Create a client  
            CalculatorClient client = new CalculatorClient();  
  
            // Connect to the discovered service endpoint  
            client.Endpoint.Address = endpointAddress;  
  
            Console.WriteLine("Invoking CalculatorService at {0}", endpointAddress);  
  
            double value1 = 100.00D;  
            double value2 = 15.99D;  
  
            // Call the Add service operation.  
            double result = client.Add(value1, value2);  
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Subtract service operation.  
            result = client.Subtract(value1, value2);  
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Multiply service operation.  
            result = client.Multiply(value1, value2);  
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Divide service operation.  
            result = client.Divide(value1, value2);  
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
            Console.WriteLine();  
  
            //Closing the client gracefully closes the connection and cleans up resources  
            client.Close();  
        }  
        static void Main(string[] args)  
        {  
            EndpointAddress endpointAddress = FindCalculatorServiceAddress();  
  
            if (endpointAddress != null)  
            {  
                InvokeCalculatorService(endpointAddress);  
            }  
  
            Console.WriteLine("Press <ENTER> to exit.");  
            Console.ReadLine();  
  
        }  
    }  
}  
```  

## <a name="see-also"></a>Consulte também

- [Visão geral de descoberta do WCF](wcf-discovery-overview.md)
- [Modelo de objeto de descoberta do WCF](wcf-discovery-object-model.md)
