---
title: Validador de senha e nome de usuário
ms.date: 03/30/2017
ms.assetid: 42f03841-286b-42d8-ba58-18c75422bc8e
ms.openlocfilehash: 5fbca30ef2dff0aebc13bda12c06adfd1989ea20
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596507"
---
# <a name="user-name-password-validator"></a>Validador de senha e nome de usuário
Este exemplo demonstra como implementar um validador UserNamePassword personalizado. Isso é útil em casos em que nenhum dos modos de validação internos do UserNamePassword é apropriado para os requisitos do aplicativo; por exemplo, quando os pares de nome de usuário/senha são armazenados em algum repositório externo, como um banco de dados. Este exemplo mostra um serviço que tem um validador personalizado que verifica dois pares de nome de usuário/senha específicos. O cliente usa um par de nome de usuário/senha para se autenticar no serviço.

> [!IMPORTANT]
> Os exemplos podem mais ser instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Security\UserNamePasswordValidator`  
  
> [!NOTE]
> Como qualquer pessoa pode construir uma credencial de nome de usuário que usa os pares de nome de usuário/senha que o validador personalizado aceita, o serviço é menos seguro do que o comportamento padrão fornecido pelo validador UserNamePassword padrão. O validador UserNamePassword padrão tenta mapear o par de nome de usuário/senha fornecido para uma conta do Windows e falha na autenticação se esse mapeamento falhar. O validador UserNamePassword personalizado neste exemplo não deve ser usado no código de produção; ele é apenas para fins ilustrativos.

 Em resumo, este exemplo demonstra como:

- O cliente pode ser autenticado usando um token de nome de usuário.

- O servidor valida as credenciais do cliente em relação a um UserNamePasswordValidator personalizado e como propagar falhas personalizadas da lógica de validação de nome de usuário e senha para o cliente.

- O servidor é autenticado usando o certificado X. 509 do servidor.

 O serviço expõe um único ponto de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração, app. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsHttpBinding` que usa o WS-Security e a autenticação de nome de usuário. O comportamento do serviço especifica o `Custom` modo para validar pares de nome de usuário/senha do cliente junto com o tipo da classe do validador. O comportamento também especifica o certificado do servidor usando o `serviceCertificate` elemento. O certificado do servidor deve conter o mesmo valor para o as `SubjectName` `findValue` no [\<serviceCertificate>](../../configure-apps/file-schema/wcf/servicecertificate-of-servicecredentials.md) .

```xml
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <!-- use host/baseAddresses to configure base address provided by host -->
      <host>
        <baseAddresses>
          <add baseAddress ="http://localhost:8001/servicemodelsamples/service" />
        </baseAddresses>
      </host>
      <!-- use base address specified above, provide one endpoint -->
      <endpoint address="username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>

  <bindings>
    <wsHttpBinding>
      <!-- username binding -->
      <binding name="Binding">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>

  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceDebug includeExceptionDetailInFaults ="true"/>
        <serviceCredentials>
          <!--
          The serviceCredentials behavior allows one to
          specify a custom validator for username/password
          combinations.
          -->
          <userNameAuthentication userNamePasswordValidationMode="Custom"
                                  customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+MyCustomUserNameValidator, service" />
          <!--
          The serviceCredentials behavior allows one to define a service certificate. A service certificate is used by a client to authenticate the service and provide message protection. You must specify a server certificate when passing username/passwords to encrypt the information as it is sent on the wire. Otherwise the username and password information would be sent as clear text. This configuration references the "localhost" certificate installed during the setup instructions.
          -->
          <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>

</system.serviceModel>
```

 A configuração de ponto de extremidade do cliente consiste em um nome de configuração, um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o modo e a mensagem apropriados `clientCredentialType` .

```xml
<system.serviceModel>

    <client>
      <!-- Username based endpoint -->
      <endpoint name="Username"
address="http://localhost:8001/servicemodelsamples/service/username"
                binding="wsHttpBinding"
                bindingConfiguration="Binding"
                behaviorConfiguration="ClientCertificateBehavior"
                contract="Microsoft.ServiceModel.Samples.ICalculator">
      </endpoint>
    </client>

    <bindings>
      <wsHttpBinding>
        <!-- Username binding -->
        <binding name="Binding">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>
    <behaviors>
      <endpointBehaviors>
        <behavior name="ClientCertificateBehavior">
          <clientCredentials>
            <serviceCertificate>
              <!--
            Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
            is in the user's Trusted People store, then it will be trusted without performing a
            validation of the certificate's issuer chain. This setting is used here for convenience so that the
            sample can be run without having to have certificates issued by a certification authority (CA).
            This setting is less secure than the default, ChainTrust. The security implications of this
            setting should be carefully considered before using PeerOrChainTrust in production code.
            -->
              <authentication certificateValidationMode="PeerOrChainTrust" />
            </serviceCertificate>
          </clientCredentials>
        </behavior>
      </endpointBehaviors>
    </behaviors>

  </system.serviceModel>
```

 A implementação do cliente solicita que o usuário insira um nome de usuário e uma senha.

