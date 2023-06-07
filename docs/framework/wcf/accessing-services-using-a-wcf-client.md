---
title: Usando um cliente WCF para acessar um serviço
description: Saiba como criar um proxy de cliente WCF para seu serviço WCF. Um aplicativo cliente usa o proxy do cliente para se comunicar com o serviço.
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- clients [WCF], consuming services
ms.assetid: d780af9f-73c5-42db-9e52-077a5e4de7fe
ms.openlocfilehash: 25446a89a0b5657d32d77e2d0d57f58f36bed71b
ms.sourcegitcommit: 358a28048f36a8dca39a9fe6e6ac1f1913acadd5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85245538"
---
# <a name="accessing-services-using-a-wcf-client"></a>Usando um cliente WCF para acessar um serviço

Depois de criar um serviço, a próxima etapa é criar um proxy de cliente WCF. Um aplicativo cliente usa o proxy do cliente WCF para se comunicar com o serviço. Normalmente, os aplicativos cliente importam os metadados de um serviço para gerar o código do cliente WCF que pode ser usado para invocar o serviço.

 As etapas básicas para criar um cliente WCF incluem o seguinte:

1. Compile o código do serviço.

2. Gere o proxy do cliente WCF.

3. Crie uma instância do proxy do cliente WCF.

O proxy do cliente WCF pode ser gerado manualmente usando a ferramenta de utilitário de metadados do modelo de serviço (SvcUtil.exe) para obter mais informações, consulte [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md). O proxy do cliente WCF também pode ser gerado no Visual Studio usando o recurso **Adicionar referência de serviço** . Para gerar o proxy do cliente WCF usando qualquer um dos métodos, o serviço deve estar em execução. Se o serviço for auto-hospedado, você deverá executar o host. Se o serviço estiver hospedado no IIS/WAS, você não precisará fazer mais nada.

## <a name="servicemodel-metadata-utility-tool"></a>Ferramenta de utilitário de metadados ServiceModel
 A [ferramenta de utilitário de metadados ServiceModel (Svcutil.exe)](servicemodel-metadata-utility-tool-svcutil-exe.md) é uma ferramenta de linha de comando para gerar código a partir de metadados. O uso a seguir é um exemplo de um comando Svcutil.exe básico.

```console
Svcutil.exe <service's Metadata Exchange (MEX) address or HTTP GET address>
```

 Como alternativa, você pode usar Svcutil.exe com arquivos WSDL (linguagem de descrição de serviços Web) e XSD (XML Schema Definition Language) no sistema de arquivos.

```console
Svcutil.exe <list of WSDL and XSD files on file system>
```

 O resultado é um arquivo de código que contém o código do cliente WCF que o aplicativo cliente pode usar para invocar o serviço.

 Você também pode usar a ferramenta para gerar arquivos de configuração.

```console
Svcutil.exe <file1 [,file2]>
```

 Se apenas um nome de arquivo for fornecido, esse será o nome do arquivo de saída. Se dois nomes de arquivo forem fornecidos, o primeiro arquivo será um arquivo de configuração de entrada cujo conteúdo será mesclado com a configuração gerada e gravado no segundo arquivo. Para obter mais informações sobre configuração, consulte [Configurando associações para serviços](configuring-bindings-for-wcf-services.md).

> [!IMPORTANT]
> As solicitações de metadados não seguras apresentam determinados riscos da mesma forma que qualquer solicitação de rede não segura: se você não tiver certeza de que o ponto de extremidade com o qual está se comunicando é quem diz ser, as informações recuperadas poderão ser metadados de um serviço mal-intencionado.

## <a name="add-service-reference-in-visual-studio"></a>Adicionar referência de serviço no Visual Studio

 Com o serviço em execução, clique com o botão direito do mouse no projeto que conterá o proxy do cliente WCF e selecione **Adicionar**  >  **referência de serviço**. Na **caixa de diálogo Adicionar referência de serviço**, digite a URL para o serviço que você deseja chamar e clique no botão **ir** . A caixa de diálogo exibirá uma lista de serviços disponíveis no endereço que você especificar. Clique duas vezes no serviço para ver os contratos e as operações disponíveis, especifique um namespace para o código gerado e clique no botão **OK** .

## <a name="example"></a>Exemplo
 O exemplo de código a seguir mostra um contrato de serviço criado para um serviço.

```csharp
// Define a service contract.
[ServiceContract(Namespace="http://Microsoft.ServiceModel.Samples")]
public interface ICalculator
{
    [OperationContract]
    double Add(double n1, double n2);
    // Other methods are not shown here.
}
```

```vb
' Define a service contract.
<ServiceContract(Namespace:="http://Microsoft.ServiceModel.Samples")> _
Public Interface ICalculator
    <OperationContract()>  _
    Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
    ' Other methods are not shown here.
End Interface
```

 A ferramenta de utilitário de metadados ServiceModel e o **Adicionar referência de serviço** no Visual Studio geram a seguinte classe de cliente WCF. A classe herda da classe genérica <xref:System.ServiceModel.ClientBase%601> e implementa a `ICalculator` interface. A ferramenta também gera a `ICalculator` interface (não mostrada aqui).

