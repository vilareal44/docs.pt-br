---
title: Certificado de mensagem de segurança
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: 909333b3-35ec-48f0-baff-9a50161896f6
ms.openlocfilehash: 1dec66631368543eecd548ac1ec9bbcda466d746
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584838"
---
# <a name="message-security-certificate"></a>Certificado de mensagem de segurança
Este exemplo demonstra como implementar um aplicativo que usa o WS-Security com a autenticação de certificado X. 509 v3 para o cliente e requer autenticação de servidor usando o certificado X. 509 v3 do servidor. Este exemplo usa configurações padrão, de modo que todas as mensagens de aplicativo entre o cliente e o servidor sejam assinadas e criptografadas. Este exemplo se baseia na [WSHttpBinding](wshttpbinding.md) e consiste em um programa de console do cliente e uma biblioteca de serviços hospedada pelo serviços de informações da Internet (IIS). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo demonstra como controlar a autenticação usando a configuração e como obter a identidade do chamador do contexto de segurança, conforme mostrado no código de exemplo a seguir.  

```csharp
public class CalculatorService : ICalculator  
{  
    public string GetCallerIdentity()  
    {  
        // The client certificate is not mapped to a Windows identity by default.  
        // ServiceSecurityContext.PrimaryIdentity is populated based on the information  
        // in the certificate that the client used to authenticate itself to the service.  
        return ServiceSecurityContext.Current.PrimaryIdentity.Name;  
    }  
    ...  
}  
```  
  
 O serviço expõe um ponto de extremidade para se comunicar com o serviço e um ponto de extremidade para expor o documento WSDL do serviço usando o protocolo WS-MetadataExchange, definido usando o arquivo de configuração (Web. config). O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) elemento padrão, que usa a segurança da mensagem como padrão. Este exemplo define o `clientCredentialType` atributo como certificado para exigir autenticação de cliente.  
  
```xml  
<system.serviceModel>  
    <protocolMapping>  
      <add scheme="http" binding="wsHttpBinding"/>  
    </protocolMapping>  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding>  
          <security mode ="Message">  
            <message clientCredentialType="Certificate" />  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
    <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
    <behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
  </system.serviceModel>  
```  
  
 O comportamento especifica as credenciais do serviço que são usadas quando o cliente autentica o serviço. O nome da entidade do certificado do servidor é especificado no `findValue` atributo no [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) elemento.  
  
```xml  
<!--For debugging purposes, set the includeExceptionDetailInFaults attribute to true.-->  
<behaviors>  
      <serviceBehaviors>  
        <behavior>  
          <serviceMetadata httpGetEnabled="True"/>  
          <serviceDebug includeExceptionDetailInFaults="False" />  
          <!--   
        The serviceCredentials behavior allows one to define a service certificate.  
        A service certificate is used by the service to authenticate itself to its clients and to provide message protection.  
        This configuration references the "localhost" certificate installed during the setup instructions.  
        -->  
          <serviceCredentials>  
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
            <clientCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certification authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust" />  
            </clientCertificate>  
          </serviceCredentials>  
        </behavior>  
      </serviceBehaviors>  
    </behaviors>  
```  
  
 A configuração de ponto de extremidade do cliente consiste em um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o modo de segurança e o modos de autenticação apropriados. Ao executar em um cenário entre computadores, verifique se o endereço do ponto de extremidade de serviço foi alterado de acordo.  
  
