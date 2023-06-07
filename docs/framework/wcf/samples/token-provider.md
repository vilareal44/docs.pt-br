---
title: Fornecedor de token
ms.date: 03/30/2017
ms.assetid: 947986cf-9946-4987-84e5-a14678d96edb
ms.openlocfilehash: acdb820206dee83ff44152a5562642a5c685d648
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84584071"
---
# <a name="token-provider"></a>Fornecedor de token
Este exemplo demonstra como implementar um provedor de token personalizado. Um provedor de token no Windows Communication Foundation (WCF) é usado para fornecer credenciais para a infraestrutura de segurança. O provedor de token em geral examina o destino e emite as credenciais apropriadas para que a infraestrutura de segurança possa proteger a mensagem. O WCF é fornecido com o provedor de token padrão do Credential Manager. O WCF também é fornecido com um provedor de token do CardSpace. Os provedores de token personalizados são úteis nos seguintes casos:

- Se você tiver um repositório de credenciais para o qual esses provedores de token não podem operar.

- Se você quiser fornecer seu próprio mecanismo personalizado para transformar as credenciais do ponto em que o usuário fornece os detalhes quando a estrutura do cliente WCF usa as credenciais.

- Se você estiver criando um token personalizado.

 Este exemplo mostra como criar um provedor de token personalizado que transforma a entrada do usuário em um formato diferente.

 Para resumir, este exemplo demonstra o seguinte:

- Como um cliente pode se autenticar usando um par de nome de usuário/senha.

- Como um cliente pode ser configurado com um provedor de token personalizado.

- Como o servidor pode validar as credenciais do cliente usando uma senha com um personalizado <xref:System.IdentityModel.Selectors.UserNamePasswordValidator> que valida a correspondência entre o nome de usuário e a senha.

- Como o servidor é autenticado pelo cliente usando o certificado X. 509 do servidor.

 Este exemplo também mostra como a identidade do chamador pode ser acessada após o processo de autenticação de token personalizado.

 O serviço expõe um único ponto de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração app. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsHttpBinding` , que usa a segurança de mensagem por padrão. Este exemplo define o padrão `wsHttpBinding` para usar a autenticação de nome de usuário do cliente. O serviço também configura o certificado de serviço usando o comportamento serviceCredentials. O comportamento serviceCredentials permite que você configure um certificado de serviço. Um certificado de serviço é usado por um cliente para autenticar o serviço e fornecer proteção de mensagem. A configuração a seguir faz referência ao certificado localhost instalado durante a configuração de exemplo, conforme descrito nas instruções de instalação a seguir.

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service"/>
          </baseAddresses>
        </host>
        <!-- use base address provided by host -->
        <endpoint address=""
                  binding="wsHttpBinding"
                  bindingConfiguration="Binding1"
                  contract="Microsoft.ServiceModel.Samples.ICalculator" />
      </service>
    </services>

    <bindings>
      <wsHttpBinding>
        <binding name="Binding1">
          <security mode="Message">
            <message clientCredentialType="UserName" />
          </security>
        </binding>
      </wsHttpBinding>
    </bindings>

    <behaviors>
      <serviceBehaviors>
        <behavior name="CalculatorServiceBehavior">
          <serviceDebug includeExceptionDetailInFaults="False" />
          <!--
        The serviceCredentials behavior allows one to define a service certificate.
        A service certificate is used by a client to authenticate the service and provide message protection.
        This configuration references the "localhost" certificate installed during the setup instructions.
        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>
  </system.serviceModel>
```

 A configuração de ponto de extremidade do cliente consiste em um nome de configuração, um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com a `Mode` mensagem e apropriada `clientCredentialType` .

```xml
<system.serviceModel>
  <client>
    <endpoint name=""
              address="http://localhost:8000/servicemodelsamples/service"
              binding="wsHttpBinding"
              bindingConfiguration="Binding1"
              contract="Microsoft.ServiceModel.Samples.ICalculator">
    </endpoint>
  </client>

  <bindings>
    <wsHttpBinding>
      <binding name="Binding1">
        <security mode="Message">
          <message clientCredentialType="UserName" />
        </security>
      </binding>
    </wsHttpBinding>
  </bindings>
</system.serviceModel>
```

 As etapas a seguir mostram como desenvolver um provedor de token personalizado e integrá-lo com a estrutura de segurança do WCF:

1. Escreva um provedor de token personalizado.

     O exemplo implementa um provedor de token personalizado que obtém o nome de usuário e a senha. A senha deve corresponder a este nome de usuário. Esse provedor de token personalizado é apenas para fins de demonstração e não é recomendado para implantação do mundo real.

     Para executar essa tarefa, o provedor de token personalizado deriva a <xref:System.IdentityModel.Selectors.SecurityTokenProvider> classe e substitui o <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%28System.TimeSpan%29> método. Esse método cria e retorna um novo `UserNameSecurityToken` .

    ```csharp
    protected override SecurityToken GetTokenCore(TimeSpan timeout)
    {
        // obtain username and password from the user using console window
        string username = GetUserName();
        string password = GetPassword();
        Console.WriteLine("username: {0}", username);

        // return new UserNameSecurityToken containing information obtained from user
        return new UserNameSecurityToken(username, password);
    }
    ```

