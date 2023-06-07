---
title: Autenticador de token
ms.date: 03/30/2017
ms.assetid: 84382f2c-f6b1-4c32-82fa-aebc8f6064db
ms.openlocfilehash: f4b49edd3b5a2cecd203feed713c7694450f7497
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84596546"
---
# <a name="token-authenticator"></a>Autenticador de token
Este exemplo demonstra como implementar um autenticador de token personalizado. Um autenticador de token no Windows Communication Foundation (WCF) é usado para validar o token usado com a mensagem, verificar se ele é consistente e autenticar a identidade associada ao token.

 Os autenticadores de token personalizados são úteis em vários casos, como:

- Quando você quiser substituir o mecanismo de autenticação padrão associado a um token.

- Quando você estiver criando um token personalizado.

 Este exemplo demonstra o seguinte:

- Como um cliente pode se autenticar usando um par de nome de usuário/senha.

- Como o servidor pode validar as credenciais do cliente usando um autenticador de token personalizado.

- Como o código do serviço WCF se vincula com o autenticador de token personalizado.

- Como o servidor pode ser autenticado usando o certificado X. 509 do servidor.

 Este exemplo também mostra como a identidade do chamador pode ser acessada do WCF após o processo de autenticação de token personalizado.

 O serviço expõe um único ponto de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração app. config. O ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsHttpBinding` , com o modo de segurança definido como Message-o modo padrão do `wsHttpBinding` . Este exemplo define o padrão `wsHttpBinding` para usar a autenticação de nome de usuário do cliente. O serviço também configura o certificado de serviço usando o `serviceCredentials` comportamento. O `securityCredentials` comportamento permite que você especifique um certificado de serviço. Um certificado de serviço é usado por um cliente para autenticar o serviço e fornecer proteção de mensagem. A configuração a seguir faz referência ao certificado localhost instalado durante a configuração de exemplo, conforme descrito nas instruções de instalação a seguir.

```xml
<system.serviceModel>
    <services>
      <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
        <host>
          <baseAddresses>
            <!-- configure base address provided by host -->
            <add baseAddress ="http://localhost:8000/servicemodelsamples/service" />
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
....        -->
          <serviceCredentials>
            <serviceCertificate findValue="localhost" storeLocation="LocalMachine" storeName="My" x509FindType="FindBySubjectName" />
          </serviceCredentials>
        </behavior>
      </serviceBehaviors>
    </behaviors>

  </system.serviceModel>
