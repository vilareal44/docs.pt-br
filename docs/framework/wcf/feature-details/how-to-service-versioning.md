---
title: Como controlar a versão de serviço
ms.date: 03/30/2017
ms.assetid: 4287b6b3-b207-41cf-aebe-3b1d4363b098
ms.openlocfilehash: beb7de63d300ad7986bfc59093006b074b9456ba
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84586930"
---
# <a name="how-to-service-versioning"></a>Como controlar a versão de serviço
Este tópico descreve as etapas básicas necessárias para criar uma configuração de roteamento que roteia mensagens para versões diferentes do mesmo serviço. Neste exemplo, as mensagens são roteadas para duas versões diferentes de um serviço de calculadora, `roundingCalc` (v1) e `regularCalc` (v2). Ambas as implementações dão suporte às mesmas operações; no entanto, o serviço mais antigo, `roundingCalc` , Arredonda todos os cálculos para o valor inteiro mais próximo antes de retornar. Um aplicativo cliente deve ser capaz de indicar se deseja usar o serviço mais recente `regularCalc` .  
  
> [!WARNING]
> Para rotear uma mensagem para uma versão de serviço específica, o serviço de roteamento deve ser capaz de determinar o destino da mensagem com base no conteúdo da mensagem. No método demonstrado abaixo, o cliente especificará a versão inserindo informações em um cabeçalho de mensagem. Há métodos de controle de versão de serviço que não exigem que os clientes passem dados adicionais. Por exemplo, uma mensagem pode ser roteada para a versão mais recente ou mais compatível de um serviço ou o roteador pode usar uma parte do envelope SOAP padrão.  
  
 As operações expostas por ambos os serviços são:  
  
- Adicionar  
  
- Subtrair  
  
- Multiplicar  
  
- Dividir  
  
 Como ambas as implementações de serviço manipulam as mesmas operações e são, essencialmente, idênticas além dos dados que elas retornam, os dados base contidos nas mensagens enviadas de aplicativos cliente não são exclusivos o suficiente para permitir que você determine como encaminhar a solicitação. Por exemplo, os filtros de ação não podem ser usados porque as ações padrão para ambos os serviços são as mesmas.  
  
 Isso pode ser resolvido de várias maneiras, como expor um ponto de extremidade específico no roteador para cada versão do serviço ou adicionar um elemento de cabeçalho personalizado à mensagem para indicar a versão do serviço.  Cada uma dessas abordagens permite que você roteie exclusivamente as mensagens de entrada para uma versão específica do serviço, mas o uso de conteúdo de mensagem exclusivo é o método preferencial de diferenciação de solicitações para versões de serviço diferentes.  
  
 Neste exemplo, o aplicativo cliente adiciona o cabeçalho personalizado ' CalcVer ' à mensagem de solicitação. Esse cabeçalho conterá um valor que indica a versão do serviço para a qual a mensagem deve ser roteada. Um valor de ' 1 ' indica que a mensagem deve ser processada pelo serviço roundingCalc, enquanto um valor de ' 2 ' indica o serviço regularCalc. Isso permite que o aplicativo cliente controle diretamente qual versão do serviço processará a mensagem.  Como o cabeçalho personalizado é um valor contido na mensagem, você pode usar um ponto de extremidade para receber mensagens destinadas a ambas as versões do serviço. O código a seguir pode ser usado no aplicativo cliente para adicionar esse cabeçalho personalizado à mensagem:  
  
```csharp  
messageHeadersElement.Add(MessageHeader.CreateHeader("CalcVer", "http://my.custom.namespace/", "2"));  
```  
  
### <a name="implement-service-versioning"></a>Implementar o controle de versão de serviço  
  
