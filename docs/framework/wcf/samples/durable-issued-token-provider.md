---
title: Provedor de tokens emitidos duráveis
ms.date: 03/30/2017
ms.assetid: 76fb27f5-8787-4b6a-bf4c-99b4be1d2e8b
ms.openlocfilehash: fed5f44e6cc40cfe2ca963077b6371c14b3b086a
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84600550"
---
# <a name="durable-issued-token-provider"></a>Provedor de tokens emitidos duráveis
Este exemplo demonstra como implementar um provedor de token emitido pelo cliente personalizado.  
  
## <a name="discussion"></a>Discussão  
 Um provedor de token no Windows Communication Foundation (WCF) é usado para fornecer credenciais para a infraestrutura de segurança. O provedor de token em geral examina o destino e emite as credenciais apropriadas para que a infraestrutura de segurança possa proteger a mensagem. O WCF é fornecido com um provedor de token do CardSpace. Os provedores de token personalizados são úteis nos seguintes casos:  
  
- Se você tiver um repositório de credenciais para o qual o provedor de token interno não pode operar.  
  
- Se você quiser fornecer seu próprio mecanismo personalizado para transformar as credenciais do ponto em que o usuário fornece os detalhes quando o cliente WCF usa as credenciais.  
  
- Se você estiver criando um token personalizado.  
  
 Este exemplo mostra como criar um provedor de token personalizado que armazena em cache os tokens emitidos por um serviço de token de segurança (STS).  
  
 Para resumir, este exemplo demonstra o seguinte:  
  
- Como um cliente pode ser configurado com um provedor de token personalizado.  
  
- Como os tokens emitidos podem ser armazenados em cache e fornecidos para o cliente WCF.  
  
- Como o servidor é autenticado pelo cliente usando o certificado X. 509 do servidor.  
  
 Este exemplo consiste em um programa de console do cliente (Client. exe), um programa de console do serviço de token de segurança (SecurityTokenService. exe) e um programa de console do serviço (Service. exe). O serviço implementa um contrato que define um padrão de comunicação de solicitação-resposta. O contrato é definido pela `ICalculator` interface, que expõe operações matemáticas (adicionar, subtrair, multiplicar e dividir). O cliente obtém um token de segurança do STS (serviço de token de segurança) e faz solicitações síncronas para o serviço para uma determinada operação matemática e o serviço responde com o resultado. A atividade do cliente fica visível na janela do console.  
  
> [!NOTE]
> Os procedimentos de instalação e as instruções de compilação para esse exemplo estão localizadas no final deste tópico.  
  
 Este exemplo expõe o contrato ICalculator usando o [\<wsHttpBinding>](../../configure-apps/file-schema/wcf/wshttpbinding.md) . A configuração dessa associação no cliente é mostrada no código a seguir.  
  
```xml  
<bindings>
  <wsFederationHttpBinding>
    <binding name="ServiceFed">
      <security mode="Message">
        <message issuedKeyType="SymmetricKey"
                 issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">
          <issuer address="http://localhost:8000/sts/windows"
                  binding="wsHttpBinding" />
        </message>
      </security>
    </binding>
  </wsFederationHttpBinding>
</bindings>  
```  
  
 No `security` elemento de `wsFederationHttpBinding` , o `mode` valor configura qual modo de segurança deve ser usado. Neste exemplo, a segurança das mensagens está sendo usada, motivo pelo qual o `message` elemento de `wsFederationHttpBinding` é especificado dentro do `security` elemento de `wsFederationHttpBinding` . O `issuer` elemento de `wsFederationHttpBinding` dentro do `message` elemento de `wsFederationHttpBinding` especifica o endereço e a associação para o serviço de token de segurança que emite um token de segurança para o cliente para que o cliente possa se autenticar no serviço de calculadora.  
  
 A configuração dessa associação no serviço é mostrada no código a seguir.  
  
```xml  
<bindings>
  <wsFederationHttpBinding>
    <binding name="ServiceFed">
      <security mode="Message">
        <message issuedKeyType="SymmetricKey"
                 issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">
          <issuerMetadata address="http://localhost:8000/sts/mex">
            <identity>
              <certificateReference storeLocation="CurrentUser"
                                    storeName="TrustedPeople"
                                    x509FindType="FindBySubjectDistinguishedName"
                                    findValue="CN=STS" />
            </identity>
          </issuerMetadata>
        </message>
      </security>
    </binding>
  </wsFederationHttpBinding>
</bindings>  
```  
  
 No `security` elemento de `wsFederationHttpBinding` , o `mode` valor configura qual modo de segurança deve ser usado. Neste exemplo, a segurança das mensagens está sendo usada, motivo pelo qual o `message` elemento de `wsFederationHttpBinding` é especificado dentro do `security` elemento de `wsFederationHttpBinding` . O `issuerMetadata` elemento de `wsFederationHttpBinding` dentro do `message` elemento de `wsFederationHttpBinding` especifica o endereço e a identidade de um ponto de extremidade que pode ser usado para recuperar metadados para o serviço de token de segurança.  
  
 O comportamento do serviço é mostrado no código a seguir.  
  
