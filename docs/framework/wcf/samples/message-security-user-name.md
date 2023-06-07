---
title: Message Security User Name
ms.date: 03/30/2017
helpviewer_keywords:
- WS Security
ms.assetid: c63cfc87-6b20-4949-93b3-bcd4b732b0a2
ms.openlocfilehash: 783969342f007895016ed4183257d6b24188d76c
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584721"
---
# <a name="message-security-user-name"></a>Message Security User Name
Este exemplo demonstra como implementar um aplicativo que usa o WS-Security com autenticação de nome de usuário para o cliente e requer autenticação de servidor usando o certificado X. 509v3 do servidor. Todas as mensagens de aplicativo entre o cliente e o servidor são assinadas e criptografadas. Por padrão, o nome de usuário e a senha fornecidos pelo cliente são usados para fazer logon em uma conta válida do Windows. Este exemplo se baseia na [WSHttpBinding](wshttpbinding.md). Este exemplo consiste em um programa de console do cliente (Client. exe) e uma biblioteca de serviços (Service. dll) hospedados pelo Serviços de Informações da Internet (IIS). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta.  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 Este exemplo também demonstra:  
  
- O mapeamento padrão para contas do Windows para que a autorização adicional possa ser executada.  
  
- Como acessar as informações de identidade do chamador do código de serviço.  
  
 O serviço expõe um único ponto de extremidade para se comunicar com o serviço, que é definido usando o arquivo de configuração Web. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) , que usa a segurança da mensagem como padrão. Este exemplo define o padrão [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) para usar a autenticação de nome de usuário do cliente. O comportamento especifica que as credenciais do usuário devem ser usadas para a autenticação do serviço. O certificado do servidor deve conter o mesmo valor para o nome da entidade como o `findValue` atributo no [\<serviceCredentials>](../../configure-apps/file-schema/wcf/servicecredentials.md) .  
  
```xml  
<system.serviceModel>  
  <protocolMapping>  
    <add scheme="http" binding="wsHttpBinding" />  
  </protocolMapping>  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      By default, Username authentication attempts to authenticate the provided  
      username as a Windows computer or domain account.  
      -->  
      <binding>  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <serviceBehaviors>  
      <behavior>  
        <!--   
      The serviceCredentials behavior allows one to define a service certificate.  
      A service certificate is used by the service to authenticate itself to the client and to provide message protection.  
      This configuration references the "localhost" certificate installed during the setup instructions.  
      -->  
        <serviceCredentials>  
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />  
        </serviceCredentials>  
        <serviceMetadata httpGetEnabled="True"/>  
        <serviceDebug includeExceptionDetailInFaults="False" />  
      </behavior>  
    </serviceBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 A configuração de ponto de extremidade do cliente consiste em um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o `securityMode` e o `authenticationMode` . Ao executar em um cenário entre computadores, o endereço do ponto de extremidade de serviço deve ser alterado de acordo.  
  
```xml  
<system.serviceModel>  
  <client>  
    <endpoint address="http://localhost/servicemodelsamples/service.svc"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              behaviorConfiguration="ClientCredentialsBehavior"  
              contract="Microsoft.ServiceModel.Samples.ICalculator" />  
  </client>  
  
  <bindings>  
    <wsHttpBinding>  
      <!--   
      This configuration defines the security mode as Message and   
      the clientCredentialType as Username.  
      -->  
      <binding name="Binding1">  
        <security mode="Message">  
          <message clientCredentialType="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true.-->  
  <behaviors>  
    <endpointBehaviors>  
      <behavior name="ClientCredentialsBehavior">  
        <!--   
      Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate   
      is in the user's Trusted People store, then it is trusted without performing a  
      validation of the certificate's issuer chain. This setting is used here for convenience so that the   
      sample can be run without having to have certificates issued by a certification authority (CA).  
      This setting is less secure than the default, ChainTrust. The security implications of this   
      setting should be carefully considered before using PeerOrChainTrust in production code.   
      -->  
        <clientCredentials>  
          <serviceCertificate>  
            <authentication certificateValidationMode="PeerOrChainTrust" />  
          </serviceCertificate>  
        </clientCredentials>  
      </behavior>  
    </endpointBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 A implementação do cliente define o nome de usuário e a senha a serem usados.  

