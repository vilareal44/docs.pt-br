---
title: Como publicar metadados para um serviço usando um arquivo de configuração
description: Saiba como publicar metadados para um serviço WCF usando um arquivo de configuração. A publicação permite que os clientes obtenham esses metadados usando uma solicitação GET ou HTTP/GET.
ms.date: 03/30/2017
ms.assetid: f061443f-92df-4824-b36a-609c4cd14a17
ms.openlocfilehash: d5d425be7f02a204476c4f6e81441aca9ea39fcc
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246812"
---
# <a name="how-to-publish-metadata-for-a-service-using-a-configuration-file"></a>Como publicar metadados para um serviço usando um arquivo de configuração
Este é um dos dois tópicos de instruções que demonstram a publicação de metadados para um serviço Windows Communication Foundation (WCF). Há duas maneiras de especificar como um serviço deve publicar metadados, usando um arquivo de configuração e usando código. Este tópico mostra como publicar metadados para um serviço usando um arquivo de configuração.  
  
> [!CAUTION]
> Este tópico mostra como publicar metadados de maneira não segura. Qualquer cliente pode recuperar os metadados do serviço. Se você precisar que seu serviço publique metadados de maneira segura, consulte [ponto de extremidade de metadados seguro personalizado](../samples/custom-secure-metadata-endpoint.md).  
  
 Para obter mais informações sobre como publicar metadados no código, consulte [como publicar metadados para um serviço usando código](how-to-publish-metadata-for-a-service-using-code.md). A publicação de metadados permite que os clientes recuperem os metadados usando uma solicitação WS-Transfer GET ou uma solicitação HTTP/GET usando a `?wsdl` cadeia de caracteres de consulta. Para ter certeza de que o código está funcionando, crie um serviço WCF básico. Para simplificar, um serviço auto-hospedado básico é fornecido no código a seguir.  
  
```csharp  
using System;  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Description;  
  
namespace Metadata.Samples  
{  
    [ServiceContract]  
    public interface ISimpleService  
    {  
        [OperationContract]  
        string SimpleMethod(string msg);  
    }  
  
    class SimpleService : ISimpleService  
    {  
        public string SimpleMethod(string msg)  
        {  
            Console.WriteLine("The caller passed in " + msg);  
            return "Hello " + msg;  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ServiceHost host = new ServiceHost(typeof(SimpleService),  
                new Uri("http://localhost:8001/MetadataSample"));
            try  
            {  
                // Open the service host to accept incoming calls  
                host.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
  
                // Close the ServiceHostBase to shutdown the service.  
                host.Close();  
            }  
            catch (CommunicationException commProblem)  
            {  
                Console.WriteLine("There was a communication problem. " + commProblem.Message);  
                Console.Read();  
            }  
        }  
    }  
}  
```  
  
 Esse serviço é um serviço hospedado automaticamente, que é configurado usando um arquivo de configuração. O arquivo de configuração a seguir serve como ponto de partida.  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service name="Metadata.Example.SimpleService">  
        <endpoint address=""  
                  binding="basicHttpBinding"  
                  contract="Metadata.Example.ISimpleService" />  
      </service>  
    </services>  
    <behaviors>  
  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
### <a name="to-publish-metadata-for-a-wcf-service-using-an-application-configuration-file"></a>Para publicar metadados para um serviço WCF usando um arquivo de configuração de aplicativo  
  
1. Dentro do arquivo de App.config, após o `</services>` elemento de fechamento, crie um `<behaviors>` elemento.  

2. Dentro do elemento `<behaviors>`, adicione um elemento `<serviceBehaviors>`.  

3. Adicione um `<behavior>` elemento ao `<serviceBehaviors>` elemento e especifique um valor para o `name` atributo do `<behavior>` elemento.  

4. Adicione um `<serviceMetadata>` elemento ao `<behavior>` elemento. Defina o `httpGetEnabled` atributo como `true` e o `policyVersion` atributo como Policy15. `httpGetEnabled`permite que o serviço responda a solicitações de metadados feitas por uma solicitação HTTP GET. `policyVersion`instrui o serviço a estar em conformidade com o WS-Policy 1,5 ao gerar metadados.  