```csharp
public partial class CalculatorClient : System.ServiceModel.ClientBase<ICalculator>, ICalculator
{
    public CalculatorClient()
    {}

    public CalculatorClient(string endpointConfigurationName) :
            base(endpointConfigurationName)
    {}

    public CalculatorClient(string endpointConfigurationName, string remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(string endpointConfigurationName,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(endpointConfigurationName, remoteAddress)
    {}

    public CalculatorClient(System.ServiceModel.Channels.Binding binding,
        System.ServiceModel.EndpointAddress remoteAddress) :
            base(binding, remoteAddress)
    {}

    public double Add(double n1, double n2)
    {
        return base.Channel.Add(n1, n2);
    }
}
```

```vb
Partial Public Class CalculatorClient
    Inherits System.ServiceModel.ClientBase(Of ICalculator)
    Implements ICalculator

    Public Sub New()
        MyBase.New
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String)
        MyBase.New(endpointConfigurationName)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String, ByVal remoteAddress As String)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal endpointConfigurationName As String,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(endpointConfigurationName, remoteAddress)
    End Sub

    Public Sub New(ByVal binding As System.ServiceModel.Channels.Binding,
        ByVal remoteAddress As System.ServiceModel.EndpointAddress)
        MyBase.New(binding, remoteAddress)
    End Sub

    Public Function Add(ByVal n1 As Double, ByVal n2 As Double) As Double
        Implements ICalculator.Add
        Return MyBase.Channel.Add(n1, n2)
    End Function
End Class
```

## <a name="using-the-wcf-client"></a>Usando o cliente WCF
 Para usar o cliente WCF, crie uma instância do cliente WCF e, em seguida, chame seus métodos, conforme mostrado no código a seguir.

```csharp
// Create a client object with the given client endpoint configuration.
CalculatorClient calcClient = new CalculatorClient("CalculatorEndpoint"));
// Call the Add service operation.
double value1 = 100.00D;
double value2 = 15.99D;
double result = calcClient.Add(value1, value2);
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
```

```vb
' Create a client object with the given client endpoint configuration.
Dim calcClient As CalculatorClient = _
New CalculatorClient("CalculatorEndpoint")

' Call the Add service operation.
Dim value1 As Double = 100.00D
Dim value2 As Double = 15.99D
Dim result As Double = calcClient.Add(value1, value2)
Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result)
```

## <a name="debugging-exceptions-thrown-by-a-client"></a>Exceções de depuração geradas por um cliente

Muitas exceções geradas por um cliente WCF são causadas por uma exceção no serviço. Alguns exemplos disso são:

- <xref:System.Net.Sockets.SocketException>: Uma conexão existente foi fechada forçosamente pelo host remoto.

- <xref:System.ServiceModel.CommunicationException>: A conexão subjacente foi fechada inesperadamente.

- <xref:System.ServiceModel.CommunicationObjectAbortedException>: A conexão de soquete foi anulada. Isso pode ser causado por um erro ao processar sua mensagem, um tempo limite de recebimento excedido pelo host remoto ou um problema de recurso de rede subjacente.

Quando ocorrem esses tipos de exceções, a melhor maneira de resolver o problema é ativar o rastreamento no lado do serviço e determinar qual exceção ocorreu lá. Para obter mais informações sobre rastreamento, consulte [rastreamento](./diagnostics/tracing/index.md) e [uso de rastreamento para solucionar problemas de seu aplicativo](./diagnostics/tracing/using-tracing-to-troubleshoot-your-application.md).

## <a name="see-also"></a>Veja também

- [Como criar um cliente](how-to-create-a-wcf-client.md)
- [Como acessar serviços com um contrato Duplex](./feature-details/how-to-access-services-with-a-duplex-contract.md)
- [Como chamar operações de serviço de maneira assíncrona](./feature-details/how-to-call-wcf-service-operations-asynchronously.md)
- [Como acessar os serviços com contratos de resposta/solicitação e unidirecionais](./feature-details/how-to-access-wcf-services-with-one-way-and-request-reply-contracts.md)
- [Como acessar um serviço WSE 3.0](./feature-details/how-to-access-a-wse-3-0-service-with-a-wcf-client.md)
- [Entendendo o código do cliente gerado](./feature-details/understanding-generated-client-code.md)
- [Como melhorar o tempo de inicialização dos aplicativos do cliente WCF usando o XmlSerializer](./feature-details/startup-time-of-wcf-client-applications-using-the-xmlserializer.md)
- [Especificando o comportamento em tempo de execução do cliente](specifying-client-run-time-behavior.md)
- [Configurando comportamentos do cliente](configuring-client-behaviors.md)
