---
title: Exemplo de segurança de mensagem
ms.date: 03/30/2017
ms.assetid: 82444166-6288-493a-85d4-85f43f134d19
ms.openlocfilehash: 935695a46e907bf1deeb2e5cb24917ba92b81fe0
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584840"
---
# <a name="message-security-sample"></a>Exemplo de segurança de mensagem
Este exemplo demonstra como implementar um aplicativo que usa o `basicHttpBinding` e a segurança de mensagem. Este exemplo é baseado no [introdução](getting-started-sample.md) que implementa um serviço de calculadora.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O modo de segurança de `basicHttpBinding` pode ser definido com os seguintes valores: `Message` ,, `Transport` `TransportWithMessageCredential` `TransportCredentialOnly` e `None` . No arquivo app. config do exemplo a seguir, a definição do ponto de extremidade especifica o `basicHttpBinding` e faz referência a uma configuração de associação denominada `Binding1` , conforme mostrado na seguinte configuração de exemplo:  
  
```xml  
<system.serviceModel>  
  <services>  
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"  
             behaviorConfiguration="CalculatorServiceBehavior">  
     <!-- This endpoint is exposed at the base address provided by -->  
     <!-- host: http://localhost:8000/ServiceModelSamples/service.-->  
     <endpoint address=""  
               binding="basicHttpBinding"  
               bindingConfiguration="Binding1"
               contract="Microsoft.ServiceModel.Samples.ICalculator" />  
    </service>  
  </services>  
  ...  
</system.serviceModel>  
```  
  
 A configuração de associação define o `mode` atributo de [\<security>](../../configure-apps/file-schema/wcf/security-of-basichttpbinding.md) para `Message` e define o `clientCredentialType` atributo de como [\<message>](../../configure-apps/file-schema/wcf/message-of-basichttpbinding.md) `Certificate` , conforme mostrado na seguinte configuração de exemplo:  
  
```xml  
<bindings>  
  <basicHttpBinding>  
    <!--   
        This configuration defines the SecurityMode as Message and   
        the clientCredentialType as Certificate.  
        -->  
    <binding name="Binding1" >  
      <security mode = "Message">  
        <message clientCredentialType="Certificate"/>  
      </security>  
    </binding>  
  </basicHttpBinding>  
</bindings>  
```  
  
 O certificado que o serviço usa para se autenticar no cliente é definido na seção comportamentos do arquivo de configuração sob o `serviceCredentials` elemento. O modo de validação que se aplica ao certificado que o cliente usa para se autenticar para o serviço também é definido na seção comportamentos sob o `clientCertificate` elemento.  
  
```xml  
<!--For debugging purposes, set the includeExceptionDetailInFaults attribute to true.-->  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="CalculatorServiceBehavior">  
      <serviceMetadata httpGetEnabled="True"/>  
      <serviceDebug includeExceptionDetailInFaults="False" />  
      <!--The serviceCredentials behavior allows one to define a -->  
      <!--service certificate. A service certificate is used by a -->  
      <!--client to authenticate the service and provide message -->  
      <!-- protection. This configuration references the "localhost"-->  
      <!--certificate installed during the setup instructions. -->  
      <serviceCredentials>  
        <serviceCertificate findValue="localhost"  
               storeLocation="LocalMachine"
               storeName="My" x509FindType="FindBySubjectName" />  
        <clientCertificate>  
          <!-- Setting the certificateValidationMode to -->  
          <!-- PeerOrChainTrust means that if the certificate -->  
          <!--is in the user's Trusted People store, then it is -->  
          <!-- trusted without performing a validation of the -->  
          <!-- certificate's issuer chain. This setting is used -->  
          <!-- here for convenience so that the sample can be run -->  
          <!-- without having to have certificates issued by a -->  
          <!-- certification authority (CA). -->  
          <!-- This setting is less secure than the default, -->  
          <!-- ChainTrust. The security implications of this -->  
          <!-- setting should be carefully considered before using -->  
          <!-- PeerOrChainTrust in production code. -->  
          <authentication
                       certificateValidationMode="PeerOrChainTrust" />  
        </clientCertificate>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 Os mesmos detalhes de ligação e segurança são especificados no arquivo de configuração do cliente.  
  
 A identidade do chamador é exibida na janela do console de serviço usando o seguinte código:  

```csharp
Console.WriteLine("Called by {0}", ServiceSecurityContext.Current.PrimaryIdentity.Name);  
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador  
  
1. Execute setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.  
  
    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando SDK do Windows. Ele requer que a variável de ambiente MSSDK aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente em um prompt de comando SDK do Windows.  
  
2. Executar o aplicativo de serviço do \service\bin.  
  
3. Executar o aplicativo cliente do \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
5. Remova os certificados executando Cleanup. bat quando tiver concluído o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço para os binários de serviço.  
  
2. Copie os arquivos de programa do serviço para o diretório de serviço no servidor. Copie também os arquivos Setup. bat, Cleanup. bat e ImportClientCert. bat para o servidor.  
  
3. Crie um diretório no computador cliente para os binários do cliente.  
  
4. Copie os arquivos de programa do cliente para o diretório do cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
5. No servidor, execute `setup.bat service` . `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
6. Edite Service. exe. config para refletir o novo nome de certificado (no `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) elemento) que é o mesmo que o nome de domínio totalmente qualificado do computador. Além disso, altere o valor do endereço base para especificar um nome de computador totalmente qualificado em vez de localhost`.`  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório do cliente no computador cliente.  
  
8. No cliente, execute `setup.bat client` . `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
9. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Você faz isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor. Além disso, altere o `findValue` atributo do [\<defaultCertificate>](../../configure-apps/file-schema/wcf/defaultcertificate-element.md) para o novo nome do certificado de serviço, que é o nome de domínio totalmente qualificado do servidor.  
  
10. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
11. No cliente, execute ImportServiceCert. bat. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
12. No servidor, execute ImportClientCert. bat, isso importa o certificado do cliente do arquivo client. cer para o repositório LocalMachine-TrustedPeople.  
  
13. Na máquina de serviço, execute Service. exe em um prompt de comando.  
  
14. No computador cliente, inicie o Client. exe em uma janela de prompt de comando.  
  
    1. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo:`certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com`  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\Basic\MessageSecurity`  
