---
title: Federação
ms.date: 03/30/2017
dev_langs:
- csharp
- vb
helpviewer_keywords:
- WCF, federation
- federation [WCF]
ms.assetid: 2f1e646f-8361-48d4-9d5d-1b961f31ede4
ms.openlocfilehash: c31c2612b595e627b0c4c2d7fbb3a359b19ee704
ms.sourcegitcommit: cdb295dd1db589ce5169ac9ff096f01fd0c2da9d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84595486"
---
# <a name="federation"></a>Federação
Este tópico fornece uma breve visão geral do conceito de segurança federada. Ele também descreve o suporte a Windows Communication Foundation (WCF) para implantar arquiteturas de segurança federadas. Para obter um aplicativo de exemplo que demonstre a Federação, consulte [exemplo de Federação](../samples/federation-sample.md).  
  
## <a name="definition-of-federated-security"></a>Definição de segurança federada  
 A segurança federada permite uma separação clara entre o serviço que um cliente está acessando e os procedimentos de autenticação e autorização associados. A segurança federada também permite a colaboração entre vários sistemas, redes e organizações em territórios de confiança diferentes.  
  
 O WCF oferece suporte para a criação e implantação de sistemas distribuídos que empregam a segurança federada.  
  
### <a name="elements-of-a-federated-security-architecture"></a>Elementos de uma arquitetura de segurança federada  
 A arquitetura de segurança federada tem três elementos principais, conforme descrito na tabela a seguir.  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|Domínio/realm|Uma única unidade de administração ou confiança de segurança. Um domínio típico pode incluir uma única organização.|  
|Federação|Uma coleção de domínios que estabeleceram confiança. O nível de confiança pode variar, mas normalmente inclui a autenticação e quase sempre inclui a autorização. Uma federação típica pode incluir um número de organizações que estabeleceram confiança para acesso compartilhado a um conjunto de recursos.|  
|STS (Serviço de Token de Segurança)|Um serviço Web que emite tokens de segurança; ou seja, ele faz asserções com base nas evidências que confiam, para quemá-la a confiar. Isso constitui a base do agente de confiança entre domínios.|  
  
### <a name="example-scenario"></a>Cenário de Exemplo  
 A ilustração a seguir mostra um exemplo de segurança federada:  
  
 ![Diagrama mostrando um cenário típico de segurança federada.](./media/federation/typical-federated-security-scenario.gif)  
  
 Esse cenário inclui duas organizações: A e B. a organização B tem um recurso da Web (um serviço Web) que alguns usuários da organização A encontraram valiosos.  
  
> [!NOTE]
> Esta seção usa o *recurso*de termos, o *serviço*e o *serviço Web* de maneira intercambiável.  
  
 Normalmente, a organização B requer que um usuário da organização A forneça alguma forma válida de autenticação antes de acessar o serviço. Além disso, a organização também pode exigir que o usuário esteja autorizado a acessar o recurso específico em questão. Uma maneira de resolver esse problema e permitir que os usuários na organização a acessem o recurso na organização B é o seguinte:  
  
- Os usuários da organização A registram suas credenciais (um nome de usuário e senha) com a organização B.  
  
- Durante o acesso aos recursos, os usuários da organização A apresentam suas credenciais para a organização B e são autenticados antes de acessar o recurso.  
  
 Essa abordagem tem três desvantagens significativas:  
  
- A organização B precisa gerenciar as credenciais de usuários da organização A, além de gerenciar as credenciais de seus usuários locais.  
  
- Os usuários da organização precisam manter um conjunto adicional de credenciais (ou seja, lembrar-se de um nome de usuário e senha adicionais) Além das credenciais que normalmente usam para obter acesso aos recursos da organização A. Isso geralmente incentiva a prática de usar o mesmo nome de usuário e senha em vários sites de serviço, o que é uma medida de segurança fraca.  
  
