---
title: Exemplo de introdução
description: Saiba como implementar um serviço típico e um cliente típico usando o WCF. Este exemplo é a base para todos os outros exemplos de tecnologia básica.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- basic samples [WCF], getting started
ms.assetid: 967a3d94-0261-49ff-b85a-20bb07f1af20
ms.openlocfilehash: b23be1b33f227154b916429c063ec4106229bb3c
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85246227"
---
# <a name="getting-started-sample"></a>Exemplo de introdução

O exemplo a Introdução demonstra como implementar um serviço típico e um cliente típico usando o Windows Communication Foundation (WCF). Este exemplo é a base para todos os outros exemplos de tecnologia básica.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\GettingStarted\GettingStarted`

O serviço descreve as operações que ele realiza em um contrato de serviço que ele expõe publicamente como metadados. O serviço também contém o código para implementar as operações.

O cliente contém uma definição do contrato de serviço e uma classe de proxy para acessar o serviço. O código do proxy é gerado a partir dos metadados de serviço usando a [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md).

No Windows Vista, o serviço está hospedado no WAS (serviço de ativação do Windows). No Windows XP e no Windows Server 2003, ele é hospedado por Serviços de Informações da Internet (IIS) e ASP.NET. Hospedar um serviço no IIS ou WAS permite que o serviço seja ativado automaticamente quando é acessado pela primeira vez.

> [!NOTE]
> Se você preferir começar com um exemplo que hospede o serviço em um aplicativo de console em vez de IIS, consulte o exemplo de hospedagem [interna](self-host.md) .

O serviço e o cliente especificam detalhes de acesso nas configurações do arquivo de configuração, que fornecem flexibilidade no momento da implantação. Isso inclui uma definição de ponto de extremidade que especifica um endereço, associação e contrato. A associação especifica os detalhes de transporte e segurança de como o serviço deve ser acessado.

O serviço configura um comportamento de tempo de execução para publicar seus metadados.

O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). O cliente faz solicitações para uma determinada operação matemática e o serviço responde com o resultado. O serviço implementa um `ICalculator` contrato que é definido no código a seguir.

```vb
' Define a service contract.
    <ServiceContract(Namespace:="http://Microsoft.Samples.GettingStarted")>
     Public Interface ICalculator
        <OperationContract()>
        Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double
        <OperationContract()>
        Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double
    End Interface
```

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
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

A implementação do serviço calcula e retorna o resultado apropriado, conforme mostrado no código de exemplo a seguir.

```vb
' Service class which implements the service contract.
Public Class CalculatorService
Implements ICalculator
Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Add
Return n1 + n2
End Function

