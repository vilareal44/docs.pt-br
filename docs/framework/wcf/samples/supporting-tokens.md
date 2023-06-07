---
title: Tokens com suporte
ms.date: 03/30/2017
ms.assetid: 65a8905d-92cc-4ab0-b6ed-1f710e40784e
ms.openlocfilehash: 9c8ee4b11cd61e51e91c2e116ab3c20448fc1a58
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84575037"
---
# <a name="supporting-tokens"></a>Tokens com suporte
O exemplo de tokens de suporte demonstra como adicionar tokens adicionais a uma mensagem que usa o WS-Security. O exemplo adiciona um token de segurança binário X. 509, além de um token de segurança de nome de usuário. O token é passado em um cabeçalho de mensagem do WS-Security do cliente para o serviço e parte da mensagem é assinada com a chave privada associada ao token de segurança X. 509 para provar a posse do certificado X. 509 para o receptor. Isso é útil no caso em que há um requisito para ter várias declarações associadas a uma mensagem para autenticar ou autorizar o remetente. O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta.

## <a name="demonstrates"></a>Demonstra
 O exemplo demonstra:

- Como um cliente pode passar tokens de segurança adicionais para um serviço.

- Como o servidor pode acessar declarações associadas a tokens de segurança adicionais.

- Como o certificado X. 509 do servidor é usado para proteger a chave simétrica usada para criptografia e assinatura de mensagens.

> [!NOTE]
> O procedimento de instalação e as instruções de Build para este exemplo estão localizados no final deste tópico.

## <a name="client-authenticates-with-username-token-and-supporting-x509-security-token"></a>O cliente autentica com o token de nome de usuário e dá suporte ao token de segurança X. 509
 O serviço expõe um único ponto de extremidade para comunicação que é criada programaticamente usando as `BindingHelper` `EchoServiceHost` classes e. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com uma associação personalizada usando `SymmetricSecurityBindingElement` e `HttpTransportBindingElement` . Este exemplo define o `SymmetricSecurityBindingElement` para usar um certificado de serviço X. 509 para proteger a chave simétrica durante a transmissão e passar um `UserNameToken` junto com o suporte `X509SecurityToken` em um cabeçalho de mensagem do WS-Security. A chave simétrica é usada para criptografar o corpo da mensagem e o token de segurança do nome de usuário. O token de suporte é passado como um token de segurança binário adicional no cabeçalho da mensagem WS-Security. A autenticidade do token de suporte é comprovada pela assinatura de parte da mensagem com a chave privada associada ao token de segurança X. 509 com suporte.

```csharp
public static Binding CreateMultiFactorAuthenticationBinding()
{
    HttpTransportBindingElement httpTransport = new HttpTransportBindingElement();

    // the message security binding element will be configured to require 2 tokens:
    // 1) A username-password encrypted with the service token
    // 2) A client certificate used to sign the message

    // Instantiate a binding element that will require the username/password token in the message (encrypted with the server cert)
    SymmetricSecurityBindingElement messageSecurity = SecurityBindingElement.CreateUserNameForCertificateBindingElement();

    // Create supporting token parameters for the client X509 certificate.
    X509SecurityTokenParameters clientX509SupportingTokenParameters = new X509SecurityTokenParameters();
    // Specify that the supporting token is passed in message send by the client to the service
    clientX509SupportingTokenParameters.InclusionMode = SecurityTokenInclusionMode.AlwaysToRecipient;
    // Turn off derived keys
    clientX509SupportingTokenParameters.RequireDerivedKeys = false;
    // Augment the binding element to require the client's X509 certificate as an endorsing token in the message
    messageSecurity.EndpointSupportingTokenParameters.Endorsing.Add(clientX509SupportingTokenParameters);

    // Create a CustomBinding based on the constructed security binding element.
    return new CustomBinding(messageSecurity, httpTransport);
}
```

 O comportamento especifica as credenciais de serviço que devem ser usadas para autenticação de cliente e também informações sobre o certificado X. 509 do serviço. O exemplo usa `CN=localhost` como um nome de assunto no certificado X. 509 do serviço.

