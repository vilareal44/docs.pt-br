---
title: <message> de <netHttpBinding>
ms.date: 03/30/2017
ms.assetid: 9def5a35-475d-40d6-b716-ccdbd93863c7
ms.openlocfilehash: 62b1793d18ddc8edc1f55b02137c4e0a9f7327d2
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73738964"
---
# <a name="message-of-nethttpbinding"></a>\<message> de \<netHttpBinding>
Define as configurações de segurança em nível de mensagem do [\<netHttpBinding>](nethttpbinding.md) .  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<netHttpBinding>**](nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-nethttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<message>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<message algorithmSuite="Basic128/Basic192/Basic256/Basic128Rsa15/Basic256Rsa15/TripleDes/TripleDesRsa15/Basic128Sha256/Basic192Sha256/TripleDesSha256/Basic128Sha256Rsa15/Basic192Sha256Rsa15/Basic256Sha256Rsa15/TripleDesSha256Rsa15"
         clientCredentialType="UserName/Certificate" />
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|algorithmSuite|Define a criptografia de mensagem e os algoritmos de encapsulamento de chaves. Esse atributo é do tipo <xref:System.ServiceModel.Security.SecurityAlgorithmSuite> , que especifica os algoritmos e os tamanhos de chave. Esses algoritmos são mapeados para aqueles especificados na especificação do WS-SecurityPolicy (Security Policy Language).<br /><br /> O valor padrão é `Basic256`.|  
|clientCredentialType|Especifica o tipo de credencial a ser usado ao executar a autenticação de cliente usando a segurança baseada em mensagem. O padrão é `UserName`.|  
  
## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype  
  
|Valor|Descrição|  
|-----------|-----------------|  
|UserName|-Requer que o cliente seja autenticado no servidor com uma credencial de nome de usuário. Essa credencial precisa ser especificada usando o `clientCredentials` elemento <>.<br />-O WCF não dá suporte ao envio de um resumo de senha ou à derivação de chaves usando senhas e usando essas chaves para segurança de mensagem. Portanto, o WCF impõe que o transporte seja protegido ao usar credenciais de nome de usuário. Para o `basicHttpBinding` , isso requer a configuração de um canal SSL.|  
|Certificado|Requer que o cliente seja autenticado no servidor usando um certificado. Nesse caso, a credencial do cliente precisa ser especificada usando <`clientCredentials`> e a `clientCertificate`> de <. Além disso, ao usar o modo de segurança de mensagem, o cliente precisa ser provisionado com o certificado de serviço. Nesse caso, a credencial de serviço precisa ser especificada usando o <xref:System.ServiceModel.Description.ClientCredentials> `ClientCredentials` elemento de classe ou comportamento e especificando o certificado de serviço usando o \<serviceCertificate> elemento de ServiceCredentials.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|<`security`> elemento de <`netHttpBinding`>|Define os recursos de segurança para o `netHttpBinding` elemento <>.|  
  
## <a name="example"></a>Exemplo  
 Este exemplo demonstra como implementar um aplicativo que usa a basicHttpBinding e a segurança da mensagem. No exemplo de configuração a seguir para um serviço, a definição do ponto de extremidade especifica basicHttpBinding e faz referência a uma configuração de associação denominada `Binding1` . O certificado que o serviço usa para se autenticar para o cliente é definido na `behaviors` seção do arquivo de configuração sob o `serviceCredentials` elemento. O modo de validação que se aplica ao certificado que o cliente usa para se autenticar para o serviço também é definido na `behaviors` seção sob o `clientCertificate` elemento.  
  
 Os mesmos detalhes de ligação e segurança são especificados no arquivo de configuração do cliente.  
  
```xml  
<system.serviceModel>
  <services>
    <service name="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <host>
        <baseAddresses>
          <add baseAddress="http://localhost:8000/ServiceModelSamples/service" />
        </baseAddresses>
      </host>
      <!-- this endpoint is exposed at the base address provided by host: http://localhost:8000/ServiceModelSamples/service  -->
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
      <!-- the mex endpoint is exposed at http://localhost:8000/ServiceModelSamples/service/mex -->
      <endpoint address="mex"
                binding="mexHttpBinding"
                contract="IMetadataExchange" />
    </service>
  </services>
  <bindings>
    <basicHttpBinding>
      <!-- This configuration defines the SecurityMode as Message and
           the clientCredentialType as Certificate. -->
      <binding name="Binding1" >
        <security mode = "Message">
          <message clientCredentialType="Certificate" />
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
  <!--For debugging purposes set the includeExceptionDetailInFaults attribute to true-->
  <behaviors>
    <serviceBehaviors>
      <behavior name="CalculatorServiceBehavior">
        <serviceMetadata httpGetEnabled="True" />
        <serviceDebug includeExceptionDetailInFaults="False" />
        <!-- The serviceCredentials behavior allows one to define a service certificate.
             A service certificate is used by a client to authenticate the service and provide message protection.
             This configuration references the "localhost" certificate installed during the setup instructions. -->
        <serviceCredentials>
          <serviceCertificate findValue="localhost"
                              storeLocation="LocalMachine"
                              storeName="My"
                              x509FindType="FindBySubjectName" />
          <clientCertificate>
            <!-- Setting the certificateValidationMode to PeerOrChainTrust means that if the certificate
                 is in the user's Trusted People store, then it will be trusted without performing a
                 validation of the certificate's issuer chain. This setting is used here for convenience so that the
                 sample can be run without having to have certificates issued by a certification authority (CA).
                 This setting is less secure than the default, ChainTrust. The security implications of this
                 setting should be carefully considered before using PeerOrChainTrust in production code. -->
            <authentication certificateValidationMode="PeerOrChainTrust" />
          </clientCertificate>
        </serviceCredentials>
      </behavior>
    </serviceBehaviors>
  </behaviors>
</system.serviceModel>
```  
  
## <a name="see-also"></a>Confira também

- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