2. Gravar Gerenciador de token de segurança personalizado.

     O <xref:System.IdentityModel.Selectors.SecurityTokenManager> é usado para criar <xref:System.IdentityModel.Selectors.SecurityTokenProvider> para o específico <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> que é passado para ele no `CreateSecurityTokenProvider` método. O Gerenciador de token de segurança também é usado para criar autenticadores de token e um serializador de token, mas eles não são cobertos por esse exemplo. Neste exemplo, o Gerenciador de token de segurança personalizado herda da <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> classe e substitui o `CreateSecurityTokenProvider` método para retornar o provedor de token de nome de usuário personalizado quando os requisitos de token passados indicam que o provedor de nome de usuário é solicitado.

    ```csharp
    public class MyUserNameSecurityTokenManager : ClientCredentialsSecurityTokenManager
    {
        MyUserNameClientCredentials myUserNameClientCredentials;

        public MyUserNameSecurityTokenManager(MyUserNameClientCredentials myUserNameClientCredentials)
            : base(myUserNameClientCredentials)
        {
            this.myUserNameClientCredentials = myUserNameClientCredentials;
        }

        public override SecurityTokenProvider CreateSecurityTokenProvider(SecurityTokenRequirement tokenRequirement)
        {
            // if token requirement matches username token return custom username token provider
            // otherwise use base implementation
            if (tokenRequirement.TokenType == SecurityTokenTypes.UserName)
            {
                return new MyUserNameTokenProvider();
            }
            else
            {
                return base.CreateSecurityTokenProvider(tokenRequirement);
            }
        }
    }
    ```

3. Grave uma credencial de cliente personalizada.

     A classe de credenciais de cliente é usada para representar as credenciais que são configuradas para o proxy do cliente e cria o Gerenciador de token de segurança que é usado para obter autenticadores de token, provedores de token e um serializador de token.

    ```csharp
    public class MyUserNameClientCredentials : ClientCredentials
    {
        public MyUserNameClientCredentials()
            : base()
        {
        }

        protected override ClientCredentials CloneCore()
        {
            return new MyUserNameClientCredentials();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            // return custom security token manager
            return new MyUserNameSecurityTokenManager(this);
        }
    }
    ```

4. Configure o cliente para usar a credencial de cliente personalizada.

     Para que o cliente use a credencial de cliente personalizada, o exemplo exclui a classe de credencial do cliente padrão e fornece a nova classe de credencial do cliente.

    ```csharp
    static void Main()
    {
        // ...
           // Create a client with given client endpoint configuration
          CalculatorClient client = new CalculatorClient();

          // set new credentials
           client.ChannelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));
         client.ChannelFactory.Endpoint.Behaviors.Add(new MyUserNameClientCredentials());
       // ...
    }
    ```

 No serviço, para exibir as informações do chamador, use o <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> conforme mostrado no exemplo de código a seguir. O <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> contém informações de declarações sobre o chamador atual.

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
        ServiceSecurityContext.Current.PrimaryIdentity.Name);
}
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação
 O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com o certificado relevante para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada:

- Criando o certificado do servidor.

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A `%SERVER_NAME%` variável especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O valor padrão nesse arquivo em lotes é localhost.

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

> [!NOTE]
> O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando SDK do Windows. Ele requer que a variável de ambiente MSSDK aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente em um prompt de comando SDK do Windows.

#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

#### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 aberto com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.  
  
2. Inicie o Service. exe em service\bin.  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Na solicitação de nome de usuário, digite um nome de utilizador.  
  
5. No prompt de senha, use a mesma cadeia de caracteres que foi digitada para o prompt de nome de usuário.  
  
6. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço para os binários de serviço.  
  
2. Copie os arquivos de programa do serviço para o diretório de serviço no computador do serviço. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
3. Você deve ter um certificado de servidor com o nome da entidade que contém o nome de domínio totalmente qualificado do computador. O arquivo Service. exe. config deve ser atualizado para refletir esse novo nome de certificado. Você pode criar um certificado de servidor modificando o arquivo em lotes setup. bat. Observe que o arquivo setup. bat deve ser executado de um Prompt de Comando do Desenvolvedor para o Visual Studio aberto com privilégios de administrador. Você deve definir a `%SERVER_NAME%` variável para o nome de host totalmente qualificado do computador que é usado para hospedar o serviço.  
  
4. Copie o certificado do servidor no repositório CurrentUser-TrustedPeople do cliente. Você não precisa fazer isso quando o certificado do servidor é emitido por um emissor confiável do cliente.  
  
5. No arquivo Service. exe. config no computador do serviço, altere o valor do endereço base para especificar um nome de computador totalmente qualificado em vez de localhost.  
  
6. No computador do serviço, execute o Service. exe em um prompt de comando.  
  
7. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No computador cliente, inicie `Client.exe` a partir de uma janela de prompt de comando.  
  
10. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