- A arquitetura não é dimensionada conforme mais organizações percebem o recurso na organização B como sendo de algum valor.  
  
 Uma abordagem alternativa, que aborda as desvantagens mencionadas anteriormente, é empregar a segurança federada. Nessa abordagem, as organizações A e B estabelecem uma relação de confiança e empregam o serviço de token de segurança (STS) para habilitar o agente da confiança estabelecida.  
  
 Em uma arquitetura de segurança federada, os usuários da organização sabem que se desejarem acessar o serviço Web na organização B que eles devem apresentar um token de segurança válido do STS na organização B, que autentica e autoriza seu acesso ao serviço específico.  
  
 Ao entrar em contato com o STS B, os usuários recebem outro nível de indireção da política associada ao STS. Eles devem apresentar um token de segurança válido do STS A (ou seja, o realm de confiança do cliente) antes que o STS B possa emitir um token de segurança. Trata-se de um registro da relação de confiança estabelecida entre as duas organizações e implica que a organização B não precisa gerenciar identidades para usuários da organização A. Na prática, o STS B geralmente tem um valor nulo `issuerAddress` e `issuerMetadataAddress` . Para obter mais informações, consulte [como: configurar um emissor local](how-to-configure-a-local-issuer.md). Nesse caso, o cliente consulta uma política local para localizar o STS A. Essa configuração é chamada de *Federação de realm inicial* e dimensiona melhor porque o STS B não precisa manter informações sobre o STS a.  
  
 Em seguida, os usuários entram em contato com o STS na organização A e obtêm um token de segurança apresentando as credenciais de autenticação que eles normalmente usam para obter acesso a qualquer outro recurso na organização A. Isso também alivia o problema de usuários que têm de manter vários conjuntos de credenciais ou usando o mesmo conjunto de credenciais em vários sites de serviço.  
  
 Depois que os usuários obtêm um token de segurança do STS A, eles apresentam o token para o STS B. a organização B continua a executar a autorização das solicitações dos usuários e emite um token de segurança para os usuários de seu próprio conjunto de tokens de segurança. Os usuários podem então apresentar seu token para o recurso na organização B e acessar o serviço.  
  
## <a name="support-for-federated-security-in-wcf"></a>Suporte para segurança federada no WCF  
 O WCF fornece suporte completo para implantar arquiteturas de segurança federadas por meio do [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) .  
  
 O [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) elemento fornece uma associação segura, confiável e interoperável que envolve o uso de http como o mecanismo de transporte subjacente para o estilo de comunicação de solicitação-resposta, empregando texto e XML como o formato de conexão para codificação.  
  
 O uso do [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) em um cenário de segurança federada pode ser dissociado em duas fases logicamente independentes, conforme descrito nas seções a seguir.  
  
### <a name="phase-1-design-phase"></a>Fase 1: fase de design  
 Durante a fase de design, o cliente usa a [ferramenta de utilitário de metadados ServiceModel (svcutil. exe)](../servicemodel-metadata-utility-tool-svcutil-exe.md) para ler a política que o ponto de extremidade de serviço expõe e para coletar os requisitos de autenticação e autorização do serviço. Os proxies apropriados são construídos para criar o seguinte padrão de comunicação de segurança federada no cliente:  
  
- Obtenha um token de segurança do STS no realm de confiança do cliente.  
  
- Apresente o token para o STS no realm de confiança do serviço.  
  
- Obtenha um token de segurança do STS no realm de confiança do serviço.  
  
- Apresente o token ao serviço para acessar o serviço.  
  
### <a name="phase-2-run-time-phase"></a>Fase 2: fase de tempo de execução  
 Durante a fase de tempo de execução, o cliente cria uma instância de um objeto da classe WCF Client e faz uma chamada usando o cliente WCF. A estrutura subjacente do WCF manipula as etapas mencionadas anteriormente no padrão de comunicação de segurança federada e permite que o cliente consuma o serviço diretamente.  
  
## <a name="sample-implementation-using-wcf"></a>Exemplo de implementação usando o WCF  
 A ilustração a seguir mostra uma implementação de exemplo para uma arquitetura de segurança federada usando o suporte nativo do WCF.  
  
 ![Diagrama mostrando uma implementação de segurança de Federação de exemplo.](./media/federation/federated-security-implementation.gif)  
  
### <a name="example-myservice"></a>Exemplo de MyService  
 O serviço `MyService` expõe um único ponto de extremidade por meio do `MyServiceEndpoint` . A ilustração a seguir mostra o endereço, a associação e o contrato associados ao ponto de extremidade.  
  
 ![Diagrama mostrando os detalhes de myserviceendpoint.](./media/federation/myserviceendpoint-details.gif)  
  
 O ponto de extremidade de serviço `MyServiceEndpoint` usa o [\<wsFederationHttpBinding>](../../configure-apps/file-schema/wcf/wsfederationhttpbinding.md) e requer um token SAML (Security Asserties Markup Language) válido com uma `accessAuthorized` declaração emitida pelo STS B. Isso é declarativamente especificado na configuração do serviço.  
  
