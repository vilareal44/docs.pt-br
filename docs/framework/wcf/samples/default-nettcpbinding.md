---
title: NetTcpBinding padrão
ms.date: 03/30/2017
helpviewer_keywords:
- Net profile TCP
ms.assetid: e8475fe6-0ecd-407a-8e7e-45860561bb74
ms.openlocfilehash: 56648b74e400085b76f4f837852791b33fbf97e0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84599991"
---
# <a name="default-nettcpbinding"></a>NetTcpBinding padrão
Este exemplo demonstra o uso da <xref:System.ServiceModel.NetTcpBinding> associação. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora. Neste exemplo, o serviço é auto-hospedado. O cliente e o serviço são aplicativos de console.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Net\TCP\Default`  
  
 A associação é especificada nos arquivos de configuração para o cliente e o serviço. O tipo de associação é especificado no `binding` atributo do [\<endpoint>](../../configure-apps/file-schema/wcf/endpoint-element.md) elemento, conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<endpoint address=""  
          binding="netTcpBinding"  
          contract="Microsoft.ServiceModel.Samples.ICalculator" />  
```  
  
 O exemplo anterior mostra como configurar um ponto de extremidade para usar a `netTcpBinding` associação com as configurações padrão. Se você quiser configurar a `netTcpBinding` associação e alterar algumas de suas configurações, será necessário definir uma configuração de associação. O ponto de extremidade deve referenciar a configuração de associação por nome com um `bindingConfiguration` atributo. Neste exemplo, a configuração de associação é nomeada `Binding1` e é definida conforme mostrado na seguinte configuração de exemplo.  
  
```xml  
<services>  
  <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
           behaviorConfiguration="CalculatorServiceBehavior">  
    ...  
    <endpoint address=""  
              binding="netTcpBinding"  
              bindingConfiguration="Binding1"
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    ...  
  </service>  
</services>  
  
<bindings>  
  <netTcpBinding>  
    <binding name="Binding1"
             closeTimeout="00:01:00"  
             openTimeout="00:01:00"
             receiveTimeout="00:10:00"
             sendTimeout="00:01:00"  
             transactionFlow="false"
             transferMode="Buffered"
             transactionProtocol="OleTransactions"  
             hostNameComparisonMode="StrongWildcard"
             listenBacklog="10"  
             maxBufferPoolSize="524288"
             maxBufferSize="65536"
             maxConnections="10"  
             maxReceivedMessageSize="65536">  
      <readerQuotas maxDepth="32"
                    maxStringContentLength="8192"
                    maxArrayLength="16384"  
                    maxBytesPerRead="4096"
                    maxNameTableCharCount="16384" />  
      <reliableSession ordered="true"
                       inactivityTimeout="00:10:00"  
                       enabled="false" />  
      <security mode="Transport">  
        <transport clientCredentialType="Windows" protectionLevel="EncryptAndSign" />  
      </security>  
    </binding>  
  </netTcpBinding>  
</bindings>  
```  
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press ENTER to terminate client.  
```  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Instale o ASP.NET 4,0 usando o comando a seguir.  
  
    ```console  
    %windir%\Microsoft.NET\Framework\v4.0.XXXXX\aspnet_regiis.exe /i /enable  
    ```  
  
2. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
3. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
4. Para executar o exemplo em uma configuração de computador único ou cruzado, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
    > [!NOTE]
    > Como o servidor é auto-hospedado, você deve especificar uma identidade no arquivo app. config do cliente para executar o exemplo em uma configuração entre computadores.  
  
    ```xml  
    <client>  
      <endpoint name=""  
          address="net.tcp://servername:9000/servicemodelsamples/service"
          binding="netTcpBinding"
          contract="Microsoft.ServiceModel.Samples.ICalculator">  
            <identity>  
              <userPrincipalName value = "user_name@service_domain"/>  
            </identity>  
      </endpoint>  
    </client>  
    ```  
