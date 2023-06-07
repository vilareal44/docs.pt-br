---
title: Exemplo de compartilhamento de porta Net.TCP
ms.date: 03/30/2017
ms.assetid: 03da5959-0574-4e91-8a53-05854b6c55dc
ms.openlocfilehash: 6c196380951d0da912cd937e3ebc38a03f80489c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584292"
---
# <a name="nettcp-port-sharing-sample"></a>Exemplo de compartilhamento de porta Net.TCP
O protocolo TCP/IP usa um número de 16 bits, chamado de porta, para diferenciar as conexões a vários aplicativos de rede em execução no mesmo computador. Se um aplicativo estiver escutando em uma porta, todo o tráfego TCP dessa porta vai para esse aplicativo. Outros aplicativos não podem escutar nessa porta ao mesmo tempo.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\TCP\PortSharing`  
  
 Muitos protocolos têm um número padrão ou padrão de porta que eles usam. Por exemplo, o protocolo HTTP geralmente usa a porta TCP 80. Serviços de Informações da Internet (IIS) tem um ouvinte para compartilhar uma porta entre vários aplicativos HTTP. O IIS escuta na porta diretamente e encaminha as mensagens para o aplicativo apropriado com base nas informações dentro do fluxo de mensagens. Isso permite que vários aplicativos HTTP usem o mesmo número de porta sem a necessidade de competir para reservar a porta para receber mensagens.  
  
 O compartilhamento de porta NetTcp é um recurso de Windows Communication Foundation (WCF) que, de maneira semelhante, permite que vários aplicativos de rede compartilhem uma única porta. O serviço de compartilhamento de porta NetTcp aceita conexões usando o protocolo net. TCP e encaminha mensagens com base em seu endereço de destino.  
  
 O serviço de compartilhamento de porta NetTcp não está habilitado por padrão. Antes de executar este exemplo, você deve habilitar manualmente o serviço. Para obter mais informações, consulte [como habilitar o serviço de compartilhamento de porta Net. TCP](../feature-details/how-to-enable-the-net-tcp-port-sharing-service.md). Se o serviço estiver desabilitado, uma exceção será lançada quando o aplicativo do servidor for iniciado.  
  
```console
Unhandled Exception: System.ServiceModel.CommunicationException: The TransportManager failed to listen on the supplied URI using the NetTcpPortSharing service: failed to start the service because it is disabled. An administrator can enable it by running 'sc.exe config NetTcpPortSharing start= demand'.. ---> System.InvalidOperationException: Cannot start service NetTcpPortSharing on computer '.'. ---> System.ComponentModel.Win32Exception: The service cannot be started, either because it is disabled or because it has no enabled devices associated with it  
```  
  
 O compartilhamento de porta está habilitado no servidor definindo a <xref:System.ServiceModel.NetTcpBinding.PortSharingEnabled%2A> propriedade da <xref:System.ServiceModel.NetTcpBinding> associação ou o <xref:System.ServiceModel.Channels.TcpTransportBindingElement> elemento de associação. O cliente não precisa saber como o compartilhamento de porta foi configurado para usá-lo no servidor.  
  
## <a name="enabling-port-sharing"></a>Habilitando o compartilhamento de porta  
 O código a seguir demonstra como habilitar o compartilhamento de porta no servidor. Ele inicia uma instância do `ICalculator` serviço em uma porta fixa com um caminho de URI aleatório. Embora dois serviços possam compartilhar a mesma porta, seus endereços de ponto de extremidade gerais ainda devem ser exclusivos para que o serviço de compartilhamento de porta NetTcp possa rotear mensagens para o aplicativo correto.  

```csharp
// Configure a binding with TCP port sharing enabled  
NetTcpBinding binding = new NetTcpBinding();  
binding.PortSharingEnabled = true;  
  
// Start a service on a fixed TCP port  
ServiceHost host = new ServiceHost(typeof(CalculatorService));  
ushort salt = (ushort)new Random().Next();  
string address = $"net.tcp://localhost:9000/calculator/{salt}";
host.AddServiceEndpoint(typeof(ICalculator), binding, address);  
host.Open();  
```

 Com o compartilhamento de porta habilitado, você pode executar o serviço várias vezes sem ter um conflito no número da porta. Se você alterar o código para desabilitar o compartilhamento de porta, a inicialização de duas cópias do serviço resultará na segunda falha com um <xref:System.ServiceModel.AddressAlreadyInUseException> .  
  
```console  
Unhandled Exception: System.ServiceModel.AddressAlreadyInUseException: There is already a listener on IP endpoint 0.0.0.0:9000.  Make sure that you are not trying to use this endpoint multiple times in your application and that there are no other applications listening on this endpoint. ---> System.Net.Sockets.SocketException: Only one usage of each socket address (protocol/network address/port) is normally permitted  
```  
  
## <a name="running-the-sample"></a>Executando o exemplo  
 Você pode usar o cliente de teste para verificar se as mensagens são roteadas corretamente para os serviços que compartilham a porta.  

```csharp
class client  
{  
   static void Main(string[] args)  
   {  
      Console.Write("Enter the service number to test: ");  
      ushort salt = ushort.Parse(Console.ReadLine());  
      string address = $"net.tcp://localhost:9000/calculator/{salt}";
      ChannelFactory<ICalculator> factory = new ChannelFactory<ICalculator>(new NetTcpBinding());  
      ICalculator proxy = factory.CreateChannel(new EndpointAddress(address));  
  
      // Call the Add service operation.  
      double value1 = 100.00D;  
      double value2 = 15.99D;  
      double result = proxy.Add(value1, value2);  
      Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Subtract service operation.  
      value1 = 145.00D;  
      value2 = 76.54D;  
      result = proxy.Subtract(value1, value2);  
      Console.WriteLine("Subtract({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Multiply service operation.  
      value1 = 9.00D;  
      value2 = 81.25D;  
      result = proxy.Multiply(value1, value2);  
      Console.WriteLine("Multiply({0},{1}) = {2}", value1, value2, result);  
  
      // Call the Divide service operation.  
      value1 = 22.00D;  
      value2 = 7.00D;  
      result = proxy.Divide(value1, value2);  
      Console.WriteLine("Divide({0},{1}) = {2}", value1, value2, result);  
  
      Console.WriteLine();  
      Console.WriteLine("Press <ENTER> to terminate client.");  
      Console.ReadLine();  
  
      factory.Close();  
   }  
}  
```

 Cada instância do serviço imprime seu número e endereço exclusivos. Por exemplo, você pode ver o texto a seguir ao executar o Service. exe.  
  
```console  
Service #4381 listening on net.tcp://localhost:9000/calculator/4381.  
Press <ENTER> to terminate service.  
```  
  
 Insira o número de serviço que você vê aqui ao executar o Client. exe.  
  
```console  
Enter the service number to test: 4381  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 Este exemplo pode ser executado em uma configuração de computador cruzado alterando o endereço gerado que o cliente usa. No Client.cs, altere a cadeia de caracteres de formato do endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Substitua todas as referências a "localhost" pelo endereço IP do computador do servidor. Você deve recompilar o exemplo depois de fazer essa alteração.  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Habilite o serviço de compartilhamento de porta NetTcp conforme descrito anteriormente na seção Introdução.  
  
4. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
5. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md). Os detalhes específicos para a execução deste exemplo estão incluídos anteriormente na seção executando a amostra.  