```xml  
<system.serviceModel>  
    <client>  
      <!-- Use a behavior to configure the client certificate to present to the service. -->  
      <endpoint address="http://localhost/servicemodelsamples/service.svc" binding="wsHttpBinding" bindingConfiguration="Binding1" behaviorConfiguration="ClientCertificateBehavior" contract="Microsoft.Samples.Certificate.ICalculator"/>  
    </client>  
  
    <bindings>  
      <wsHttpBinding>  
        <!--   
        This configuration defines the security mode as Message and   
        the clientCredentialType as Certificate.  
        -->  
        <binding name="Binding1">  
          <security mode="Message">  
            <message clientCredentialType="Certificate"/>  
          </security>  
        </binding>  
      </wsHttpBinding>  
    </bindings>  
...  
</system.serviceModel>  
```  
  
 A implementação do cliente pode definir o certificado a ser usado, seja pelo arquivo de configuração ou por meio de código. O exemplo a seguir mostra como definir o certificado a ser usado no arquivo de configuração.  
  
```xml  
<system.serviceModel>  
  ...  
  
<behaviors>  
      <endpointBehaviors>  
        <behavior name="ClientCertificateBehavior">  
          <!--   
        The clientCredentials behavior allows one to define a certificate to present to a service.  
        A certificate is used by a client to authenticate itself to the service and provide message integrity.  
        This configuration references the "client.com" certificate installed during the setup instructions.  
        -->  
          <clientCredentials>  
            <clientCertificate findValue="client.com" storeLocation="CurrentUser" storeName="My" x509FindType="FindBySubjectName"/>  
            <serviceCertificate>  
              <!--   
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
            is in the user's Trusted People store, then it will be trusted without performing a  
            validation of the certificate's issuer chain. This setting is used here for convenience so that the   
            sample can be run without having to have certificates issued by a certificate authority (CA).  
            This setting is less secure than the default, ChainTrust. The security implications of this   
            setting should be carefully considered before using PeerOrChainTrust in production code.   
            -->  
              <authentication certificateValidationMode="PeerOrChainTrust"/>  
            </serviceCertificate>  
          </clientCredentials>  
        </behavior>  
      </endpointBehaviors>  
    </behaviors>  
  
</system.serviceModel>  
```  
  
 O exemplo a seguir mostra como chamar o serviço em seu programa.  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Call the GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```
  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
CN=client.com  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 O arquivo em lotes setup. bat incluído com os exemplos de segurança de mensagem permite que você configure o cliente e o servidor com certificados relevantes para executar um aplicativo hospedado que requer segurança baseada em certificado. O arquivo em lotes pode ser executado em três modos. Para executar em modo de computador único, digite **Setup. bat** em um prompt de comando do desenvolvedor para o Visual Studio; para o tipo de modo de serviço **Setup. bat Service**; e, para o modo de cliente, digite **Setup. bat cliente**. Use o modo de cliente e servidor ao executar o exemplo entre computadores. Consulte o procedimento de instalação no final deste tópico para obter detalhes. Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada:  
  
- Criando o certificado do cliente.  
  
     A linha a seguir no arquivo em lotes cria o certificado do cliente. O nome do cliente especificado é usado no nome da entidade do certificado criado. O certificado é armazenado no `My` repositório no `CurrentUser` local da loja.  
  
    ```bat
    echo ************  
    echo making client cert  
    echo ************  
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe  
    ```  
  
- Instalar o certificado do cliente no repositório de certificados confiáveis do servidor.  
  
     A linha a seguir no arquivo em lotes copia o certificado do cliente no repositório TrustedPeople do servidor para que o servidor possa fazer as decisões relevantes de confiança ou sem confiança. Para que um certificado instalado no repositório TrustedPeople seja confiável por um serviço Windows Communication Foundation (WCF), o modo de validação de certificado do cliente deve ser definido como `PeerOrChainTrust` ou `PeerTrust` . Consulte o exemplo de configuração de serviço anterior para saber como isso pode ser feito usando um arquivo de configuração.  
  
    ```bat
    echo ************  
    echo copying client cert to server's LocalMachine store  
    echo ************  
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople
    ```  
  
- Criando o certificado do servidor.  
  
     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado.  
  
    ```bat
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     A variável% SERVER_NAME% especifica o nome do servidor. O certificado é armazenado no armazenamento LocalMachine. Se o arquivo em lotes setup. bat for executado com um argumento de serviço (como, **serviço setup. bat**),% server_name% conterá o nome de domínio totalmente qualificado do computador. Caso contrário, o padrão é localhost.  
  
