---
title: Fornecedor de token SAML
ms.date: 03/30/2017
ms.assetid: eb16e5e2-4c8d-4f61-a479-9c965fcec80c
ms.openlocfilehash: db1307b0f440f8bd55f1728b6645aec706dfe442
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84602408"
---
# <a name="saml-token-provider"></a>Fornecedor de token SAML
Este exemplo demonstra como implementar um provedor de token SAML de cliente personalizado. Um provedor de token no Windows Communication Foundation (WCF) é usado para fornecer credenciais para a infraestrutura de segurança. O provedor de token em geral examina o destino e emite as credenciais apropriadas para que a infraestrutura de segurança possa proteger a mensagem. O WCF é fornecido com o provedor de token padrão do Credential Manager. O WCF também é fornecido com um provedor de token do CardSpace. Os provedores de token personalizados são úteis nos seguintes casos:

- Se você tiver um repositório de credenciais para o qual esses provedores de token não podem operar.

- Se você quiser fornecer seu próprio mecanismo personalizado para transformar as credenciais do ponto em que o usuário fornece os detalhes quando a estrutura do cliente WCF usa as credenciais.

- Se você estiver criando um token personalizado.

 Este exemplo mostra como criar um provedor de token personalizado que permite que um token SAML obtido fora da estrutura do cliente WCF seja usado.

 Para resumir, este exemplo demonstra o seguinte:

- Como um cliente pode ser configurado com um provedor de token personalizado.

- Como um token SAML pode ser passado para as credenciais de cliente personalizadas.

- Como o token SAML é fornecido para a estrutura do cliente WCF.

- Como o servidor é autenticado pelo cliente usando o certificado X. 509 do servidor.

 O serviço expõe dois pontos de extremidade para se comunicar com o serviço, definido usando o arquivo de configuração app. config. Cada ponto de extremidade consiste em um endereço, uma associação e um contrato. A associação é configurada com um padrão `wsFederationHttpBinding` , que usa a segurança da mensagem. Um ponto de extremidade espera que o cliente autentique com um token SAML que usa uma chave de prova simétrica, enquanto o outro espera que o cliente se autentique com um token SAML que usa uma chave de prova assimétrica. O serviço também configura o certificado de serviço usando o `serviceCredentials` comportamento. O `serviceCredentials` comportamento permite que você configure um certificado de serviço. Um certificado de serviço é usado por um cliente para autenticar o serviço e fornecer proteção de mensagem. A configuração a seguir faz referência ao certificado "localhost" instalado durante a configuração de exemplo, conforme descrito nas instruções de instalação no final deste tópico. O `serviceCredentials` comportamento também permite que você configure certificados confiáveis para assinar tokens SAML. A configuração a seguir faz referência ao certificado ' Alice ' instalado durante o exemplo.

