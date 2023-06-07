---
title: Política de autorização
ms.date: 03/30/2017
ms.assetid: 1db325ec-85be-47d0-8b6e-3ba2fdf3dda0
ms.openlocfilehash: 5b93f7e05261d9770650335160ddb56404aed94d
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84585500"
---
# <a name="authorization-policy"></a>Política de autorização

Este exemplo demonstra como implementar uma política de autorização de declaração personalizada e um Gerenciador de autorização de serviço personalizado associado. Isso é útil quando o serviço faz verificações de acesso baseadas em declaração para operações de serviço e antes das verificações de acesso, concede ao chamador determinados direitos. Este exemplo mostra o processo de adição de declarações, bem como o processo de fazer uma verificação de acesso em relação ao conjunto de declarações finalizado. Todas as mensagens de aplicativo entre o cliente e o servidor são assinadas e criptografadas. Por padrão, com a `wsHttpBinding` associação, um nome de usuário e senha fornecidos pelo cliente são usados para fazer logon em uma conta válida do Windows NT. Este exemplo demonstra como utilizar um personalizado <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> para autenticar o cliente. Além disso, este exemplo mostra o cliente que está Autenticando para o serviço usando um certificado X. 509. Este exemplo mostra uma implementação de <xref:System.IdentityModel.Policy.IAuthorizationPolicy> e <xref:System.ServiceModel.ServiceAuthorizationManager> , que entre eles concedem acesso a métodos específicos do serviço para usuários específicos. Este exemplo é baseado no [nome de usuário de segurança da mensagem](message-security-user-name.md), mas demonstra como executar uma transformação de declaração antes da <xref:System.ServiceModel.ServiceAuthorizationManager> chamada.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

 Em resumo, este exemplo demonstra como:

- O cliente pode ser autenticado usando um nome de usuário-senha.

- O cliente pode ser autenticado usando um certificado X. 509.

- O servidor valida as credenciais do cliente em relação a um `UsernamePassword` validador personalizado.

- O servidor é autenticado usando o certificado X. 509 do servidor.

- O servidor pode usar o <xref:System.ServiceModel.ServiceAuthorizationManager> para controlar o acesso a determinados métodos no serviço.

- Como implementar <xref:System.IdentityModel.Policy.IAuthorizationPolicy> .

O serviço expõe dois pontos de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração app. config. Cada ponto de extremidade consiste em um endereço, uma associação e um contrato. Uma associação é configurada com uma `wsHttpBinding` associação padrão que usa o WS-Security e a autenticação de nome de usuário do cliente. A outra associação é configurada com uma `wsHttpBinding` associação padrão que usa WS-Security e autenticação de certificado do cliente. O [\<behavior>](../../configure-apps/file-schema/wcf/behavior-of-endpointbehaviors.md) especifica que as credenciais de usuário devem ser usadas para autenticação de serviço. O certificado do servidor deve conter o mesmo valor para a `SubjectName` propriedade que o `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) .

```xml
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <host>
        <baseAddresses>
          <!-- configure base address provided by host -->
          <add baseAddress ="http://localhost:8001/servicemodelsamples/service"/>
        </baseAddresses>
      </host>
      <!-- use base address provided by host, provide two endpoints -->
      <endpoint address="username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
      <endpoint address="certificate"
                binding="wsHttpBinding"
                bindingConfiguration="Binding2"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>

  <bindings>
    <wsHttpBinding>
      <!-- Username binding -->
      <binding name="Binding1">
        <security mode="Message">
    <message clientCredentialType="UserName" />
        </security>
      </binding>
      <!-- X509 certificate binding -->
      <binding name="Binding2">
        <security mode="Message">
          <message clientCredentialType="Certificate" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>

  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior" >
        <serviceDebug includeExceptionDetailInFaults ="true" />
        <serviceCredentials>
          <!--
          The serviceCredentials behavior allows one to specify a custom validator for username/password combinations.
          -->
          <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyCustomUserNameValidator, service" />
          <!--
          The serviceCredentials behavior allows one to specify authentication constraints on client certificates.
          -->
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
          <!--
          The serviceCredentials behavior allows one to define a service certificate.
          A service certificate is used by a client to authenticate the service and provide message protection.
          This configuration references the "localhost" certificate installed during the setup instructions.
          -->
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
        </serviceCredentials>
        <serviceAuthorization serviceAuthorizationManagerType="Microsoft.ServiceModel.Samples.MyServiceAuthorizationManager, service">
          <!--
          The serviceAuthorization behavior allows one to specify custom authorization policies.
          -->
          <authorizationPolicies>
            <add policyType="Microsoft.ServiceModel.Samples.CustomAuthorizationPolicy.MyAuthorizationPolicy, PolicyLibrary" />
          </authorizationPolicies>
        </serviceAuthorization>
      </behavior>
    </serviceBehaviors>
  </behaviors>