```csharp
override protected void InitializeRuntime()
{
    // Extract the ServiceCredentials behavior or create one.
    ServiceCredentials serviceCredentials =
        this.Description.Behaviors.Find<ServiceCredentials>();
    if (serviceCredentials == null)
    {
        serviceCredentials = new ServiceCredentials();
        this.Description.Behaviors.Add(serviceCredentials);
    }

    // Set the service certificate
    serviceCredentials.ServiceCertificate.SetCertificate(
                                       "CN=localhost");

/*
Setting the CertificateValidationMode to PeerOrChainTrust means that if the certificate is in the Trusted People store, then it will be trusted without performing a validation of the certificate's issuer chain. This setting is used here for convenience so that the sample can be run without having to have certificates issued by a certification authority (CA).
This setting is less secure than the default, ChainTrust. The security implications of this setting should be carefully considered before using PeerOrChainTrust in production code.
*/
    serviceCredentials.ClientCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.PeerOrChainTrust;

    // Create the custom binding and add an endpoint to the service.
    Binding multipleTokensBinding =
         BindingHelper.CreateMultiFactorAuthenticationBinding();
    this.AddServiceEndpoint(typeof(IEchoService),
                          multipleTokensBinding, string.Empty);
    base.InitializeRuntime();
}
```

 Código de serviço:

```csharp
[ServiceBehavior(IncludeExceptionDetailInFaults = true)]
public class EchoService : IEchoService
{
    public string Echo()
    {
        string userName;
        string certificateSubjectName;
        GetCallerIdentities(
            OperationContext.Current.ServiceSecurityContext,
            out userName,
            out certificateSubjectName);
            return $"Hello {userName}, {certificateSubjectName}";
    }

    public void Dispose()
    {
    }

    bool TryGetClaimValue<TClaimResource>(ClaimSet claimSet,
            string claimType, out TClaimResource resourceValue)
            where TClaimResource : class
    {
        resourceValue = default(TClaimResource);
        IEnumerable<Claim> matchingClaims =
            claimSet.FindClaims(claimType, Rights.PossessProperty);
        if(matchingClaims == null)
            return false;
        IEnumerator<Claim> enumerator = matchingClaims.GetEnumerator();
        if (enumerator.MoveNext())
        {
            resourceValue =
              (enumerator.Current.Resource == null) ? null :
              (enumerator.Current.Resource as TClaimResource);
            return true;
        }
        else
        {
            return false;
        }
    }

    // Returns the username and certificate subject name provided by
    //the client
    void GetCallerIdentities(ServiceSecurityContext
        callerSecurityContext,
        out string userName, out string certificateSubjectName)
    {
        userName = null;
        certificateSubjectName = null;

       // Look in all the claimsets in the authorization context
       foreach (ClaimSet claimSet in
               callerSecurityContext.AuthorizationContext.ClaimSets)
       {
            if (claimSet is WindowsClaimSet)
            {
                // Try to find a Name claim. This will have been
                // generated from the windows username.
                string tmpName;
                if (TryGetClaimValue<string>(claimSet, ClaimTypes.Name,
                                                      out tmpName))
                {
                    userName = tmpName;
                }
            }
            else if (claimSet is X509CertificateClaimSet)
            {
                // Try to find an X500DistinguishedName claim. This will
                // have been generated from the client certificate.
                X500DistinguishedName tmpDistinguishedName;
                if (TryGetClaimValue<X500DistinguishedName>(claimSet,
                               ClaimTypes.X500DistinguishedName,
                               out tmpDistinguishedName))
                {
                    certificateSubjectName = tmpDistinguishedName.Name;
                }
            }
        }
    }
}
```

 O ponto de extremidade do cliente é configurado de forma semelhante ao ponto de extremidade de serviço. O cliente usa a mesma `BindingHelper` classe para criar uma associação. O restante da instalação está localizado na `Client` classe. O cliente define informações sobre o token de segurança de nome de usuário, o token de segurança X. 509 com suporte e informações sobre o certificado de serviço X. 509 no código de instalação para a coleção de comportamentos de ponto de extremidade do cliente.

