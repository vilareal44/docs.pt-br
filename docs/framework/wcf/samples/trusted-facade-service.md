---
title: Serviço de fachada confiável
ms.date: 03/30/2017
ms.assetid: c34d1a8f-e45e-440b-a201-d143abdbac38
ms.openlocfilehash: e7aa5e96fb8104c8140a8cebc6be45d2000821aa
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84591287"
---
# <a name="trusted-facade-service"></a>Serviço de fachada confiável
Este exemplo de cenário demonstra como fluir informações de identidade do chamador de um serviço para outro usando a infraestrutura de segurança do Windows Communication Foundation (WCF).  
  
 É um padrão de design comum expor a funcionalidade fornecida por um serviço para a rede pública usando um serviço de fachada. O serviço de fachada normalmente reside na rede de perímetro (também conhecida como DMZ, zona desmilitarizada e sub-rede filtrada) e se comunica com um serviço de back-end que implementa a lógica de negócios e tem acesso a dados internos. O canal de comunicação entre o serviço de fachada e o serviço de back-end passa por um firewall e geralmente é limitado apenas para uma única finalidade.  
  
 Este exemplo consiste nos seguintes componentes:  
  
- Cliente de calculadora  
  
- Serviço de fachada da calculadora  
  
- Serviço de back-end da calculadora  
  
 O serviço de fachada é responsável por validar a solicitação e autenticar o chamador. Após a autenticação e a validação bem-sucedidas, ele encaminha a solicitação para o serviço de back-end usando o canal de comunicação controlado da rede de perímetro para a rede interna. Como parte da solicitação encaminhada, o serviço de fachada inclui informações sobre a identidade do chamador para que o serviço de back-end possa usar essas informações em seu processamento. A identidade do chamador é transmitida usando um `Username` token de segurança dentro do cabeçalho da mensagem `Security` . O exemplo usa a infraestrutura de segurança do WCF para transmitir e extrair essas informações do `Security` cabeçalho.  
  
> [!IMPORTANT]
> O serviço de back-end confia no serviço de fachada para autenticar o chamador. Por isso, o serviço de back-end não autentica o chamador novamente; Ele usa as informações de identidade fornecidas pelo serviço de fachada na solicitação encaminhada. Devido a essa relação de confiança, o serviço de back-end deve autenticar o serviço de fachada para garantir que a mensagem encaminhada venha de uma fonte confiável – nesse caso, o serviço de fachada.  
  
## <a name="implementation"></a>Implementação  
 Há dois caminhos de comunicação neste exemplo. O primeiro é entre o cliente e o serviço de fachada, o segundo é entre o serviço de fachada e o serviço de back-end.  
  
### <a name="communication-path-between-client-and-faade-service"></a>Caminho de comunicação entre o cliente e o serviço de fachada  
 O cliente para o caminho de comunicação do serviço de fachada usa `wsHttpBinding` com um `UserName` tipo de credencial de cliente. Isso significa que o cliente usa o nome de usuário e a senha para autenticar o serviço de fachada e o serviço de fachada usa o certificado X. 509 para autenticar o cliente. A configuração de associação é semelhante ao exemplo a seguir.  
  
```xml  
<bindings>  
  <wsHttpBinding>  
    <binding name="Binding1">  
      <security mode="Message">  
        <message clientCredentialType="UserName"/>  
      </security>  
    </binding>  
  </wsHttpBinding>  
</bindings>  
```  
  
 O serviço de fachada autentica o chamador usando a `UserNamePasswordValidator` implementação personalizada. Para fins de demonstração, a autenticação só garante que o nome de usuário do chamador corresponda à senha apresentada. Na situação do mundo real, o usuário provavelmente é autenticado usando Active Directory ou provedor de associação ASP.NET personalizado. A implementação do validador reside no `FacadeService.cs` arquivo.  
  
```csharp  
public class MyUserNamePasswordValidator : UserNamePasswordValidator  
{  
    public override void Validate(string userName, string password)  
    {  
        // check that username matches password  
        if (null == userName || userName != password)  
        {  
            Console.WriteLine("Invalid username or password");  
            throw new SecurityTokenValidationException(  
                       "Invalid username or password");  
        }  
    }  
}  
```  
  
 O validador personalizado está configurado para ser usado dentro do `serviceCredentials` comportamento no arquivo de configuração do serviço de fachada. Esse comportamento também é usado para configurar o certificado X. 509 do serviço.  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="FacadeServiceBehavior">  
      <!--The serviceCredentials behavior allows you to define -->  
      <!--a service certificate. -->  
      <!--A service certificate is used by the service to  -->  
      <!--authenticate itself to its clients and to provide  -->  
      <!--message protection. -->  
      <!--This configuration references the "localhost"  -->  
      <!--certificate installed during the setup instructions. -->  
      <serviceCredentials>  
        <serviceCertificate
               findValue="localhost"
               storeLocation="LocalMachine"
               storeName="My"
               x509FindType="FindBySubjectName" />  
        <userNameAuthentication userNamePasswordValidationMode="Custom"  
            customUserNamePasswordValidatorType=  
           "Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator,  
            FacadeService"/>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