- Instalar o certificado do servidor no repositório de certificados confiáveis do cliente.  
  
     A linha a seguir copia o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.  
  
    ```console  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- Concedendo permissões na chave privada do certificado.  
  
     As linhas a seguir no arquivo setup. bat tornam o certificado do servidor armazenado no repositório LocalMachine acessível para a conta de processo de trabalho do ASP.NET.  
  
    ```bat
    echo ************  
    echo setting privileges on server certificates  
    echo ************  
    for /F "delims=" %%i in ('"%ProgramFiles%\ServiceModelSampleTools\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i  
    set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE  
    (ver | findstr /C:"5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET  
    echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R  
    iisreset  
    ```  
  
    > [!NOTE]
    > Se você estiver usando uma edição que não seja a U. S. inglês do Windows, deverá editar o arquivo setup. bat e substituir o nome da conta "NT AUTHORITY\NETWORK SERVICE" pelo equivalente regional.  
  
> [!NOTE]
> As ferramentas usadas neste arquivo em lotes estão localizadas em C:\Arquivos de Programas\microsoft Visual Studio 8 \ Common7\tools ou C:\Arquivos de Programas\microsoft SDKs\Windows\v6.0\bin. Um desses diretórios deve estar no caminho do sistema. Se você tiver o Visual Studio instalado, a maneira mais fácil de obter esse diretório em seu caminho é abrir o Prompt de Comando do Desenvolvedor para o Visual Studio. Clique em **Iniciar**e selecione **todos os programas**, **Visual Studio 2012**, **ferramentas**. Esse prompt de comando tem os caminhos apropriados já configurados. Caso contrário, você deve adicionar o diretório apropriado ao caminho manualmente.  
  
> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verificar o seguinte diretório (padrão) antes de continuar:  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está no seguinte diretório:  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Basic\Binding\WS\MessageSecurity`  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador  
  
1. Abra um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios de administrador e execute Setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.  
  
    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado de um Prompt de Comando do Desenvolvedor para o Visual Studio. Ele requer que a variável de ambiente Path aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente dentro de um Prompt de Comando do Desenvolvedor para o Visual Studio (2010).  
  
2. Verifique o acesso ao serviço usando um navegador inserindo o endereço `http://localhost/servicemodelsamples/service.svc` .  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço. Crie um aplicativo virtual chamado servicemodelsamples para esse diretório usando a ferramenta de gerenciamento do Serviços de Informações da Internet (IIS).  
  
2. Copie os arquivos de programa do serviço do \inetpub\wwwroot\servicemodelsamples para o diretório virtual no computador do serviço. Certifique-se de copiar os arquivos no subdiretório \bin. Copie também os arquivos Setup. bat, Cleanup. bat e ImportClientCert. bat para o computador de serviço.  
  
3. Crie um diretório no computador cliente para os binários do cliente.  
  
4. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
5. No servidor, execute o **serviço setup. bat** em um prompt de comando do desenvolvedor para o Visual Studio com privilégios de administrador. A execução de **Setup. bat** com o argumento de **serviço** cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
6. Edite Web. config para refletir o novo nome de certificado (no `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador.  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
8. No cliente, execute o **cliente setup. bat** em um prompt de comando do desenvolvedor para o Visual Studio com privilégios de administrador. A execução de **Setup. bat** com o argumento **Client** cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
9. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Faça isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor.  
  
10. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
11. No cliente, execute ImportServiceCert. bat em um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
12. No servidor, execute ImportClientCert. bat em um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos. Isso importa o certificado do cliente do arquivo client. cer para o repositório LocalMachine-TrustedPeople.  
  
13. No computador cliente, inicie o Client. exe em uma janela de prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .  