```xml  
<behavior name="ServiceBehavior">
  <serviceDebug includeExceptionDetailInFaults="true" />
  <serviceMetadata httpGetEnabled="true" />
  <serviceCredentials>
    <issuedTokenAuthentication>
      <knownCertificates>
        <add storeLocation="LocalMachine"
              storeName="TrustedPeople"
              x509FindType="FindBySubjectDistinguishedName"
              findValue="CN=STS" />
      </knownCertificates>
    </issuedTokenAuthentication>
    <serviceCertificate storeLocation="LocalMachine"
                        storeName="My"
                        x509FindType="FindBySubjectDistinguishedName"
                        findValue="CN=localhost" />
  </serviceCredentials>
</behavior>  
```  
  
 O `issuedTokenAuthentication` elemento dentro do `serviceCredentials` elemento permite que o serviço especifique restrições nos tokens que permite que os clientes apresentem durante a autenticação. Essa configuração especifica que os tokens assinados por um certificado cujo nome da entidade é CN = STS são aceitos pelo serviço.  
  
 O serviço de token de segurança expõe um único ponto de extremidade usando o wsHttpBinding padrão. O serviço de token de segurança responde à solicitação de clientes para tokens e, desde que o cliente seja autenticado usando uma conta do Windows, emite um token que contém o nome de usuário do cliente como uma declaração no token emitido. Como parte da criação do token, o serviço de token de segurança assina o token usando a chave privada associada ao certificado CN = STS. Além disso, ele cria uma chave simétrica e a criptografa usando a chave pública associada ao certificado CN = localhost. Ao retornar o token para o cliente, o serviço de token de segurança também retorna a chave simétrica. O cliente apresenta o token emitido para o serviço de calculadora e comprova que ele conhece a chave simétrica assinando a mensagem com essa chave.  
  
## <a name="custom-client-credentials-and-token-provider"></a>Credenciais de cliente personalizadas e provedor de token  
 As etapas a seguir mostram como desenvolver um provedor de token personalizado que armazena em cache os tokens emitidos e o integra com o WCF: segurança.  
  
### <a name="to-develop-a-custom-token-provider"></a>Para desenvolver um provedor de token personalizado  
  
1. Escreva um provedor de token personalizado.  
  
     O exemplo implementa um provedor de token personalizado que retorna um token de segurança recuperado de um cache.  
  
     Para executar essa tarefa, o provedor de token personalizado deriva a <xref:System.IdentityModel.Selectors.SecurityTokenProvider> classe e substitui o <xref:System.IdentityModel.Selectors.SecurityTokenProvider.GetTokenCore%2A> método. Esse método tenta obter um token do cache ou, se um token não puder ser encontrado no cache, recuperará um token do provedor subjacente e, em seguida, armazenará esse token em cache. Em ambos os casos, o método retorna um `SecurityToken` .  
  
    ```csharp  
    protected override SecurityToken GetTokenCore(TimeSpan timeout)  
    {  
      GenericXmlSecurityToken token;  
      if (!this.cache.TryGetToken(target, issuer, out token))  
      {  
        token = (GenericXmlSecurityToken) this.innerTokenProvider.GetToken(timeout);  
        this.cache.AddToken(token, target, issuer);  
      }  
      return token;  
    }  
    ```  
  
2. Gravar Gerenciador de token de segurança personalizado.  
  
     O <xref:System.IdentityModel.Selectors.SecurityTokenManager> é usado para criar um <xref:System.IdentityModel.Selectors.SecurityTokenProvider> para um específico <xref:System.IdentityModel.Selectors.SecurityTokenRequirement> que é passado para ele no `CreateSecurityTokenProvider` método. O Gerenciador de token de segurança também é usado para criar autenticadores de token e serializadores de token, mas eles não são cobertos por esse exemplo. Neste exemplo, o Gerenciador de token de segurança personalizado herda da <xref:System.ServiceModel.ClientCredentialsSecurityTokenManager> classe e substitui o `CreateSecurityTokenProvider` método para retornar o provedor de token personalizado quando os requisitos de token passados indicam que um token emitido é solicitado.  
  
    ```csharp
    class DurableIssuedTokenClientCredentialsTokenManager :  
     ClientCredentialsSecurityTokenManager  
    {  
      IssuedTokenCache cache;  
  
      public DurableIssuedTokenClientCredentialsTokenManager ( DurableIssuedTokenClientCredentials creds ): base(creds)  
      {  
        this.cache = creds.IssuedTokenCache;  
      }  
  
      public override SecurityTokenProvider CreateSecurityTokenProvider ( SecurityTokenRequirement tokenRequirement )  
      {  
        if (IsIssuedSecurityTokenRequirement(tokenRequirement))  
        {  
          return new DurableIssuedSecurityTokenProvider ((IssuedSecurityTokenProvider)base.CreateSecurityTokenProvider( tokenRequirement), this.cache);  
        }  
        else
        {  
          return base.CreateSecurityTokenProvider(tokenRequirement);  
        }  
      }  
    }  
    ```  
  