```xml
<system.serviceModel>
 <services>
  <service
          name="Microsoft.ServiceModel.Samples.CalculatorService"
          behaviorConfiguration="CalculatorServiceBehavior">
   <host>
    <baseAddresses>
     <!-- configure base address provided by host -->
     <add
  baseAddress="http://localhost:8000/servicemodelsamples/service/" />
    </baseAddresses>
   </host>
   <!-- use base address provided by host -->
   <!-- Endpoint that expect SAML tokens with Symmetric proof keys -->
   <endpoint address="calc/symm"
             binding="wsFederationHttpBinding"
             bindingConfiguration="Binding1"
             contract="Microsoft.ServiceModel.Samples.ICalculator" />
   <!-- Endpoint that expect SAML tokens with Asymmetric proof keys -->
   <endpoint address="calc/asymm"
             binding="wsFederationHttpBinding"
             bindingConfiguration="Binding2"
             contract="Microsoft.ServiceModel.Samples.ICalculator" />
  </service>
 </services>

 <bindings>
  <wsFederationHttpBinding>
   <!-- Binding that expect SAML tokens with Symmetric proof keys -->
   <binding name="Binding1">
    <security mode="Message">
     <message negotiateServiceCredential ="false"
              issuedKeyType="SymmetricKey"
              issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1"  />
    </security>
   </binding>
   <!-- Binding that expect SAML tokens with Asymmetric proof keys -->
   <binding name="Binding2">
    <security mode="Message">
     <message negotiateServiceCredential ="false"
              issuedKeyType="AsymmetricKey"
              issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1"  />
    </security>
   </binding>
  </wsFederationHttpBinding>
 </bindings>

 <behaviors>
  <serviceBehaviors>
   <behavior name="CalculatorServiceBehavior">
    <!--
    The serviceCredentials behavior allows one to define a service certificate.
    A service certificate is used by a client to authenticate the service and provide message protection.
    This configuration references the "localhost" certificate installed during the setup instructions.
    -->
    <serviceCredentials>
     <!-- Set allowUntrustedRsaIssuers to true to allow self-signed, asymmetric key based SAML tokens -->
     <issuedTokenAuthentication allowUntrustedRsaIssuers ="true" >
      <!-- Add Alice to the list of certs trusted to issue SAML tokens -->
      <knownCertificates>
       <add storeLocation="LocalMachine"
            storeName="TrustedPeople"
            x509FindType="FindBySubjectName"
            findValue="Alice"/>
      </knownCertificates>
     </issuedTokenAuthentication>
     <serviceCertificate storeLocation="LocalMachine"
                         storeName="My"
                         x509FindType="FindBySubjectName"
                         findValue="localhost"  />
    </serviceCredentials>
   </behavior>
  </serviceBehaviors>
 </behaviors>

</system.serviceModel>
```

 As etapas a seguir mostram como desenvolver um provedor de token SAML personalizado e integrá-lo ao WCF: estrutura de segurança:

1. Escreva um provedor de token SAML personalizado.

     O exemplo implementa um provedor de token SAML personalizado que retorna um token de segurança com base em uma Asserção SAML fornecida no momento da construção.

     Para executar essa tarefa, o provedor de token personalizado é derivado da <xref:System.IdentityModel.Selectors.SecurityTokenProvider> classe e substitui o <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%2A> método. Esse método cria e retorna um novo `SecurityToken` .

    ```csharp
    protected override SecurityToken GetTokenCore(TimeSpan timeout)
    {
     // Create a SamlSecurityToken from the provided assertion
     SamlSecurityToken samlToken = new SamlSecurityToken(assertion);

     // Create a SecurityTokenSerializer that will be used to
     // serialize the SamlSecurityToken
     WSSecurityTokenSerializer ser = new WSSecurityTokenSerializer();
     // Create a memory stream to write the serialized token into
     // Use an initial size of 64Kb
     MemoryStream s = new MemoryStream(UInt16.MaxValue);

     // Create an XmlWriter over the stream
     XmlWriter xw = XmlWriter.Create(s);

     // Write the SamlSecurityToken into the stream
     ser.WriteToken(xw, samlToken);

     // Seek back to the beginning of the stream
     s.Seek(0, SeekOrigin.Begin);

     // Load the serialized token into a DOM
     XmlDocument dom = new XmlDocument();
     dom.Load(s);

     // Create a KeyIdentifierClause for the SamlSecurityToken
     SamlAssertionKeyIdentifierClause samlKeyIdentifierClause = samlToken.CreateKeyIdentifierClause<SamlAssertionKeyIdentifierClause>();

    // Return a GenericXmlToken from the XML for the
    // SamlSecurityToken, the proof token, the valid from and valid
    // until times from the assertion and the key identifier clause
    // created above
    return new GenericXmlSecurityToken(dom.DocumentElement, proofToken, assertion.Conditions.NotBefore, assertion.Conditions.NotOnOrAfter, samlKeyIdentifierClause, samlKeyIdentifierClause, null);
    }
    ```