</system.serviceModel>
```

Cada configuração de ponto de extremidade de cliente consiste em um nome de configuração, um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o modo de segurança apropriado, conforme especificado nesse caso, no [\<security>](../../configure-apps/file-schema/wcf/security-of-wshttpbinding.md) e `clientCredentialType` conforme especificado no [\<message>](../../configure-apps/file-schema/wcf/message-of-wshttpbinding.md) .

```xml
<system.serviceModel>

    <client>
      <!-- Username based endpoint -->
      <endpoint name="Username"
            address="http://localhost:8001/servicemodelsamples/service/username"
    binding="wsHttpBinding"
    bindingConfiguration="Binding1"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator" >
      </endpoint>
      <!-- X509 certificate based endpoint -->
      <endpoint name="Certificate"
                        address="http://localhost:8001/servicemodelsamples/service/certificate"
                binding="wsHttpBinding"
            bindingConfiguration="Binding2"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>

    <bindings>
      <wsHttpBinding>
        <!-- Username binding -->
      <binding name="Binding1">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
        <!-- X509 certificate binding -->
        <binding name="Binding2">
          <security mode="Message">
            <message clientCredentialType="Certificate" />
          </security>
        </binding>
    </wsHttpBinding>
    </bindings>

    <behaviors>
      <behavior name="ClientCertificateBehavior">
        <clientCredentials>
          <serviceCertificate>
            <!--
            Setting the certificateValidationMode to PeerOrChainTrust
            means that if the certificate
            is in the user's Trusted People store, then it will be
            trusted without performing a
            validation of the certificate's issuer chain. This setting
            is used here for convenience so that the
            sample can be run without having to have certificates
            issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust.
            The security implications of this
            setting should be carefully considered before using
            PeerOrChainTrust in production code.
            -->
            <authentication certificateValidationMode = "PeerOrChainTrust" />
          </serviceCertificate>
        </clientCredentials>
      </behavior>
    </behaviors>

  </system.serviceModel>
```

Para o ponto de extremidade baseado em nome de usuário, a implementação do cliente define o nome de usuário e a senha a serem usados.

```csharp
// Create a client with Username endpoint configuration
CalculatorClient client1 = new CalculatorClient("Username");

client1.ClientCredentials.UserName.UserName = "test1";
client1.ClientCredentials.UserName.Password = "1tset";

try
{
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = client1.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
    ...
}
catch (Exception e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
}

client1.Close();
```

Para o ponto de extremidade baseado em certificado, a implementação do cliente define o certificado do cliente a ser usado.

```csharp
// Create a client with Certificate endpoint configuration
CalculatorClient client2 = new CalculatorClient("Certificate");

client2.ClientCredentials.ClientCertificate.SetCertificate(StoreLocation.CurrentUser, StoreName.My, X509FindType.FindBySubjectName, "test1");

try
{
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = client2.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
    ...
}
catch (Exception e)
{
    Console.WriteLine("Call failed : {0}", e.Message);
}

client2.Close();
```

Este exemplo usa um personalizado <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> para validar nomes de usuário e senhas. O exemplo implementa `MyCustomUserNamePasswordValidator` , derivado de <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> . Consulte a documentação sobre <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> para obter mais informações. Para fins de demonstração da integração com o <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> , este exemplo de validador personalizado implementa o <xref:System.IdentityModel.Selectors.UserNamePasswordValidator.Validate%2A> método para aceitar pares de nome de usuário/senha em que o nome de usuário corresponde à senha, conforme mostrado no código a seguir.

```csharp
public class MyCustomUserNamePasswordValidator : UserNamePasswordValidator
{
  // This method validates users. It allows in two users,
  // test1 and test2 with passwords 1tset and 2tset respectively.
  // This code is for illustration purposes only and
  // MUST NOT be used in a production environment because it
  // is NOT secure.
  public override void Validate(string userName, string password)
  {
    if (null == userName || null == password)
    {
      throw new ArgumentNullException();
    }

    if (!(userName == "test1" && password == "1tset") && !(userName == "test2" && password == "2tset"))
    {
      throw new SecurityTokenException("Unknown Username or Password");
    }
  }
}
```

Depois que o validador for implementado no código de serviço, o host de serviço deverá ser informado sobre a instância do validador a ser usada. Isso é feito usando o seguinte código:

```csharp
Servicehost.Credentials.UserNameAuthentication.UserNamePasswordValidationMode = UserNamePasswordValidationMode.Custom;
serviceHost.Credentials.UserNameAuthentication.CustomUserNamePasswordValidator = new MyCustomUserNamePasswordValidatorProvider();
```

Ou você pode fazer a mesma coisa na configuração:

```xml
<behavior>
    <serviceCredentials>
      <!--
      The serviceCredentials behavior allows one to specify a custom validator for username/password combinations.
      -->
      <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.MyCustomUserNameValidator, service" />
    ...
    </serviceCredentials>
