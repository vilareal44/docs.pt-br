---
title: Duplex
ms.date: 03/30/2017
helpviewer_keywords:
- Duplex Service Contract
ms.assetid: bc5de6b6-1a63-42a3-919a-67d21bae24e0
ms.openlocfilehash: 77eab6a4975fc67c20558a53f399c7e709215587
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575193"
---
# <a name="duplex"></a>Duplex

O exemplo de duplex demonstra como definir e implementar um contrato duplex. A comunicação duplex ocorre quando um cliente estabelece uma sessão com um serviço e fornece ao serviço um canal no qual o serviço pode enviar mensagens de volta ao cliente. Este exemplo é baseado na [introdução](getting-started-sample.md). Um contrato duplex é definido como um par de interfaces — uma interface primária do cliente para o serviço e uma interface de retorno de chamada do serviço para o cliente. Neste exemplo, a `ICalculatorDuplex` interface permite que o cliente execute operações matemáticas, calculando o resultado em uma sessão. O serviço retorna resultados na `ICalculatorDuplexCallback` interface. Um contrato duplex requer uma sessão, porque um contexto deve ser estabelecido para correlacionar o conjunto de mensagens que estão sendo enviadas entre o cliente e o serviço.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS). O contrato duplex é definido da seguinte maneira:

```csharp
[ServiceContract(Namespace = "http://Microsoft.ServiceModel.Samples", SessionMode=SessionMode.Required,
                 CallbackContract=typeof(ICalculatorDuplexCallback))]
public interface ICalculatorDuplex
{
    [OperationContract(IsOneWay = true)]
    void Clear();
    [OperationContract(IsOneWay = true)]
    void AddTo(double n);
    [OperationContract(IsOneWay = true)]
    void SubtractFrom(double n);
    [OperationContract(IsOneWay = true)]
    void MultiplyBy(double n);
    [OperationContract(IsOneWay = true)]
    void DivideBy(double n);
}

public interface ICalculatorDuplexCallback
{
    [OperationContract(IsOneWay = true)]
    void Result(double result);
    [OperationContract(IsOneWay = true)]
    void Equation(string eqn);
}
```

A `CalculatorService` classe implementa a `ICalculatorDuplex` interface primária. O serviço usa o <xref:System.ServiceModel.InstanceContextMode.PerSession> modo de instância para manter o resultado de cada sessão. Uma propriedade privada chamada `Callback` é usada para acessar o canal de retorno de chamada para o cliente. O serviço usa o retorno de chamada para enviar mensagens de volta para o cliente por meio da interface de retorno de chamada.

```csharp
[ServiceBehavior(InstanceContextMode = InstanceContextMode.PerSession)]
public class CalculatorService : ICalculatorDuplex
{
    double result = 0.0D;
    string equation;

    public CalculatorService()
    {
        equation = result.ToString();
    }

    public void Clear()
    {
        Callback.Equation($"{equation} = {result}");
        equation = result.ToString();
    }

    public void AddTo(double n)
    {
        result += n;
        equation += $" + {n}";
        Callback.Result(result);
    }

    //...

    ICalculatorDuplexCallback Callback
    {
        get
        {
            return OperationContext.Current.GetCallbackChannel<ICalculatorDuplexCallback>();
        }
    }
}
```

O cliente deve fornecer uma classe que implemente a interface de retorno de chamada do contrato duplex, para receber mensagens do serviço. No exemplo, uma `CallbackHandler` classe é definida para implementar a `ICalculatorDuplexCallback` interface.

```csharp
public class CallbackHandler : ICalculatorDuplexCallback
{
   public void Result(double result)
   {
      Console.WriteLine("Result({0})", result);
   }

   public void Equation(string equation)
   {
      Console.WriteLine("Equation({0}", equation);
   }
}
```

O proxy que é gerado para um contrato duplex requer que um seja <xref:System.ServiceModel.InstanceContext> fornecido na construção. Isso <xref:System.ServiceModel.InstanceContext> é usado como o site de um objeto que implementa a interface de retorno de chamada e manipula as mensagens que são enviadas de volta do serviço. Um <xref:System.ServiceModel.InstanceContext> é construído com uma instância da `CallbackHandler` classe. Esse objeto manipula as mensagens enviadas do serviço para o cliente na interface de retorno de chamada.

```csharp
// Construct InstanceContext to handle messages on callback interface.
InstanceContext instanceContext = new InstanceContext(new CallbackHandler());

// Create a client.
CalculatorDuplexClient client = new CalculatorDuplexClient(instanceContext);

Console.WriteLine("Press <ENTER> to terminate client once the output is displayed.");
Console.WriteLine();

// Call the AddTo service operation.
double value = 100.00D;
client.AddTo(value);

// Call the SubtractFrom service operation.
value = 50.00D;
client.SubtractFrom(value);

// Call the MultiplyBy service operation.
value = 17.65D;
client.MultiplyBy(value);

// Call the DivideBy service operation.
value = 2.00D;
client.DivideBy(value);

// Complete equation.
client.Clear();

Console.ReadLine();

//Closing the client gracefully closes the connection and cleans up resources.
client.Close();
```

A configuração foi modificada para fornecer uma associação que dá suporte à comunicação de sessão e duplex. O `wsDualHttpBinding` dá suporte à comunicação de sessão e permite a comunicação duplex fornecendo conexões http duplas, uma para cada direção. No serviço, a única diferença na configuração é a associação usada. No cliente, você deve configurar um endereço que o servidor pode usar para se conectar ao cliente, conforme mostrado na seguinte configuração de exemplo.

```xml
<client>
  <endpoint name=""
            address="http://localhost/servicemodelsamples/service.svc"
            binding="wsDualHttpBinding"
            bindingConfiguration="DuplexBinding"
            contract="Microsoft.ServiceModel.Samples.ICalculatorDuplex" />
</client>

<bindings>
  <!-- Configure a binding that support duplex communication. -->
  <wsDualHttpBinding>
    <binding name="DuplexBinding"
             clientBaseAddress="http://localhost:8000/myClient/">
    </binding>
  </wsDualHttpBinding>
</bindings>
```

Ao executar o exemplo, você vê as mensagens que são retornadas para o cliente na interface de retorno de chamada que é enviada do serviço. Cada resultado intermediário é exibido, seguido de toda a equação após a conclusão de todas as operações. Pressione ENTER para desligar o cliente.

### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para criar a edição C#, C++ ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).

    > [!IMPORTANT]
    > Ao executar o cliente em uma configuração entre computadores, certifique-se de substituir "localhost" no `address` atributo do elemento [ \<endpoint> \<client> de](../../configure-apps/file-schema/wcf/endpoint-of-client.md) e no `clientBaseAddress` atributo do elemento [\<binding>](../../configure-apps/file-schema/wcf/bindings.md) do [\<wsDualHttpBinding>](../../configure-apps/file-schema/wcf/wsdualhttpbinding.md) elemento pelo nome do computador apropriado, conforme mostrado a seguir:.

    ```xml
    <client>
        <endpoint name = ""
        address="http://service_machine_name/servicemodelsamples/service.svc"
        ... />
    </client>
    ...
    <wsDualHttpBinding>
        <binding name="DuplexBinding" clientBaseAddress="http://client_machine_name:8000/myClient/">
        </binding>
    </wsDualHttpBinding>
    ```

> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.
>
> `<InstallDrive>:\WF_WCF_Samples`
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Contract\Service\Duplex`