2. Gravar Gerenciador de token de segurança personalizado.

     A <xref:System.IdentityModel.Selectors.SecurityTokenManager> classe é usada para criar <xref:System.IdentityModel.Selectors.SecurityTokenProvider> para o específico <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> que é passado para ela no `CreateSecurityTokenProvider` método. Um Gerenciador de token de segurança também é usado para criar autenticadores de token e serializador de token, mas eles não são cobertos por esse exemplo. Neste exemplo, o Gerenciador de token de segurança personalizado herda da <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> classe e substitui o `CreateSecurityTokenProvider` método para retornar o provedor de token SAML personalizado quando os requisitos de token passados indicam que o token SAML é solicitado. Se a classe de credenciais do cliente (consulte a etapa 3) não tiver especificado uma asserção, o Gerenciador de token de segurança criará uma instância apropriada.

    ```csharp
    public class SamlSecurityTokenManager : ClientCredentialsSecurityTokenManager
    {
     SamlClientCredentials samlClientCredentials;

     public SamlSecurityTokenManager ( SamlClientCredentials samlClientCredentials)
      : base(samlClientCredentials)
     {
      // Store the creating client credentials
      this.samlClientCredentials = samlClientCredentials;
     }

     public override SecurityTokenProvider CreateSecurityTokenProvider ( SecurityTokenRequirement tokenRequirement )
     {
      // If token requirement matches SAML token return the
      // custom SAML token provider
      if (tokenRequirement.TokenType == SecurityTokenTypes.Saml ||
          tokenRequirement.TokenType == "http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1")
      {
       // Retrieve the SAML assertion and proof token from the
       // client credentials
       SamlAssertion assertion = this.samlClientCredentials.Assertion;
       SecurityToken prooftoken = this.samlClientCredentials.ProofToken;

       // If either the assertion of proof token is null...
       if (assertion == null || prooftoken == null)
       {
        // ...get the SecurityBindingElement and then the
        // specified algorithm suite
        SecurityBindingElement sbe = null;
        SecurityAlgorithmSuite sas = null;

        if ( tokenRequirement.TryGetProperty<SecurityBindingElement> ( "http://schemas.microsoft.com/ws/2006/05/servicemodel/securitytokenrequirement/SecurityBindingElement", out sbe))
        {
         sas = sbe.DefaultAlgorithmSuite;
        }

        // If the token requirement is for a SymmetricKey based token..
        if (tokenRequirement.KeyType == SecurityKeyType.SymmetricKey)
        {
         // Create a symmetric proof token
         prooftoken = SamlUtilities.CreateSymmetricProofToken ( tokenRequirement.KeySize );
         // and a corresponding assertion based on the claims specified in the client credentials
         assertion = SamlUtilities.CreateSymmetricKeyBasedAssertion ( this.samlClientCredentials.Claims, new X509SecurityToken ( samlClientCredentials.ClientCertificate.Certificate ), new X509SecurityToken ( samlClientCredentials.ServiceCertificate.DefaultCertificate ), (BinarySecretSecurityToken)prooftoken, sas);
        }
        // otherwise...
        else
        {
         // Create an asymmetric proof token
         prooftoken = SamlUtilities.CreateAsymmetricProofToken();
         // and a corresponding assertion based on the claims
         // specified in the client credentials
         assertion = SamlUtilities.CreateAsymmetricKeyBasedAssertion ( this.samlClientCredentials.Claims, prooftoken, sas );
        }
       }

       // Create a SamlSecurityTokenProvider based on the assertion and proof token
       return new SamlSecurityTokenProvider(assertion, prooftoken);
      }
      // otherwise use base implementation
      else
      {
       return base.CreateSecurityTokenProvider(tokenRequirement);
      }
    }
    ```

3. Grave uma credencial de cliente personalizada.

     A classe de credenciais de cliente é usada para representar as credenciais que são configuradas para o proxy do cliente e cria um Gerenciador de token de segurança que é usado para obter autenticadores de token, provedores de token e o serializador de token.

    ```csharp
    public class SamlClientCredentials : ClientCredentials
    {
     ClaimSet claims;
     SamlAssertion assertion;
     SecurityToken proofToken;

     public SamlClientCredentials() : base()
     {
      // Set SupportInteractive to false to suppress Cardspace UI
      base.SupportInteractive = false;
     }

     protected SamlClientCredentials(SamlClientCredentials other) : base ( other )
     {
      // Just do reference copy given sample nature
      this.assertion = other.assertion;
      this.claims = other.claims;
      this.proofToken = other.proofToken;
     }

     public SamlAssertion Assertion { get { return assertion; } set { assertion = value; } }

     public SecurityToken ProofToken { get { return proofToken; } set { proofToken = value; } }
     public ClaimSet Claims { get { return claims; } set { claims = value; } }

     protected override ClientCredentials CloneCore()
     {
      return new SamlClientCredentials(this);
     }

     public override SecurityTokenManager CreateSecurityTokenManager()
     {
      // return custom security token manager
      return new SamlSecurityTokenManager(this);
     }
    }
    ```