### <a name="communication-path-between-faade-service-and-backend-service"></a>Caminho de comunicação entre o serviço de fachada e de back-end  
 O serviço de fachada para o caminho de comunicação do serviço de back-end usa um `customBinding` que consiste em vários elementos de ligação. Essa associação realiza duas coisas. Ele autentica o serviço de fachada e o serviço de back-end para garantir que a comunicação seja segura e proveniente de uma fonte confiável. Além disso, ele também transmite a identidade do chamador inicial dentro do `Username` token de segurança. Nesse caso, somente o nome de usuário do chamador inicial é transmitido para o serviço de back-end, a senha não é incluída na mensagem. Isso ocorre porque o serviço de back-end confia no serviço de fachada para autenticar o chamador antes de encaminhar a solicitação para ele. Como o serviço de fachada se autentica no serviço de back-end, o serviço de back-end pode confiar nas informações contidas na solicitação encaminhada.  
  
 A seguir está a configuração de associação para este caminho de comunicação.  
  
```xml  
<bindings>  
  <customBinding>  
    <binding name="ClientBinding">  
      <security authenticationMode="UserNameOverTransport"/>  
      <windowsStreamSecurity/>  
      <tcpTransport/>  
    </binding>  
  </customBinding>  
</bindings>  
```  
  
 O [\<security>](../../configure-apps/file-schema/wcf/security-of-custombinding.md) elemento Binding cuida da transmissão e da extração do nome de usuário do chamador inicial. O [\<windowsStreamSecurity>](../../configure-apps/file-schema/wcf/windowsstreamsecurity.md) e [\<tcpTransport>](../../configure-apps/file-schema/wcf/tcptransport.md) cuidam da autenticação de serviços de fachada e de back-end e proteção de mensagens.  
  
 Para encaminhar a solicitação, a implementação do serviço de fachada deve fornecer o nome de usuário do chamador inicial para que a infraestrutura de segurança do WCF possa colocá-la na mensagem encaminhada. O nome de usuário do chamador inicial é fornecido na implementação do serviço de fachada, definindo-o na `ClientCredentials` Propriedade na instância de proxy do cliente que o serviço de fachada usa para se comunicar com o serviço de back-end.  
  
 O código a seguir mostra como o `GetCallerIdentity` método é implementado no serviço de fachada. Outros métodos usam o mesmo padrão.  
  
```csharp  
public string GetCallerIdentity()  
{  
    CalculatorClient client = new CalculatorClient();  
    client.ClientCredentials.UserName.UserName = ServiceSecurityContext.Current.PrimaryIdentity.Name;  
    string result = client.GetCallerIdentity();  
    client.Close();  
    return result;  
}  
```  
  
 Conforme mostrado no código anterior, a senha não é definida na `ClientCredentials` propriedade, somente o nome de usuário é definido. A infraestrutura de segurança do WCF cria um token de segurança de nome de usuário sem uma senha nesse caso, que é exatamente o que é necessário nesse cenário.  
  
 No serviço de back-end, as informações contidas no token de segurança de nome de usuário devem ser autenticadas. Por padrão, a segurança do WCF tenta mapear o usuário para uma conta do Windows usando a senha fornecida. Nesse caso, não há nenhuma senha fornecida e o serviço de back-end não é necessário para autenticar o nome de usuário porque a autenticação já foi executada pelo serviço de fachada. Para implementar essa funcionalidade no WCF, `UserNamePasswordValidator` é fornecido um personalizado que apenas impõe que um nome de usuário seja especificado no token e não execute nenhuma autenticação adicional.  
  
```csharp  
public class MyUserNamePasswordValidator : UserNamePasswordValidator  
{  
    public override void Validate(string userName, string password)  
    {  
        // Ignore the password because it is empty,
        // we trust the facade service to authenticate the client.  
        // Accept the username information here so that the
        // application gets access to it.  
        if (null == userName)  
        {  
            Console.WriteLine("Invalid username");  
            throw new
             SecurityTokenValidationException("Invalid username");  
        }  
    }  
}  
```  
  
 O validador personalizado está configurado para ser usado dentro do `serviceCredentials` comportamento no arquivo de configuração do serviço de fachada.  
  
