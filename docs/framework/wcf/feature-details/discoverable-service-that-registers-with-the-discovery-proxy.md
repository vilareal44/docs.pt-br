---
title: Como implementar um serviço de descoberta que registra usando o proxy de descoberta
ms.date: 03/30/2017
ms.assetid: eb275bc1-535b-44c8-b9f3-0b75e9aa473b
ms.openlocfilehash: bf878dff59a9a258567ff99098b0b3f8761194e2
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599224"
---
# <a name="how-to-implement-a-discoverable-service-that-registers-with-the-discovery-proxy"></a>Como implementar um serviço de descoberta que registra usando o proxy de descoberta
Este tópico é o segundo de quatro tópicos que discute como implementar um proxy de descoberta. No tópico anterior, [como implementar um proxy de descoberta](how-to-implement-a-discovery-proxy.md), você implementou um proxy de descoberta. Neste tópico, você cria um serviço WCF que envia mensagens de anúncio ( `Hello` e `Bye` ) para o proxy de descoberta, fazendo com que ele se registre e cancele seu registro com o proxy de descoberta.

### <a name="to-define-the-service-contract"></a>Para definir o contrato de serviço

1. Adicione um novo projeto de aplicativo de console à `DiscoveryProxyExample` solução chamada `Service` .

2. Adicione referências aos assemblies a seguir:

    1. System.ServiceModel

    2. System. ServiceModel. Discovery

3. Adicione uma nova classe ao projeto chamado `CalculatorService` .

4. Adicionar as seguintes instruções de uso.

    ```csharp
    using System;
    using System.ServiceModel;
    ```

5. Em CalculatorService.cs, defina o contrato de serviço.

    ```csharp
    // Define a service contract.
    [ServiceContract(Namespace = "http://Microsoft.Samples.Discovery")]
    public interface ICalculatorService
    {
        [OperationContract]
        double Add(double n1, double n2);
        [OperationContract]
        double Subtract(double n1, double n2);
        [OperationContract]
        double Multiply(double n1, double n2);
        [OperationContract]
        double Divide(double n1, double n2);
    }
    ```

6. Também em CalculatorService.cs, implemente o contrato de serviço.

    ```csharp
    // Service class which implements the service contract.
    public class CalculatorService : ICalculatorService
    {
        public double Add(double n1, double n2)
        {
            double result = n1 + n2;
            Console.WriteLine("Received Add({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Subtract(double n1, double n2)
        {
            double result = n1 - n2;
            Console.WriteLine("Received Subtract({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Multiply(double n1, double n2)
        {
            double result = n1 * n2;
            Console.WriteLine("Received Multiply({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Divide(double n1, double n2)
        {
            double result = n1 / n2;
            Console.WriteLine("Received Divide({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }
    }
    ```

### <a name="to-host-the-service"></a>Para hospedar o serviço

1. Abra o arquivo Program.cs que foi gerado quando você criou o projeto.

2. Adicionar as seguintes instruções de uso.

    ```csharp
    using System;
    using System.ServiceModel;
    using System.ServiceModel.Description;
    using System.ServiceModel.Discovery;
    ```

3. Dentro do `Main()` método, adicione o seguinte código:

    ```csharp
    // Define the base address of the service
    Uri baseAddress = new Uri("net.tcp://localhost:9002/CalculatorService/" + Guid.NewGuid().ToString());
    // Define the endpoint address where announcement messages will be sent
    Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");

    // Create the service host
    ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress);
    try
    {
        // Add a service endpoint
        ServiceEndpoint netTcpEndpoint = serviceHost.AddServiceEndpoint(typeof(ICalculatorService),
            new NetTcpBinding(), string.Empty);

        // Create an announcement endpoint, which points to the Announcement Endpoint hosted by the proxy service.
        AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(),
            new EndpointAddress(announcementEndpointAddress));

        // Create a ServiceDiscoveryBehavior and add the announcement endpoint
        ServiceDiscoveryBehavior serviceDiscoveryBehavior = new ServiceDiscoveryBehavior();
        serviceDiscoveryBehavior.AnnouncementEndpoints.Add(announcementEndpoint);

        // Add the ServiceDiscoveryBehavior to the service host to make the service discoverable
        serviceHost.Description.Behaviors.Add(serviceDiscoveryBehavior);

        // Start listening for messages
        serviceHost.Open();

        Console.WriteLine("Calculator Service started at {0}", baseAddress);
        Console.WriteLine();
        Console.WriteLine("Press <ENTER> to terminate the service.");
        Console.WriteLine();
        Console.ReadLine();

        serviceHost.Close();
    }
    catch (CommunicationException e)
    {
        Console.WriteLine(e.Message);
    }
    catch (TimeoutException e)
    {
        Console.WriteLine(e.Message);
    }

    if (serviceHost.State != CommunicationState.Closed)
    {
        Console.WriteLine("Aborting the service...");
        serviceHost.Abort();
    }
    ```