4. Configure o cliente para usar a credencial de cliente personalizada.

     O exemplo exclui a classe de credencial de cliente padrão e fornece a nova classe de credencial de cliente para que o cliente possa usar a credencial de cliente personalizada.

    ```csharp
    // Create new credentials class
    SamlClientCredentials samlCC = new SamlClientCredentials();

    // Set the client certificate. This is the cert that will be used to sign the SAML token in the symmetric proof key case
    samlCC.ClientCertificate.SetCertificate(StoreLocation.CurrentUser, StoreName.My, X509FindType.FindBySubjectName, "Alice");

    // Set the service certificate. This is the cert that will be used to encrypt the proof key in the symmetric proof key case
    samlCC.ServiceCertificate.SetDefaultCertificate(StoreLocation.CurrentUser, StoreName.TrustedPeople, X509FindType.FindBySubjectName, "localhost");

    // Create some claims to put in the SAML assertion
    IList<Claim> claims = new List<Claim>();
    claims.Add(Claim.CreateNameClaim(samlCC.ClientCertificate.Certificate.Subject));
    ClaimSet claimset = new DefaultClaimSet(claims);
    samlCC.Claims = claimset;

    // set new credentials
    client.ChannelFactory.Endpoint.Behaviors.Remove(typeof(ClientCredentials));
    client.ChannelFactory.Endpoint.Behaviors.Add(samlCC);
    ```

 No serviço, as declarações associadas ao chamador são exibidas. Quando você executa o exemplo, as solicitações de operação e as respostas são exibidas na janela do console do cliente. Pressione ENTER na janela do cliente para desligar o cliente.

## <a name="setup-batch-file"></a>Arquivo em lotes de instalação
 O arquivo em lotes setup. bat incluído com este exemplo permite que você configure o servidor com o certificado relevante para executar um aplicativo auto-hospedado que requer segurança baseada em certificado do servidor. Esse arquivo em lotes deve ser modificado para funcionar em computadores ou para funcionar em um caso não hospedado.

 Veja a seguir uma breve visão geral das diferentes seções dos arquivos em lotes para que eles possam ser modificados para serem executados na configuração apropriada.