3. Grave uma credencial de cliente personalizada.  
  
     Uma classe de credenciais de cliente é usada para representar as credenciais que são configuradas para o proxy do cliente e cria o Gerenciador de token de segurança que é usado para obter autenticadores de token, provedores de token e serializadores de token.  
  
    ```csharp
    public class DurableIssuedTokenClientCredentials : ClientCredentials  
    {  
      IssuedTokenCache cache;  
  
      public DurableIssuedTokenClientCredentials() : base()  
      {  
      }  
  
      DurableIssuedTokenClientCredentials ( DurableIssuedTokenClientCredentials other) : base(other)  
      {  
        this.cache = other.cache;  
      }  
  
      public IssuedTokenCache IssuedTokenCache  
      {  
        get  
        {  
          return this.cache;  
        }  
        set  
        {  
          this.cache = value;  
        }  
      }  
  
      protected override ClientCredentials CloneCore()  
      {  
        return new DurableIssuedTokenClientCredentials(this);  
      }  
  
      public override SecurityTokenManager CreateSecurityTokenManager()  
      {  
        return new DurableIssuedTokenClientCredentialsTokenManager ((DurableIssuedTokenClientCredentials)this.Clone());  
      }  
    }  
    ```  
  
4. Implemente o cache de token. A implementação de exemplo usa uma classe base abstrata por meio da qual os consumidores de um determinado cache de token interagem com o cache.  
  
    ```csharp
    public abstract class IssuedTokenCache  
    {  
      public abstract void AddToken ( GenericXmlSecurityToken token, EndpointAddress target, EndpointAddress issuer);  
      public abstract bool TryGetToken(EndpointAddress target, EndpointAddress issuer, out GenericXmlSecurityToken cachedToken);  
    }  
    // Configure the client to use the custom client credential.  
    ```  
  
     Para que o cliente use a credencial de cliente personalizada, o exemplo exclui a classe de credencial do cliente padrão e fornece a nova classe de credencial do cliente.  
  
    ```csharp
    clientFactory.Endpoint.Behaviors.Remove<ClientCredentials>();  
    DurableIssuedTokenClientCredentials durableCreds = new DurableIssuedTokenClientCredentials();  
    durableCreds.IssuedTokenCache = cache;  
    durableCreds.ServiceCertificate.Authentication.CertificateValidationMode = X509CertificateValidationMode.PeerOrChainTrust;  
    clientFactory.Endpoint.Behaviors.Add(durableCreds);  
    ```  
  
## <a name="running-the-sample"></a>Executando o exemplo  
 Consulte as instruções a seguir para executar o exemplo. Quando você executa o exemplo, a solicitação para o token de segurança é mostrada na janela do console do serviço de token de segurança. As solicitações e respostas da operação são exibidas nas janelas do console do cliente e do serviço. Pressione ENTER em qualquer uma das janelas do console para desligar o aplicativo.  
  
## <a name="the-setupcmd-batch-file"></a>O arquivo em lotes setup. cmd  
 O arquivo em lotes setup. cmd incluído neste exemplo permite que você configure o servidor e o serviço de token de segurança com certificados relevantes para executar um aplicativo auto-hospedado. O arquivo em lotes cria dois certificados no repositório de certificados CurrentUser/TrustedPeople. O primeiro certificado tem um nome de assunto de CN = STS e é usado pelo serviço de token de segurança para assinar os tokens de segurança que ele emite para o cliente. O segundo certificado tem um nome de assunto de CN = localhost e é usado pelo serviço de token de segurança para criptografar um segredo para que o serviço possa descriptografá-lo.  
  
### <a name="to-set-up-build-and-run-the-sample"></a>Para configurar, compilar, e executar o exemplo  
  
1. Execute o arquivo setup. cmd para criar os certificados necessários.  
  
2. Para compilar a solução, siga as instruções em [criando os exemplos de Windows Communication Foundation](building-the-samples.md). Verifique se todos os projetos na solução são criados (Shared, RSTRSTR, Service, SecurityTokenService e Client).  
  
3. Verifique se o Service. exe e o SecurityTokenService. exe estão sendo executados com privilégios de administrador.  
  
4. Execute Client. exe.  
  
### <a name="to-clean-up-after-the-sample"></a>Para limpar após o exemplo  
  
1. Execute o Cleanup. cmd na pasta Samples depois de concluir a execução do exemplo.  
  
> [!IMPORTANT]
> Os exemplos podem já estar instalados no seu computador. Verifique o seguinte diretório (padrão) antes de continuar.  
>
> `<InstallDrive>:\WF_WCF_Samples`  
>
> Se esse diretório não existir, vá para [Windows Communication Foundation (WCF) e exemplos de Windows Workflow Foundation (WF) para .NET Framework 4](https://www.microsoft.com/download/details.aspx?id=21459) para baixar todos os Windows Communication Foundation (WCF) e [!INCLUDE[wf1](../../../../includes/wf1-md.md)] exemplos. Este exemplo está localizado no seguinte diretório.  
>
> `<InstallDrive>:\WF_WCF_Samples\WCF\Extensibility\Security\DurableIssuedTokenProvider`  