1. Crie a configuração básica do serviço de roteamento especificando o ponto de extremidade de serviço exposto pelo serviço. O exemplo a seguir define um único ponto de extremidade de serviço, que será usado para receber mensagens. Ele também define os pontos de extremidade do cliente que serão usados para enviar mensagens aos `roundingCalc` Serviços (v1) e `regularCalc` (v2).  
  
    ```xml  
    <services>  
        <service behaviorConfiguration="routingConfiguration"  
                 name="System.ServiceModel.Routing.RoutingService">  
          <host>  
            <baseAddresses>  
              <add baseAddress="http://localhost/routingservice/router" />  
            </baseAddresses>  
          </host>  
          <!--Set up the inbound endpoint for the Routing Service-->  
          <endpoint address="calculator"  
                    binding="wsHttpBinding"  
                    name="routerEndpoint"  
                    contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
        </service>  
    </services>  
    <client>  
    <!--set up the destination endpoints-->  
          <endpoint name="regularCalcEndpoint"  
                    address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                    binding="netTcpBinding"  
                    contract="*" />  
  
          <endpoint name="roundingCalcEndpoint"  
                    address="http://localhost:8080/servicemodelsamples/service/"  
                    binding="wsHttpBinding"  
                    contract="*" />  
        </client>  
    ```  
  
2. Defina os filtros usados para rotear mensagens para os pontos de extremidade de destino.  Para este exemplo, o filtro XPath é usado para detectar o valor do cabeçalho personalizado "CalcVer" para determinar a qual versão a mensagem deve ser roteada. Um filtro XPath também é usado para detectar mensagens que não contêm o cabeçalho "CalcVer". O exemplo a seguir define os filtros e a tabela de namespace necessários.  
  
    ```xml  
    <!-- use the namespace table element to define a prefix for our custom namespace-->  
    <namespaceTable>  
      <add prefix="custom" namespace="http://my.custom.namespace/"/>  
    </namespaceTable>  
    <filters>  
      <!--define the different message filters-->  
      <!--define an xpath message filter to look for the  
          custom header containing a value of 2-->  
      <filter name="XPathFilterRegular" filterType="XPath"  
              filterData="sm:header()/custom:CalcVer = '2'"/>  
      <!--define an xpath message filter to look for the  
          custom header containing a value of 1-->  
      <filter name="XPathFilterRounding" filterType="XPath"  
              filterData="sm:header()/custom:CalcVer = '1'"/>  
       <!--define an xpath message filter to look for  
           messages that do not contain the custom header-->  
       <filter name="XPathFilterNoHeader" filterType="XPath"  
               filterData="count(sm:header()/custom:CalcVer)=0"/>  
    </filters>
    ```  
  
    > [!NOTE]
    > O prefixo do namespace S12 é definido por padrão na tabela de namespace e representa o namespace `http://www.w3.org/2003/05/soap-envelope` .
  
3. Defina a tabela de filtros, que associa cada filtro a um ponto de extremidade do cliente. Se a mensagem contiver o cabeçalho "CalcVer" com um valor de 1, ele será enviado para o serviço regularCalc. Se o cabeçalho contiver um valor de 2, ele será enviado para o serviço roundingCalc. Se nenhum cabeçalho estiver presente, a mensagem será roteada para o regularCalc.  
  
     O seguinte define a tabela de filtros e adiciona os filtros definidos anteriormente.  
  
    ```xml  
    <filterTables>  
      <filterTable name="filterTable1">  
          <!--add the filters to the message filter table-->  
          <!--look for the custom header = 1, and if we find it,  
              send the message to the rounding calc endpoint-->  
          <add filterName="XPathFilterRounding" endpointName="roundingCalcEndpoint"/>  
          <!--look for the custom header = 2, and if we find it,  
              send the message to the rounding calc endpoint-->  
          <add filterName="XPathFilterRegular" endpointName="regularCalcEndpoint"/>  
          <!--look for the absence of the custom header, and if  
              it is not present, assume the v1 endpoint-->  
          <add filterName="XPathFilterNoHeader" endpointName="roundingCalcEndpoint"/>  
      </filterTable>  
    </filterTables>  
    ```  
  