```

 A configuração de ponto de extremidade do cliente consiste em um nome de configuração, um endereço absoluto para o ponto de extremidade de serviço, a associação e o contrato. A associação de cliente é configurada com o `Mode` e o `clientCredentialType` .

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

 A implementação do cliente define o nome de usuário e a senha a serem usados.

```csharp
static void Main()
{
     ...
     client.ClientCredentials.UserNamePassword.UserName = username;
     client.ClientCredentials.UserNamePassword.Password = password;
     ...
}
```

## <a name="custom-token-authenticator"></a>Autenticador de token personalizado
 Use as seguintes etapas para criar um autenticador de token personalizado:

1. Escreva um autenticador de token personalizado.

     O exemplo implementa um autenticador de token personalizado que valida se o nome de usuário tem um formato de email válido. Ele deriva o <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator> . O método mais importante nessa classe é <xref:System.IdentityModel.Selectors.UserNameSecurityTokenAuthenticator.ValidateUserNamePasswordCore%28System.String%2CSystem.String%29> . Nesse método, o autenticador valida o formato do nome de usuário e também que o nome do host não é de um domínio não autorizado. Se ambas as condições forem atendidas, ele retornará uma coleção somente leitura de <xref:System.IdentityModel.Policy.IAuthorizationPolicy> instâncias que é usada para fornecer declarações que representam as informações armazenadas dentro do token de nome de usuário.

    ```csharp
    protected override ReadOnlyCollection<IAuthorizationPolicy> ValidateUserNamePasswordCore(string userName, string password)
    {
        if (!ValidateUserNameFormat(userName))
            throw new SecurityTokenValidationException("Incorrect UserName format");

        ClaimSet claimSet = new DefaultClaimSet(ClaimSet.System, new Claim(ClaimTypes.Name, userName, Rights.PossessProperty));
        List<IIdentity> identities = new List<IIdentity>(1);
        identities.Add(new GenericIdentity(userName));
        List<IAuthorizationPolicy> policies = new List<IAuthorizationPolicy>(1);
        policies.Add(new UnconditionalPolicy(ClaimSet.System, claimSet, DateTime.MaxValue.ToUniversalTime(), identities));
        return policies.AsReadOnly();
    }
    ```

2. Forneça uma política de autorização retornada pelo autenticador de token personalizado.

     Este exemplo fornece sua própria implementação de <xref:System.IdentityModel.Policy.IAuthorizationPolicy> chamada `UnconditionalPolicy` que retorna o conjunto de declarações e identidades que foram passadas a ele em seu construtor.

    ```csharp
    class UnconditionalPolicy : IAuthorizationPolicy
    {
        String id = Guid.NewGuid().ToString();
        ClaimSet issuer;
        ClaimSet issuance;
        DateTime expirationTime;
        IList<IIdentity> identities;

        public UnconditionalPolicy(ClaimSet issuer, ClaimSet issuance, DateTime expirationTime, IList<IIdentity> identities)
        {
            if (issuer == null)
                throw new ArgumentNullException("issuer");
            if (issuance == null)
                throw new ArgumentNullException("issuance");

            this.issuer = issuer;
            this.issuance = issuance;
            this.identities = identities;
            this.expirationTime = expirationTime;
        }

        public string Id
        {
            get { return this.id; }
        }

        public ClaimSet Issuer
        {
            get { return this.issuer; }
        }

        public DateTime ExpirationTime
        {
            get { return this.expirationTime; }
        }

        public bool Evaluate(EvaluationContext evaluationContext, ref object state)
        {
            evaluationContext.AddToTarget(this, this.issuance);

            if (this.identities != null)
            {
                object value;
                IList<IIdentity> contextIdentities;
                if (!evaluationContext.Properties.TryGetValue("Identities", out value))
                {
                    contextIdentities = new List<IIdentity>(this.identities.Count);
                    evaluationContext.Properties.Add("Identities", contextIdentities);
                }
                else
                {
                    contextIdentities = value as IList<IIdentity>;
                }
                foreach (IIdentity identity in this.identities)
                {
                    contextIdentities.Add(identity);
                }
            }

            evaluationContext.RecordExpirationTime(this.expirationTime);
            return true;
        }
    }
    ```

3. Escreva um Gerenciador de token de segurança personalizado.

     O <xref:System.IdentityModel.Selectors.SecurityTokenManager> é usado para criar um <xref:System.IdentityModel.Selectors.SecurityTokenAuthenticator> para <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> objetos específicos que são passados para ele no `CreateSecurityTokenAuthenticator` método. O Gerenciador de token de segurança também é usado para criar provedores de token e serializadores de token, mas eles não são cobertos por esse exemplo. Neste exemplo, o Gerenciador de token de segurança personalizado herda da <xref:System.ServiceModel.Security.ServiceCredentialsSecurityTokenManager> classe e substitui o `CreateSecurityTokenAuthenticator` método para retornar o autenticador de token de nome de usuário personalizado quando os requisitos de token passados indicam que o autenticador de nome de usuário é solicitado.

    ```csharp
    public class MySecurityTokenManager : ServiceCredentialsSecurityTokenManager
    {
        MyUserNameCredential myUserNameCredential;

        public MySecurityTokenManager(MyUserNameCredential myUserNameCredential)
            : base(myUserNameCredential)
        {
            this.myUserNameCredential = myUserNameCredential;
        }

        public override SecurityTokenAuthenticator CreateSecurityTokenAuthenticator(SecurityTokenRequirement tokenRequirement, out SecurityTokenResolver outOfBandTokenResolver)
        {
            if (tokenRequirement.TokenType ==  SecurityTokenTypes.UserName)
            {
                outOfBandTokenResolver = null;
                return new MyTokenAuthenticator();
            }
            else
            {
                return base.CreateSecurityTokenAuthenticator(tokenRequirement, out outOfBandTokenResolver);
            }
        }
    }
    ```

4. Grave uma credencial de serviço personalizada.

     A classe de credenciais de serviço é usada para representar as credenciais que são configuradas para o serviço e cria um Gerenciador de token de segurança que é usado para obter autenticadores de token, provedores de token e serializadores de token.

    ```csharp
    public class MyUserNameCredential : ServiceCredentials
    {

        public MyUserNameCredential()
            : base()
        {
        }

        protected override ServiceCredentials CloneCore()
        {
            return new MyUserNameCredential();
        }

        public override SecurityTokenManager CreateSecurityTokenManager()
        {
            return new MySecurityTokenManager(this);
        }

    }
    ```

5. Configure o serviço para usar a credencial de serviço personalizado.

     Para que o serviço use a credencial de serviço personalizado, excluímos a classe de credencial de serviço padrão após capturar o certificado de serviço que já está pré-configurado na credencial de serviço padrão e configurar a nova instância de credencial de serviço para usar os certificados de serviço pré-configurados e adicionar essa nova instância de credencial de serviço a comportamentos de serviço.

    ```csharp
    ServiceCredentials sc = serviceHost.Credentials;
    X509Certificate2 cert = sc.ServiceCertificate.Certificate;
    MyUserNameCredential serviceCredential = new MyUserNameCredential();
    serviceCredential.ServiceCertificate.Certificate = cert;
    serviceHost.Description.Behaviors.Remove((typeof(ServiceCredentials)));
    serviceHost.Description.Behaviors.Add(serviceCredential);
    ```

 Para exibir as informações do chamador, você pode usar o <xref:System.ServiceModel.ServiceSecurityContext.PrimaryIdentity%2A> conforme mostrado no código a seguir. O <xref:System.ServiceModel.ServiceSecurityContext.Current%2A> contém informações de declarações sobre o chamador atual.

```csharp
static void DisplayIdentityInformation()
{
    Console.WriteLine("\t\tSecurity context identity  :  {0}",
            ServiceSecurityContext.Current.PrimaryIdentity.Name);
     return;
}
```

 Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação
 O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com certificados relevantes para executar um aplicativo auto-hospedado que requer segurança baseada em certificado de servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada.

- Criando o certificado do servidor.

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A `%SERVER_NAME%` variável especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O padrão neste arquivo em lotes é localhost.

    ```console
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

    > [!NOTE]
    > O arquivo em lotes de instalação foi projetado para ser executado em um prompt de comando SDK do Windows. Ele requer que a variável de ambiente MSSDK aponte para o diretório em que o SDK está instalado. Essa variável de ambiente é definida automaticamente em um prompt de comando SDK do Windows.

#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

#### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 aberto com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.  
  
2. Inicie o Service. exe em service\bin.  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço para os binários de serviço.  
  
2. Copie os arquivos de programa do serviço para o diretório de serviço no computador do serviço. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
3. Você deve ter um certificado de servidor com o nome da entidade que contém o nome de domínio totalmente qualificado do computador. O arquivo app. config do serviço deve ser atualizado para refletir esse novo nome de certificado. Você pode criar um usando o setup. bat se definir a `%SERVER_NAME%` variável para o nome de host totalmente qualificado do computador no qual o serviço será executado. Observe que o arquivo setup. bat deve ser executado de um Prompt de Comando do Desenvolvedor para o Visual Studio aberto com privilégios de administrador.  
  
4. Copie o certificado do servidor no repositório CurrentUser-TrustedPeople do cliente. Você não precisa fazer isso, exceto quando o certificado do servidor é emitido por um emissor confiável do cliente.  
  
5. No arquivo app. config no computador do serviço, altere o valor do endereço base para especificar um nome de computador totalmente qualificado em vez de localhost.  
  
6. No computador do serviço, execute o Service. exe em um prompt de comando.  
  
7. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No computador cliente, inicie o Client. exe em um prompt de comando.  
  
10. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