Public Function Subtract(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Subtract
Return n1 - n2
End Function

Public Function Multiply(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Multiply
Return n1 * n2
End Function

Public Function Divide(ByVal n1 As Double, ByVal n2 As Double) As Double Implements ICalculator.Divide
Return n1 / n2
End Function
End Class
```

```csharp
// Service class that implements the service contract.
public class CalculatorService : ICalculator
{
    public double Add(double n1, double n2)
    {
        return n1 + n2;
    }
    public double Subtract(double n1, double n2)
    {
        return n1 - n2;
    }
    public double Multiply(double n1, double n2)
    {
        return n1 * n2;
    }
    public double Divide(double n1, double n2)
    {
        return n1 / n2;
    }
}
```

O serviço expõe um ponto de extremidade para se comunicar com o serviço, definido usando um arquivo de configuração (Web.config), conforme mostrado na seguinte configuração de exemplo.

```xaml
<services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
        <!-- ICalculator is exposed at the base address provided by
         host: http://localhost/servicemodelsamples/service.svc.  -->
       <endpoint address=""
              binding="wsHttpBinding"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />
       ...
    </service>
</services>
```

O serviço expõe o ponto de extremidade no endereço base fornecido pelo IIS ou pelo host. A associação é configurada com um padrão <xref:System.ServiceModel.WSHttpBinding> , que fornece comunicação http e protocolos de serviço Web padrão para endereçamento e segurança. O contrato é `ICalculator` implementado pelo serviço.

Conforme configurado, o serviço pode ser acessado pelo `http://localhost/servicemodelsamples/service.svc` cliente do no mesmo computador. Para clientes em computadores remotos acessarem o serviço, um nome de domínio totalmente qualificado deve ser especificado em vez de localhost.

A estrutura não expõe metadados por padrão. Como tal, o serviço ativa o <xref:System.ServiceModel.Description.ServiceMetadataBehavior> e expõe um ponto de extremidade de intercâmbio de metadados (MEX) em `http://localhost/servicemodelsamples/service.svc/mex` . A configuração a seguir demonstra isso.

```xaml
<system.serviceModel>
  <services>
    <service
        name="Microsoft.ServiceModel.Samples.CalculatorService"
        behaviorConfiguration="CalculatorServiceBehavior">
      ...
      <!-- the mex endpoint is exposed at
       http://localhost/servicemodelsamples/service.svc/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>

  <!--For debugging purposes set the includeExceptionDetailInFaults
   attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True"/>
        <serviceDebug includeExceptionDetailInFaults="False" />
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```

O cliente se comunica usando um determinado tipo de contrato usando uma classe de cliente que é gerada pela [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md). Esse cliente gerado está contido no arquivo generatedClient.cs ou generatedClient. vb. Esse utilitário recupera metadados para um determinado serviço e gera um cliente para ser usado pelo aplicativo cliente para se comunicar usando um determinado tipo de contrato. O serviço hospedado deve estar disponível para gerar o código do cliente, pois o serviço é usado para recuperar os metadados atualizados.

 Execute o seguinte comando no prompt de comando do SDK no diretório do cliente para gerar o proxy de tipo:

```console
svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /out:generatedClient.cs
```

Para gerar o cliente no Visual Basic Digite o seguinte no prompt de comando do SDK:

`Svcutil.exe /n:"http://Microsoft.ServiceModel.Samples,Microsoft.ServiceModel.Samples" http://localhost/servicemodelsamples/service.svc/mex /l:vb /out:generatedClient.vb`

Usando o cliente gerado, o cliente pode acessar um determinado ponto de extremidade de serviço Configurando o endereço e a associação apropriados. Assim como o serviço, o cliente usa um arquivo de configuração (App.config) para especificar o ponto de extremidade com o qual deseja se comunicar. A configuração de ponto de extremidade do cliente consiste em um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato, conforme mostrado no exemplo a seguir.

```xaml
<client>
     <endpoint
         address="http://localhost/servicemodelsamples/service.svc"
         binding="wsHttpBinding"
         contract=" Microsoft.ServiceModel.Samples.ICalculator" />
</client>
```

A implementação do cliente instancia o cliente e usa a interface digitada para começar a se comunicar com o serviço, conforme mostrado no código de exemplo a seguir.

```vb
' Create a client
Dim client As New CalculatorClient()

' Call the Add service operation.
            Dim value1 = 100.0R
            Dim value2 = 15.99R
            Dim result = client.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)

' Call the Subtract service operation.
value1 = 145.00R
value2 = 76.54R
result = client.Subtract(value1, value2)
Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result)

' Call the Multiply service operation.
value1 = 9.00R
value2 = 81.25R
result = client.Multiply(value1, value2)
Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result)

' Call the Divide service operation.
value1 = 22.00R
value2 = 7.00R
result = client.Divide(value1, value2)
Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result)

'Closing the client gracefully closes the connection and cleans up resources
```

```csharp
// Create a client.
CalculatorClient client = new CalculatorClient();

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

//Closing the client releases all communication resources.
client.Close();
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

```output
Add(100,15.99) = 115.99
Subtract(145,76.54) = 68.46
Multiply(9,81.25) = 731.25
Divide(22,7) = 3.14285714285714

Press <ENTER> to terminate client.
```

O exemplo a Introdução mostra a maneira padrão de criar um serviço e um cliente. A outra compilação [básica](basic-sample.md) neste exemplo para demonstrar recursos específicos do produto.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou entre computadores, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

## <a name="see-also"></a>Veja também

- [Como hospedar um serviço do WCF em um aplicativo gerenciado](../how-to-host-a-wcf-service-in-a-managed-application.md)
- [Como hospedar um serviço WCF no IIS](../feature-details/how-to-host-a-wcf-service-in-iis.md)