- Criando o certificado do servidor:

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do servidor a ser usado. A `%SERVER_NAME%` variável especifica o nome do servidor. Altere essa variável para especificar seu próprio nome de servidor. O valor padrão nesse arquivo em lotes é localhost.

     O certificado é armazenado no meu repositório (pessoal) no local de armazenamento de LocalMachine.

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr LocalMachine -ss My -a sha1 -n CN=%SERVER_NAME% -sky exchange -pe
    ```

- Instalando o certificado do servidor no repositório de certificados confiáveis do cliente:

     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — essa etapa de popular o repositório de certificados do cliente com o certificado do servidor não será necessária.

    ```console
    certmgr.exe -add -r LocalMachine -s My -c -n %SERVER_NAME% -r LocalMachine -s TrustedPeople
    ```

- Criando o certificado do emissor.

     As linhas a seguir do arquivo em lotes setup. bat criam o certificado do emissor a ser usado. A `%USER_NAME%` variável especifica o nome do emissor. Altere essa variável para especificar seu próprio nome de emissor. O valor padrão nesse arquivo em lotes é Alice.

     O certificado é armazenado em meu repositório no local de armazenamento CurrentUser.

    ```console
    echo ************
    echo Server cert setup starting
    echo %SERVER_NAME%
    echo ************
    echo making server cert
    echo ************
    makecert.exe -sr CurrentUser -ss My -a sha1 -n CN=%USER_NAME% -sky exchange -pe
    ```

- Instalar o certificado do emissor no repositório de certificados confiáveis do servidor.

     As linhas a seguir no arquivo em lotes setup. bat copiam o certificado do servidor no repositório de pessoas confiáveis do cliente. Essa etapa é necessária porque os certificados gerados pelo MakeCert. exe não são implicitamente confiáveis pelo sistema cliente. Se você já tiver um certificado com raiz em um certificado raiz confiável do cliente — por exemplo, um certificado emitido pela Microsoft — esta etapa de popular o repositório de certificados do servidor com o certificado do emissor não será necessária.

    ```console
    certmgr.exe -add -r CurrentUser -s My -c -n %USER_NAME% -r LocalMachine -s TrustedPeople
    ```

#### <a name="to-set-up-and-build-the-sample"></a>Para configurar e compilar o exemplo

1. Verifique se você executou o [procedimento de configuração única para os exemplos de Windows Communication Foundation](one-time-setup-procedure-for-the-wcf-samples.md).

2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md).

> [!NOTE]
> Se você usar svcutil. exe para regenerar a configuração para este exemplo, certifique-se de modificar o nome do ponto de extremidade na configuração do cliente para corresponder ao código do cliente.

#### <a name="to-run-the-sample-on-the-same-computer"></a>Para executar o exemplo no mesmo computador

1. Execute setup. bat da pasta de instalação de exemplo dentro de um prompt de comando do Visual Studio 2012 executado com privilégios de administrador. Isso instala todos os certificados necessários para executar o exemplo.

    > [!NOTE]
    > O arquivo em lotes setup. bat foi projetado para ser executado em um prompt de comando do Visual Studio 2012. A variável de ambiente PATH definida no prompt de comando do Visual Studio 2012 aponta para o diretório que contém os executáveis exigidos pelo script setup. bat.  
  
2. Inicie o Service. exe em service\bin.  
  
3. Inicie o Client. exe em \client\bin. A atividade do cliente é exibida no aplicativo de console do cliente.  
  
4. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-run-the-sample-across-computers"></a>Para executar o exemplo entre computadores  
  
1. Crie um diretório no computador de serviço para os binários de serviço.  
  
2. Copie os arquivos de programa do serviço para o diretório de serviço no computador do serviço. Copie também os arquivos Setup. bat e Cleanup. bat para o computador de serviço.  
  
3. Você deve ter um certificado de servidor com o nome da entidade que contém o nome de domínio totalmente qualificado do computador. O arquivo Service. exe. config deve ser atualizado para refletir esse novo nome de certificado. Você pode criar um certificado de servidor modificando o arquivo em lotes setup. bat. Observe que o arquivo setup. bat deve ser executado em um Prompt de Comando do Desenvolvedor para que a janela do Visual Studio seja aberta com privilégios de administrador. Você deve definir a `%SERVER_NAME%` variável para o nome de host totalmente qualificado do computador que é usado para hospedar o serviço.  
  
4. Copie o certificado do servidor no repositório CurrentUser-TrustedPeople do cliente. Essa etapa não é necessária quando o certificado do servidor é emitido por um emissor confiável do cliente.  
  
5. No arquivo Service. exe. config no computador do serviço, altere o valor do endereço base para especificar um nome de computador totalmente qualificado em vez de localhost.  
  
6. No computador do serviço, execute o Service. exe em um prompt de comando.  
  
7. Copie os arquivos de programas do cliente da pasta \client\bin\, na pasta específica do idioma, para o computador cliente.  
  
8. No arquivo client. exe. config no computador cliente, altere o valor de endereço do ponto de extremidade para corresponder ao novo endereço do serviço.  
  
9. No computador cliente, inicie `Client.exe` a partir de uma janela de prompt de comando.  
  
10. Se o cliente e o serviço não puderem se comunicar, consulte [dicas de solução de problemas para exemplos do WCF](https://docs.microsoft.com/previous-versions/dotnet/netframework-3.5/ms751511(v=vs.90)).  
  
#### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. bat na pasta Samples depois de concluir a execução do exemplo.  
