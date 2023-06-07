---
title: <transport> de <basicHttpBinding>
ms.date: 03/30/2017
ms.assetid: 4c5ba293-3d7e-47a6-b84e-e9022857b7e5
ms.openlocfilehash: c563339e4f854cc4e60f92dd5b8c0b39112dc000
ms.sourcegitcommit: b16c00371ea06398859ecd157defc81301c9070f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/06/2020
ms.locfileid: "73736113"
---
# <a name="transport-of-basichttpbinding"></a>\<transport> de \<basicHttpBinding>
Define as propriedades que controlam os parâmetros de autenticação para o transporte HTTP.  
  
[**\<configuration>**](../configuration-element.md)\
&nbsp;&nbsp;[**\<system.serviceModel>**](system-servicemodel.md)\
&nbsp;&nbsp;&nbsp;&nbsp;[**\<bindings>**](bindings.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<basicHttpBinding>**](basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<binding>**\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;[**\<security>**](security-of-basichttpbinding.md)\
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;**\<transport>**  
  
## <a name="syntax"></a>Sintaxe  
  
```xml  
<basicHttpBinding>
  <binding>
    <security mode="None|Transport|Message|TransportWithMessageCredential|TransportCredentialOnly">
      <transport clientCredentialType="None|Basic|Digest|Ntlm|Windows"
                 proxyCredentialType="None|Basic|Digest|Ntlm|Windows"
                 realm="String">
        <extendedProtectionPolicy policyEnforcement="Never|WhenSupported|Always"
                                  protectionScenario="TransportSelected|TrustedProxy">
          <customServiceNames>
          </customServiceNames>
        </extendedProtectionPolicy>
      </transport>
    </security>
  </binding>
</basicHttpBinding>
```  
  
## <a name="attributes-and-elements"></a>Atributos e elementos  
 As seções a seguir descrevem atributos, elementos filho e elementos pai.  
  
### <a name="attributes"></a>Atributos  
  
|Atributo|Descrição|  
|---------------|-----------------|  
|clientCredentialType|-Especifica o tipo de credencial a ser usado ao executar a autenticação de cliente usando a autenticação HTTP.  O padrão é `None`. Esse atributo é do tipo <xref:System.ServiceModel.HttpClientCredentialType> .|  
|proxyCredentialType|-Especifica o tipo de credencial a ser usado ao executar a autenticação de cliente de dentro de um domínio usando um proxy sobre HTTP. Esse atributo é aplicável somente quando o `mode` atributo do elemento pai `security` é `Transport` ou `TransportCredentialsOnly` . Esse atributo é do tipo <xref:System.ServiceModel.HttpProxyCredentialType> .|  
|territórios|Uma cadeia de caracteres que especifica o realm usado pelo esquema de autenticação HTTP para Digest ou autenticação básica. O padrão é uma cadeia de caracteres vazia.|  
|policyEnforcement|Essa enumeração especifica quando o <xref:System.Security.Authentication.ExtendedProtection.ExtendedProtectionPolicy> deve ser imposto.<br /><br /> 1. nunca – a política nunca é imposta (a proteção estendida está desabilitada).<br />2. WhenSupported – a política será imposta somente se o cliente oferecer suporte à proteção estendida.<br />3. sempre – a política é sempre imposta. Os clientes que não dão suporte à proteção estendida não serão autenticados.|  
|protectionScenario|Essa enumeração Especifica o cenário de proteção imposto pela política.|  
  
## <a name="clientcredentialtype-attribute"></a>Atributo clientCredentialtype  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|As mensagens não são protegidas durante a transferência.|  
|Basic|Especifica autenticação básica.|  
|Digest|Especifica a autenticação Digest.|  
|Ntlm|Especifica a autenticação NTLM quando possível, e se a autenticação do Windows falhar.|  
|Windows|Especifica a autenticação integrada do Windows.|  
  
## <a name="proxycredentialtype-attribute"></a>Atributo proxyCredentialType  
  
|Valor|Descrição|  
|-----------|-----------------|  
|Nenhum|-As mensagens não são protegidas durante a transferência.|  
|Basic|Especifica a autenticação básica, conforme definido pela RFC 2617 – autenticação HTTP: autenticação básica e resumida.|  
|Digest|Especifica a autenticação Digest, conforme definido pela RFC 2617 – autenticação HTTP: autenticação básica e resumida.|  
|Ntlm|Especifica a autenticação NTLM quando possível, e se a autenticação do Windows falhar.|  
|Windows|Especifica a autenticação integrada do Windows.|  
|Certificado|Executa a autenticação de cliente usando um certificado. Essa opção só funcionará se o `Mode` atributo do `security` elemento pai estiver definido como transporte e não funcionará se estiver definido como TransportCredentialOnly.|  
  
### <a name="child-elements"></a>Elementos filho  
 Nenhum  
  
### <a name="parent-elements"></a>Elementos pai  
  
|Elemento|Descrição|  
|-------------|-----------------|  
|[\<security>](security-of-basichttpbinding.md)|Define os recursos de segurança para o [\<basicHttpBinding>](basichttpbinding.md) .|  
  
## <a name="example"></a>Exemplo  
 O exemplo a seguir demonstra o uso de segurança de transporte SSL com a associação básica. Por padrão, a associação básica dá suporte à comunicação HTTP.  
  
```xml  
<system.serviceModel>
  <services>
    <service type="Microsoft.ServiceModel.Samples.CalculatorService"
             behaviorConfiguration="CalculatorServiceBehavior">
      <endpoint address=""
                binding="basicHttpBinding"
                bindingConfiguration="Binding1"
                contract="Microsoft.ServiceModel.Samples.ICalculator" />
    </service>
  </services>
  <bindings>
    <basicHttpBinding>
      <!-- Configure basicHttpBinding with Transport security -->
      <!-- mode and clientCredentialType set to None. -->
      <binding name="Binding1">
        <security mode="Transport">
          <transport clientCredentialType="None"
                     proxyCredentialType="None">
            <extendedProtectionPolicy policyEnforcement="WhenSupported"
                                      protectionScenario="TransportSelected">
              <customServiceNames>
              </customServiceNames>
            </extendedProtectionPolicy>
          </transport>
        </security>
      </binding>
    </basicHttpBinding>
  </bindings>
</system.serviceModel>
```  
  
## <a name="see-also"></a>Confira também

- <xref:System.ServiceModel.Configuration.BasicHttpSecurityElement.Transport%2A>
- <xref:System.ServiceModel.BasicHttpSecurity.Transport%2A>
- <xref:System.ServiceModel.Configuration.HttpTransportSecurityElement>
- <xref:System.ServiceModel.HttpTransportSecurity>
- [Protegendo serviços e clientes](../../../wcf/feature-details/securing-services-and-clients.md)
- [Associações](../../../wcf/bindings.md)
- [Configurando associações fornecidas pelo sistema](../../../wcf/feature-details/configuring-system-provided-bindings.md)
- [Usando associações para configurar serviços e clientes](../../../wcf/using-bindings-to-configure-services-and-clients.md)
- [\<binding>](bindings.md)