```csharp
// Create a client.  
CalculatorClient client = new CalculatorClient();  
  
// Configure client with valid computer or domain account (username,password).  
client.ClientCredentials.UserName.UserName = username;  
client.ClientCredentials.UserName.Password = password.ToString();  
  
// Call GetCallerIdentity service operation.  
Console.WriteLine(client.GetCallerIdentity());  
...  
//Closing the client gracefully closes the connection and cleans up resources.  
client.Close();  
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.  
  
```console  
MyMachine\TestAccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
Press <ENTER> to terminate client.  
```  
  
 O arquivo em lotes setup. bat incluído com os exemplos do MessageSecurity permite que você configure o servidor com um certificado relevante para executar um aplicativo hospedado que requer segurança baseada em certificado. O arquivo em lotes pode ser executado em dois modos. Para executar o arquivo em lotes no modo de computador único, digite `setup.bat` na linha de comando. Para executá-lo no tipo de modo de serviço `setup.bat service` . Use esse modo ao executar o exemplo em computadores. Consulte o procedimento de instalação no final deste tópico para obter detalhes.  
  
 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes.  
  
- Criando o certificado do servidor  
  
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
  
     A variável% SERVER_NAME% especifica o nome do servidor. O certificado é armazenado no armazenamento LocalMachine. Se o arquivo em lotes setup. bat for executado com um argumento de serviço (como `setup.bat service` ), o% server_name% conterá o nome de domínio totalmente qualificado do computador.  Caso contrário, o padrão é localhost.  
  
- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente  
  
     A linha a seguir copia o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.  
  
    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
- Concedendo permissões na chave privada do certificado  
  
     As linhas a seguir no arquivo em lotes setup. bat tornam o certificado do servidor armazenado no repositório LocalMachine acessível para a conta de processo de trabalho do ASP.NET.  
  
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
    > Se você estiver usando uma edição que não seja a U. S. em inglês do Windows, deverá editar o arquivo setup. bat e substituir o `NT AUTHORITY\NETWORK SERVICE` nome da conta pelo equivalente regional.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador  
  
1. Verifique se o caminho inclui a pasta em que o MakeCert. exe e o FindPrivateKey. exe estão localizados.  
  
2. Execute setup. bat da pasta de instalação de exemplo em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.  
  
    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado de um Prompt de Comando do Desenvolvedor para o Visual Studio. Ele requer que a variável de ambiente Path aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente dentro de um Prompt de Comando do Desenvolvedor para o Visual Studio.  
  
3. Verifique o acesso ao serviço usando um navegador inserindo o endereço `http://localhost/servicemodelsamples/service.svc` .
  
4. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
5. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço. Crie um aplicativo virtual chamado servicemodelsamples para esse diretório usando a ferramenta de gerenciamento de Serviços de Informações da Internet.  
  
2. Copie os arquivos de programa do serviço do \inetpub\wwwroot\servicemodelsamples para o diretório virtual no computador do serviço. Certifique-se de copiar os arquivos no subdiretório \bin. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
3. Crie um diretório no computador cliente para os binários do cliente.  
  
4. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
5. No servidor, execute `setup.bat service` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
6. Edite Web. config para refletir o novo nome de certificado (no atributo FindValue no elemento Service-Certificate) que é o mesmo que o nome de domínio totalmente qualificado do computador`.`  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No cliente, execute ImportServiceCert. bat em um Prompt de Comando do Desenvolvedor para Visual Studio aberto com privilégios de administrador. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
10. No computador cliente, inicie o Client. exe em um prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
    > [!NOTE]
    > Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .  
