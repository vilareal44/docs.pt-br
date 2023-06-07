---
title: Provedor de função e associação
ms.date: 03/30/2017
ms.assetid: 0d11a31c-e75f-4fcf-9cf4-b7f26e056bcd
ms.openlocfilehash: e77e353fba194cb25b466387cf9def6773635e00
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591755"
---
# <a name="membership-and-role-provider"></a>Provedor de função e associação
O exemplo de provedor de associação e função demonstra como um serviço pode usar a associação ASP.NET e os provedores de função para autenticar e autorizar clientes.  
  
 Neste exemplo, o cliente é um aplicativo de console (. exe) e o serviço é hospedado pelo Serviços de Informações da Internet (IIS).  
  
> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.  
  
 O exemplo demonstra como:  
  
- Um cliente pode se autenticar usando a combinação nome de usuário-senha.  
  
- O servidor pode validar as credenciais do cliente em relação ao provedor de associação do ASP.NET.  
  
- O servidor pode ser autenticado usando o certificado X. 509 do servidor.  
  
- O servidor pode mapear o cliente autenticado para uma função usando o provedor de função ASP.NET.  
  
- O servidor pode usar o `PrincipalPermissionAttribute` para controlar o acesso a determinados métodos que são expostos pelo serviço.  
  
 Os provedores de associação e de função são configurados para usar um armazenamento apoiado por SQL Server. Uma cadeia de conexão e várias opções são especificadas no arquivo de configuração de serviço. O provedor de associação recebe o nome `SqlMembershipProvider` enquanto o provedor de função recebe o nome `SqlRoleProvider` .  
  
```xml  
<!-- Set the connection string for SQL Server -->  
<connectionStrings>  
  <add name="SqlConn"
       connectionString="Data Source=localhost;Integrated Security=SSPI;Initial Catalog=aspnetdb;" />  
</connectionStrings>  
  
<system.web>  
  <!-- Configure the Sql Membership Provider -->  
  <membership defaultProvider="SqlMembershipProvider" userIsOnlineTimeWindow="15">  
    <providers>  
      <clear />  
      <add
        name="SqlMembershipProvider"
        type="System.Web.Security.SqlMembershipProvider"
        connectionStringName="SqlConn"  
        applicationName="MembershipAndRoleProviderSample"  
        enablePasswordRetrieval="false"  
        enablePasswordReset="false"  
        requiresQuestionAndAnswer="false"  
        requiresUniqueEmail="true"  
        passwordFormat="Hashed" />  
    </providers>  
  </membership>  
  
  <!-- Configure the Sql Role Provider -->  
  <roleManager enabled ="true"
               defaultProvider ="SqlRoleProvider" >  
    <providers>  
      <add name ="SqlRoleProvider"
           type="System.Web.Security.SqlRoleProvider"
           connectionStringName="SqlConn"
           applicationName="MembershipAndRoleProviderSample"/>  
    </providers>  
  </roleManager>  
</system.web>  
```  
  
 O serviço expõe um único ponto de extremidade para se comunicar com o serviço, que é definido usando o arquivo de configuração Web. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsHttpBinding` , que usa a autenticação do Windows por padrão. Este exemplo define o padrão `wsHttpBinding` para usar a autenticação de nome de usuário. O comportamento especifica que o certificado do servidor deve ser usado para autenticação de serviço. O certificado do servidor deve conter o mesmo valor para o `SubjectName` como o `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) elemento de configuração. Além disso, o comportamento especifica que a autenticação de pares de nome de usuário-senha é executada pelo provedor de associação ASP.NET e o mapeamento de função é executado pelo provedor de função ASP.NET especificando os nomes definidos para os dois provedores.  
  