```csharp
// Get the username and password
Console.WriteLine("Username authentication required.");
Console.WriteLine("Provide a username.");
Console.WriteLine("   Enter username: (test1)");
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
            password = password.Substring(0, password.Length - 1);
        }
        info = Console.ReadKey(true);
    }
}
for (int i = 0; i < password.Length; i++)
{
    Console.Write("*");
}
Console.WriteLine();
// Create a proxy with Certificate endpoint configuration
CalculatorProxy proxy = new CalculatorProxy("Username")
try
{
  proxy.ClientCredentials.Username.Username = username;
  proxy.ClientCredentials.Username.Password = password;
    // Call the Add service operation.
    double value1 = 100.00D;
    double value2 = 15.99D;
    double result = proxy.Add(value1, value2);
    Console.WriteLine("Add({0},{1}) = {2}", value1, value2, result);
  }
  catch (Exception e)
  {
      Console.WriteLine("Call failed:");
      while (e != null)
      {
          Console.WriteLine("\t{0}", e.Message);
          e = e.InnerException;
      }
      proxy.Abort();
  }
}
```

 Este exemplo usa um UserNamePasswordValidator personalizado para validar os pares de nome de usuário/senha. O exemplo implementa `CustomUserNamePasswordValidator` , derivado de <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> . Consulte a documentação para <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> obter mais informações. Este exemplo de validador personalizado específico implementa o `Validate` método para aceitar dois pares de nome de usuário/senha específicos, conforme mostrado no código a seguir.

```csharp
public class CustomUserNameValidator : UserNamePasswordValidator
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
   throw new FaultException("Unknown Username or Incorrect Password");
   }
  }
 }
```

 Depois que o validador for implementado no código de serviço, o host de serviço deverá ser informado sobre a instância do validador a ser usada. Isso é feito usando o código a seguir.

```csharp
serviceHost.Credentials.UserNameAuthentication.UserNamePasswordValidationMode = UserNamePasswordValidationMode.Custom;
serviceHost.Credentials. UserNameAuthentication.CustomUserNamePasswordValidator = new CustomUserNamePasswordValidator();
```

 Ou você pode fazer a mesma coisa na configuração da seguinte maneira.

```xml
<behaviors>
 <serviceBehaviors>
  <behavior name="CalculatorServiceBehavior">
  ...
   <serviceCredentials>
    <!--
    The serviceCredentials behavior allows one to specify authentication constraints on username / password combinations.
    -->
    <userNameAuthentication userNamePasswordValidationMode="Custom" customUserNamePasswordValidatorType="Microsoft.ServiceModel.Samples.CalculatorService+CustomUserNameValidator, service" />
   ...
   </serviceCredentials>
  </behavior>
 </serviceBehaviors>
</behaviors>
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. O cliente deve chamar com êxito todos os métodos. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação
 O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com certificados relevantes para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar entre computadores ou para funcionar em um caso não hospedado automaticamente.

 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada.

- Criando o certificado do servidor:

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A variável% SERVER_NAME% especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O valor padrão é localhost.

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss MY -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente:

     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r CurrentUser -s TrustedPeople
    ```

#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

2. Para executar o exemplo em uma configuração de computador único ou cruzado, use as instruções a seguir.

#### <a name="to-run-the-sample-on-the-same-machine"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.  
  
2. Inicie o Service. exe em service\bin.  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-run-the-sample-across-machines"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço para os binários de serviço.  
  
2. Copie os arquivos de programa de serviço no diretório de serviço no computador de serviço. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
3. Você precisa de um certificado de servidor com o nome da entidade que contém o nome de domínio totalmente qualificado do computador. O arquivo de configuração do servidor deve ser atualizado para refletir esse novo nome de certificado.  
  
4. Copie o certificado do servidor no repositório CurrentUser-TrustedPeople do cliente. Você precisará fazer isso somente se o certificado do servidor não for emitido por um emissor confiável.  
  
5. No arquivo app. config no computador do serviço, altere o valor do endereço base para especificar um nome de máquina totalmente qualificado em vez de localhost.  
  
6. Na máquina de serviço, inicie o Service. exe em uma janela de prompt de comando.  
  
7. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No computador cliente, inicie o Client. exe em uma janela de prompt de comando.  
  
10. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo. Isso remove o certificado do servidor do repositório de certificados.  
