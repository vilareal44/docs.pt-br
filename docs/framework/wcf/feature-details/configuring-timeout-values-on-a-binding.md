---
title: Configurando valores de tempo limite em uma associação
description: Saiba como gerenciar configurações de tempo limite para associações do WCF para melhorar o desempenho, a usabilidade e a segurança do seu serviço.
ms.date: 03/30/2017
ms.assetid: b5c825a2-b48f-444a-8659-61751ff11d34
ms.openlocfilehash: c41824a242d9b42290183cd70b9acf5b8ee59e6b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245109"
---
# <a name="configuring-timeout-values-on-a-binding"></a>Configurando valores de tempo limite em uma associação
Há várias configurações de tempo limite disponíveis nas associações do WCF. Definir essas configurações de tempo limite corretamente pode melhorar não apenas o desempenho do serviço, mas também desempenhar uma função na usabilidade e na segurança do seu serviço. Os seguintes tempos limite estão disponíveis em associações WCF:  
  
1. OpenTimeout  
  
2. CloseTimeout  
  
3. SendTimeout  
  
4. ReceiveTimeout  
  
## <a name="wcf-binding-timeouts"></a>Tempos limite de associação do WCF  
 Cada uma das configurações discutidas neste tópico é feita na própria associação, seja no código ou na configuração. O código a seguir mostra como definir os tempos limite de forma programática em uma associação do WCF no contexto de um serviço hospedado automaticamente.  
  
```csharp  
public static void Main()
{
    Uri baseAddress = new Uri("http://localhost/MyServer/MyService");

    try
    {
        ServiceHost serviceHost = new ServiceHost(typeof(CalculatorService));

        WSHttpBinding binding = new WSHttpBinding();
        binding.OpenTimeout = new TimeSpan(0, 10, 0);
        binding.CloseTimeout = new TimeSpan(0, 10, 0);
        binding.SendTimeout = new TimeSpan(0, 10, 0);
        binding.ReceiveTimeout = new TimeSpan(0, 10, 0);

        serviceHost.AddServiceEndpoint("ICalculator", binding, baseAddress);
        serviceHost.Open();

        // The service can now be accessed.
        Console.WriteLine("The service is ready.");
        Console.WriteLine("Press <ENTER> to terminate service.");
        Console.WriteLine();
        Console.ReadLine();
    }
    catch (CommunicationException ex)
    {
        // Handle exception ...
    }
}
```  
  
 O exemplo a seguir mostra como configurar tempos limite em uma associação em um arquivo de configuração.  
  
```xml  
<configuration>
  <system.serviceModel>
    <bindings>
      <wsHttpBinding>
        <binding openTimeout="00:10:00"
                 closeTimeout="00:10:00"
                 sendTimeout="00:10:00"
                 receiveTimeout="00:10:00">
        </binding>
      </wsHttpBinding>
    </bindings>
  </system.serviceModel>
</configuration>
```  
  
 Mais informações sobre essas configurações podem ser encontradas na documentação da <xref:System.ServiceModel.Channels.Binding> classe.  
  
### <a name="client-side-timeouts"></a>Tempos limite do lado do cliente  
 No lado do cliente:  
  
1. SendTimeout – usado para inicializar o OperationTimeout, que governa todo o processo de envio de uma mensagem, incluindo o recebimento de uma mensagem de resposta para uma operação de serviço de solicitação/resposta. Esse tempo limite também se aplica ao enviar mensagens de resposta de um método de contrato de retorno de chamada.  
  
2. OpenTimeout – usado ao abrir canais quando nenhum valor de tempo limite explícito é especificado.  
  
3. CloseTimeout – usado ao fechar canais quando nenhum valor de tempo limite explícito é especificado.  
  
4. ReceiveTimeout – não é usado.  
  
### <a name="service-side-timeouts"></a>Tempos limite do lado do serviço  
 No lado do serviço:  
  
1. SendTimeout, OpenTimeout, CloseTimeout são os mesmos que no cliente.  
  
2. ReceiveTimeout – usado pela camada do Service Framework para inicializar o tempo limite de ociosidade da sessão que controla por quanto tempo uma sessão pode ficar ociosa antes de atingir o tempo limite.