```xml  
<system.serviceModel>  
  
  <protocolMapping>  
    <add scheme="http" binding="wsHttpBinding" />  
  </protocolMapping>  
  
  <bindings>  
    <wsHttpBinding>  
      <!-- Set up a binding that uses Username as the client credential type -->  
      <binding>  
        <security mode ="Message">  
          <message clientCredentialType ="UserName"/>  
        </security>  
      </binding>  
    </wsHttpBinding>  
  </bindings>  
  
  <behaviors>  
    <serviceBehaviors>  
      <behavior>  
        <!-- Configure role based authorization to use the Role Provider -->  
        <serviceAuthorization principalPermissionMode ="UseAspNetRoles"  
                              roleProviderName ="SqlRoleProvider" />  
        <serviceCredentials>  
          <!-- Configure user name authentication to use the Membership Provider -->  
          <userNameAuthentication userNamePasswordValidationMode ="MembershipProvider"
                                  membershipProviderName ="SqlMembershipProvider"/>  
          <!-- Configure the service certificate -->  
          <serviceCertificate storeLocation ="LocalMachine"
                              storeName ="My"
                              x509FindType ="FindBySubjectName"  
                              findValue ="localhost" />  
        </serviceCredentials>  
        <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->  
        <serviceDebug includeExceptionDetailInFaults="false" />  
        <serviceMetadata httpGetEnabled="true"/>  
      </behavior>  
    </serviceBehaviors>  
  </behaviors>  
</system.serviceModel>  
```  
  
 Quando você executa o exemplo, o cliente chama as várias operações de serviço em três contas de usuário diferentes: Alice, Bob e Charlie. As solicitações e respostas da operação são exibidas na janela do console do cliente. Todas as quatro chamadas feitas como usuário "Alice" devem ter sucesso. O usuário "Bob" deve obter um erro de acesso negado ao tentar chamar o método de divisão. O usuário "Charlie" deve receber um erro de acesso negado ao tentar chamar o método multiplicar. Pressione ENTER na janela do cliente para desligar o cliente.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [executando os exemplos de Windows Communication Foundation](running-the-samples.md).  
  
2. Verifique se você configurou o [banco de dados ASP.NET Serviços de aplicativos](https://go.microsoft.com/fwlink/?LinkId=94997).  
  
    > [!NOTE]
    > Se você estiver executando o SQL Server Express Edition, o nome do servidor será .\SQLEXPRESS. Esse servidor deve ser usado ao configurar o banco de dados de Serviços de Aplicativos do ASP.NET, bem como na cadeia de conexão Web. config.  
  
    > [!NOTE]
    > A conta de processo de trabalho do ASP.NET deve ter permissões no banco de dados criado nesta etapa. Use o utilitário sqlcmd ou SQL Server Management Studio para fazer isso.  
  
3. Para executar o exemplo em uma configuração de computador único ou entre computadores, use as instruções a seguir.  
  
### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador  
  
1. Certifique-se de que o caminho inclui a pasta onde o MakeCert. exe está localizado.  
  
2. Execute setup. bat da pasta de instalação de exemplo em um Prompt de Comando do Desenvolvedor para o Visual Studio executar com privilégios de administrador. Isso instala os certificados de serviço necessários para executar o exemplo.  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço. Crie um aplicativo virtual chamado servicemodelsamples para esse diretório usando a ferramenta de gerenciamento do Serviços de Informações da Internet (IIS).  
  
2. Copie os arquivos de programa do serviço do \inetpub\wwwroot\servicemodelsamples para o diretório virtual no computador do serviço. Certifique-se de copiar os arquivos no subdiretório \bin. Copie também os arquivos Setup. bat, GetComputerName. vbs e Cleanup. bat para o computador de serviço.  
  
3. Crie um diretório no computador cliente para os binários do cliente.  
  
4. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
5. No servidor, abra um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos e execute `setup.bat service` . `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
6. Edite Web. config para refletir o novo nome de certificado (no `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador.  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório cliente no computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No cliente, abra um Prompt de Comando do Desenvolvedor para o Visual Studio com privilégios administrativos e execute ImportServiceCert. bat. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
10. No computador cliente, inicie o Client. exe em um prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
> [!NOTE]
> Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado Windows Communication Foundation (WCF) exemplos que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .  
  
## <a name="the-setup-batch-file"></a>O arquivo em lotes de instalação  
 O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com certificados relevantes para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.  
  
 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada.  
  
- Criando o certificado do servidor.  
  
     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A variável% SERVER_NAME% especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. Esse arquivo em lotes o padroniza para localhost.  
  
     O certificado é armazenado no meu repositório (pessoal) no local de armazenamento de LocalMachine.  
  
    ```console
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
- Instalar o certificado do servidor no repositório de certificados confiáveis do cliente.  
  
     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.  
  
    ```bat  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