```xml  
<system.serviceModel>  
  <services>  
    <service type="FederationSample.MyService"
        behaviorConfiguration='MyServiceBehavior'>  
        <endpoint address=""  
            binding=" wsFederationHttpBinding"  
            bindingConfiguration='MyServiceBinding'  
            contract="Federation.IMyService" />  
   </service>  
  </services>  
  
  <bindings>  
    <wsFederationHttpBinding>  
    <!-- This is the binding used by MyService. It redirects   
    clients to STS-B. -->  
      <binding name='MyServiceBinding'>  
        <security mode="Message">  
           <message issuedTokenType=  
"http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">  
           <issuer address="http://localhost/FederationSample/STS-B/STS.svc" />  
            <issuerMetadata
           address=  
"http://localhost/FederationSample/STS-B/STS.svc/mex" />  
         <requiredClaimTypes>  
            <add claimType="http://tempuri.org:accessAuthorized" />  
         </requiredClaimTypes>  
        </message>  
      </security>  
      </binding>  
    </wsFederationHttpBinding>  
  </bindings>  
  
  <behaviors>  
    <behavior name='MyServiceBehavior'>  
      <serviceAuthorization
operationRequirementType="FederationSample.MyServiceOperationRequirement, MyService" />  
       <serviceCredentials>  
         <serviceCertificate findValue="CN=FederationSample.com"  
         x509FindType="FindBySubjectDistinguishedName"  
         storeLocation='LocalMachine'  
         storeName='My' />  
      </serviceCredentials>  
    </behavior>  
  </behaviors>  
</system.serviceModel>  
```  
  