```xml  
<behaviors>  
  <serviceBehaviors>  
    <behavior name="BackendServiceBehavior">  
      <serviceCredentials>  
        <userNameAuthentication userNamePasswordValidationMode="Custom"  
           customUserNamePasswordValidatorType=  
          "Microsoft.ServiceModel.Samples.MyUserNamePasswordValidator,
           BackendService"/>  
      </serviceCredentials>  
    </behavior>  
  </serviceBehaviors>  
</behaviors>  
```  
  
 Para extrair as informações de nome de usuário e informações sobre a conta de serviço de fachada confiável, a implementação do serviço de back-end usa a `ServiceSecurityContext` classe. O código a seguir mostra como o `GetCallerIdentity` método é implementado.  
  
```csharp  
public string GetCallerIdentity()  
{  
    // Facade service is authenticated using Windows authentication.  
    //Its identity is accessible.  
    // On ServiceSecurityContext.Current.WindowsIdentity.  
    string facadeServiceIdentityName =
          ServiceSecurityContext.Current.WindowsIdentity.Name;  
  
    // The client name is transmitted using Username authentication on
    //the message level without the password  
    // using a supporting encrypted UserNameToken.  
    // Claims extracted from this supporting token are available in
    // ServiceSecurityContext.Current.AuthorizationContext.ClaimSets
    // collection.  
    string clientName = null;  
    foreach (ClaimSet claimSet in
        ServiceSecurityContext.Current.AuthorizationContext.ClaimSets)  
    {  
        foreach (Claim claim in claimSet)  
        {  
            if (claim.ClaimType == ClaimTypes.Name &&
                                   claim.Right == Rights.Identity)  
            {  
                clientName = (string)claim.Resource;  
                break;  
            }  
        }  
    }  
    if (clientName == null)  
    {  
        // In case there was no UserNameToken attached to the request.  
        // In the real world implementation the service should reject
        // this request.  
        return "Anonymous caller via " + facadeServiceIdentityName;  
    }  
  
    return clientName + " via " + facadeServiceIdentityName;  
}  
```  
  
 As informações da conta do serviço de fachada são extraídas usando a `ServiceSecurityContext.Current.WindowsIdentity` propriedade. Para acessar as informações sobre o chamador inicial, o serviço de back-end usa a `ServiceSecurityContext.Current.AuthorizationContext.ClaimSets` propriedade. Ele procura uma `Identity` declaração com um tipo `Name` . Essa declaração é gerada automaticamente pela infraestrutura de segurança do WCF por meio das informações contidas no `Username` token de segurança.  
  
## <a name="running-the-sample"></a>Executando o exemplo  
 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente. Você pode pressionar ENTER nas janelas do console do serviço de back-end e de fachada para desligar os serviços.  
  
```console  
Username authentication required.  
Provide a valid machine or domain ac  
   Enter username:  
user  
   Enter password:  
****  
user via MyMachine\testaccount  
Add(100,15.99) = 115.99  
Subtract(145,76.54) = 68.46  
Multiply(9,81.25) = 731.25  
Divide(22,7) = 3.14285714285714  
  
Press <ENTER> to terminate client.  
```  
  
 O arquivo em lotes setup. bat incluído com o exemplo de cenário fachada confiável permite que você configure o servidor com um certificado relevante para executar o serviço de fachada que requer segurança baseada em certificado para se autenticar no cliente. Consulte o procedimento de instalação no final deste tópico para obter detalhes.  
  
 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes.  
  
- Criando o certificado do servidor.  
  
     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado.  
  
    ```console  
    echo ************  
    echo Server cert setup starting  
    echo %SERVER_NAME%  
    echo ************  
    echo making server cert  
    echo ************  
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe  
    ```  
  
     A `%SERVER_NAME%` variável especifica o nome do servidor-o valor padrão é localhost. O certificado é armazenado no armazenamento LocalMachine.  
  
- Instalar o certificado do serviço de fachada no repositório de certificados confiáveis do cliente.  
  
     A linha a seguir copia o certificado do serviço de fachada para o repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.  
  
    ```console  
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople  
    ```  
  
#### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).  
  
2. Para criar a edição C# ou Visual Basic .NET da solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).  
  
#### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador  
  
1. Verifique se o caminho inclui a pasta onde o MakeCert. exe está localizado.  
  
2. Execute setup. bat na pasta de instalação de exemplo. Isso instala todos os certificados necessários para executar o exemplo.  
  
3. Iniciar o BackendService. exe do diretório \BackendService\bin em uma janela de console separada  
  
4. Iniciar o FacadeService. exe do diretório \FacadeService\bin em uma janela de console separada  
  
5. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
6. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Scenario\TrustedFacade`  