```csharp
 static void Main()
 {
     // Create the custom binding and an endpoint address for
     // the service.
     Binding multipleTokensBinding =
         BindingHelper.CreateMultiFactorAuthenticationBinding();
         EndpointAddress serviceAddress = new EndpointAddress(
         "http://localhost/servicemodelsamples/service.svc");
       ChannelFactory<IEchoService> channelFactory = null;
       IEchoService client = null;

       Console.WriteLine("Username authentication required.");
       Console.WriteLine(
         "Provide a valid machine or domain account. [domain\\user]");
       Console.WriteLine("   Enter username:");
       string username = Console.ReadLine();
       Console.WriteLine("   Enter password:");
       string password = "";
       ConsoleKeyInfo info = Console.ReadKey(true);
       while (info.Key != ConsoleKey.Enter)
       {
           if (info.Key != ConsoleKey.Backspace)
           {
               if (info.KeyChar != '\0')
               {
                   password += info.KeyChar;
                }
                info = Console.ReadKey(true);
            }
            else if (info.Key == ConsoleKey.Backspace)
            {
                if (password != "")
                {
                    password =
                       password.Substring(0, password.Length - 1);
                }
                info = Console.ReadKey(true);
            }
         }
         for (int i = 0; i < password.Length; i++)
            Console.Write("*");
         Console.WriteLine();
         try
         {
           // Create a proxy with the previously create binding and
           // endpoint address
              channelFactory =
                 new ChannelFactory<IEchoService>(
                     multipleTokensBinding, serviceAddress);
           // configure the username credentials, the client
           // certificate and the server certificate on the channel
           // factory
           channelFactory.Credentials.UserName.UserName = username;
           channelFactory.Credentials.UserName.Password = password;
           channelFactory.Credentials.ClientCertificate.SetCertificate(
           "CN=client.com", StoreLocation.CurrentUser, StoreName.My);
              channelFactory.Credentials.ServiceCertificate.SetDefaultCertificate(
           "CN=localhost", StoreLocation.LocalMachine, StoreName.My);
           client = channelFactory.CreateChannel();
           Console.WriteLine("Echo service returned: {0}",
                                           client.Echo());

           ((IChannel)client).Close();
           channelFactory.Close();
        }
        catch (CommunicationException e)
        {
         Abort((IChannel)client, channelFactory);
         // if there is a fault then print it out
         FaultException fe = null;
         Exception tmp = e;
         while (tmp != null)
         {
            fe = tmp as FaultException;
            if (fe != null)
            {
                break;
            }
            tmp = tmp.InnerException;
        }
        if (fe != null)
        {
           Console.WriteLine("The server sent back a fault: {0}",
         fe.CreateMessageFault().Reason.GetMatchingTranslation().Text);
        }
        else
        {
         Console.WriteLine("The request failed with exception: {0}",e);
        }
    }
    catch (TimeoutException)
    {
        Abort((IChannel)client, channelFactory);
        Console.WriteLine("The request timed out");
    }
    catch (Exception e)
    {
         Abort((IChannel)client, channelFactory);
          Console.WriteLine(
          "The request failed with unexpected exception: {0}", e);
    }
    Console.WriteLine();
    Console.WriteLine("Press <ENTER> to terminate client.");
    Console.ReadLine();
}
```

## <a name="displaying-callers-information"></a>Exibindo informações de chamadores
 Para exibir as informações do chamador, você pode usar o `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets` conforme mostrado no código a seguir. O `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets` contém declarações de autorização associadas ao chamador atual. Essas declarações são fornecidas automaticamente pelo Windows Communication Foundation (WCF) para cada token recebido na mensagem.