4. Para avaliar as mensagens de entrada em relação aos filtros contidos na tabela de filtros, você deve associar a tabela de filtros aos pontos de extremidade de serviço usando o comportamento de roteamento. O exemplo a seguir demonstra a associação `filterTable1` com os pontos de extremidade de serviço:  
  
    ```xml  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
    ```  
  
## <a name="example"></a>Exemplo  
 A seguir está uma lista completa do arquivo de configuração.  
  
```xml  
<?xml version="1.0" encoding="utf-8" ?>  
<!-- Copyright (c) Microsoft Corporation. All rights reserved -->  
<configuration>  
  <system.serviceModel>  
    <services>  
      <service behaviorConfiguration="routingConfiguration"  
               name="System.ServiceModel.Routing.RoutingService">  
        <host>  
          <baseAddresses>  
            <add baseAddress="http://localhost/routingservice/router" />  
          </baseAddresses>  
        </host>  
        <!--Set up the inbound endpoint for the Routing Service-->  
        <endpoint address="calculator"  
                  binding="wsHttpBinding"  
                  name="routerEndpoint"  
                  contract="System.ServiceModel.Routing.IRequestReplyRouter" />  
      </service>  
    </services>  
    <behaviors>  
      <!--default routing service behavior definition-->  
      <serviceBehaviors>  
        <behavior name="routingConfiguration">  
          <routing filterTableName="filterTable1" />  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  
    <client>  
<!--set up the destination endpoints-->  
      <endpoint name="regularCalcEndpoint"  
                address="net.tcp://localhost:9090/servicemodelsamples/service/"  
                binding="netTcpBinding"  
                contract="*" />  
  
      <endpoint name="roundingCalcEndpoint"  
                address="http://localhost:8080/servicemodelsamples/service/"  
                binding="wsHttpBinding"  
                contract="*" />  
    </client>  
    <routing>  
      <!-- use the namespace table element to define a prefix for our custom namespace-->  
      <namespaceTable>  
        <add prefix="custom" namespace="http://my.custom.namespace/"/>  
      </namespaceTable>  
      <filters>  
        <!--define the different message filters-->  
        <!--define an xpath message filter to look for the  
            custom header containing a value of 2-->  
        <filter name="XPathFilterRegular" filterType="XPath"  
                filterData="sm:header()/custom:CalcVer = '2'"/>  
        <!--define an xpath message filter to look for the  
            custom header containing a value of 1-->  
        <filter name="XPathFilterRounding" filterType="XPath"  
                filterData="sm:header()/custom:CalcVer = '1'"/>  
        <!--define an xpath message filter to look for  
            messages that do not contain the custom header-->  
        <filter name="XPathFilterNoHeader" filterType="XPath"  
                filterData="count(sm:header()/custom:CalcVer)=0"/>  
      </filters>  
      <filterTables>  
        <filterTable name="filterTable1">  
            <!--add the filters to the message filter table-->  
            <!--look for the custom header = 1, and if we find it,  
                send the message to the rounding calc endpoint-->  
            <add filterName="XPathFilterRounding" endpointName="roundingCalcEndpoint"/>  
            <!--look for the custom header = 2, and if we find it,  
                send the message to the rounding calc endpoint-->  
            <add filterName="XPathFilterRegular" endpointName="regularCalcEndpoint"/>  
            <!--look for the absence of the custom header, and if  
                it is not present, assume the v1 endpoint-->  
            <add filterName="XPathFilterNoHeader" endpointName="roundingCalcEndpoint"/>  
        </filterTable>  
      </filterTables>  
    </routing>  
  </system.serviceModel>  
</configuration>  
```  
  
## <a name="example"></a>Exemplo  
 A seguir está uma lista completa do aplicativo cliente.  
  