> [!NOTE]
> Um ponto sutil deve ser observado sobre as declarações exigidas pelo `MyService` . A segunda figura indica que o `MyService` requer um token SAML com a `accessAuthorized` declaração. Para ser mais preciso, isso especifica o tipo de declaração que o `MyService` requer. O nome totalmente qualificado desse tipo de declaração é `http://tempuri.org:accessAuthorized` (junto com o namespace associado), que é usado no arquivo de configuração de serviço. O valor dessa declaração indica a presença dessa declaração e é considerado como definido `true` pelo STS B.  
  
 Em tempo de execução, essa política é imposta pela `MyServiceOperationRequirement` classe que é implementada como parte do `MyService` .  
  
 [!code-csharp[C_Federation#0](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#0)]
 [!code-vb[C_Federation#0](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#0)]  
[!code-csharp[C_Federation#1](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#1)]
[!code-vb[C_Federation#1](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#1)]  
  
#### <a name="sts-b"></a>STS B  
 A ilustração a seguir mostra o STS B. Como mencionado anteriormente, um STS (serviço de token de segurança) também é um serviço Web e pode ter seus pontos de extremidade associados, política e assim por diante.  
  
 ![Diagrama mostrando o serviço de token de segurança B.](./media/federation/myservice-security-token-service-b.gif)  
  
 O STS B expõe um único ponto de extremidade, chamado `STSEndpoint` que pode ser usado para solicitar tokens de segurança. Especificamente, o STS B emite tokens SAML com a `accessAuthorized` declaração, que pode ser apresentada no `MyService` site do serviço para acessar o serviço. No entanto, o STS B exige que os usuários apresentem um token SAML válido emitido pelo STS A que contém a `userAuthenticated` declaração. Isso é declarativamente especificado na configuração do STS.  
  
```xml  
<system.serviceModel>  
  <services>  
    <service type="FederationSample.STS_B" behaviorConfiguration=  
     "STS-B_Behavior">  
    <endpoint address=""  
              binding="wsFederationHttpBinding"  
              bindingConfiguration='STS-B_Binding'  
      contract="FederationSample.ISts" />  
    </service>  
  </services>  
  <bindings>  
    <wsFederationHttpBinding>  
    <!-- This is the binding used by STS-B. It redirects clients to   
         STS-A. -->  
      <binding name='STS-B_Binding'>  
        <security mode='Message'>  
          <message issuedTokenType="http://docs.oasis-open.org/wss/oasis-wss-saml-token-profile-1.1#SAMLV1.1">  
          <issuer address='http://localhost/FederationSample/STS-A/STS.svc' />  
          <issuerMetadata address='http://localhost/FederationSample/STS-A/STS.svc/mex'/>  
          <requiredClaimTypes>  
            <add claimType='http://tempuri.org:userAuthenticated'/>  
          </requiredClaimTypes>  
          </message>  
        </security>  
    </binding>  
   </wsFederationHttpBinding>  
  </bindings>  
  <behaviors>  
  <behavior name='STS-B_Behavior'>  
    <serviceAuthorization   operationRequirementType='FederationSample.STS_B_OperationRequirement, STS_B' />  
    <serviceCredentials>  
      <serviceCertificate findValue='CN=FederationSample.com'  
      x509FindType='FindBySubjectDistinguishedName'  
       storeLocation='LocalMachine'  
       storeName='My' />  
     </serviceCredentials>  
   </behavior>  
  </behaviors>  
</system.serviceModel>  
```  
  
> [!NOTE]
> Novamente, a `userAuthenticated` declaração é o tipo de declaração exigido pelo STS B. O nome totalmente qualificado desse tipo de declaração é `http://tempuri.org:userAuthenticated` (junto com o namespace associado), que é usado no arquivo de configuração do STS. O valor dessa declaração indica a presença dessa declaração e é considerado como definido `true` pelo STS a.  
  
 Em tempo de execução, a `STS_B_OperationRequirement` classe impõe essa política, que é implementada como parte do STS B.  
  
 [!code-csharp[C_Federation#2](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#2)]
 [!code-vb[C_Federation#2](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#2)]  
  
 Se a verificação de acesso estiver clara, o STS B emitirá um token SAML com a `accessAuthorized` declaração.  
  
 [!code-csharp[C_Federation#3](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#3)]
 [!code-vb[C_Federation#3](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#3)]  
  
#### <a name="sts-a"></a>STS A  
 A ilustração a seguir mostra o STS A.  
  
 ![Federação](media/sts-b.gif "STS_B")  
  
 Semelhante ao STS B, o STS a também é um serviço Web que emite tokens de segurança e expõe um único ponto de extremidade para essa finalidade. No entanto, ele usa uma associação diferente ( `wsHttpBinding` ) e exige que os usuários apresentem um CardSpace válido com uma `emailAddress` declaração. Em resposta, ele emite tokens SAML com a `userAuthenticated` declaração. Isso é declarativamente especificado na configuração do serviço.  
  
```xml  
<system.serviceModel>  
  <services>  
    <service type="FederationSample.STS_A" behaviorConfiguration="STS-A_Behavior">  
      <endpoint address=""  
                binding="wsHttpBinding"  
                bindingConfiguration="STS-A_Binding"  
                contract="FederationSample.ISts">  
       <identity>  
       <certificateReference findValue="CN=FederationSample.com"
                       x509FindType="FindBySubjectDistinguishedName"  
                       storeLocation="LocalMachine"
                       storeName="My" />  
       </identity>  
    </endpoint>  
  </service>  
</services>  
  
<bindings>  
  <wsHttpBinding>  
  <!-- This is the binding used by STS-A. It requires users to present  
   a CardSpace. -->  
    <binding name='STS-A_Binding'>  
      <security mode='Message'>  
        <message clientCredentialType="CardSpace" />  
      </security>  
    </binding>  
  </wsHttpBinding>  
</bindings>  
  
<behaviors>  
  <behavior name='STS-A_Behavior'>  
    <serviceAuthorization operationRequirementType=  
     "FederationSample.STS_A_OperationRequirement, STS_A" />  
      <serviceCredentials>  
  <serviceCertificate findValue="CN=FederationSample.com"  
                     x509FindType='FindBySubjectDistinguishedName'  
                     storeLocation='LocalMachine'  
                     storeName='My' />  
      </serviceCredentials>  
    </behavior>  
  </behaviors>  
</system.serviceModel>  
```  
  
 Em tempo de execução, a `STS_A_OperationRequirement` classe impõe essa política, que é implementada como parte do STS A.  
  
 [!code-csharp[C_Federation#4](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#4)]
 [!code-vb[C_Federation#4](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#4)]  
  
 Se o acesso for `true` , o STS A emite um token SAML com `userAuthenticated` Claim.  
  
 [!code-csharp[C_Federation#5](../../../../samples/snippets/csharp/VS_Snippets_CFX/c_federation/cs/source.cs#5)]
 [!code-vb[C_Federation#5](../../../../samples/snippets/visualbasic/VS_Snippets_CFX/c_federation/vb/source.vb#5)]  
  
### <a name="client-at-organization-a"></a>Cliente na organização A  
 A ilustração a seguir mostra o cliente na organização A, juntamente com as etapas envolvidas em fazer uma `MyService` chamada de serviço. Os outros componentes funcionais também estão incluídos para fins de integridade.  
  
 ![Diagrama mostrando as etapas em uma chamada de serviço de MyService.](./media/federation/federation-myservice-service-call-process.gif)  
  
## <a name="summary"></a>Resumo  
 A segurança federada fornece uma divisão de responsabilidade limpa e ajuda a criar arquiteturas de serviço seguras e escalonáveis. Como uma plataforma para a criação e implantação de aplicativos distribuídos, o WCF fornece suporte nativo para implementar a segurança federada.  
  
## <a name="see-also"></a>Consulte também

- [Segurança](security.md)