</behavior>
```

O Windows Communication Foundation (WCF) fornece um modelo rico baseado em declarações para executar verificações de acesso. O <xref:System.ServiceModel.ServiceAuthorizationManager> objeto é usado para executar a verificação de acesso e determinar se as declarações associadas ao cliente atendem aos requisitos necessários para acessar o método de serviço.

Para fins de demonstração, este exemplo mostra uma implementação do <xref:System.ServiceModel.ServiceAuthorizationManager> que implementa o <xref:System.ServiceModel.ServiceAuthorizationManager.CheckAccessCore%2A> método para permitir o acesso de um usuário a métodos com base em declarações do tipo `http://example.com/claims/allowedoperation` cujo valor é o URI da ação que tem permissão para ser chamada.

```csharp
public class MyServiceAuthorizationManager : ServiceAuthorizationManager
{
  protected override bool CheckAccessCore(OperationContext operationContext)
  {
    string action = operationContext.RequestContext.RequestMessage.Headers.Action;
    Console.WriteLine("action: {0}", action);
    foreach(ClaimSet cs in operationContext.ServiceSecurityContext.AuthorizationContext.ClaimSets)
    {
      if ( cs.Issuer == ClaimSet.System )
      {
        foreach (Claim c in cs.FindClaims("http://example.com/claims/allowedoperation", Rights.PossessProperty))
        {
          Console.WriteLine("resource: {0}", c.Resource.ToString());
          if (action == c.Resource.ToString())
            return true;
        }
      }
    }
    return false;
  }
}
```

Depois que o personalizado <xref:System.ServiceModel.ServiceAuthorizationManager> é implementado, o host de serviço deve ser informado sobre o <xref:System.ServiceModel.ServiceAuthorizationManager> a ser usado. Isso é feito conforme mostrado no código a seguir.

```xml
<behavior>
    ...
    <serviceAuthorization serviceAuthorizationManagerType="Microsoft.ServiceModel.Samples.MyServiceAuthorizationManager, service">
        ...
    </serviceAuthorization>
</behavior>
```

O <xref:System.IdentityModel.Policy.IAuthorizationPolicy> método principal a ser implementado é o <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29> método.

```csharp
public class MyAuthorizationPolicy : IAuthorizationPolicy
{
    string id;

    public MyAuthorizationPolicy()
    {
    id =  Guid.NewGuid().ToString();
    }

    public bool Evaluate(EvaluationContext evaluationContext,
                                            ref object state)
    {
        bool bRet = false;
        CustomAuthState customstate = null;

        if (state == null)
        {
            customstate = new CustomAuthState();
            state = customstate;
        }
        else
            customstate = (CustomAuthState)state;
        Console.WriteLine("In Evaluate");
        if (!customstate.ClaimsAdded)
        {
           IList<Claim> claims = new List<Claim>();

           foreach (ClaimSet cs in evaluationContext.ClaimSets)
              foreach (Claim c in cs.FindClaims(ClaimTypes.Name,
                                         Rights.PossessProperty))
                  foreach (string s in
                        GetAllowedOpList(c.Resource.ToString()))
                  {
                       claims.Add(new
               Claim("http://example.com/claims/allowedoperation",
                                    s, Rights.PossessProperty));
                            Console.WriteLine("Claim added {0}", s);
                      }
                   evaluationContext.AddClaimSet(this,
                           new DefaultClaimSet(this.Issuer,claims));
                   customstate.ClaimsAdded = true;
                   bRet = true;
                }
         else
         {
              bRet = true;
         }
         return bRet;
     }
...
}
```

O código anterior mostra como o <xref:System.IdentityModel.Policy.IAuthorizationPolicy.Evaluate%28System.IdentityModel.Policy.EvaluationContext%2CSystem.Object%40%29> método verifica se nenhuma declaração nova foi adicionada, o que afeta o processamento e adiciona declarações específicas. As declarações que são permitidas são obtidas do `GetAllowedOpList` método, que é implementado para retornar uma lista específica de operações que o usuário tem permissão para executar. A política de autorização adiciona declarações para acessar a operação específica. Isso é usado posteriormente pelo <xref:System.ServiceModel.ServiceAuthorizationManager> para executar decisões de verificação de acesso.