```csharp
bool TryGetClaimValue<TClaimResource>(ClaimSet claimSet, string
                         claimType, out TClaimResource resourceValue)
    where TClaimResource : class
{
    resourceValue = default(TClaimResource);
    IEnumerable<Claim> matchingClaims =
    claimSet.FindClaims(claimType, Rights.PossessProperty);
    if (matchingClaims == null)
          return false;
    IEnumerator<Claim> enumerator = matchingClaims.GetEnumerator();
    if (enumerator.MoveNext())
    {
        resourceValue = (enumerator.Current.Resource == null) ? null : (enumerator.Current.Resource as TClaimResource);
        return true;
    }
    else
    {
         return false;
    }
}

// Returns the username and certificate subject name provided by the client
void GetCallerIdentities(ServiceSecurityContext callerSecurityContext, out string userName, out string certificateSubjectName)
{
    userName = null;
    certificateSubjectName = null;

    // Look in all the claimsets in the authorization context
    foreach (ClaimSet claimSet in
      callerSecurityContext.AuthorizationContext.ClaimSets)
    {
        if (claimSet is WindowsClaimSet)
        {
            // Try to find a Name claim. This will have been generated
            //from the windows username.
            string tmpName;
            if (TryGetClaimValue<string>(claimSet, ClaimTypes.Name,
                                                     out tmpName))
            {
                userName = tmpName;
            }
        }
        else if (claimSet is X509CertificateClaimSet)
         {
            //Try to find an X500DistinguishedName claim.
            //This will have been generated from the client
            //certificate.
            X500DistinguishedName tmpDistinguishedName;
            if (TryGetClaimValue<X500DistinguishedName>(claimSet,
               ClaimTypes.X500DistinguishedName,
               out tmpDistinguishedName))
            {
                    certificateSubjectName = tmpDistinguishedName.Name;
            }
        }
    }
}
```

## <a name="running-the-sample"></a>Executando o exemplo
 Quando você executa o exemplo, o cliente solicita primeiro que você forneça o nome de usuário e a senha para o token de nome de usuário. Certifique-se de fornecer valores corretos para sua conta do sistema, pois o WCF no serviço mapeia os valores fornecidos no token de nome de usuário para a identidade fornecida pelo sistema. Depois disso, o cliente exibe a resposta do serviço. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação
 O arquivo em lotes setup. bat incluído neste exemplo permite que você configure o servidor com certificados relevantes para executar o aplicativo Serviços de Informações da Internet (IIS) hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada.

### <a name="creating-the-client-certificate"></a>Criando o certificado do cliente
 As linhas a seguir do arquivo em lotes setup. bat criam o certificado do cliente a ser usado. A `%CLIENT_NAME%` variável especifica o assunto do certificado do cliente. Este exemplo usa "client.com" como o nome da entidade.

 O certificado é armazenado no meu repositório (pessoal) no `CurrentUser` local do repositório.

```console
echo ************
echo making client cert
echo ************
makecert.exe -sr CurrentUser -ss MY -a sha1 -n CN=%CLIENT_NAME% -sky exchange -pe
```

### <a name="installing-the-client-certificate-into-the-servers-trusted-store"></a>Instalando o certificado do cliente no repositório confiável do servidor
 A linha a seguir no arquivo em lotes setup. bat copia o certificado do cliente no repositório de pessoas confiáveis do servidor. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema do servidor. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

```console
echo ************
echo copying client cert to server's CurrentUserstore
echo ************
certmgr.exe -add -r CurrentUser -s My -c -n %CLIENT_NAME% -r LocalMachine -s TrustedPeople
```

### <a name="creating-the-server-certificate"></a>Criando o certificado do servidor
 As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A `%SERVER_NAME%` variável especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O padrão neste arquivo em lotes é localhost.

 O certificado é armazenado no meu repositório (pessoal) no local de armazenamento de LocalMachine. O certificado é armazenado no armazenamento LocalMachine para os serviços hospedados pelo IIS. Para serviços hospedados internamente, você deve modificar o arquivo em lotes para armazenar o certificado do servidor no local do repositório CurrentUser, substituindo a cadeia de caracteres LocalMachine por CurrentUser.

```console
echo ************
echo Server cert setup starting
echo %SERVER_NAME%
echo ************
echo making server cert
echo ************
makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
```

### <a name="installing-server-certificate-into-clients-trusted-certificate-store"></a>Instalando o certificado do servidor no repositório de certificados confiáveis do cliente
 As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

```console
echo ************
echo copying server cert to client's TrustedPeople store
echo ************certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
```