```csharp  
using System;  
using System.ServiceModel;  
using System.ServiceModel.Channels;  
  
namespace Microsoft.Samples.AdvancedFilters  
{  
    //The service contract is defined in generatedClient.cs, generated from the service by the svcutil tool.  
  
    //Client implementation code.  
    class Client  
    {  
        static void Main()  
        {  
            //Print out the welcome text  
            Console.WriteLine("This sample routes the Calculator Sample through the new WCF RoutingService");  
            Console.WriteLine("Wait for all the services to indicate that they've started, then press");  
            Console.WriteLine("<ENTER> to start the client.");  
  
            while (Console.ReadLine() != "quit")  
            {  
                //Offer the Address configuration for the client  
                Console.WriteLine("");  
                Console.WriteLine("Welcome to the Calculator Client!");  
  
                EndpointAddress epa;  
                //set the default address as the general router endpoint  
                epa = new EndpointAddress("http://localhost/routingservice/router/calculator");  
  
                //set up the CalculatorClient with the EndpointAddress, the WSHttpBinding, and the ICalculator contract  
                //We use the WSHttpBinding so that the outgoing has a message envelope, which we'll manipulate in a minute  
                CalculatorClient client = new CalculatorClient(new WSHttpBinding(), epa);  
                //client.Endpoint.Contract = ContractDescription.GetContract(typeof(ICalculator));  
  
                //Ask the customer if they want to add a custom header to the outgoing message.  
                //The Router will look for this header, and if so ignore the endpoint the message was  
                //received on, and instead direct the message to the RoundingCalcService.  
                Console.WriteLine("");  
                Console.WriteLine("Which calculator service should be used?");  
                Console.WriteLine("Enter 1 for the rounding calculator, 2 for the regular calculator.");  
                Console.WriteLine("[1] or [2]?");  
  
                string header = Console.ReadLine();  
  
                //get the current operationContextScope from the client's inner channel  
                using (OperationContextScope ocs = new OperationContextScope((client.InnerChannel)))  
                {  
                    //get the outgoing message headers element (collection) from the context  
                    MessageHeaders messageHeadersElement = OperationContext.Current.OutgoingMessageHeaders;  
  
                    //if they wanted to create the header, go ahead and add it to the outgoing message  
                    if (header != null && (header=="1" || header=="2"))  
                    {  
                        //create a new header "RoundingCalculator", no specific namespace, and set the value to
                        //the value of header.  
                        //the Routing Service will look for this header in order to determine if the message  
                        //should be routed to the RoundingCalculator  
                        messageHeadersElement.Add(MessageHeader.CreateHeader("CalcVer", "http://my.custom.namespace/", header));  
                    }  
                    else //incorrect choice, no header added  
                    {  
                        Console.WriteLine("Incorrect value entered, not adding a header");  
                    }  
  
                        //call the client operations  
                        CallClient(client);  
                }  
  
                //close the client to clean it up  
                client.Close();  
                Console.WriteLine();  
                Console.WriteLine("Press <ENTER> to run the client again or type 'quit' to quit.");  
            }  
        }  
  
        private static void CallClient(CalculatorClient client)  
        {  
            Console.WriteLine("");  
            Console.WriteLine("Sending!");  
            // Call the Add service operation.  
            double value1 = 100.00D;  
            double value2 = 15.99D;  
            double result = client.Add(value1, value2);  
            Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Subtract service operation.  
            value1 = 145.00D;  
            value2 = 76.54D;  
            result = client.Subtract(value1, value2);  
            Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Multiply service operation.  
            value1 = 9.00D;  
            value2 = 81.25D;  
            result = client.Multiply(value1, value2);  
            Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
            // Call the Divide service operation.  
            value1 = 22.00D;  
            value2 = 7.00D;  
            result = client.Divide(value1, value2);  
            Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
        }  
    }  
}  
```  
  
## <a name="see-also"></a>Consulte também

- [Serviços de roteamento](../samples/routing-services.md)