Você concluiu a implementação de um serviço detectável. Continue em [como: implementar um aplicativo cliente que usa o proxy de descoberta para encontrar um serviço](client-app-discovery-proxy-to-find-a-service.md).

## <a name="example"></a>Exemplo
 Esta é a lista completa do código usado neste tópico.

```csharp
// CalculatorService.cs
//----------------------------------------------------------------
// Copyright (c) Microsoft Corporation.  All rights reserved.
//----------------------------------------------------------------

using System;
using System.ServiceModel;

namespace Microsoft.Samples.Discovery
{
    // Define a service contract.
    [ServiceContract(Namespace = "http://Microsoft.Samples.Discovery")]
    public interface ICalculatorService
    {
        [OperationContract]
        double Add(double n1, double n2);
        [OperationContract]
        double Subtract(double n1, double n2);
        [OperationContract]
        double Multiply(double n1, double n2);
        [OperationContract]
        double Divide(double n1, double n2);
    }

    // Service class which implements the service contract.
    public class CalculatorService : ICalculatorService
    {
        public double Add(double n1, double n2)
        {
            double result = n1 + n2;
            Console.WriteLine("Received Add({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }
  
        public double Subtract(double n1, double n2)
        {
            double result = n1 - n2;
            Console.WriteLine("Received Subtract({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Multiply(double n1, double n2)
        {
            double result = n1 * n2;
            Console.WriteLine("Received Multiply({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }

        public double Divide(double n1, double n2)
        {
            double result = n1 / n2;
            Console.WriteLine("Received Divide({0},{1})", n1, n2);
            Console.WriteLine("Return: {0}", result);
            return result;
        }
    }
}
```

```csharp
// Program.cs
//----------------------------------------------------------------
// Copyright (c) Microsoft Corporation.  All rights reserved.
//----------------------------------------------------------------

using System;
using System.ServiceModel;
using System.ServiceModel.Description;
using System.ServiceModel.Discovery;

namespace Microsoft.Samples.Discovery
{
    class Program
    {
        public static void Main()
        {
            Uri baseAddress = new Uri("net.tcp://localhost:9002/CalculatorService/" + Guid.NewGuid().ToString());
            Uri announcementEndpointAddress = new Uri("net.tcp://localhost:9021/Announcement");

            ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService), baseAddress);
            try
            {
                ServiceEndpoint netTcpEndpoint = serviceHost.AddServiceEndpoint(typeof(ICalculatorService),
                    new NetTcpBinding(), string.Empty);

                // Create an announcement endpoint, which points to the Announcement Endpoint hosted by the proxy service.
                AnnouncementEndpoint announcementEndpoint = new AnnouncementEndpoint(new NetTcpBinding(),
                    new EndpointAddress(announcementEndpointAddress));

                ServiceDiscoveryBehavior serviceDiscoveryBehavior = new ServiceDiscoveryBehavior();
                serviceDiscoveryBehavior.AnnouncementEndpoints.Add(announcementEndpoint);

                // Make the service discoverable
                serviceHost.Description.Behaviors.Add(serviceDiscoveryBehavior);

                serviceHost.Open();

                Console.WriteLine("Calculator Service started at {0}", baseAddress);
                Console.WriteLine();
                Console.WriteLine("Press <ENTER> to terminate the service.");
                Console.WriteLine();
                Console.ReadLine();

                serviceHost.Close();
            }
            catch (CommunicationException e)
            {
                Console.WriteLine(e.Message);
            }
            catch (TimeoutException e)
            {
                Console.WriteLine(e.Message);
            }

            if (serviceHost.State != CommunicationState.Closed)
            {
                Console.WriteLine("Aborting the service...");
                serviceHost.Abort();
            }
        }
    }
}
```

## <a name="see-also"></a>Consulte também

- [Descoberta de WCF](wcf-discovery.md)
- [Como implementar um proxy de descoberta](how-to-implement-a-discovery-proxy.md)
- [Como implementar um aplicativo cliente que utiliza o proxy de descoberta para encontrar um serviço](client-app-discovery-proxy-to-find-a-service.md)