### <a name="enabling-access-to-the-certificates-private-key"></a>Habilitando o acesso à chave privada do certificado
 Para habilitar o acesso à chave privada do certificado do serviço hospedado pelo IIS, a conta de usuário sob a qual o processo hospedado pelo IIS está em execução deve receber as permissões apropriadas para a chave privada. Isso é feito pelas últimas etapas do script setup. bat.

```console
echo ************
echo setting privileges on server certificates
echo ************
for /F "delims=" %%i in ('"%ProgramFiles%\ServiceModelSampleTools\FindPrivateKey.exe" My LocalMachine -n CN^=%SERVER_NAME% -a') do set PRIVATE_KEY_FILE=%%i
set WP_ACCOUNT=NT AUTHORITY\NETWORK SERVICE
(ver | findstr /C:"5.1") && set WP_ACCOUNT=%COMPUTERNAME%\ASPNET
echo Y|cacls.exe "%PRIVATE_KEY_FILE%" /E /G "%WP_ACCOUNT%":R
iisreset
```

##### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo

1. Certifique-se de ter executado o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

3. Para executar o exemplo em uma configuração de computador único ou cruzado, use as instruções a seguir.

##### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 executado com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat. Certifique-se de remover os certificados executando Cleanup. bat quando terminar com o exemplo. Outros exemplos de segurança usam os mesmos certificados.  
  
2. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
3. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
##### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço. Crie um aplicativo virtual chamado servicemodelsamples para esse diretório usando a ferramenta de gerenciamento do Serviços de Informações da Internet (IIS).  
  
2. Copie os arquivos de programa do serviço do \inetpub\wwwroot\servicemodelsamples para o diretório virtual no computador do serviço. Certifique-se de copiar os arquivos no subdiretório \bin. Copie também os arquivos Setup. bat, Cleanup. bat e ImportClientCert. bat para o computador de serviço.  
  
3. Crie um diretório no computador cliente para os binários do cliente.  
  
4. Copie os arquivos de programa do cliente para o diretório do cliente no computador cliente. Copie também os arquivos Setup. bat, Cleanup. bat e ImportServiceCert. bat para o cliente.  
  
5. No servidor, execute `setup.bat service` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `service` argumento cria um certificado de serviço com o nome de domínio totalmente qualificado do computador e exporta o certificado de serviço para um arquivo chamado Service. cer.  
  
6. Edite Web. config para refletir o novo nome de certificado (no `findValue` atributo no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) ), que é o mesmo que o nome de domínio totalmente qualificado do computador.  
  
7. Copie o arquivo Service. cer do diretório de serviço para o diretório do cliente no computador cliente.  
  
8. No cliente, execute `setup.bat client` em um prompt de comando do desenvolvedor para Visual Studio aberto com privilégios de administrador. `setup.bat`A execução com o `client` argumento cria um certificado de cliente chamado Client.com e exporta o certificado do cliente para um arquivo chamado Client. cer.  
  
9. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço. Faça isso substituindo localhost pelo nome de domínio totalmente qualificado do servidor.  
  
10. Copie o arquivo client. cer do diretório cliente para o diretório de serviço no servidor.  
  
11. No cliente, execute ImportServiceCert. bat. Isso importa o certificado de serviço do arquivo Service. cer para o repositório CurrentUser-TrustedPeople.  
  
12. No servidor, execute ImportClientCert. bat, isso importa o certificado do cliente do arquivo client. cer para o repositório LocalMachine-TrustedPeople.  
  
13. No computador cliente, inicie o Client. exe em uma janela de prompt de comando. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
##### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
- Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
> [!NOTE]
> Esse script não remove certificados de serviço em um cliente ao executar esse exemplo em computadores. Se você tiver executado os exemplos do WCF que usam certificados entre computadores, certifique-se de limpar os certificados de serviço que foram instalados no repositório CurrentUser-TrustedPeople. Para fazer isso, use o seguinte comando: `certmgr -del -r CurrentUser -s TrustedPeople -c -n <Fully Qualified Server Machine Name>` por exemplo: `certmgr -del -r CurrentUser -s TrustedPeople -c -n server1.contoso.com` .