5. Adicione um `behaviorConfiguration` atributo ao `<service>` elemento e especifique o `name` atributo do `<behavior>` elemento adicionado na etapa 1, conforme mostrado no exemplo de código a seguir.  
  
    ```xml  
    <services>  
      <service  
          name="Metadata.Example.SimpleService"  
          behaviorConfiguration="SimpleServiceBehavior">  
        ...  
      </service>  
    </services>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="SimpleServiceBehavior">  
          <serviceMetadata httpGetEnabled="True" policyVersion="Policy15" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
6. Adicione um ou mais `<endpoint>` elementos com o contrato definido como `IMetadataExchange` , conforme mostrado no exemplo de código a seguir.  
  
    ```xml  
    <services>  
      <service  
          name="Metadata.Example.SimpleService"  
          behaviorConfiguration="SimpleServiceBehavior">  
  
        <endpoint address=""  
                  binding="wsHttpBinding"  
                  contract="Metadata.Example.ISimpleService" />  
  
        <endpoint address="mex"  
                  binding="mexHttpBinding"  
                  contract="IMetadataExchange" />  
      </service>  
    </services>  
    ```  
  
7. Para os pontos de extremidade de metadados adicionados na etapa anterior, defina o `binding` atributo como um dos seguintes:  
  
    - `mexHttpBinding`para publicação HTTP.  
  
    - `mexHttpsBinding`para publicação HTTPS.  
  
    - `mexNamedPipeBinding`para publicação de pipe nomeado.  
  
    - `mexTcpBinding`para publicação de TCP.  
  
8. Para os pontos de extremidade de metadados adicionados em uma etapa anterior, defina o endereço igual a:  
  
    - Uma cadeia de caracteres vazia para usar o endereço base do aplicativo host como o ponto de publicação se o endereço base for o mesmo que a associação de metadados.  
  
    - Um endereço relativo se o aplicativo host tiver um endereço base.  
  
    - Um endereço absoluto.  
  
9. Crie e execute o aplicativo de console.  
  
10. Use o Internet Explorer para navegar até o endereço base do serviço ( `http://localhost:8001/MetadataSample` neste exemplo) e verificar se a publicação de metadados está ativada. Caso contrário, uma mensagem na parte superior da página resultante será exibida: "a publicação de metadados para este serviço está atualmente desabilitada".  
  
### <a name="to-use-default-endpoints"></a>Para usar pontos de extremidade padrão  
  
1. Para configurar metadados em um serviço que usa pontos de extremidade padrão, especifique o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> no arquivo de configuração como no exemplo anterior, mas não especifique nenhum ponto de extremidade. O arquivo de configuração teria a seguinte aparência.  
  
    ```xml  
    <configuration>  
      <system.serviceModel>  
        <behaviors>  
          <serviceBehaviors>  
            <behavior name="SimpleServiceBehavior">  
              <serviceMetadata httpGetEnabled="True" policyVersion="Policy12" />  
            </behavior>  
          </serviceBehaviors>  
        </behaviors>  
  
      </system.serviceModel>  
    </configuration>  
    ```  
  
     Como o serviço tem um <xref:System.ServiceModel.Description.ServiceMetadataBehavior> com o `httpGetEnabled` definido como `true` , o serviço tem metadados de publicação habilitados e, como nenhum ponto de extremidade foi explicitamente adicionado, o tempo de execução adiciona os pontos de extremidade padrão. Para obter mais informações sobre pontos de extremidade, associações e comportamentos padrão, confira [Configuração simplificada](../simplified-configuration.md) e [Configuração simplificada para serviços WCF](../samples/simplified-configuration-for-wcf-services.md).  
  
## <a name="example"></a>Exemplo  
 O exemplo de código a seguir mostra a implementação de um serviço WCF básico e o arquivo de configuração que publica metadados para o serviço.  
  
```csharp  
using System;  
using System.Runtime.Serialization;  
using System.ServiceModel;  
using System.ServiceModel.Description;  
  
namespace Metadata.Samples  
{  
    [ServiceContract]  
    public interface ISimpleService  
    {  
        [OperationContract]  
        string SimpleMethod(string msg);  
    }  
  
    class SimpleService : ISimpleService  
    {  
        public string SimpleMethod(string msg)  
        {  
            Console.WriteLine("The caller passed in " + msg);  
            return "Hello " + msg;  
        }  
    }  
  
    class Program  
    {  
        static void Main(string[] args)  
        {  
            ServiceHost host = new ServiceHost(typeof(SimpleService),  
                new Uri("http://localhost:8001/MetadataSample"));
            try  
            {  
                // Open the service host to accept incoming calls  
                host.Open();  
  
                // The service can now be accessed.  
                Console.WriteLine("The service is ready.");  
                Console.WriteLine("Press <ENTER> to terminate service.");  
                Console.WriteLine();  
                Console.ReadLine();  
  
                // Close the ServiceHostBase to shutdown the service.  
                host.Close();  
            }  
            catch (CommunicationException commProblem)  
            {  
                Console.WriteLine("There was a communication problem. " + commProblem.Message);  
                Console.Read();  
            }  
        }  
    }  
}  
```  
  
```xml  
<configuration>  
  <system.serviceModel>  
    <behaviors>  
      <serviceBehaviors>  
        <behavior name="SimpleServiceBehavior">  
          <serviceMetadata httpGetEnabled="True" policyVersion="Policy12" />  
          <serviceDebug includeExceptionDetailInFaults="False" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="see-also"></a>Veja também

- <xref:System.ServiceModel.Description.ServiceMetadataBehavior>
- [Como hospedar um serviço do WCF em um aplicativo gerenciado](../how-to-host-a-wcf-service-in-a-managed-application.md)
- [Self-Host](../samples/self-host.md)
- [Visão geral da arquitetura de metadados](metadata-architecture-overview.md)
- [Utilizando metadados](using-metadata.md)
- [Como publicar metadados utilizando código para um serviço](how-to-publish-metadata-for-a-service-using-code.md)