Depois que o personalizado <xref:System.IdentityModel.Policy.IAuthorizationPolicy> é implementado, o host de serviço deve ser informado sobre as políticas de autorização a serem usadas.

```xml
<serviceAuthorization>
       <authorizationPolicies>
            <add policyType='Microsoft.ServiceModel.Samples.CustomAuthorizationPolicy.MyAuthorizationPolicy, PolicyLibrary' />
       </authorizationPolicies>
</serviceAuthorization>
```

Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. O cliente chama com êxito os métodos Add, subtrair e Multiple e obtém uma mensagem "acesso negado" ao tentar chamar o método de divisão. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação

O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com certificados relevantes para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor.

Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada:

- Criando o certificado do servidor.

    As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A variável% SERVER_NAME% especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O valor padrão é localhost.

    ```bat
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente.

    As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

- Criando o certificado do cliente.

    As linhas a seguir do arquivo em lotes setup. bat criam o certificado do cliente a ser usado. A variável% USER_NAME% especifica o nome do servidor. Esse valor é definido como "test1" porque esse é o nome que o `IAuthorizationPolicy` procura. Se você alterar o valor de% USER_NAME%, deverá alterar o valor correspondente no `IAuthorizationPolicy.Evaluate` método.

    O certificado é armazenado no meu repositório (pessoal) no local de armazenamento CurrentUser.

    ```bat
    echo ************
    echo making client cert
    echo ************
    makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe
    ```

- Instalando o certificado do cliente no repositório de certificados confiáveis do servidor.

    As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do cliente para o repositório de pessoas confiáveis. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema de servidor. Se você já tiver um certificado com raiz em um certificado raiz confiável — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do servidor com o certificado do cliente não será necessária.

    ```console
    certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople
    ```

### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

2. Para executar o exemplo em uma configuração de computador único ou entre computadores, use as instruções a seguir.

> [!NOTE]
> Se você usar svcutil. exe para regenerar a configuração para este exemplo, certifique-se de modificar o nome do ponto de extremidade na configuração do cliente para corresponder ao código do cliente.

### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Abra Prompt de Comando do Desenvolvedor para Visual Studio com privilégios de administrador e execute *Setup. bat* na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado do Prompt de Comando do Desenvolvedor para o Visual Studio. A variável de ambiente PATH definida no Prompt de Comando do Desenvolvedor para o Visual Studio aponta para o diretório que contém os executáveis exigidos pelo script *Setup. bat* .

1. Inicie o Service. exe de *create\bin*.

1. Inicie o Client. exe em *\client\bin*. A atividade do cliente é exibida no aplicativo de console do cliente.

Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).

### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores

1. Crie um diretório no computador de serviço.

2. Copie os arquivos de programa do serviço do *\service\bin* para o diretório no computador do serviço. Copie também os arquivos Setup. bat, Cleanup. bat, GetComputerName. vbs e ImportClientCert. bat para o computador de serviço.

3. Crie um diretório no computador cliente para os binários do cliente.

4. Copie os arquivos de programa do cliente para o diretório cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.

5. No servidor, execute `setup.bat service` no prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador.

    `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado *Service. cer*.

6. Edite *Service. exe. config* para refletir o novo nome de certificado (no `findValue` atributo em [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador. Altere também o **ComputerName** no \<service> / \<baseAddresses> elemento do localhost para o nome totalmente qualificado do seu computador de serviço.

7. Copie o arquivo *Service. cer* do diretório de serviço para o diretório cliente no computador cliente.

8. No cliente, execute `setup.bat client` no prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador.

    `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado **Test1** e exporta o certificado do cliente para um arquivo chamado *Client. cer*.

9. No arquivo *Client. exe. config* no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Faça isso substituindo **localhost** pelo nome de domínio totalmente qualificado do servidor.

10. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.

11. No cliente, execute *ImportServiceCert. bat* no prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador.

    Isso importa o certificado de serviço do arquivo Service. cer para o repositório **CurrentUser-TrustedPeople** .

12. No servidor, execute *ImportClientCert. bat* no prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador.

    Isso importa o certificado do cliente do arquivo client. cer para o repositório **LocalMachine-TrustedPeople** .

13. No computador servidor, inicie o Service. exe na janela do prompt de comando.

14. No computador cliente, inicie o Client. exe em uma janela de prompt de comando.

    Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).

### <a name="clean-up-after-the-sample"></a>Limpar após o exemplo

Para limpar após o exemplo, execute *Cleanup. bat* na pasta Samples quando terminar de executar o exemplo. Isso remove os certificados de cliente e servidor do repositório de certificados.

> [!NOTE]
> Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado os exemplos do WCF que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .
