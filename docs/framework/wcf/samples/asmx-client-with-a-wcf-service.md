---
title: Cliente ASMX com um serviço do WCF
ms.date: 03/30/2017
ms.assetid: 3ea381ee-ac7d-4d62-8c6c-12dc3650879f
ms.openlocfilehash: fd13d4907f1be09440387a36e14ecdc4926ba7e7
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84594771"
---
# <a name="asmx-client-with-a-wcf-service"></a>Cliente ASMX com um serviço do WCF

Este exemplo demonstra como criar um serviço usando Windows Communication Foundation (WCF) e, em seguida, acessar o serviço de um cliente não WCF, como um cliente ASMX.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Este exemplo consiste em um programa de console do cliente (. exe) e uma biblioteca de serviços (. dll) hospedados pelo Serviços de Informações da Internet (IIS). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas ( `Add` , `Subtract` , `Multiply` e `Divide` ). O cliente ASMX faz solicitações síncronas para uma operação matemática e o serviço responde com o resultado.

O serviço implementa um `ICalculator` contrato, conforme definido no código a seguir.

```csharp
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples"), XmlSerializerFormat]
public interface ICalculator
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

O <xref:System.Runtime.Serialization.DataContractSerializer> e <xref:System.Xml.Serialization.XmlSerializer> mapeiam os tipos CLR para uma representação XML. O <xref:System.Runtime.Serialization.DataContractSerializer> interpreta algumas representações XML de forma diferente do XmlSerializer. Geradores de proxy não WCF, como WSDL. exe, geram uma interface mais utilizável quando o XmlSerializer está sendo usado. O <xref:System.ServiceModel.XmlSerializerFormatAttribute> é aplicado à `ICalculator` interface do, para garantir que o XmlSerializer seja usado para mapear tipos CLR para XML. A implementação do serviço calcula e retorna o resultado apropriado.

O serviço expõe um único ponto de extremidade para se comunicar com o serviço, definido usando um arquivo de configuração (Web. config). O ponto de extremidade consiste em um endereço, uma associação e um contrato. O serviço expõe o ponto de extremidade no endereço base fornecido pelo host do Serviços de Informações da Internet (IIS). O `binding` atributo é definido como BasicHttpBinding que fornece comunicações http usando SOAP 1,1, que é compatível com WS-I BasicProfile 1,1, conforme mostrado na configuração de exemplo a seguir.

```xml
<services>
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"
           behaviorConfiguration="CalculatorServiceBehavior">
    <!-- This endpoint is exposed at the base address provided by the host: http://localhost/servicemodelsamples/service.svc.  -->
    <endpoint address=""
              binding="basicHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </service>
</services>
```

O cliente ASMX se comunica com o serviço WCF usando um proxy tipado que é gerado pelo utilitário WSDL (Web Services Description Language) (WSDL. exe). O proxy tipado está contido no arquivo generatedClient.cs. O utilitário WSDL recupera metadados para o serviço especificado e gera um proxy de tipo para ser usado por um cliente para se comunicar. Por padrão, a estrutura não expõe nenhum metadado. Para expor os metadados necessários para gerar o proxy, você deve adicionar um [\<serviceMetadata>](../../configure-apps/file-schema/wcf/servicemetadata.md) e definir seu `httpGetEnabled` atributo `True` como, conforme mostrado na configuração a seguir.

```xml
<behaviors>
  <serviceBehaviors>
    <behavior name="CalculatorServiceBehavior">
      <!-- Setting httpGetEnabled to True on the serviceMetadata
           behavior exposes the service's wsdl at <base address>?wsdl :
           http://localhost/servicemodelsamples/service.svc?wsdl -->
      <serviceMetadata httpGetEnabled="True"/>
      <serviceDebug includeExceptionDetailInFaults="False" />
    </behavior>
  </serviceBehaviors>
</behaviors>
```

Execute o comando a seguir em um prompt de comando no diretório do cliente para gerar o proxy de tipo.

```console
wsdl /n:Microsoft.ServiceModel.Samples /o:generatedClient.cs /urlkey:CalculatorServiceAddress http://localhost/servicemodelsamples/service.svc?wsdl
```

Usando o proxy de tipo gerado, o cliente pode acessar um determinado ponto de extremidade de serviço Configurando o endereço apropriado. O cliente usa um arquivo de configuração (App. config) para especificar o ponto de extremidade com o qual se comunicar.

```xml
<appSettings>
  <add key="CalculatorServiceAddress"
       value="http://localhost/ServiceModelSamples/service.svc"/>
</appSettings>
```

A implementação do cliente constrói uma instância do proxy digitado para começar a se comunicar com o serviço.

```csharp
// Create a client to the CalculatorService.
using (CalculatorService client = new CalculatorService())
{
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

Console.WriteLine();
Console.WriteLine("Press <ENTER> to terminate client.");
Console.ReadLine();
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```console
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

> [!NOTE]
> Para obter mais informações sobre como passar e retornar tipos de dados complexos, consulte: [Associação de dados em um cliente Windows Forms](data-binding-in-a-windows-forms-client.md), [Associação de dados em um cliente Windows Presentation Foundation](data-binding-in-a-wpf-client.md)e [Associação de dados em um cliente ASP.net](data-binding-in-an-aspnet-client.md)

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Services\Interop\ASMX`
